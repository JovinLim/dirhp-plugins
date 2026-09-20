---
name: di-options-and-snapshots
description: >-
  Manage DI workspaces, options, immutable snapshots, focus and visibility, load/reference,
  dependency-aware recipes, regeneration, and recovery.
---

# DI options, snapshots, and recipes

Use `di_get_guide` for the `options-and-snapshots` lease, import, recovery, and receipt rules.
For state-read refusals use `freshness`; for object history use `event-log`; for legacy names
in receipts use `vocabulary`. Load those guides only when relevant.

Workspace → option → immutable snapshots. The active option receives authoring; focus routes reads
but does not control visibility. Use isolate/show operations deliberately. Give each reusable
component its own option and compose them from a container option (guide `options-and-snapshots`); do not
fork a rival *direction* unless the user asks for one. Start with `di_find_options` so an existing
option is reused when it already answers the task.

For a new component in place, save the parent first, then use
`di_create_nested_option(name=..., at=[x,y,z])`. The child gets its own root and editable worktree;
the parent gets a `compose` reference. `at` is a parent-relative offset. For rotation, author local geometry with datum helpers;
update datums, save the child, then focus and save the parent.
The child worktree is instance 1; use `di_add_design_to_option(target_option_id, key, at)` for additional copies. Target a snapshot instead of an option to pin that exact state. Computed or repeated placements use the same tool with `placements={"$output":"alias.placements"}`. Move a one-off with `di_set_option_origin(loaded_snapshot_id=...)`; remove it with `di_remove_graph_reference`.

Saving keeps the drawn placement. `placement_behind_head` means a following copy has an older snapshot than its source option; use `di_refresh_loaded_snapshots` when the user wants the latest design. `di_check_snapshot_freshness` checks geometry, not source-option advancement. Native Rhino Undo/Redo restores direct placement/removal records with the geometry. Retry `placement_sync_pending` after synchronization.

To stage already-created or already-cached Rhino objects into an option without saving, use
`di_adopt_cached_objects` (fetch guide `object-cache`); it never defaults to all cached objects and
refuses the whole request if any target object is missing or foreign-owned. Objects a recipe step
already declared as `created` output are already owned — do not adopt them again.

Saved-parent composed reads report `pending_nested_options` until the child has a snapshot.
Parent saving then requires `save_referenced_option_first`; it names the unsaved child.
Staging reads do not compose references. Parent saves pin the child's head, so old parent snapshots
keep their original child version. References without datum bindings do not propagate moves. Managed datum inheritance propagates placement
on explicit update or run; visibility, unloading, and deletion remain separate. A `focus_sync_failed` warning means creation succeeded: retry
`di_focus_option` on the returned child instead of creating it again.

Semantic manifests are saved beside geometry. Runs persist ordered dependencies, fingerprints,
resolved globals, and source hashes; inspection views expose this evidence.

Recipe rules:

1. Inspect `di_recipe_context` and append utilities, backend functions, policy steps, or
   deterministic snippets. Search `di_list_recipes` / `di_list_policies` / `di_find_recipe_steps`
   before creating. Use a registered recipe when the same step pack should be reused; use
   `$di-design-logic` to choose run vs install vs dirty-reactive. Use `$di-rhino-authoring` for
   Rhino geometry/output contracts.
2. Wire nested `$output` references only to earlier declared paths. Use `$global` for durable
   manifest input; external globals need a pinned reference and exact `manifest_hash`.
3. Treat compiler errors—unknown/forward output, missing path, type mismatch, dependency cycle,
   stale graph barrier, or unpinned global—as authoring errors. DI validates order and never
   reorders steps. Proven errors block execution, not saving; warnings describe what could not
   be proven. A step that can never succeed is removed with `di_remove_step_from_option(position)` — removal is the
   one exception to order immutability; survivors renumber densely, reordering stays forbidden. It
   refuses while another step still binds `$output` to the removed alias (rebind or remove those
   first; `force` does not bypass it). Recorded run proof no longer matches afterwards.
   Regenerate explicitly with
   `di_run_option_recipe(from_position=<regenerate_from>)`, using the returned position. Saving
   does not rerun the option recipe.
4. Run the recipe. Globals resolve once as `ctx.globals`; the local manifest is rechecked before
   side effects and completion. Failure rolls back the host transaction. Use
   `di_capture_viewport` separately when visual review is needed.
