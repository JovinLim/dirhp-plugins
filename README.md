# dirhp-plugins

Public install source for the dirhp assistant plugin. It holds only the skills and the MCP
connector config. It is not the product source.

dirhp lets Claude, ChatGPT and Codex read and edit Rhino 8 models. You need two parts: the dirhp
Rhino plugin, and the dirhp assistant plugin from this repo.

## Plugin

| Marketplace | Plugin |
|---|---|
| `dirhp-marketplace` | `dirhp` |

The connector points at one server, `https://api.dirhp.io/mcp/`. See
[clients/plugin/README.md](clients/plugin/README.md) for what the plugin runs, sends and fetches.

## Sign-in

On first connect a browser window opens for OAuth. Sign in with your dirhp email and password.
There is no token to paste. Create an account at https://dirhp.io/get-started.

## Install the Rhino plugin

Requires Rhino 8 on Windows or Mac.

1. Open Rhino 8, then Tools > Package Manager (or run `PackageManager`).
2. Search for `dirhp`, select it, then select Install.
3. Restart Rhino. In the dirhp panel choose Sign in with browser.

## Install the assistant plugin

Claude Code:

```bash
claude plugin marketplace add JovinLim/dirhp-plugins
claude plugin install dirhp@dirhp-marketplace
```

Start `claude`, run `/mcp`, select `dirhp` and choose Authenticate.

Claude Desktop, claude.ai, Codex and the ChatGPT desktop app: follow the steps at
https://dirhp.io/help (Get started, Connect your own AI app). They use a `dirhp.zip` download.

## Links

- Setup: https://dirhp.io/get-started
- Help: https://dirhp.io/help
- Privacy: https://api.dirhp.io/privacy
- Terms: https://api.dirhp.io/terms/pilot
- Support: help@dirhp.io
