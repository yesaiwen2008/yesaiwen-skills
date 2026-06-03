<div align="center">

**中文** · [English](./README.en.md)

# 🎬 赛文 Skills

#### 我自己每天在用的 AI Skill，开源在这里

[![License](https://img.shields.io/badge/License-MIT-3B82F6?style=for-the-badge)](./LICENSE)
[![Skills](https://img.shields.io/badge/Skills-1-10B981?style=for-the-badge)](#-skills)
[![AgentSkills](https://img.shields.io/badge/AgentSkills-Standard-8B5CF6?style=for-the-badge)](https://agentskills.io)

![Hermes](https://img.shields.io/badge/Hermes-Skill-D97706?style=flat-square)
![Claude Code](https://img.shields.io/badge/Claude_Code-Skill-D97706?style=flat-square&logo=anthropic&logoColor=white)
![Codex](https://img.shields.io/badge/Codex-Skill-10B981?style=flat-square&logo=openai&logoColor=white)
![OpenCode](https://img.shields.io/badge/OpenCode-Skill-3B82F6?style=flat-square)

</div>

都是在自己工作流里跑通了一段时间，确实省事，才搬出来开源的。

这里的每个 Skill 都是 Agent 能直接加载的结构化指令集，遵循 [Agent Skills](https://agentskills.io) 开放标准。Hermes、Claude Code、Codex、OpenCode 都能装。

---

## 📋 目录

| 名字 | 一句话 |
|---|---|
| 🎬 [**short-video-inspiration（短视频灵感提取）**](#-short-video-inspiration短视频灵感提取) | 丢一个短视频链接，自动下载音频、转文字、输出可实践的灵感笔记 |

---

## 📦 安装方式

在 Hermes、Claude Code、Codex、OpenCode 等支持 Skill 的 Agent 里，直接说：

```
帮我安装这个 skill：https://github.com/yesaiwen/yesaiwen-skills/tree/main/short-video-inspiration
```

Agent 会自己 clone 到对应目录，不用你操心路径。

**手动安装：**

```bash
# Hermes
cd ~/.hermes/skills/
git clone https://github.com/yesaiwen/yesaiwen-skills.git
# 然后按需把 skill 目录移到合适位置

# Claude Code
# 把 SKILL.md 内容复制到项目的 CLAUDE.md 或 .claude/skills/ 目录

# Codex
# 把 SKILL.md 内容添加到 AGENTS.md
```

---

## ✨ Skills

<a id="-skills"></a>

<table>
<tr><td>

### 🎬 short-video-inspiration（短视频灵感提取）

> *"刷到一个好视频，想记下来，但手动打字太慢了——丢个链接给 Agent，它帮你把灵感提出来。"*

从短视频链接（抖音、TikTok、B站、小红书视频帖）自动提取灵感笔记。通过哼哼猫 API 下载视频音频，Whisper 转文字，输出结构化笔记：**可实践要点 + 完整原文 + 来源链接**。

**它能做什么**

- 📥 输入视频链接，自动提取音频
- 🎯 输出可实践的灵感要点，不只是简单复述
- 📝 结构化笔记（要点 + 原文 + 来源链接 + 日期）
- 🔀 同一话题多来源自动合并到一份笔记
- 🎬 TikTok 优先使用平台内置字幕（比 Whisper 更准确）
- 🌐 支持抖音、TikTok、B站、小红书视频帖

**前提条件**

1. **哼哼猫 API Key** — 从 [api.meowload.net](https://api.meowload.net) 获取，设置为环境变量：
   ```bash
   export MEOWLOAD_API_KEY="your-api-key-here"
   ```
2. **faster-whisper** — `pip install faster-whisper`
3. **ffmpeg** — 系统级安装

**怎么触发**

```
帮我提取这个视频的灵感：https://v.douyin.com/xxxxx/
分析一下这个视频：https://www.tiktok.com/xxxxx
这个视频说了什么：https://www.bilibili.com/video/xxxxx
```

**⚠️ 限制**

- 小红书**图文帖**：CDN 反爬导致服务器 IP 无法下载图片（视频帖不受影响）
- Whisper `base` 模型速度快但可能有识别错误，重要内容建议人工校对

→ [SKILL.md](./short-video-inspiration/SKILL.md)

</td></tr>
</table>

---

## 🌟 关于

我是赛文，AI 自媒体科普博主，专注小白 AI 入门，目标是做到全网第一。这些 skill 都是我自己每天在用的，开源出来如果对你有帮助，给个 ⭐ 就行。

有问题或建议，欢迎在 Issues 里说一声。

---

<div align="center">

[MIT License](./LICENSE) · 自由使用 / 修改 / 再分发

Made by [@yesaiwen](https://github.com/yesaiwen)

</div>