5. Regenerate from a changed producer. DI expands through transitive consumers and hydrates a
   skipped producer only when its output and every captured dependency still match. Rhino-mutating
   output is never treated as pure data.
6. To iterate, return `{"$goto": {"position": N, "bind": {"N": {...}}}}` beside your ordinary
   result: the cursor resumes at N with those bindings for the rest of the run. The key is stripped,
   so it never reaches `ctx["outputs"]`. Backward only, never across a policy or placement
   step, rebinding only inside the replayed span. Bindings are run-scoped — keep a converged value
   with `di_set_recipe_step_bindings`. Decide when to stop from your own prior output
   (`ctx["outputs"][<your alias>]`), carrying forward only what the next pass needs: the run stops
   with `recipe_state_budget_exhausted` if outputs grow unbounded, `recipe_run_budget_exhausted` on
   wall-clock, `recipe_jump_budget_exhausted` at `max_jumps`, `recipe_cancelled` on Esc. All honour
   `safe_stop`. A replayed Rhino step regenerates over its own output, so passes are throwaway.
7. To keep a pass, append `di_add_save_step_to_option`. Reaching it seals what was built so far and
   continues in a fresh transaction, making the option's snapshot history the iteration record. The
   recipe is then atomic **per segment**: a later failure rolls back only to the last save. A save
   placed mid-recipe seals a snapshot whose recipe is longer than what it actually built (as a
   safe-stop does), so put it last in the loop body. Needs a live document and a run from position
   1; `sealed_snapshots` reports what it sealed. It is also the one point where the graph catches
   up, so a `requires_graph` step after it reads current data.

Compile-check before running: `view="summary"` returns `valid` and `counts.diagnostics` without executing anything. Read recipes through bounded views, never whole. `di_inspect_option_recipe` takes `option_id` for an
option's effective state, `snapshot_id` for an exact historical recipe, or neither for the focused
option, with `view` = `summary` | `steps` | `step` | `output` | `run_log`. `di_trace_option_recipe`
answers who consumes what from one origin (`alias`, `position`, or an `output_handle` such as
`layout.fin_boxes`), upstream/downstream/both, bounded by depth or a destination path. Paginate
with `limit`/`next_cursor`; a `recipe_changed` or `run_changed` reply means restart without the
cursor. Contract authority reads `registered`, `inferred`, `observed`, or `unknown` — observed
values exist only while the run's fingerprints still match, so never quote stale evidence as
current. Run responses are compact: pull outputs and transcripts from the `output` and `run_log`
views rather than expecting them inline.

Use `di_compare_design_states` and history summaries for change. Load absent saved geometry; use graph
references or snapshot placement for read-only composition. To put one loaded occurrence onto
another snapshot of the same option without creating a duplicate, call
`di_update_loaded_snapshot`. Additive `di_load_snapshot` remains the way to keep two versions
visible at once. Resolve node ownership before editing composed results. Treat `DI:generated_by` as
recipe ownership metadata, never a hand-edit target.
Use `role="data"` for `$global`-only references. Reverts do not re-resolve globals: read
`pinned_globals`; explain `was`/`now` and ask on `drifted`, but proceed on `current`.

Save when requested or already authorized. Saving records the current geometry without running
the option recipe. An invalid chain or missing run proof does not block saving: the receipt reports
`recipe_proof` (`verified`, `unverified`, or `invalid`) and `blockers`. There is no `save_ready`
field. A no-change save keeps the current latest snapshot. Marked dirty policies run during saving;
a failed batch rolls back its declarations and adds a warning, while the geometry still saves.
After a dropped connection, inspect `di_get_recipe_run_status`. An unknown outcome does not block
saving, loading, or another run. Inspect the live result; reload a snapshot with `di_load_snapshot`
if needed. Rerun explicitly when recipe verification is required.

**Done:** the intended option owns the result, recipe proof and unresolved diagnostics are reported,
composed content remains read-only, and the final saved or unsaved state matches the request.

## Option datums

Use `di_set_option_datum` to stage position, orientation, signed axes, or live alignment. `di_update_option_datums` applies inherited changes to managed worktrees; references without datum bindings remain independent. Datum readiness contributes to the reported recipe proof, including for options without recipes; it does not block saving. Fetch the `datums` guide for placement and recovery contracts.
