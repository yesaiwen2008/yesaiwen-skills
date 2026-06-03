---
name: short-video-inspiration
description: Extract inspiration notes from short video links (Douyin, TikTok, Bilibili, etc.). Downloads audio via API, transcribes with Whisper, and outputs structured notes.
tools:
  - terminal
  - write_file
  - read_file
  - search_files
---

# Short Video Inspiration Extractor

Extract actionable insights and transcripts from short video links (Douyin / TikTok / Bilibili / Xiaohongshu video posts).

> ⚠️ This skill processes **video links only**. It does NOT handle image-based posts (e.g., Xiaohongshu photo posts).

## Prerequisites

### 1. Video Extraction API (MeowLoad)

You need an API key from [哼哼猫 (MeowLoad)](https://hhm123.com/user/developer). Set it as an environment variable:

```bash
export MEOWLOAD_API_KEY="your-api-key-here"
```

### 2. Audio Transcription

Install [faster-whisper](https://github.com/SYSTRAN/faster-whisper):

```bash
pip install faster-whisper
```

Also requires [ffmpeg](https://ffmpeg.org/) for audio extraction:

```bash
# Ubuntu/Debian
sudo apt install ffmpeg

# macOS
brew install ffmpeg
```

## Workflow

### Step 1: Extract Video URL via API

```bash
curl -s -X POST https://api.meowload.net/openapi/extract/post \
  -H "Content-Type: application/json" \
  -H "x-api-key: $MEOWLOAD_API_KEY" \
  -H "accept-language: zh" \
  -d '{"url": "USER_PROVIDED_URL"}'
```

From the response `medias` array, find the item with `media_type: "video"` and use its `resource_url` to download the video.

<details>
<summary>📖 API Response Structure (click to expand)</summary>

**Video posts:**
```json
{
  "text": "Post description|||Content...",
  "medias": [
    {
      "media_type": "video",
      "resource_url": "https://sns-video-hw.xhscdn.com/stream/1/110/258/..._258.mp4",
      "preview_url": "https://sns-webpic.xhscdn.com/...?imageView2/2/w/0/format/jpg",
      "headers": {"Referer": "https://www.xiaohongshu.com/", "User-Agent": "Mozilla/5.0..."}
    }
  ],
  "overseas": 0,
  "id": "note_id"
}
```

</details>

### Step 2: Download Video & Extract Audio

```bash
# Download video
mkdir -p /tmp/inspiration
curl -s -L -o /tmp/inspiration/video.mp4 "$RESOURCE_URL"

# Extract audio as WAV (16kHz mono, optimized for speech recognition)
ffmpeg -i /tmp/inspiration/video.mp4 -vn -acodec pcm_s16le -ar 16000 -ac 1 /tmp/inspiration/audio.wav -y
```

### Step 3: Transcribe with Whisper

```python
from faster_whisper import WhisperModel

model = WhisperModel("base", device="cpu", compute_type="int8")
segments, info = model.transcribe("/tmp/inspiration/audio.wav", language="zh", vad_filter=True)
for seg in segments:
    print(seg.text)
```

> **Model choice:** `base` is fast (~1 min per 1 min audio). For higher accuracy, use `large-v3` (~5+ min per 1 min audio).

### Step 4: Get Platform Built-in Subtitles (Optional — TikTok)

For TikTok videos, platform subtitles are more accurate than Whisper:

1. `browser_navigate` to open the TikTok video page
2. `browser_console` execute:
   ```javascript
   window['__$UNIVERSAL_DATA$__']['__DEFAULT_SCOPE__']['webapp.video-detail'].itemInfo.itemStruct.video.subtitleInfos
   ```
3. Find the subtitle with `LanguageCodeName: "cmn-Hans-CN"` and get its URL
4. `curl` download the WebVTT content

### Step 5: Generate Notes

**Output location:** User-configurable. Default suggestion: `notes/` directory.

**File naming:** `{topic-keywords}-inspiration.md`

**Note structure:**

```markdown
# {Video Title / Topic}

## Actionable Takeaways

- {Point 1 — practical, not just restating the video}
- {Point 2}
- {Point 3}

---

## Transcript

{Full transcript text}

**Source:** {Platform} — {URL}
**Date:** {YYYY-MM-DD}
```

**For multiple sources on the same topic**, merge into one note:

```markdown
# {Topic}

## Key Points

### From: {Source 1 Title}

- {Points...}

### From: {Source 2 Title}

- {Points...}

---

## Transcripts

### Source 1: {Title}

{Transcript...}

**Link:** {URL}

---

### Source 2: {Title}

{Transcript...}

**Link:** {URL}
```

### Step 6: Output Summary in Chat

Always output a brief summary of actionable takeaways in the chat so the user gets immediate value without opening the file.

## Key Rules

- **API key** must be set via `$MEOWLOAD_API_KEY` environment variable — never hardcode
- **Whisper model:** Use `base` by default for speed; `large-v3` only when accuracy is critical
- **Transcription errors:** Whisper may produce homophone errors (especially for technical terms). Review and correct before saving
- **Tags:** Include `#inspiration` and platform-specific tags (e.g., `#douyin`, `#tiktok`, `#bilibili`)
- **Takeaways** should be actionable and relevant to the user's context — not just a summary of what was said

## Limitations

- **Xiaohongshu image posts:** CDN anti-scraping prevents downloading images from server IPs. Video posts work fine. See [references/xhs-cdn-anti-scraping.md](references/xhs-cdn-anti-scraping.md) for details and workarounds
- **Whisper accuracy:** The `base` model is fast but may misrecognize domain-specific terms. Always review the transcript before saving
