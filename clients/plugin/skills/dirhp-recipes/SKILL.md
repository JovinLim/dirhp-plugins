---
name: dirhp-recipes
description: Create, change, or run reusable dirhp recipes and policies for parameters, regeneration, design variants, or semantics that must stay current as the model is edited. Use dirhp-geometry-authoring for direct geometry edits.
---

# Author and run recipes

1. Confirm that parameters, regeneration, variants, or repeated design iteration are required.
   Arrays and loops alone need no recipe. Resolve consequential uncertainty before creation;
   later conversion can replace objects and break identity references.
2. Call `orient_me`; reuse the session/orientation. If absent or
   ambiguous, ask for the file name or the session label shown in the dirhp panel. Select the target option and inspect
   `dirhp_recipe_context`, `dirhp_list_recipes`, and `dirhp_find_recipe_steps` to reuse existing work.
   Recipe and policy catalogs belong to the session's workspace: pass `session_id` to catalog tools.
3. Keep steps that must regenerate together in the same concept; separate independent concepts.
   Fetch `design-logic` for composition choices and `recipe-authoring` for snippet/output contracts.
   Add code with `dirhp_add_snippet_to_option`, parameters, and keyed `created` outputs.
   Those outputs are already owned; do not adopt them or hand-edit their lifecycle tags.
4. Change values with `dirhp_set_recipe_step_bindings`, code with `dirhp_update_option_snippet`.
   Do not add steps merely to change values. Use `dirhp_add_policy_step_to_option` for semantic
   declarations that must refresh on reruns; keep explicit IDs and step ownership.
   Use `dirhp-semantic-framework` when authoring those declarations.
   For backend-only automation needing no Rhino round-trip, use a catalog policy, not a step:
   `dirhp_list_policies`, `dirhp_register_policy`, then `dirhp_run_policy` (once) or `dirhp_install_policy`
   (`rerun_on_dirty=true` reruns after each settled edit); inspect with `dirhp_inspect_option_policies`.
   Fetch `dirhp_get_guide("semantic-framework", sections=["Policies"])`; guide `design-logic` covers
   recipe-vs-policy and timing. A policy failing at save warns (`dirty_policy_failed`) and declares
   nothing; the save still seals.
5. Run `dirhp_run_option_recipe` within the authorized edit scope. Inspect outputs and semantic
   refresh failures before dependent work. Use measurements for numeric claims and actual images
   for visual claims. Fetch `recipe-execution` for proof, inspection, step removal, or recovery;
   inspect partial effects and preserve valid work after failure.
6. Report run results and unresolved diagnostics. Recipe runs stage changes; they do not authorize
   saving. Use `dirhp-options-and-versions` only when saving was requested or already authorized.

Use injected `__rhino_doc__` in snippets, never `ActiveDoc`; keep units/datums explicit and apply
transforms once (guide `datums`). Composed content is read-only.
Call exposed tools directly; otherwise use `dirhp_invoke(tool_name=..., session_id=..., arguments={...})`.
Batch missing contracts with `dirhp_discover_tools(names=[...])`; reuse loaded contracts and guides.
