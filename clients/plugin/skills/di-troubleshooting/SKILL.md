---
name: di-troubleshooting
description: >-
  Recover a dropped DI connection ("no session" / "not connected" / control socket disconnected).
  Not for Rhino command rejection (`RunScript returned false`) or stale graph reads.
---

# Reconnect dropped DI session

**Use when:** DI worked earlier; calls now fail with "no session" / "not connected", or saves report
control socket disconnected.

**Not this:** default graph reads use live staging. For `stage_unavailable`, inspect the capture
error; do not save merely to make a read possible. For a saved-graph freshness refusal, inspect
the requested scope and returned fix; save only within the user’s authorized workflow.
`Rhino rejected` / `RunScript returned false` → surface error; re-pair won't help.

**Not enrolled** is a different failure: Rhino reports it is not linked to a dirhp account, or its
sockets report an auth rejection. Re-pairing cannot fix it — that machine holds no credential the
backend accepts. Have the user run **DI_Connect** (or **DI_Enroll**), then redeem the printed
enrollment code with `di_connect(code=...)`. Once per machine; then continue with the same connect
flow below.

1. User runs **DI_Connect** in the target document and pastes its one code.
2. `di_connect(code=...)`. If `pending`, wait and retry the same code.
3. Reuse returned `session_id`; retry the failed call.
4. Still failing after successful re-pair → stop looping; surface the error.

**Done:** failed call succeeds after re-pair with the same durable session when the document matches.

## Option datums

For stale datum dependencies, run `di_update_option_datums`. For unknown host outcomes, retry the pending update before staging another placement. Missing references or changed multi-face qualifications require rebinding. A mismatched host datum is reported in recipe proof, including for options without recipes; it does not block saving. Inspect `di_get_option_datum` and the `datums` guide.
