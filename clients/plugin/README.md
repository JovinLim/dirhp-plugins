# dirhp plugin for Claude

Skills and a connector that let Claude read and edit Rhino 8 models through dirhp. dirhp turns a Rhino document into a spatial-semantic graph (objects, properties, relations, intents) that Claude can query, measure, annotate and change, with saved versions and options you can load, compare and restore.

## Requirements

- A dirhp account.
- Rhino 8 with the dirhp Rhino plugin installed and signed in.

Setup: https://dirhp.io/get-started. Help: https://dirhp.io/help.

## Install

```
/plugin install dirhp --marketplace JovinLim/dirhp-plugins
```

## Sign-in

The first time Claude connects, a browser window opens for OAuth. Sign in with your dirhp email and password. There is no token to paste.

## What it runs, sends and fetches

- The plugin ships only Markdown skills and an MCP server config (`.mcp.json`). It runs no local code and installs no executables.
- The MCP config points at one remote server, `https://api.dirhp.io/mcp/`, over HTTPS. It is the only host the plugin contacts.
- Each tool call sends the arguments Claude chooses (queries, object ids, names, parameters) to that server and returns data from your dirhp workspace.
- Changes to a Rhino document are made by the dirhp Rhino plugin running in your own Rhino, not by this plugin.

## Skills

- `dirhp-graph-query`: read-only SQL over model records and properties.
- `dirhp-semantic-framework`: create and edit intents, properties, relations and representations.
- `dirhp-geometry-authoring`: create, edit or adopt Rhino geometry in an option.
- `dirhp-design-composition`: nested components; place, update and remove version occurrences.
- `dirhp-options-and-versions`: options; save, load, tag, restore and compare versions.
- `dirhp-recipes`: reusable recipes and policies for parameters and regeneration.
- `dirhp-measurement`: dimensions, distances, clearances, clashes and spacing.
- `dirhp-annotations`: dimensions, labels, leaders and witness curves.
- `dirhp-review-views`: plan and section views, image capture, isolating designs.
- `dirhp-troubleshooting`: recover a dropped dirhp connection.

## Privacy, terms, support

- Privacy: https://api.dirhp.io/privacy
- Terms: https://api.dirhp.io/terms/pilot
- Support: help@dirhp.io
- License: proprietary, free to use with a dirhp account. See `LICENSE`.
