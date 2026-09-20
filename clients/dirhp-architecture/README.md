# DI Architecture

An optional skill pack for architectural work with DI. Install it alongside the base **dirhp**
plugin. The base plugin provides the DI connection and generic operating skills; this pack adds
architectural decisions and workflows. It contains no MCP server, credentials, hooks, or automatic
project setup.

The generated `dirhp-dev-a` and `dirhp-dev-b` plugins also supply the required base connection
and skills. Enable only one DI connection plugin at a time.

## Install

For Codex, install both packs from the repository marketplace:

```bash
codex plugin marketplace add JovinLim/dirhp
codex plugin add dirhp@dirhp-marketplace
codex plugin add dirhp-architecture@dirhp-marketplace
```

If the base plugin is already installed, add only `dirhp-architecture`. Start a new thread,
connect through the base plugin, and use `$di-architecture`.
See [Codex setup](../README.md#codex-install-both-di-packs) for app installation, local
development, and marketplace refresh commands. Codex installation does not require a ZIP build.

For Claude Desktop, add `https://github.com/JovinLim/dirhp` through Customize → Plugins →
Add marketplace → Add from a repository. Install **dirhp** and **dirhp-architecture**.
If the marketplace is already connected, select **Update**, then install the architecture pack.
The marketplace changes must be pushed to GitHub first. No ZIP build is needed.

For Claude Code, see [Claude setup](../README.md#claude-install-both-di-packs).

For ZIP installation:

Run `./scripts/build-clients.sh` from the dirhp repository. It creates:

- `dist/dirhp-claude.zip` — base DI plugin.
- `dist/dirhp-architecture.zip` — this pack, with both client manifests at the archive root.

For Claude Desktop, upload both ZIP files through Customize → Plugins. If the base plugin is
already installed, add only the architecture ZIP. Start a new thread after installation.
For clients that load local plugin directories, use the extracted architecture directory alongside
the base plugin. This pack is listed in the root Claude and Codex marketplaces. Client installation
does not register recipes, install policies on options, or alter a Rhino document.

## Skills

| Skill | Use |
|---|---|
| `di-architecture` | Turn an architectural brief into a scoped DI task |
| `di-architecture-systems` | Compose components, datums, and repeatable design logic |
| `di-architecture-evaluation` | Evaluate geometric criteria with a stated measurement basis |
| `di-architecture-options` | Develop and compare alternatives using consistent evidence |

Examples: “Compare these two massing options against the brief”; “Make this façade bay reusable”;
“Check the spacing of these columns.” Direct space intents and representation choices are covered;
automatic room lifecycle is not implied.

## Compatibility and scope

This first release was authored against the DI contracts in repository revision `3224cbc`.
The skills fetch guides and tool contracts from the running backend as needed. Availability is
checked per task; a missing operation is reported rather than replaced with an invented API.
The base plugin's package version alone does not establish backend capability.

Static package validation and archive checks do not establish live Rhino coverage. Maintainer
evaluation scenarios are kept outside the shipped pack in `clients/evaluations/`.

The architecture skills are maintained here. They are not generated from the base plugin's
playbooks. Pack updates do not update project vocabulary, saved recipes, or installed policies.
