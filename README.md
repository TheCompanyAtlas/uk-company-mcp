# UK Company Registry MCP

Connect your AI assistant to about 5.7 million live UK companies on the Companies House register through [The Company Atlas](https://thecompanyatlas.com) and the Model Context Protocol.

Search:

- Companies in England and Wales, Scotland and Northern Ireland by name, as a substring or a prefix
- Company number lookups, with zero-padding for short numbers (`445790` → `00445790`)
- Company type (private and public limited companies, LLPs, limited partnerships, community interest companies, charitable incorporated organisations, overseas entities and more) and company status (active, proposal to strike off, liquidation, administration, receivership)
- Registered office postcode, SIC 2007 nature-of-business codes and incorporation date
- Former company names

Dissolved companies, officers, persons with significant control and filed accounts are not included.

The MCP connection is provided by The Company Atlas. This repository does not run a separate server.

## Connect

```text
https://thecompanyatlas.com/mcp/uk
```

Add this URL as a custom MCP connector in Claude, Cursor, or another MCP-compatible client. Sign in with Google when prompted.

- Transport: Streamable HTTP
- Auth: Google OAuth
- Docs: [thecompanyatlas.com/tools/uk-companies-house](https://thecompanyatlas.com/tools/uk-companies-house)

## Tools

### `UK_search_companies`

Search UK companies. Filters are combined with AND logic. Include `q`, `entityType` or `status`; `postcode`, `sic` and the incorporation dates need `q` as well.

| Parameter | Type | Description |
|---|---|---|
| `q` | string, optional | Company name, 3–160 characters |
| `nameMatch` | enum, optional | `contains` (default) or `startsWith`; use `startsWith` for brand names |
| `entityType` | string, optional | Company type substring, e.g. `Community Interest Company` |
| `status` | enum, optional | Company status, e.g. `Active`, `Active - Proposal to Strike off`, `Liquidation`, `In Administration` |
| `postcode` | string, optional | Registered office postcode district (e.g. `W14`, `M4`) or longer prefix (e.g. `M4 5`) |
| `sic` | string, optional | SIC 2007 code, e.g. `62012` |
| `incorporatedFrom` / `incorporatedTo` | date, optional | Incorporation date range, `YYYY-MM-DD` |
| `page` / `limit` | integer, optional | Pagination, `limit` up to 50 (default 20) |

Returns `{ data, total, page, limit }`. `id` is `GB` + the 8-character company number.

### `UK_get_company`

One company by company number (e.g. `00445790`, `445790` or `SC123456`), with former names.

| Parameter | Type | Description |
|---|---|---|
| `companyNumber` | string | Companies House company number |

## Example prompts

- *"Companies starting with Tesco that are in liquidation."*
- *"Solar companies incorporated since 2024."*
- *"Bakeries with a registered office in the M4 postcode district."*
- *"Former names of company 09202627."*

## Setup — Claude

1. Go to [claude.ai/customize/connectors](https://claude.ai/customize/connectors)
2. Click **Add custom connector**
3. Name it **UK Companies** and paste `https://thecompanyatlas.com/mcp/uk`
4. Save and sign in with Google when prompted
