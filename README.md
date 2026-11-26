# SEA Cold Chain market totals (MCP server)

[![SEA Cold Chain MCP connector](https://glama.ai/mcp/connectors/com.seacoldchain/markets/badges/score.svg)](https://glama.ai/mcp/connectors/com.seacoldchain/markets)

A free, read-only [Model Context Protocol](https://modelcontextprotocol.io) server with cold storage market totals for
Southeast Asia: operating sites, pallet capacity, investment-grade buildings, the pipeline under construction and the
coverage share, each with its as-of date and a citation line.

- **Endpoint:** `https://seacoldchain.com/mcp` (Streamable HTTP, no sign-in, no API key)
- **Registry name:** `com.seacoldchain/markets` in the [official MCP Registry](https://registry.modelcontextprotocol.io)
- **Markets:** Thailand, Vietnam, Cambodia, the Philippines, Singapore, Indonesia, Malaysia
- **Publisher:** [SEA Cold Chain](https://seacoldchain.com), a site-level register of cold storage buildings in
  Southeast Asia ([how it is built](https://seacoldchain.com/method))
- **Updates:** [SEA Cold Chain on LinkedIn](https://www.linkedin.com/company/seacoldchain)

The server is hosted; this repository holds its description and registry file only.

## Tools

| tool | what it returns |
|---|---|
| `list_markets` | the markets with public totals, each with its operating site count, as-of date and page |
| `market_totals` | totals for one market (name, page slug or ISO code, e.g. `Vietnam`, `philippines`, `TH`), as text and as structured JSON |
| `method` | how the register is built: sources, estimates, tiers, refresh rules |

All tools are read-only and idempotent.

## Connect

**Claude Code**

```
claude mcp add --transport http seacoldchain https://seacoldchain.com/mcp
```

**Claude (desktop or web):** Settings > Connectors > Add custom connector, URL `https://seacoldchain.com/mcp`.

**VS Code** (`.vscode/mcp.json`)

```json
{ "servers": { "seacoldchain": { "type": "http", "url": "https://seacoldchain.com/mcp" } } }
```

**Cursor** (`~/.cursor/mcp.json`)

```json
{ "mcpServers": { "seacoldchain": { "url": "https://seacoldchain.com/mcp" } } }
```

**Clients that only speak stdio**

```json
{ "mcpServers": { "seacoldchain": { "command": "npx", "args": ["-y", "mcp-remote", "https://seacoldchain.com/mcp"] } } }
```

## Example

`market_totals` with `{"market": "Vietnam"}` (25 November 2026):

```
Vietnam cold storage, Q4 2026: 166 operating sites, at least 1,458,000 pallet positions, and 8 sites under
construction or planned.
- Operating cold storage sites: 166 (as of 2026-11-23)
- Pallet positions, at least: 1,458,000 (74% stated by a named source, the rest estimated and labelled)
- Investment-grade buildings (tier T1 or T2): 109
- Under construction or planned: 8

Source: SEA Cold Chain, seacoldchain.com/markets/vietnam/2026-q4, as of 2026-11-23.
```

Figures change as the register is checked; the call returns the current ones.

## Reading the figures

- Pallet positions are a floor (the capacity of the sites held, with estimates at the low end of their range), not a
  market size. The coverage share says how much of the market's estimated capacity is held.
- Market totals only: no individual site, operator or location. Groups under 3 sites are combined.
- Each quarter's figure is fixed at its own dated page (for example `/markets/vietnam/2026-q4`), so a citation keeps
  pointing at the number quoted.

## Data and licence

The figures are published under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/): reuse them freely with the
citation line each call returns. The same totals as a file:
[Kaggle](https://www.kaggle.com/datasets/seacoldchain/southeast-asia-cold-storage-market-totals) and
[Hugging Face](https://huggingface.co/datasets/SEAColdChain/southeast-asia-cold-storage-market-totals).
This repository's text and registry file: MIT.

The site-level register behind the totals (each building with its operator, capacity, year opened, temperature regime,
grade, tier and a source for every value) is sold per market: info@seacoldchain.com.
