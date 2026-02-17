# Usage Examples

## Basic conversion

```
/url-to-markdown https://developers.cloudflare.com/browser-rendering/
```

Fetches `https://developers.cloudflare.com/browser-rendering/` and returns the page content as clean Markdown:

```markdown
title: Browser Rendering
description: Control headless browsers with Cloudflare's Workers Browser Rendering API...

# Browser Rendering

Control headless browsers with Cloudflare's Workers Browser Rendering API. Automate tasks, take screenshots, convert pages to PDFs, and test web apps.
```

## Keep images

```
/url-to-markdown https://blog.cloudflare.com/some-post --images
```

Retains `![alt](url)` image syntax in the output instead of stripping them.

## JS-heavy site (browser rendering)

```
/url-to-markdown https://react.dev/learn --browser
```

Forces headless browser rendering for single-page apps that require JavaScript to render content.

## Get raw JSON response

```
/url-to-markdown https://example.com --json
```

Returns the full API response including metadata:

```json
{
  "success": true,
  "url": "https://example.com",
  "title": "Example Domain",
  "content": "# Example Domain\n\nThis domain is for use in illustrative examples...",
  "method": "Cloudflare Workers AI",
  "duration_ms": 42
}
```

## Combine flags

```
/url-to-markdown https://docs.github.com/en/rest --images --browser
```

Uses browser rendering and keeps images in the output.

## Natural language (auto-invoked by Claude)

Since this skill can be auto-invoked, these prompts will trigger it:

- "Convert this URL to markdown: https://example.com"
- "Fetch the content of https://example.com as markdown"
- "Read this webpage for me: https://example.com"
- "Get the markdown version of https://docs.python.org"
