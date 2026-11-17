---
name: datax-portugal-eu-oracle
description: Query official Portugal & EU registries (IMPIC Portal BASE (Contratos & Anúncios via dados.gov.pt), TED PRT, SEDIA & EU FSF) via USDC/EURC UTXO micropayments.
---

# DataX Portugal & EU Intelligence Oracle

Base URL: `https://datax-pt.duckdns.org`
Discovery: `GET /llms.txt`, `GET /mcp.json`, `GET /.well-known/agent-card.json`, `GET /openapi.json`

## Pricing (USDC / MiCA EURC on Base, Polygon, or Solana)
- **$0.10 (`100,000` micros):** `search_tenders`, `find_funded_companies`. Returns opaque `item_ref` (`dx_...`).
- **$0.30 (`300,000` micros):** `check_company_risk` (Screen 9-digit corporate NIPC or company_name across EU FSF sanctions & BASE/TED awards.).
- **$0.50 (`500,000` micros):** `get_tender_detail` (unlocks `boe_id`, corporate IDs, verbatim text & official URLs from any `item_ref`).

## Minimal Flow
1. `POST /api/v1/challenge` -> sign EIP-712 (EVM) or Ed25519 (Solana) -> `POST /api/v1/issue-token` with `scope_id = "ffffffffffffffff"`.
2. Call `POST /api/v1/execute/boe-spain` with `X-Payment-Proof: <token>` and use returned `change_token` for subsequent calls.
> **Session Persistence Note:** `mcp_bridge.py` rotates `change_token` in memory during the active `stdio` session only. Every paid result includes its `idempotency_key`. If the process dies with balance left, recover the last `change_token` via `GET /api/v1/execute/reconcile?idempotency_key=<key>` with `X-Payer-Auth` (wallet-signed, valid 300 s, from `/api/v1/challenge` + `/api/v1/issue-token` with `scope_id = "eeeeeeeeeeeeeeee"`). For multi-process agents, pre-split via `POST /api/v1/split-token`. All tokens, including change tokens, expire 24 h after the **original** `issue-token`.
