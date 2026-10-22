# MedicineData MCP · 医药数据社

Remote [Model Context Protocol](https://modelcontextprotocol.io) server for a **Chinese pharmaceutical industry database**. 21 billed read-only tools plus a free `list_capabilities` catalogue connect Claude, Cursor, Cline or any Streamable HTTP MCP client to drug pipelines, targets, indications, clinical trials, companies, licensing deals, VC funding, patents, conference abstracts and drug sales.

- **Endpoint**: `https://medicine.shanchuangkj.com/mcp/`
- **Transport**: Streamable HTTP (stateless)
- **Auth**: header `X-API-Key: mds-...`
- **Get a key**: <https://medicine.shanchuangkj.com/mcp> — 2,000 free rows for new accounts
- **Official MCP Registry**: `com.shanchuangkj/medicine-data`

## Quick start

Claude Desktop / Claude Code / Cursor:

```json
{
  "mcpServers": {
    "medicine-data": {
      "type": "http",
      "url": "https://medicine.shanchuangkj.com/mcp/",
      "headers": {
        "X-API-Key": "mds-your-key"
      }
    }
  }
}
```

Call `list_capabilities` first — it is free and returns the full catalogue, limits and plans. The metadata handshake (`initialize`, `tools/list`) is open so directories can verify the server; every `tools/call` requires a key.

## Tools (21 billed + 1 free, 22 total)

| Domain | Tools |
| --- | --- |
| Drugs & pipelines | `search_drugs`, `get_drug_detail`, `search_drugs_by_target`, `search_drugs_by_indication` |
| Targets | `search_targets`, `get_target_detail` |
| Indications | `search_indications`, `get_indication_detail` |
| Clinical trials | `search_clinical_trials`, `get_clinical_trial_detail` |
| Companies | `search_companies`, `get_company_detail` |
| Licensing / BD deals | `search_deals`, `get_deal_detail` |
| VC funding | `search_investments`, `search_investors` |
| Patents | `search_patents`, `get_patent_detail` |
| Guidelines & literature | `search_meeting_reports`, `get_meeting_report_detail` |
| Drug sales & market | `search_market_sales` |
| Free | `list_capabilities` |

Data scale: 76k drugs, 9.5k targets, 8.7k indications, 26k China-registered trials plus global trials, 51k companies, 4k licensing deals, 18k funding events, 25k investors, 27k patents, 450k conference abstracts (ASCO / ESMO / SABCS), and drug sales 2016–2024 (overall, hospital, online pharmacy).

## Pricing

Billed per **row actually returned** (0 rows, bad parameters and server errors are free; subscription calls do not consume the top-up balance — they count against the daily row quota instead).

- New account: 2,000 rows free, valid 7 days
- Weekly ¥39: up to 5,000 rows/day / 7 days
- Monthly ¥99: up to 5,000 rows/day / 30 days
- Non-expiring top-ups: 3,000 rows ¥9.9 · 8,000 ¥19.9 · 25,000 ¥49.9 · 125,000 ¥199 · 400,000 ¥499
- Rate limits: 60 requests/minute, 32KB max response (larger queries return an error, never a truncated result)

Prices and limits are served live by `list_capabilities`; the website is the source of truth.

## What this server does **not** cover

- No PubMed records (the underlying `rs_pubmed` table is empty); only ASCO/ESMO/SABCS-style conference abstracts
- The China clinical-trial table has no status semantics — `status` is returned as `null`
- No offline pharmacy sell-through, no share prices / financial statements / market cap, no drug price or reimbursement standards
- Intended for industry research and investment analysis, **not** medical advice or diagnosis

## Links

- Product & pricing: <https://medicine.shanchuangkj.com/mcp> (English: <https://medicine.shanchuangkj.com/en/mcp>)
- Registry entry: <https://registry.modelcontextprotocol.io/v0/servers?search=com.shanchuangkj>
- Contact: <https://medicine.shanchuangkj.com/mcp>

This repository contains public documentation only; the service is closed-source.
