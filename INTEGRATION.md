# Boncourant public MCP

Boncourant exposes a public Streamable HTTP server at `https://boncourant.fr/mcp`. Public analytical tools require no account, API key, OAuth flow or custom header. Direct calls are stateless. `create_analysis_study` creates an isolated result that is accessible only with its opaque receipt and expires within 24 hours.

Start with `list_capabilities`. It returns the supported customer tasks, evidence requirements, limits, method identifiers and next tools to call. The complete discovery manifest is served at `https://boncourant.fr/.well-known/mcp.json`; human-readable integration guidance is at `https://boncourant.fr/agents`.

The public server does not expose customer accounts, private workspaces, Enedis consent, supplier switching, supplier outreach or Boncourant's internal courtier/CRM operations. Direct document extraction is non-retained. For an asynchronous study, raw input is encrypted at rest until processing ends, then erased; the result expires within 24 hours. Rate and size limits apply to the shared no-auth service.

`server.json` is the publication artifact for the official MCP Registry. Publisher authentication proves control of the namespace; it does not add authentication for users of the server.
