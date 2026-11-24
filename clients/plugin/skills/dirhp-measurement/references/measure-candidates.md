# Measure each candidate

Use this when a name matches several objects (`count` > 1) and the request is a read, for example
"how far is Reception 01 from the wall". Pass the list as `steps` to `dirhp_run_steps` through
`dirhp_invoke` (no option rows, no save). Replace `<id>` and the selector with real values.
```json
[{"type": "backend_function", "alias": "walls", "operation_key": "graph.select@1",
  "bindings": {"where": {"name": "Wall"}}},
 {"type": "backend_function", "alias": "pairs", "operation_key": "graph.pairs@1",
  "bindings": {"left": ["<id>"], "right": {"$output": "walls.ids"}, "nearest": 4}},
 {"type": "backend_function", "alias": "d", "operation_key": "measure.pair@1",
  "bindings": {"pairs": {"$output": "pairs.pairs"}}}]
```
- Narrow first with the request's context: add `relation` (for example a bounding or containment
  edge) or a tighter `where`. `nearest` ranks by bbox gap, a proxy, not a measurement.
- `outputs.d.readings` holds one reading per pair, keyed `<a>~<b>`, about 0.7 KB each. Keep the
  candidate set small (about 5).
- Answer for the best candidate only if you say which one and why, and list the others with their
  readings. For an edit, ask the designer to choose first. Never pick silently.
- These readings have no kernel accuracy and no `scope` (current focus only). For a tolerance
  decision, re-measure the chosen pair with `dirhp_measure`.
