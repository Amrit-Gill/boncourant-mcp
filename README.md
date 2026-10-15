# Boncourant MCP

Public integration documentation for [Boncourant](https://boncourant.fr), the French electricity-cost optimizer and auditor. This repository contains documentation and public discovery metadata for the hosted service.

## Connect

- **Endpoint:** `https://boncourant.fr/mcp`
- **Transport:** Streamable HTTP
- **Authentication:** no account, API key or OAuth required for public tools
- **Official registry name:** `fr.boncourant/boncourant`
- **Documentation:** https://boncourant.fr/agents
- **Live discovery manifest:** https://boncourant.fr/.well-known/mcp.json
- **OpenAPI:** https://boncourant.fr/openapi.json
- **Agent context:** https://boncourant.fr/llms

Add the endpoint as a remote HTTP MCP server in your client. Clients using the `mcpServers` URL configuration format can use:

```json
{
  "mcpServers": {
    "boncourant": {
      "url": "https://boncourant.fr/mcp"
    }
  }
}
```

Client configuration formats vary; use your client's remote Streamable HTTP server instructions. No local package or server process is needed.

## Capabilities

Start with `list_capabilities` to discover supported tasks, evidence requirements and limits. The public server exposes 19 tools as of 15 October 2026:

`list_capabilities`, `hc_schedule_change_impact`, `simulate`, `analyze_bill`, `get_methodology`, `decompose_invoice`, `annualize_invoice`, `check_subscribed_power`, `analyze_renewal_terms`, `assess_historical_optimality`, `renewal_advisor`, `extract_energy_document`, `run_method`, `assess_ppa`, `create_analysis_study`, `get_analysis_study`, `get_analysis_report`, `eex_connector`, `pro_catalog`.

Use the live manifest or MCP `tools/list` for current schemas. Available checks depend on the supplied evidence. Retrospective billing findings and prospective savings estimates are distinct; incomplete evidence does not establish a billing error or whole-market optimality.

## Privacy and boundaries

The public MCP service does not expose private customer accounts, workspaces, Enedis consent, supplier switching or supplier outreach. Direct document extraction is non-retained. Asynchronous studies use opaque receipts; raw input is encrypted until processing ends, then erased, and results expire within 24 hours. Shared-service rate and size limits apply. Review the [integration guidance](INTEGRATION.md) and [privacy policy](https://boncourant.fr/rgpd) before submitting documents.

## Public metadata

`server.json` is the official registry publication descriptor. `discovery.json` is a dated snapshot of the public discovery manifest, not a replacement for live discovery. This repository does not contain the hosted application's implementation.

Contact: [hello@boncourant.fr](mailto:hello@boncourant.fr).
