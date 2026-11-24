# Follow relationships

`contained_in` points child → parent. Symmetric relations such as `connects_to` are stored once;
check both endpoints. Join on graph and endpoint IDs. Use `dirhp_resolve_scope` for stored scopes.
Examples use model IDs and properties; replace placeholders and confirm keys before querying.

Neighbours: `connects_to` (curve endpoints that coincide) is the dirhp symmetric relation; it must exist in the selected graph.
`<from_id>` is a returned node ID, not a name or Rhino GUID.

```sql
SELECT DISTINCT n.node_id, n.name
FROM edges e JOIN nodes n ON n.graph_id = e.graph_id
  AND n.node_id = CASE WHEN e.source_id = '<from_id>' THEN e.target_id ELSE e.source_id END
WHERE e.graph_id = '<graph_id>' AND e.relation = 'connects_to'
  AND '<from_id>' IN (e.source_id, e.target_id)
ORDER BY n.node_id
```

### Containment tree

Walk down from a parent (`target_id`) to its children (`source_id`). Walk up by swapping the two.

```sql
WITH RECURSIVE tree(graph_id, node_id, depth, path) AS (
  SELECT n.graph_id, n.node_id, 0, ARRAY[n.node_id]
  FROM nodes n JOIN graphs g USING (graph_id)
  WHERE g.focused AND n.properties #>> '{kind,value}' = 'building'
  UNION ALL
  SELECT e.graph_id, e.source_id, t.depth + 1, t.path || e.source_id
  FROM tree t
  JOIN edges e ON e.graph_id = t.graph_id AND e.target_id = t.node_id
    AND e.relation = 'contained_in'
  WHERE t.depth < 10 AND e.source_id <> ALL (t.path)
)
SELECT node_id, depth FROM tree ORDER BY depth, node_id
```
