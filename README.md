# skills

A personal collection of agent skills for Claude Code, installable via [`npx skills`](https://github.com/vercel-labs/skills).

## Installation

```bash
npx skills add cscxj/skills --skill release-notice
```

## Available Skills

### release-notice

Converts plain text release notes into styled HTML notification files.

Triggers on: `发版通知`, `release notice`, `生成发版`, `版本通知`

Given free-form release notes, generates 8 fully self-contained HTML files (en-US, zh-CN, ja-JP, fr-FR, de-DE, es-ES, pt-BR, it-IT) with color-coded badges and a card layout — ready for use in browser email previews.

**Example usage:**

```
v2.5.0  2024-01-15
新增 AI 助手多轮对话支持
修复导出功能在 Safari 下崩溃的问题
```

Output: `release-notice-v2.5.0/` folder with 8 translated HTML files.
