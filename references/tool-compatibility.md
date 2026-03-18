# Tool Compatibility

## Codex and OpenClaw

- Place the `bilibili-transcribe-summary` folder inside the tool's skills directory.
- The `SKILL.md` metadata is written to trigger on Bilibili links and summary or transcription intent.
- Run `scripts/bilibili_pipeline.mjs` exactly as documented in `SKILL.md`.

## Claude Code

If Claude Code does not load `SKILL.md` automatically, copy the trigger rules and workflow summary into the project's `CLAUDE.md` and keep using the same Node script.

Suggested Claude Code rule:

- When the user provides a Bilibili link or `BV...` id and asks for summary, transcript, subtitles, or content analysis, run `node scripts/bilibili_pipeline.mjs run <url> --output-dir ./output`.
- Prefer subtitles when available.
- Otherwise use the anonymous audio URL, rename `.m4s` to `.mp3`, call SiliconFlow ASR, and summarize the returned transcript.
- If the user gives no summary preference, default to a key-point summary.
- If `SILICONFLOW_API_KEY` is missing, tell the user how to set it before running ASR.