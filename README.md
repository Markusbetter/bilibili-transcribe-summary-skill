# Bilibili Transcribe Summary Skill

[中文说明](#中文说明)

## English

This repository contains a portable Bilibili video transcript and summary sill

### What it does

- Detects Bilibili standard links, BV ids, and `b23.tv` short links
- Checks whether Node.js 18+ is installed on the shared versions
- Prefers official subtitles when available
- Otherwise extracts the anonymous audio URL from the page play info
- Downloads the audio as `.m4s` and renames it to `.mp3` without transcoding
- Calls SiliconFlow ASR when an API key is present
- Produces a transcript and then summarizes it according to the user's request
- Defaults to a key-point summary if the user gives no special instructions
- Prints transcript text to stdout when available, so callers do not need an extra file-read step

### Directory layout

- `skill/bilibili-transcribe-summary/`: Codex or Cursor style general version
- `skill/bilibili-transcribe-summary-ClaudeClaude/`: Claude Code version
- `skill/bilibili-transcribe-summary-openclaw/`: OpenClaw Chinese version
- `skill/bilibili-transcribe-summary-forme/`: personal version with easier local API-key options and no repeated pre-check reminders

### First-run readiness marker for shared versions

The shared versions write `.skill-ready.json` into the output directory after the first successful `probe` or `run`.

That marker is meant to reduce repeated setup chatter:

- First successful run: create the readiness marker
- Later runs: skip repeated dependency and API-key reminders unless the script actually fails

### Shared workflow command

```bash
node scripts/bilibili_pipeline.mjs probe "https://www.bilibili.com/video/BV1R6PzzAE9k" --output-dir ./output
node scripts/bilibili_pipeline.mjs run "https://b23.tv/lsocHNd" --output-dir ./output
```

### Requirements

- Node.js 18+
- Network access
- SiliconFlow API key when subtitle fallback to ASR is needed（ free to ues,no need to pay）

Create an API key here:

- https://cloud.siliconflow.cn/me/account/ak

### Notes

- This skill has been verified with both full Bilibili links and `b23.tv` short links.
- In the validated setup, renaming the downloaded `.m4s` audio file to `.mp3` was sufficient for SiliconFlow transcription, so no transcoding step was required.
- Anonymous access is opportunistic and may fail for some videos.

## 中文说明

本项目提供了一套可移植的 B 站视频转录与总结的skill，全流程免费，不用折腾部署大模型，可用于 Codex、Claude Code 和 OpenClaw。

### 功能说明

- 识别 B 站标准视频链接、BV 号和 `b23.tv` 短链接
- 会检查本机是否安装 Node.js 18+
- 优先尝试官方字幕
- 如果没有字幕，则从页面播放信息中提取匿名音频地址
- 下载得到 `.m4s` 后直接改名为 `.mp3`，不进行转码
- 在有 API key 时调用硅基流动 ASR
- 生成文字稿后再根据用户要求做总结
- 如果用户没有额外要求，默认输出重点总结
- 成功拿到文字稿时会直接输出到 stdout，不必再额外读取 txt 文件



### 首次成功标记

版本现在会在输出目录写入 `.skill-ready.json`。

这个文件的作用是减少重复提示：

- 第一次成功执行后生成标记
- 后续再次使用时，默认跳过重复的依赖 / API key 提示
- 只有脚本真的失败了，再回退到安装和配置引导

### 核心命令

```bash
node scripts/bilibili_pipeline.mjs probe "https://www.bilibili.com/video/BV1R6PzzAE9k" --output-dir ./output
node scripts/bilibili_pipeline.mjs run "https://b23.tv/lsocHNd" --output-dir ./output
```

### 依赖要求

- Node.js 18+
- 可联网环境
- 在字幕不可用、需要 ASR 时，需要硅基流动 API key(目前使用是免费的)

获取 API key 的地址：

- https://cloud.siliconflow.cn/me/account/ak

### 说明

- 当前方案已经验证可处理标准 B 站链接和 `b23.tv` 短链接。
- 在已验证流程中，下载得到的 `.m4s` 音频文件直接改名为 `.mp3` 后即可送去硅基流动转写，无需转码。
- 匿名获取音频并不保证对所有视频都稳定有效，部分视频仍可能失败。
