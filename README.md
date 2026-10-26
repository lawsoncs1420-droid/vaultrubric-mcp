# VaultRubric MCP server

[![LightNow capabilities](https://lightnow.ai/badge/com.vaultrubric/vaultrubric)](https://lightnow.ai/servers/com.vaultrubric/vaultrubric)

<img src="https://vaultrubric.com/icon.png" alt="VaultRubric" width="64">

Free, public, **read-only** [Model Context Protocol](https://modelcontextprotocol.io) server for [VaultRubric](https://vaultrubric.com). VaultRubric publishes evidence-based comparisons of where to keep money: crypto exchanges, hardware and software wallets, high-yield savings accounts, and brokerages/IRAs.

- **Endpoint:** `https://vaultrubric.com/mcp` (Streamable HTTP, JSON-RPC over POST)
- **Auth:** none · **Writes:** none · **Rate limit:** 60 requests/minute per IP
- **Registry name:** `com.vaultrubric/vaultrubric` ([official MCP Registry](https://registry.modelcontextprotocol.io/v0/servers?search=com.vaultrubric))
- **Docs:** https://vaultrubric.com/developers · **OpenAPI (REST mirror):** https://vaultrubric.com/openapi.json

The server is hosted by VaultRubric, so there is nothing to install. This repo only holds the listing metadata.

## Tools

| Tool | What it returns |
|---|---|
| `list_categories` | Categories and platform counts |
| `search_platforms(query, category)` | Matching platforms with score and data coverage |
| `get_platform(name)` | Sourced facts, per-criterion scores with basis and source URLs, incidents, as-of dates |
| `compare_platforms(platforms, weights)` | 2–6 platforms in one category with optional 0–10 weights. Applies the 75% coverage gate, the 60% hack cap and the too-close-to-call (<3 pts) rule |
| `get_verdict(slug)` | Published answer for a comparison page, e.g. `coinbase-vs-kraken` |
| `check_platform_safety(platform)` | Customer-funds hacks, incidents, regulatory actions, security score, with sources |

Older names `compare` and `is_safe` still work as aliases. Every tool declares an `outputSchema` and returns matching `structuredContent`. Every response includes `"attribution": "Source: VaultRubric (vaultrubric.com)"` and the relevant page URLs. Scoring rules: https://vaultrubric.com/methodology

## Connect

**Claude** (claude.ai / Desktop): Settings → Connectors → Add custom connector → `https://vaultrubric.com/mcp`
**Claude Code:** `claude mcp add --transport http vaultrubric https://vaultrubric.com/mcp`
**ChatGPT** (developer mode): Settings → Connectors → Create → MCP server URL `https://vaultrubric.com/mcp`, no authentication. For a custom GPT, import the OpenAPI spec as an Action.
**Cursor / other clients** (`mcp.json`):

```json
{ "mcpServers": { "vaultrubric": { "url": "https://vaultrubric.com/mcp" } } }
```

**Raw request:**

```bash
curl -s https://vaultrubric.com/mcp -H 'Content-Type: application/json' \
  -d '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"check_platform_safety","arguments":{"platform":"Coinbase"}}}'
```

## License and attribution

The data is free to use, cite and redistribute **with attribution**: "Source: VaultRubric (vaultrubric.com)", linking the page or data file used. See [LICENSE](LICENSE). The data is informational only and is not investment, tax or legal advice. Facts carry as-of dates and can change.

Contact: laws@mail.grokbot.com
