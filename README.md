# dirhp-plugins

Install target for dirhp client packs. This is not the product source: it holds only what a
chat client needs to connect to a hosted dirhp backend.

You need three things, in this order:

1. An account token (`dirhp_…`), sent to you separately.
2. The **dirhp** plugin from this repo (skills + MCP connector).
3. The Rhino plugin package, `dirhp.yak` — it is not in this repository.

## Claude Code

```bash
claude plugin marketplace add JovinLim/dirhp-plugins
claude plugin install dirhp@dirhp-marketplace
claude plugin install dirhp-architecture@dirhp-marketplace
```

Start a new session. Send the account token as `Authorization: Bearer dirhp_…` in the MCP headers.

## Claude Desktop

1. Customize → Plugins → **+** → Add marketplace → Add from a repository.
2. Enter `https://github.com/JovinLim/dirhp-plugins`.
3. Install **dirhp**. Optionally install **dirhp-architecture** (it needs the base plugin).
4. Start a new session.

The first DI tool opens a consent page. Paste the account token there. Do not add a second custom
connector — the plugin already is one.

To pick up a published update, select **Update** on this marketplace and start a new session.

## Codex

```bash
codex plugin marketplace add JovinLim/dirhp-plugins
codex plugin add dirhp@dirhp-marketplace
codex plugin add dirhp-architecture@dirhp-marketplace
```

In the app: Settings → Plugins → **+ Add More…** → `https://github.com/JovinLim/dirhp-plugins`.
Send the account token as `Authorization: Bearer dirhp_…` in the MCP headers.

## ChatGPT

ChatGPT cannot install skills. Settings → Connectors → Advanced → Developer mode, then create a
custom connector with the `/mcp/` URL you were given. Authenticate on the consent page with the
same account token.

Requires ChatGPT Plus, Pro, Business, Enterprise, or Edu.

## Rhino

Drag `dirhp.yak` into Rhino 8 to install it.

The backend URL is already baked in. Do not set `DI_API_URL`.

Then sign in:

1. In Rhino, run the `dirhp_Panel` command.
2. Click **Login** and paste your account token (`dirhp_…`) when the panel asks for the dirhp
   account token.

[IMAGE: dirhp panel showing the Login button and the account token prompt]

To open the web viewer, run `dirhp_Web` or click **Open dirhp web** in the panel.

## Packs

| Pack | What |
|---|---|
| **dirhp** | Skills and the MCP connector. Install this. |
| **dirhp-architecture** | Briefs, component systems, geometric evaluation, option studies. Optional; requires **dirhp**. |

Enable only one dirhp connection plugin per session.

## If something goes wrong

- **Skills but no tools** — the plugin installed, the connector did not. The backend at the URL
  inside the plugin has to be reachable. Ask for a republish, then update the marketplace. Do not
  add a duplicate custom connector.
- **Rhino says it is not signed in** — run `dirhp_Panel`, click **Login**, and paste the account
  token again.
- **Asked to authorize again** — normal after a backend restart; paste the account token.
- **Lost the account token** — it cannot be recovered. Ask for a new one; the old one should be
  revoked.
