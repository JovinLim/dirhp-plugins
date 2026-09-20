---
name: di-design-logic
description: Choose direct authoring, one-time execution, reusable logic, or installed regeneration. Compose evaluation with semantic updates without overwriting unrelated results.
---

# Execution semantics

Stored requirements/relations need no executable logic. Geometry, meaning, and execution timing
are independent choices. Saving, naming, repetition, and loops do not require recipes.

| Need | Mechanism |
|---|---|
| Fixed geometry | `di_run_rhino_python` + adoption where needed; `$di-rhino-authoring` |
| Evaluate | Selectors, `graph.select@1`, `graph.related@1`, `graph.pairs@1`, `measure.*@1` |
| Known object facts | Direct User Text; `DI:class` convention. Classification only for unknown shape-inferred membership |
| Groups/spatial claims/relations | Direct intent/representation authoring |
| Execute existing logic once | `di_run_policy`, `di_run_recipe`, `di_run_steps`; no option-recipe attachment |
| Reuse on demand | `di_register_*` + `di_run_*`; reusability does not imply automatic execution |
| Regenerate with option | `di_install_*`; dirty reruns additionally require policy `rerun_on_dirty=true` |

Search `di_list_policies`, `di_list_recipes`, `di_find_recipe_steps` before writing reusable logic.
`di_copy_step_from_snapshot` reuses a proven step.

## Composition contracts

- Policy: backend-only `backend_function` + `policy` steps. Evaluation produces `$output`;
  policy patch applies ready `intents`/`edges`/`representations`. The patch is one step's write
  contract, not the entire evaluation workflow.
- Example: `starter.enclosed_space@1` selects boundaries, evaluates `geometry.enclosure@1`,
  declares representations, reconciles owned records with `replace_owned`.
- Query scope: `definition.mode="query"` retains selector-based membership. Classification is
  reviewed congruence, not a live spatial rule.
- Read `design-logic` before composing automation or encoding computed host sets as installed
  behavior; some reshape primitives are not registered. It includes executable examples.
- Named-snapshot evaluation: `scope.snapshot_id`. Writes: editable staging only.
  Save separately with `di_save_snapshot` when authorized.
- Reconcile only policy-owned results. Unresolved evidence is not a negative finding.
