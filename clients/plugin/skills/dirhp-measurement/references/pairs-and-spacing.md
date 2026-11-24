# Measure pairs and spacing

- Inspect `relation`, `readings`, `witness`, `convention`, and `convention_unavailable` (a labelled fallback to `nearest_faces`, not a refusal). Conventions: `nearest_faces`, `centerline`, `clear`, `overall`, `centroid`. Negative clearance means overlap or containment, never zero.
- `witness` describes the measured endpoints and direction; it is not a general separation vector.
  Mesh overlap depth can be volume-derived with a zero-length witness. Never use that magnitude
  as a translation to resolve a clash.
- Before a numeric edit, measure and identify the moving operand, target, direction, and constraints
  from the request/model. Ask only for missing choices that affect the result; remeasure after editing.
- Pair/series calls use `convention` and, where supported, `along`; do not pass an extents `frame`.

Use `dirhp_measure(a=id, b=id)` for one pair. Set operands return a minimum with `nearest_faces`
only; `unsupported_set_convention` refuses other conventions. A set result names one winner,
not every clash. Use `dirhp_measure_series(node_ids=[...])` or `scope_id` for consecutive spacing;
a grid needs `along`. A two-member scope is not a pair.
`detail="full"` adds operand provenance; compact results already include warnings.
