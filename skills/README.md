# Skills

A **skill** is a task procedure Hermes can follow — a set of instructions, plus
any supporting files, that teach the agent how to do something.

## Folder layout

```
skills/<name>/
├── manifest.json     # metadata (validated against schemas/skill.schema.json)
├── SKILL.md          # the skill instructions (entry file)
└── icon.svg          # or icon.png (512×512)
```

## manifest.json

```json
{
  "schemaVersion": "1",
  "type": "skill",
  "id": "ziqx/web-scraper",
  "name": "Web Scraper",
  "version": "1.0.0",
  "description": "Scrape and structure web pages into clean JSON",
  "author": { "name": "ziqx", "url": "https://github.com/ziqx" },
  "license": "MIT",
  "tags": ["web", "data"],
  "icon": "icon.svg",
  "compatibility": { "hermes": ">=0.3.0", "desktop": ">=1.2.0" },
  "permissions": ["network"],
  "entry": "SKILL.md",
  "funding": { "address": "0x…", "token": "HERMES", "chain": "base" }
}
```

## SKILL.md

The entry file holds the actual procedure. Start with a short description of
what the skill does and when to use it, then the step-by-step instructions.

```markdown
# Web Scraper

Scrape a URL and return structured JSON. Use when the user wants data extracted
from a web page.

## Steps
1. Fetch the page.
2. Identify the main content region.
3. Extract fields into JSON …
```

## Checklist

- [ ] Folder name is the skill name (e.g. `web-scraper`), matching the name part of `id`
- [ ] `manifest.json` validates (`python scripts/validate.py`)
- [ ] `SKILL.md` present and referenced by `entry`
- [ ] Icon present (SVG, or 512×512 PNG)
- [ ] `version` bumped (semver) and `compatibility` honest
