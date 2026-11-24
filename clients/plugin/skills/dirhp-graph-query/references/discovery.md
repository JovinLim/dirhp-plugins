# Find graphs and keys

All selected columns are dirhp schema; returned names and IDs are design data.

```sql
SELECT graph_id, source, focused, root_name, option_name, version_id
FROM graphs
ORDER BY focused DESC, graph_id
```

Key discovery: `<graph_id>` is a placeholder for the selected graph. Returned keys are not a
fixed vocabulary; `nodes` is only a count alias.

```sql
SELECT source, key, count(DISTINCT node_id) AS nodes
FROM node_keys
WHERE graph_id = '<graph_id>'
GROUP BY source, key
ORDER BY key, source
```

Use `graphs.focused` only when it matches the requested design. Retain graph IDs for later reads.
If the document is absent or ambiguous, ask the user for its file name or the session label shown in the dirhp panel; reuse returned orientation.
Explicit staging scope excludes composed references.
