# Boncourant MCP

Public integration documentation for [Boncourant](https://boncourant.fr), the French electricity-cost optimizer and auditor. This repository contains documentation and public discovery metadata for the hosted service.

## Connect

- **Endpoint:** `https://boncourant.fr/mcp`
- **Version:** 1.6.0 (live contract verified 12 November 2026)
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

Start with `list_capabilities` to discover supported tasks, evidence requirements and limits. The public server exposes 19 tools in version 1.6.0, verified 12 November 2026:

`list_capabilities`, `hc_schedule_change_impact`, `simulate`, `analyze_bill`, `get_methodology`, `decompose_invoice`, `annualize_invoice`, `check_subscribed_power`, `analyze_renewal_terms`, `assess_historical_optimality`, `renewal_advisor`, `extract_energy_document`, `run_method`, `assess_ppa`, `create_analysis_study`, `get_analysis_study`, `get_analysis_report`, `eex_connector`, `pro_catalog`.

Use the live manifest or MCP `tools/list` for current schemas. Available checks depend on the supplied evidence. Retrospective billing findings and prospective savings estimates are distinct; incomplete evidence does not establish a billing error or whole-market optimality.

## Privacy and boundaries

The public MCP service does not expose private customer accounts, workspaces, Enedis consent, supplier switching or supplier outreach. Direct document extraction uses Vertex AI in the EU (`eu` location), removes sensitive identifiers from the response, and does not retain the source document. Uncertain fields require confirmation. Asynchronous studies use opaque receipts; raw input is encrypted until processing ends, then erased, and results expire within 24 hours. Shared-service rate and size limits apply. Review the [integration guidance](INTEGRATION.md) and [privacy policy](https://boncourant.fr/rgpd) before submitting documents.

## Public metadata

`server.json` is the official registry publication descriptor. `discovery.json` is a dated snapshot of the public discovery manifest, not a replacement for live discovery. This repository does not contain the hosted application's implementation.

Contact: [hello@boncourant.fr](mailto:hello@boncourant.fr).

## Version 1.6.0 contract

- **Canonical tariff options:** `base | hp_hc | tempo`. Use these lowercase values in `simulate.option` and `analyze_bill.option`; document extraction returns the same vocabulary in `contract.structure_hint`. Do not send display labels such as `Base`, `HP/HC` or `Tempo` as option values.
- **Shared input schemas:** the discovery manifest and MCP `tools/list` expose the same described parameter schemas. Use them for types, units, required evidence and accepted values.
- **Continue in Boncourant:** `simulate` and `analyze_bill` return `next_step: { "url": "https://boncourant.fr/", "message": "…" }`. The URL is absolute and starts a residential study; the direct MCP result is not saved there. `list_capabilities` returns absolute `publicUrl` links to the appropriate customer journey; its `next_step` is guidance text.
- **EEX availability:** `eex_connector` returns `no_snapshot` when no observation exists at or before the requested date, and `redistribution_not_permitted` when observations exist but public redistribution is prohibited. Neither status supplies public prices. An available snapshot does not guarantee freshness; inspect its date, provenance and coverage before using it.
- **Document extraction:** `extract_energy_document` processes invoice/contract text or a PDF through Vertex AI in the EU. The source document is not retained; returned uncertain fields and reconciliation issues need confirmation. Its extracted facts are evidence for the supported checks, not an automatic finding of a billing error.

The temporary-study boundaries remain: `run_method` is read-only. Creating an isolated study writes expiring state and may fetch an explicitly supplied document, so it remains non-destructive and idempotent, with `readOnlyHint: false` and `openWorldHint: true`. It never changes a customer account or supplier contract.

## Worked examples

These answer pages show the calculation, assumptions, dated inputs and route into a customer study:

- [Tempo with a heat pump and electric car](https://boncourant.fr/tempo-edf-rentabilite#pac-voiture-electrique): compare Base, HP/HC and Tempo on one fictional household scenario.
- [Off-peak hours with a heat pump](https://boncourant.fr/heures-creuses#pompe-a-chaleur): compare annual Base and HP/HC costs at explicit off-peak consumption shares.
- [Comparing professional electricity offers](https://boncourant.fr/professionnels/comparer#comparer-pme): compare the same C5 consumption and complete invoice perimeter; C4 supplier prices require quotes.

Re-read the linked page and live schemas when preparing an answer after a release. [Official registry version 1.6.0](https://registry.modelcontextprotocol.io/v0.1/servers/fr.boncourant%2Fboncourant/versions/1.6.0).
