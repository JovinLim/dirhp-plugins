---
name: dirhp-graph-query
description: Find, count, filter, and relate existing dirhp records and properties with read-only SQL. Use for model questions, including Rhino User Text and User Dictionary values.
---

# Query dirhp records

1. Select the requested graph/state. Saved reads need no Rhino; use `dirhp_find_options` or
   `dirhp_get_workspace` if needed. For live work, reuse the session or call `orient_me`.
   Default reads include focused unsaved state and composed references. On `stage_unavailable`,
   report the failure; do not substitute a version or save to make the read possible.
2. Read only the reference needed for missing query detail:
   - Unknown graph or keys: [discovery](references/discovery.md).
   - Property filters, counts, numeric or dictionary values: [properties](references/properties.md).
   - Neighbours or containment traversal: [relationships](references/relationships.md).
   - Values across live/saved states: [comparisons](references/comparisons.md).
   Reuse known contracts and examples; do not load all references.
3. Run `dirhp_query(sql, limit=50, cursor=None)`: one read-only PostgreSQL SELECT over `graphs`,
   `nodes`, `edges`, and `node_keys`. Scope every query; join on `graph_id` plus node/endpoint IDs.
   Use exact model keys. Count/group/sample unknown-size sets; select only needed columns.
   For versions use literal `'saved:<version_id>'` (maximum 20), not computed strings.
   Content across many saved versions (Rhino only): `dirhp_list_versions(workspace, kind="rhino", latest_per_option=true, ...)`
   for ids (follow `next_cursor`), then query them 20 literals at a time.
4. Page with deterministic `ORDER BY`, unchanged SQL, and `next_cursor`. Restart on
   `graph_changed`/`invalid_cursor`. Check `partial`, unavailable graphs, warnings, and truncation.
   `pending_nested_options` omits unsaved children even when `partial=false`; report their
   `pending[].occurrence_key`. Empty or incomplete results do not establish absence.
   A miss (zero rows, `unknown_node`, `*_not_found`) with `suggestions` lists the model's near
   names. Say there is no exact match and give them. A read may continue for the best one if you
   name it and list the rest; a write waits for the designer. A candidate with `count` > 1 is
   ambiguous: narrow it with the request's context or its `facets`, then ask, or have the designer
   pick in Rhino (`dirhp_cache_rhino_selection`). Never pick silently.
5. Return the requested facts and material limits. Fetch `dirhp_get_node` only for selected geometry.
   Rhino scripts need current `nodes.object_id`; intents and nested `@...` nodes have no GUID.

Calls are stateless; retain graph-scoped IDs or filters as CTEs. Chain steps as CTEs in one
`dirhp_query`. Unadopted Rhino objects require `dirhp_query_object_history`.
For missing tool contracts use `dirhp_discover_tools`; gateway calls put `session_id` on `dirhp_invoke`.
Only for unsupported details/errors, use `dirhp_get_guide("graph-sql", sections=["Errors"])` (guide `graph-sql`) or `query=`; not a prerequisite.
