# dirhp in ChatGPT

ChatGPT has no plugin format: it cannot install skills, only add a remote MCP server as a **custom
connector**. So there is nothing to build here — this directory is the instructions.

Everything the skills would have taught is served by the backend instead: the MCP server's own
`instructions`, and the `di_list_guides` / `di_get_guide` tools, which are promoted to first-class
tools precisely so a skill-less client can find them.

## Requirements

- ChatGPT **Plus, Pro, Business, Enterprise or Edu** — developer mode is not available on Free, and
  a workspace admin can switch it off org-wide or allowlist specific connectors
- the backend reachable over **public HTTPS** speaking streamable HTTP (the Tailscale Funnel URL)

## Install

1. Get the URL:
   ```bash
   ./scripts/di-endpoint.sh    # → https://<host>; the connector URL is that + /mcp/
   ```
2. In ChatGPT: **Settings → Connectors → Advanced → Developer mode**, enable it.
3. **Create** a custom connector, paste the URL, and authenticate.
   - open backend: no credential
   - `DI_REQUIRE_AUTH`: the DI consent page appears on first use and asks for your account token

Registration uses CIMD (ChatGPT's preferred path) with DCR as the fallback; both are advertised.

## Project instructions

ChatGPT reads the server's `instructions` on connect, which covers the cold start. If you want the
routing pinned in a Project as well, paste this:

```
This project talks to a live Rhino document through the dirhp connector.

Start with di_connect. If several documents are present, ask me to run DI_Connect in Rhino and give
you the pairing code. Then call orient_me.

Before authoring, measuring, annotating, or composing a recipe or policy, call di_list_guides and
read the relevant guide with di_get_guide — the conventions, refusal codes and safety gates live
there, not in the tool schemas. Fetch design-logic before attaching a recipe.

Most tools are discovered rather than listed: di_discover_tools() for categories,
di_discover_tools(category) for an index, di_discover_tools(names=[...]) for contracts, then
di_invoke(tool_name, session_id, arguments) to call one.

Ask me approval questions in chat, not through prompt_user_text — I need to look at Rhino first.
```

## Caveats

- The connector goes stale whenever the backend moves to another dev machine; the funnel hostname is
  per-node, so re-paste the URL from `./scripts/di-endpoint.sh`.
- Only connect MCP servers you trust — a connector can read what the agent sends it.
