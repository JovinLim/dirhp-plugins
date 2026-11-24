# Find every clashing pair

Pass this list as `steps` to `dirhp_run_steps` through `dirhp_invoke` (no option rows,
no save). Discover its contract only if not already loaded. dirhp defines the field names, operation
keys, and `$output` binding syntax. `pairs` and `clash` are chosen aliases; output paths must match
them. Replace each `<id>` with an actual operand ID.
```json
[{"type": "backend_function", "alias": "pairs", "operation_key": "graph.pairs@1",
  "bindings": {"left": ["<id>", "<id>", "<id>"], "max_distance": 0}},
 {"type": "backend_function", "alias": "clash", "operation_key": "measure.pair@1",
  "bindings": {"pairs": {"$output": "pairs.pairs"}}}]
```
- `left` alone self-joins; add `right` for set against set. `max_distance: 0` keeps pairs whose bboxes touch or intersect (a proxy filter, not a measurement). Over `max_pairs` (default 500) the run fails. For all clashes, raise the limit or partition candidates while retaining cross-partition pairs. Narrowing IDs or using `nearest` restricts coverage; report that restriction.
- Keep `outputs.clash.readings` whose `relation` is `overlap` or `contained` (negative `distance`). Check both `outputs.pairs.refusals` and `outputs.clash.refusals`; rejected candidates and pairs are unresolved, not "no clash".
- Only closed meshes report overlap. Planar or bbox-only operands that cross read `touching` at `0.0`: unresolved, not clear.
