# dirhp in ChatGPT

The ChatGPT desktop app installs the dirhp plugin the same way Codex does — see
[DISTRIBUTION.md](../../DISTRIBUTION.md) section 1 and [`../README.md`](../README.md). This file is
the connector-only fallback for when plugin install is unavailable: add the remote MCP server as a
**custom connector**. There is nothing to build here — this directory is the instructions.

A connector gets no skills. Everything they would have taught is served by the backend instead: the
MCP server's own instructions and the direct `dirhp_get_guide` tool.

## Requirements

- ChatGPT **Plus, Pro, Business, Enterprise or Edu** — developer mode is not available on Free, and
  a workspace admin can switch it off org-wide or allowlist specific connectors
- the backend reachable over **public HTTPS** speaking streamable HTTP (the Tailscale Funnel URL)

## Install

1. Get the URL:
   ```bash
   ./scripts/dirhp-endpoint.sh    # → https://<host>; the connector URL is that + /mcp/
   ```
2. In ChatGPT: **Settings → Connectors → Advanced → Developer mode**, enable it.
3. **Create** a custom connector, paste the URL, and authenticate.
   - open backend: no credential
   - `DIRHP_REQUIRE_AUTH`: the dirhp consent page appears on first use and asks you to sign in with your email and password

Registration uses CIMD (ChatGPT's preferred path) with DCR as the fallback; both are advertised.

## Project instructions

ChatGPT reads the server's `instructions` on connect, which covers the cold start. If you want the
routing pinned in a Project as well, paste this:

```
This project talks to a live Rhino document through the dirhp connector.

Before working in a Rhino document, call orient_me. If several documents are connected, call
list_active_sessions and ask me which file; for same-named files ask for the session label shown in
the dirhp panel. If none is present, ask me to open the dirhp panel in Rhino and click Login. No connection is needed to browse saved
workspaces and options.

Before authoring, measuring, annotating, or composing a recipe or policy, search with dirhp_get_guide(query=...)
and read the relevant guide with dirhp_get_guide — the conventions, refusal codes and safety gates live
there, not in the tool schemas. Fetch design-logic before attaching a recipe.

Most tools are discovered rather than listed: dirhp_discover_tools() for categories,
dirhp_discover_tools(category) for an index, dirhp_discover_tools(names=[...]) for contracts, then
dirhp_invoke(tool_name, session_id, arguments) to call one.

Ask me factual and approval questions in chat — I need to look at Rhino before answering review questions.
```

## Caveats

- The connector goes stale whenever the backend moves to another dev machine; the funnel hostname is
  per-node, so re-paste the URL from `./scripts/dirhp-endpoint.sh`.
- Don't add the custom connector if the plugin is installed — that gives you the same backend twice.
- Only connect MCP servers you trust — a connector can read what the agent sends it.
