# DataX Portugal & EU Truth Engine — MCP Server (`stdio`)

Zero-dependency **Model Context Protocol (MCP)** `stdio` bridge (`Python 3.9+` standard library only — no `pip install` required) connecting AI agents (Claude Desktop, Cursor, OpenClaw, custom MCP clients) to live official **Portugal & EU** registries with non-custodial pay-per-call settlement in **USDC** (Base, Polygon, Solana) and **MiCA EURC** (Base).

- **Live Endpoint:** `https://datax-pt.duckdns.org`
- **Machine Discovery:** [`/llms.txt`](https://datax-pt.duckdns.org/llms.txt) · [`/mcp.json`](https://datax-pt.duckdns.org/mcp.json) · [`/.well-known/agent-card.json`](https://datax-pt.duckdns.org/.well-known/agent-card.json) · [`/openapi.json`](https://datax-pt.duckdns.org/openapi.json) · [`/.well-known/ai-crawl-pricing.json`](https://datax-pt.duckdns.org/.well-known/ai-crawl-pricing.json)
- **Agent Payment Protocol & Wallet Guide:** See [`SKILL.md`](./SKILL.md)

---

## Official Data Sources Indexed

- **Portuguese Public Procurement (`IMPIC / Portal BASE` via `dados.gov.pt`):** Daily official contracts (`PA`) and announcements (`PC`) with contractual price, CPV, NUTS region, and corporate winners (9-digit corporate NIPC `[5679]\d{8}` only; natural persons and sole traders excluded per GDPR).
- **EU Registries:** **EU TED v3** procurement (`PRT`, `Sec TE`), **EU SEDIA** grants (`Sec EU`), and **EU Consolidated Financial Sanctions List (`FSF`, `Sec SF`)**.

---

## MCP Tools Exposed (7 Tools)

| Tool | Price (`USDC` / `EURC`) | Purpose |
| :--- | :---: | :--- |
| `get_pricing` | **Free** | Fetch supported networks, contracts, treasuries, and tier prices. |
| `get_catalog` | **Free** | Check live freshness (`data_version`) and record counts by section. |
| `search_tenders` | **`0.10`** | FTS5 BM25 + BI filter search across Portugal & EU registries; returns opaque `item_ref` (`dx_...`). |
| `find_distressed_auctions` | **`0.10`** | Real-estate auction endpoint (populated on Spain node; empty on Portugal-only DB). |
| `find_funded_companies` | **`0.10`** | Companies winning Portuguese public contracts (IMPIC BASE) or EU grants. |
| `check_company_risk` | **`0.30`** | Screen 9-digit corporate NIPC or company_name across EU FSF sanctions & BASE/TED awards. |
| `get_tender_detail` | **`0.50`** | Unlocks full verbatim record, winner corporate IDs, CPVs & official URLs from any `item_ref`. |

---

## Quick Setup (Claude Desktop / Cursor)

```json
{
  "mcpServers": {
    "datax-portugal-eu": {
      "command": "python3",
      "args": ["/absolute/path/to/mcp_bridge.py"],
      "env": {
        "DATAX_BASE_URL": "https://datax-pt.duckdns.org",
        "DATAX_TOKEN": "<unspent-utxo-token-from-issue-token>"
      }
    }
  }
}
```
