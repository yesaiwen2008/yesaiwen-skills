<div align="center">

**[中文](./README.md)** · English

# 🎬 Yesaiwen Skills

#### AI Skills I use daily, open-sourced here

[![License](https://img.shields.io/badge/License-MIT-3B82F6?style=for-the-badge)](./LICENSE)
[![Skills](https://img.shields.io/badge/Skills-1-10B981?style=for-the-badge)](#-skills)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-8B5CF6?style=for-the-badge)](https://agentskills.io)

![Hermes](https://img.shields.io/badge/Hermes-Skill-D97706?style=flat-square)
![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-D97706?style=flat-square&logo=anthropic&logoColor=white)
![Codex](https://img.shields.io/badge/Codex-Skill-10B981?style=flat-square&logo=openai&logoColor=white)

</div>

These are skills I've been using in my own workflow for a while. Only the ones that actually save time make it here.

Each Skill is a structured instruction set that agents can load directly, following the [Agent Skills](https://agentskills.io) open standard. Works with Hermes, Claude Code, Codex, and OpenCode.

---

## 📋 Index

| Name | One-liner |
|---|---|
| 🎬 [**short-video-inspiration**](#-short-video-inspiration) | Drop a short video link, auto-extract audio, transcribe, and output actionable inspiration notes |

---

## 📦 Installation

In any Skill-compatible agent (Hermes, Claude Code, Codex, OpenCode), just say:

```
Install this skill: https://github.com/yesaiwen/yesaiwen-skills/tree/main/short-video-inspiration
```

The agent will clone it to the right directory automatically.

**Manual install:**

```bash
# Hermes
cd ~/.hermes/skills/
git clone https://github.com/yesaiwen/yesaiwen-skills.git

# Claude Code
# Copy SKILL.md content into your project's CLAUDE.md or .claude/skills/ directory

# Codex
# Add SKILL.md content to AGENTS.md
```

---

## ✨ Skills

<a id="-skills"></a>

<table>
<tr><td>

### 🎬 short-video-inspiration

> *"Found a great video, want to capture the insight, but typing it out is too slow — just give the link to your Agent."*

Extract actionable inspiration notes from short video links (Douyin, TikTok, Bilibili, Xiaohongshu video posts). Downloads audio via MeowLoad API, transcribes with Whisper, outputs structured notes: **actionable takeaways + full transcript + source link**.

**What it does**

- 📥 Input a video link, auto-extract audio
- 🎯 Output actionable takeaways, not just a summary
- 📝 Structured notes (key points + transcript + source + date)
- 🔀 Auto-merge multiple sources on the same topic into one note
- 🎬 TikTok: prioritizes platform built-in subtitles (more accurate than Whisper)
- 🌐 Supports Douyin, TikTok, Bilibili, Xiaohongshu video posts

**Prerequisites**

1. **MeowLoad API Key** — Get one from [api.meowload.net](https://api.meowload.net), set as env var:
   ```bash
   export MEOWLOAD_API_KEY="your-api-key-here"
   ```
2. **faster-whisper** — `pip install faster-whisper`
3. **ffmpeg** — System-level install

**How to trigger**

```
Extract inspiration from this video: https://v.douyin.com/xxxxx/
Analyze this video: https://www.tiktok.com/xxxxx
What does this video say: https://www.bilibili.com/video/xxxxx
```

**⚠️ Limitations**

- Xiaohongshu **image posts**: CDN anti-scraping blocks image downloads from server IPs (video posts unaffected)
- Whisper `base` model is fast but may have transcription errors; review important content before saving

→ [SKILL.md](./short-video-inspiration/SKILL.md)

</td></tr>
</table>

---

## 🌟 About

I'm Yesaiwen (赛文), an AI content creator focused on beginner-friendly AI tutorials. These are skills I use daily. If they help you, give it a ⭐.

Questions or suggestions? Drop by Issues.

---

<div align="center">

[MIT License](./LICENSE) · Free to use / modify / redistribute

Made by [@yesaiwen](https://github.com/yesaiwen)

</div>
