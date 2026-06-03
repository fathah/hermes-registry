# MCP Servers

An **MCP** entry is a [Model Context Protocol](https://modelcontextprotocol.io)
server that exposes tools and/or resources to Hermes.

## Folder layout

```
mcp/<name>/
├── manifest.json     # metadata (validated against schemas/mcp.schema.json)
├── icon.svg          # or icon.png (512×512)
└── server/           # the MCP server source code
```

## manifest.json

```json
{
  "schemaVersion": "1",
  "type": "mcp",
  "id": "ziqx/postgres",
  "name": "Postgres MCP",
  "version": "1.0.0",
  "description": "Query and inspect Postgres databases",
  "author": { "name": "ziqx", "url": "https://github.com/ziqx" },
  "license": "MIT",
  "tags": ["database", "sql"],
  "icon": "icon.png",
  "compatibility": { "hermes": ">=0.3.0", "desktop": ">=1.2.0" },
  "permissions": ["network"],

  "transport": "stdio",
  "command": "python",
  "args": ["server/main.py"],
  "env": { "LOG_LEVEL": "info" },
  "configSchema": {
    "type": "object",
    "properties": {
      "DATABASE_URL": { "type": "string", "description": "Postgres connection string" }
    },
    "required": ["DATABASE_URL"]
  },

  "funding": { "address": "0x…", "token": "H1", "chain": "base" }
}
```

### Transport

- `stdio` — Hermes spawns `command` + `args` and talks over stdin/stdout.
- `http` — provide a `url` instead of `command`/`args`.

## Secrets & config

**Never commit secrets.** Declare what the server needs via `configSchema`. The
user supplies those values at install time (the desktop app stores them in the
OS keychain). The registry only ships the schema, not the values.

## Checklist

- [ ] Folder name is the MCP name (e.g. `postgres`), matching the name part of `id`
- [ ] `manifest.json` validates (`python scripts/validate.py`)
- [ ] `server/` runs with the declared `command`/`args` or `url`
- [ ] All required secrets declared in `configSchema` — none hardcoded
- [ ] Icon present (SVG, or 512×512 PNG)
- [ ] `version` bumped (semver) and `compatibility` honest
