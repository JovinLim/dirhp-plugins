---
name: dirhp-measurement
description: Obtain dimensions, distances, clearances, clashes, and spacing from live or saved dirhp geometry.
---

# Measure dirhp geometry

1. Select operands and state. Omitted `scope` reads current focus including unsaved edits;
   `{"version_id":...}` reads that version; `{"option_id":...}` reads its latest version;
   add `"state":"staging"` for live capture. Do not save merely to measure.
2. Choose the measurement and load detail only when needed:
   - Size: `dirhp_get_extents(target=..., frame=...)`. Read returned axes, `frame`, and
     `frame_unavailable`; magnitude order (`major/median/minor`) does not identify height.
   - Pair clearance or repeated spacing: [pairs and spacing](references/pairs-and-spacing.md).
   - Every clashing pair: [batch clashes](references/batch-clashes.md).
   - Several objects match the name (`count` > 1) in a read: [measure each candidate](references/measure-candidates.md).
   - Undeclared geometry, reference operands, or thickness keys:
     [representations](references/representations.md); inspect `dirhp_list_representations`.
3. Check `source`, `fidelity`, `reading_fidelity`, `reliable`, refusals, and fallback warnings.
   Use `accuracy="kernel"` for tolerance decisions requiring exact authored surfaces.
   Hidden objects remain measurable; visible-only work needs `visibility.display_eligible` filtering.
4. Report value, units, state, frame/convention, and material limits. For clashes include `relation`:
   negative clearance means overlap/containment. Planar or bbox-only intersections can return
   `touching` at zero; this does not prove clearance. An overlap magnitude is not a move vector.
   Measure before numeric edits and remeasure after them; measurement alone authorizes no edit.

Use `dirhp-annotations` to display a reading. Backend recipe measurements use `measure.node_extents@1`,
`measure.extents@1`, `measure.pair@1`, or `measure.series@1`; they have no kernel accuracy or
`width_keys`. Discover missing contracts in a batch. For refusal recovery not covered by the selected reference use
`dirhp_get_guide("measuring-operations", sections=["Refusal codes"])` (guide `measuring-operations`).
