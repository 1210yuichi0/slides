---
name: defuddle
description: Extract clean markdown content from web pages using Defuddle CLI, removing clutter and navigation to save tokens. Use instead of WebFetch when the user provides a URL to read or analyze, for online documentation, articles, blog posts, or any standard web page.
---

# Defuddle

Use Defuddle CLI to extract clean readable content from web pages. Prefer over WebFetch for standard web pages — it removes navigation, ads, and clutter, reducing token usage.

If not installed: `npm install -g defuddle-cli`

## Usage

Always use `--md` for markdown output:

```bash
defuddle parse <url> --md
```

Save to file:

```bash
defuddle parse <url> --md -o content.md
```

Extract specific metadata:

```bash
defuddle parse <url> -p title
defuddle parse <url> -p description
defuddle parse <url> -p domain
```

## Output formats

| Flag | Format |
|------|--------|
| `--md` | Markdown (default choice) |
| `--json` | JSON with both HTML and markdown |
| (none) | HTML |
| `-p <name>` | Specific metadata property |

## 空出力時のフォールバック

以下のサイトは空出力になる場合がある:

| サイト種別 | 原因 | 対処 |
|---|---|---|
| docsify サイト（marpit.marp.app 等） | JS レンダリング | raw GitHub ソースを直接取得 |
| GitHub `?tab=` URL | クエリパラメータ | ベースURLで再試行 |
| SPA / 認証必須ページ | JS or auth | WebFetch にフォールバック |

```bash
# 空出力なら ?tab= を外して再試行
defuddle parse "https://github.com/org/repo" --md

# それでも空なら WebFetch or raw URL
# https://raw.githubusercontent.com/org/repo/main/README.md
```

**ルール**: 出力が空または極端に短い場合は、WebFetch か raw URL に切り替えること。同じコマンドをリトライしない。
