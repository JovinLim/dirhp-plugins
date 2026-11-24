# Filter and count properties

Keys are exact and case-sensitive: `name` differs from `dirhp:name`. Confirm model keys and values;
examples do not define required properties. Specify `node_keys.source` where it matters.
User Text keeps its key spelling; User Dictionary keys use `User Dictionary.<key>`.
Joins can duplicate nodes: use `EXISTS` for filters and count distinct identities per graph.
Exclude `framework_kind = 'graph_boundary'` when counting design entities from `nodes`.

Count: `user_text` is a dirhp source value. `zone` and `north` are illustrative model-defined key/value
strings; use them only if confirmed in the design. `matches` is a SQL alias.

```sql
SELECT count(DISTINCT node_id) AS matches
FROM node_keys
WHERE graph_id = '<graph_id>' AND source = 'user_text'
  AND key = 'zone' AND value #>> '{}' = 'north'
```

A missing key is NULL; a value that is not a number stays NULL instead of failing the query.

```sql
SELECT n.node_id, h.value #>> '{}' AS raw,
       CASE WHEN jsonb_typeof(h.value) = 'number' THEN (h.value #>> '{}')::numeric END AS height
FROM nodes n JOIN graphs g USING (graph_id)
LEFT JOIN node_keys h ON h.graph_id = n.graph_id AND h.node_id = n.node_id
  AND h.source = 'user_text' AND h.key = 'height'
WHERE g.focused AND n.layer = 'Rooms'
ORDER BY n.node_id
```

### Nested User Dictionary values

The top-level entry is a `node_keys` row; deeper paths come from `properties`.

```sql
SELECT n.node_id,
       n.properties #>> '{User Dictionary,value,fire,rating}' AS rating,
       CASE WHEN jsonb_typeof(n.properties #> '{User Dictionary,value,fire,test,minutes}') = 'number'
            THEN (n.properties #>> '{User Dictionary,value,fire,test,minutes}')::numeric END AS minutes
FROM nodes n JOIN graphs g USING (graph_id)
WHERE g.focused AND n.properties #> '{User Dictionary,value,fire}' IS NOT NULL
ORDER BY n.node_id
```

Keys one level inside a dictionary entry:

```sql
SELECT k.key, sub.key AS nested_key, count(*) AS nodes
FROM node_keys k JOIN graphs g USING (graph_id)
CROSS JOIN LATERAL jsonb_object_keys(
  CASE WHEN jsonb_typeof(k.value) = 'object' THEN k.value ELSE '{}' END) AS sub(key)
WHERE g.focused AND k.source = 'user_dictionary'
GROUP BY 1, 2
ORDER BY 1, 2
```
