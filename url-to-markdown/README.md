# Markdown.new: url-to-markdown

A Claude Code skill that converts any URL to clean Markdown using the [markdown.new](https://markdown.new) API.

Built by [Emre Elbeyoglu](https://x.com/elbeyoglu) for the markdown.new community.

## Install

**Personal (all projects):**

```bash
cp -r url-to-markdown-skill ~/.claude/skills/url-to-markdown
```

**Project-level:**

```bash
cp -r url-to-markdown-skill .claude/skills/url-to-markdown
```

## Usage

```
/url-to-markdown <url> [--images] [--browser] [--json]
```

| Flag        | Description                                |
| ----------- | ------------------------------------------ |
| `--images`  | Keep images in the markdown output         |
| `--browser` | Force browser rendering for JS-heavy sites |
| `--json`    | Return full JSON response with metadata    |

## How it works

The skill calls the markdown.new API, which uses a 3-tier conversion pipeline:

1. **Workers AI** — Fast HTML-to-Markdown conversion (~580ms)
2. **Browser Rendering** — Headless browser for JS-heavy SPAs (~1.6-2s)

Results are cached for 5 minutes. No API key required (500 requests/day per IP).
