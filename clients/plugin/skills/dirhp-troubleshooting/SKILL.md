---
name: dirhp-troubleshooting
description: >-
  Recover a dropped dirhp connection ("no session" / "not connected" / control socket disconnected).
  Not for Rhino command rejection (`RunScript returned false`) or stale graph reads.
---

# Reconnect dropped dirhp session

**Use when:** dirhp worked earlier; calls now fail with "no session" / "not connected", or saves report
control socket disconnected.

**Not this:** default graph reads use live staging. For `stage_unavailable`, inspect the capture
error; do not save merely to make a read possible. For a saved-graph freshness refusal, inspect
the requested scope and returned fix; save only within the user’s authorized workflow.
`Rhino rejected` / `RunScript returned false` → surface error; reconnecting won't help.

**Not enrolled** is a different failure: Rhino reports it is not linked to a dirhp account, or its
sockets report an auth rejection. Reconnecting cannot fix it — that machine holds no credential the
backend accepts. Have the user open the dirhp panel in Rhino (`dirhp`) and click **Login**. Rhino shows a code; they sign in in the browser, type the code and click Link Rhino.
Once per machine; then continue with the same connect flow below.

1. `orient_me()` when one document is connected. Several: `list_active_sessions`, ask the user which
   file by `doc_name`, then pass that `session_id`.
2. Open but not connected: `list_present_documents` -> `dirhp_activate_session`. Same-named files: ask
   the user for the session label shown in the dirhp panel.
3. Nothing present (e.g. the device token was revoked and Rhino fell back to the Login view): ask the
   user to open the dirhp panel in Rhino and click **Login**.
4. Reuse returned `session_id`; retry the failed call.
5. Still failing after reconnecting -> stop looping; surface the error.

**Recipe run reports `rhino_disconnected`:** with `document_mutated: false`, nothing reached
Rhino; reconnect as above, then run again. With `document_state: "unknown"`, the connection
dropped mid-run: after reconnecting, call `dirhp_get_recipe_run_status` before you run again or
repair geometry.

**Done:** failed call succeeds after reconnecting with the same durable session when the document matches.
