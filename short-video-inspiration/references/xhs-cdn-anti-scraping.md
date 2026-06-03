# XHS CDN Anti-Scraping Details

## Problem
When downloading XHS images from server IPs, the CDN returns **completely wrong images** instead of the actual post images.

## Tested Approaches (all failed)

| Method | CDN Domain | Result |
|--------|-----------|--------|
| MeowLoad API URLs + Referer header | ci.xiaohongshu.com | Wrong images |
| SSR data URLs + Referer header | sns-webpic-qc.xhscdn.com | Wrong images |
| ci.xiaohongshu.com without Referer | ci.xiaohongshu.com | Wrong images (different size) |
| Various User-Agent strings | both | Wrong images |
| Without !h5_1080jpg suffix | sns-webpic-qc.xhscdn.com | Empty response |
| ci.xiaohongshu.com without imageView2 params | ci.xiaohongshu.com | Wrong images (larger size) |

## Root Cause
- XHS CDN checks **IP address range**, not headers
- Cloud server IPs (AWS, etc.) get served different content
- Family broadband IPs get correct images
- No header-based bypass is possible

## What "Wrong Images" Look Like
- Random lifestyle photos
- Chinese calligraphy rubbings
- Tourist photos
- Other XHS posts' images entirely
- Occasionally image 1-2 are correct by coincidence

## XHS Video Posts: NOT Affected ✅
- Video URLs (`sns-video-hw.xhscdn.com`) download correctly from server IPs
- No CDN anti-scraping for video content — only image CDN is affected
- MeowLoad API returns `media_type: "video"` with `resource_url` that works directly

## Working Solutions for Image Posts
1. **User shares images in chat** → use vision analysis / OCR
2. **User syncs via file sync tool** → read from synced directory
3. **Residential proxy IP** (~$2.5/GB for global, China IP costs more)

## MeowLoad API Response Structure

**XHS image posts:**
```json
{
  "text": "Post description|||Content...",
  "medias": [
    {"media_type": "image", "resource_url": "https://ci.xiaohongshu.com/...?imageView2/2/w/0/format/jpg/v3&c=v1"},
    ...
  ],
  "overseas": 0,
  "id": "note_id"
}
```

**XHS video posts:**
```json
{
  "text": "Post description|||Tag content...",
  "medias": [
    {
      "media_type": "video",
      "resource_url": "https://sns-video-hw.xhscdn.com/stream/1/110/258/..._258.mp4",
      "preview_url": "https://sns-webpic.xhscdn.com/...?imageView2/2/w/0/format/jpg",
      "headers": {"Referer": "https://www.xiaohongshu.com/", "User-Agent": "Mozilla/5.0..."}
    }
  ],
  "overseas": 0
}
```
