# Update components and recover partial effects

- `dirhp_refresh_loaded_versions` pushes saved heads to following copies/instances; `option_id` limits it to one option's followers (with `loaded_version_ids`: intersection). Saving an option never updates placed instances; this call does. Copies with unsaved edits are skipped as
  `dirty` until the designer picks `replace` or `combine` per copy (`modes`; see dirhp-options-and-versions load and restore). Read `skipped` vs `failed` in the result: `recipe_owned`
  -> change step bindings and rerun; a placement inside a placed design moves after the one holding it; the whole call is one Rhino undo step;
  `detached_edit` -> save as new version/option when authorized. `failed` -> fix and rerun (safe), except `state: rollback_unconfirmed`: designer checks the model first. `placement_behind_head` needs a refresh;
  `dirhp_check_version_freshness` does not check this condition.
- A nested child updates to what its parent's (target) version saved for it, not its option's latest, unless the designer
  ticks it. When a child could take a newer save, or has unsaved edits and would move, the update first returns `choices_required` (`child_choices`, `choice_token`)
  and applies nothing; see dirhp-options-and-versions load and restore. A pinned child set to `latest` is re-pinned there. Its parent is then listed in `needs_save` (not edited): save it to record the child.
- `dirhp_update_loaded_version` changes one occurrence to another version in the same option history;
  it creates no duplicate. Updating an owner keeps its placements as drawn; read `placements_kept` for ones that differ. Its nested children take what the target saved (asked like an update: `choices_required`). Recipe-owned occurrences must change through their step and rerun.
  Use `dirhp_load_version` for two versions side by side. Restore saved bytes; do not silently regenerate.
- A nested child records the version it has drawn. Behind its option's latest, it stays there until updated or
  swapped; an editable one moves with `dirhp_update_graph_reference(target_kind="option", target_id=<its own option>)`.
  Nested children are editable for editors, pinned or following; a pinned one stays pinned.
  Saving a child moves its parent's reference (a pin moves to the saved version); saving the parent saves its edited children first. `nested_child_unsaved` on a parent's
  save means that child's unsaved work is not in it; the save still happened.
- After a nested swap, `unload_dirty` names copies with unsaved work: save or undo them, then swap. `rhino_unconfirmed`
  means Rhino did not confirm the result: use Rhino Undo, then check `dirhp_list_loaded_versions`.
- `focus_sync_failed` means creation succeeded. Focus the returned child; do not recreate it.
- Unsaved children produce `pending_nested_options` and parent saves can refuse with
  `save_referenced_option_first`. Save the child first only within existing save authorization.
- Retry `placement_sync_pending` after synchronization. Preserve valid partial work.
