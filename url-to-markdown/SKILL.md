---
name: url-to-markdown
version: 1.0.0
description: Convert any URL to clean Markdown using the markdown.new service. Built by Emre Elbeyoglu (https://x.com/elbeyoglu) for markdown.new.
author: Emre Elbeyoglu <https://x.com/elbeyoglu>
license: MIT
repository: https://github.com/markdown-new/url-to-markdown-skill
argument-hint: <url> [--images] [--browser]
allowed-tools: Bash(curl *), Read, Write, Edit
---

# URL to Markdown

Convert any URL to clean, readable Markdown using the [markdown.new](https://markdown.new) API.

## How to use

The user provides a URL via `$ARGUMENTS`. Parse the arguments:

- **`$0`** (required): The URL to convert
- **`--images`** flag: Retain images in the output (default: strip images)
- **`--browser`** flag: Force browser rendering for JS-heavy sites (slower but more complete)
- **`--json`** flag: Return raw JSON response instead of just markdown content

## Conversion steps

1. **Parse arguments** from `$ARGUMENTS` to extract the URL and any flags.

2. **Call the markdown.new API** using curl:

```bash
curl -s 'https://markdown.new/' \
  -H 'Content-Type: application/json' \
  -d '{"url": "<URL>", "retain_images": <true|false>, "method": "<auto|browser>"}'
```

Set `retain_images` to `true` if `--images` flag is present, otherwise `false`.
Set `method` to `"browser"` if `--browser` flag is present, otherwise `"auto"`.

3. **Extract the result** from the JSON response. The response format is:

```json
{
  "success": true,
  "url": "https://example.com",
  "title": "Page Title",
  "content": "# Markdown content...",
  "method": "Cloudflare Workers AI",
  "duration_ms": 580
}
```

Use `python3` or `jq` to parse the JSON:

```bash
curl -s 'https://markdown.new/' \
  -H 'Content-Type: application/json' \
  -d '{"url": "<URL>"}' | python3 -c "
import sys, json
data = json.load(sys.stdin)
if data.get('success'):
    print(data['content'])
else:
    print('Error:', data.get('error', 'Unknown error'), file=sys.stderr)
    sys.exit(1)
"
```

4. **Present the result** to the user:
   - If `--json` flag: show the full JSON response formatted with `json.dumps(data, indent=2)`
   - Otherwise: display the markdown content directly
   - If the user seems to want the content saved to a file, ask where to save it before writing
   - Always mention the page title (if available) and conversion method used

## Error handling

- If `success` is `false`, show the error message from the response
- If the curl request fails (network error, timeout), inform the user and suggest:
  - Checking the URL is accessible
  - Trying with `--browser` flag for JS-heavy sites
  - Trying again in a moment (the service may be rate limited)

## Rate limits

The markdown.new API has rate limits:

- 500 requests/day per IP without an API key
- Results are cached for 5 minutes, so repeated conversions of the same URL are fast

## Examples

- For usage examples, see [examples/usage.md](examples/usage.md)
