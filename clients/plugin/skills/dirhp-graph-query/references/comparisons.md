# Compare specific values across states

Use `dirhp_compare_design_states` for a semantic change summary. SQL is for selected stored values.
Resolve the requested version ID first. Use literal `'saved:<version_id>'` graph IDs, at most
20 per call; computed strings do not load versions. Saved-only reads need no Rhino.
Confirm the focused graph is the requested live state. Never replace unavailable staging with
an older version. Compare matching ownership/occurrence scope; independently created alternatives
need an explicit correspondence rather than an assumed shared node ID.

The example uses the model key `zone`; replace it with a confirmed key. Retain added/removed values
with a full join and compare NULL safely with `IS DISTINCT FROM`.

```sql
SELECT coalesce(l.node_id, s.node_id) AS node_id, l.zone AS live, s.zone AS saved
FROM (SELECT k.node_id, k.value #>> '{}' AS zone FROM node_keys k JOIN graphs g USING (graph_id)
      WHERE g.focused AND k.source = 'user_text' AND k.key = 'zone') l
FULL JOIN (SELECT node_id, value #>> '{}' AS zone FROM node_keys
           WHERE graph_id = 'saved:<version_id>' AND source = 'user_text' AND key = 'zone') s
  ON s.node_id = l.node_id
WHERE l.zone IS DISTINCT FROM s.zone
ORDER BY 1
```
