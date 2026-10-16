# RestoSignals MCP — restaurants and bars before they open

Remote MCP server, REST API and agent skill for **pre-opening restaurant leads in the US**.
RestoSignals fuses public liquor-license applications, food-service permits and new-business
filings into one deduped venue, scores how close it is to opening (`opening_score` 0–100), and
attaches the owner contact found in those filings.

Built for anyone who sells to new restaurants — POS and merchant-services resellers, beverage
distributors, insurers, equipment suppliers — and for agents that prospect on their behalf.

- **MCP endpoint:** `https://api.restosignals.com/mcp` (Streamable HTTP)
- **Auth:** API key in `X-API-Key` (or `Authorization: Bearer pk_...`)
- **Free key:** https://restosignals.com/signup (100 lead credits, no card)
- **Docs:** https://restosignals.com/restoapi · **OpenAPI:** https://api.restosignals.com/openapi.json
- **Website:** https://restosignals.com

## Quick start

Add the server to Claude Desktop / Claude Code, Cursor, Windsurf or any MCP client:

```json
{
  "mcpServers": {
    "restosignals": {
      "url": "https://api.restosignals.com/mcp",
      "headers": { "X-API-Key": "pk_YOUR_KEY" }
    }
  }
}
```

Claude Code one-liner:

```bash
claude mcp add --transport http restosignals https://api.restosignals.com/mcp --header "X-API-Key: pk_YOUR_KEY"
```

Then ask: *"Which restaurants are about to open in New York? Strongest leads first."*

## Tools

| Tool | What it does | Cost |
|---|---|---|
| `coverage` | Live states with venue and hot-lead counts. Call this first. | Free |
| `new_openings` | Scored upcoming venues for a state (`state`, `min_score`, `limit`) | 1 credit per lead |
| `venue_signals` | The raw liquor / permit / business filings fused into one venue | 1 credit |
| `watch_area` | Webhook that fires when a new matching venue appears | Free |

## Coverage

Live with hot leads: **NY, TX, IL, CA, OR**. CO and WA have venues but are still thin. Refreshed
daily. Always call `coverage` for the current list; nothing outside it is claimed.

Contact depth (subscribers): owner name from the filing everywhere, owner mailing address in TX
and NY, phone in NYC. Free keys see that a contact exists, not the contact itself.

## Install as an agent skill / Claude Code plugin

This repo is also a Claude Code plugin marketplace with the `restosignals` skill:

```
/plugin marketplace add paperandbeyond23-gif/restosignals-mcp
/plugin install restosignals@restosignals
```

Or copy [`skills/restosignals/SKILL.md`](skills/restosignals/SKILL.md) into your agent's skills folder.

## REST

```bash
curl -s "https://api.restosignals.com/v1/openings?state=NY&min_opening_score=40&limit=5" \
  -H "X-API-Key: pk_YOUR_KEY"
```

Endpoints: `/v1/openings`, `/v1/openings/latest`, `/v1/signals`, `/v1/export` (CSV),
`/v1/webhooks`, `/v1/account`. Full spec in the [OpenAPI file](https://api.restosignals.com/openapi.json).

## Notes

Data comes from public government open-data. RestoSignals is not affiliated with any
government agency. This repo holds the client config, skill and docs; the server is hosted.

License: MIT (this repo's contents).
