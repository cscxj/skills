---
name: release-notice
description: Use when a product manager provides plain text release notes and wants to generate a styled HTML release notification file matching the design spec. Triggers on phrases like "发版通知", "release notice", "生成发版", "版本通知".
---

# Release Notice HTML Generator

## Overview

Converts plain text release notes into a styled HTML file that matches the official release notification design. The design uses a left-border card layout with color-coded badges (New / Improved / Fixed / Removed).

## Input Format

The PM provides free-form text. You must parse it to extract:

- **Version number** (e.g., `v2.3.0`) — look for patterns like `v\d+\.\d+`, `版本`, `Version`
- **Release date** — look for date patterns or `日期`
- **Items** — each item has:
  - `title`: feature/fix name
  - `tag`: one of `New` / `Improved` / `Fixed` / `Removed`
  - `description`: one sentence summary

**Tag inference rules** (when PM doesn't specify):
| Keywords | Tag |
|----------|-----|
| 新增、新功能、上线、支持、增加 | `New` |
| 优化、改进、提升、升级、重构 | `Improved` |
| 修复、解决、fix、bug | `Fixed` |
| 下线、移除、删除、废弃 | `Removed` |

## Design Spec (from Figma node 3028-15680)

**Layout:** `padding: 28px`, `gap: 24px` between items, white background

**Each item card:**
- Left border: `2px solid #C2DEFF`
- Padding: `0 20px`, vertical `2px`
- Title row gap: `8px`

**Title text:** `font-size: 16px`, `font-weight: 600`, `color: #0F1629`, `letter-spacing: -0.32px`

**Description text:** `font-size: 14px`, `font-weight: 400`, `color: #303947`, `letter-spacing: -0.28px`, `line-height: 1.5`

**Badge styles by tag:**

| Tag | Background | Text Color | Text |
|-----|-----------|------------|------|
| New | `rgba(0,180,42,0.06)` | `#00B42A` | New / 新功能 |
| Improved | `rgba(43,114,253,0.06)` | `#2B72FD` | Improved / 优化 |
| Fixed | `rgba(255,125,0,0.06)` | `#FF7D00` | Fixed / 修复 |
| Removed | `rgba(245,63,63,0.06)` | `#F53F3F` | Removed / 下线 |

**Badge common styles:** `padding: 2px 8px`, `border-radius: 6px`, `font-size: 13px`, `font-weight: 500`

## Output

Generate **8 HTML files** — one per language — inside a folder named `release-notice-{version}/` in the **current working directory**.

File naming:
| Language | File name | `<html lang>` |
|----------|-----------|--------------|
| English (US) | `en-US.html` | `en` |
| 简体中文 | `zh-CN.html` | `zh-CN` |
| 日本語 | `ja-JP.html` | `ja` |
| Français | `fr-FR.html` | `fr` |
| Deutsch | `de-DE.html` | `de` |
| Español | `es-ES.html` | `es` |
| Português | `pt-BR.html` | `pt` |
| Italiano | `it-IT.html` | `it` |

Each file must:
1. Be fully self-contained (no external CSS/JS dependencies)
2. Use `font-family: Inter, -apple-system, BlinkMacSystemFont, sans-serif`
3. Translate ALL text content into that file's language (title, badge labels, and item content)
4. Render correctly in browser email previews

**Translation rules:**
- Translate the header title (e.g., "Release Notes", "发版通知", "リリースノート", etc.)
- Translate badge labels per language (see table below)
- **Translate item titles and descriptions** into the target language — do NOT leave them in the source language
- Keep version number and date untranslated

**Badge label translations:**

| Tag | en | zh-CN | ja | fr | de | es | pt | it |
|-----|----|-------|----|----|----|----|----|----|
| New | New | 新功能 | 新機能 | Nouveau | Neu | Nuevo | Novo | Nuovo |
| Improved | Improved | 优化 | 改善 | Amélioré | Verbessert | Mejorado | Melhorado | Migliorato |
| Fixed | Fixed | 修复 | 修正 | Corrigé | Behoben | Corregido | Corrigido | Corretto |
| Removed | Removed | 下线 | 削除 | Supprimé | Entfernt | Eliminado | Removido | Rimosso |

**Header title translations:**

| en | zh-CN | ja | fr | de | es | pt | it |
|----|-------|----|----|----|----|----|----|
| Release Notes | 发版通知 | リリースノート | Notes de version | Versionshinweise | Notas de versión | Notas de versão | Note di rilascio |

## HTML Template

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>发版通知 {version}</title>
<style>
  * { margin: 0; padding: 0; box-sizing: border-box; }
  body {
    font-family: Inter, -apple-system, BlinkMacSystemFont, 'PingFang SC', sans-serif;
    background: #fff;
  }
  .container {
    display: flex;
    flex-direction: column;
    gap: 24px;
  }
  .item {
    border-left: 2px solid #C2DEFF;
    padding: 2px 20px;
    display: flex;
    flex-direction: column;
    gap: 6px;
  }
  .item-title-row {
    display: flex;
    align-items: center;
    gap: 8px;
  }
  .item-title {
    font-size: 16px;
    font-weight: 600;
    color: #0F1629;
    letter-spacing: -0.32px;
    line-height: 1.5;
    white-space: nowrap;
  }
  .badge {
    display: inline-flex;
    align-items: center;
    justify-content: center;
    padding: 2px 8px;
    border-radius: 6px;
    font-size: 13px;
    font-weight: 500;
    letter-spacing: -0.26px;
    line-height: 1.5;
    white-space: nowrap;
  }
  .badge-new      { background: rgba(0,180,42,0.06);  color: #00B42A; }
  .badge-improved { background: rgba(43,114,253,0.06); color: #2B72FD; }
  .badge-fixed    { background: rgba(255,125,0,0.06); color: #FF7D00; }
  .badge-removed  { background: rgba(245,63,63,0.06); color: #F53F3F; }
  .item-desc {
    font-size: 14px;
    font-weight: 400;
    color: #303947;
    letter-spacing: -0.28px;
    line-height: 1.5;
  }
</style>
</head>
<body>
<div class="container">
  <!-- repeat for each item -->
  <div class="item">
    <div class="item-title-row">
      <span class="item-title">{title}</span>
      <span class="badge badge-{tag-class}">{tag-label}</span>
    </div>
    <p class="item-desc">{description}</p>
  </div>

</div>
</body>
</html>
```

## Step-by-Step Workflow

1. **Parse** the PM's plain text — extract version, date, items (regardless of input language)
2. **Infer tags** for any items without explicit labels (use keyword table above)
3. **Create folder** `release-notice-{version}/` in the current working directory
4. **For each of the 8 languages:**
   a. Translate all item titles and descriptions into the target language
   b. Use the correct badge labels and header title for that language
   c. Set the correct `<html lang>` attribute
   d. Write the file as `{locale}.html` inside the folder
5. **Confirm** output folder path and list all 8 files to the user

## Example

**Input:**
```
v2.5.0  2024-01-15
AI 助手全面升级 — 支持多轮对话、上下文记忆，响应速度提升 3 倍
数据看板重新设计 — 全新可视化组件，支持自定义维度和实时刷新
修复导出功能在 Safari 下崩溃的问题
```

**Output file:** `release-notice-v2.5.0.html` with 3 items:
- "AI 助手全面升级" → `New` (新增功能)
- "数据看板重新设计" → `Improved` (重新设计=优化)
- "修复导出功能..." → `Fixed`
