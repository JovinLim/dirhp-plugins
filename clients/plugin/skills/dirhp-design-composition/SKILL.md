---
name: dirhp-design-composition
description: Create nested dirhp components; place, update, and remove version occurrences. Use for component ownership and placement, not design alternatives or camera views.
---

# Compose design components

1. Find and reuse the source with `dirhp_find_options`. Identify the parent option, source version,
   occurrence key, units, and datum. For live changes reuse the session or call `orient_me`.
2. Create only the requested composition:
   - New child: within save authorization, save the parent, `dirhp_create_nested_option(name=..., at=...)`,
     author, save the child, focus the parent, then save the parent. `at` is parent-relative.
     If required saves are not authorized, resolve that before this dependent workflow.
   - Existing component: `dirhp_add_design_to_option(target_option_id, key, at)`; target a version
     to pin it. Computed placements can bind `placements={"$output":"alias.placements"}`.
   - Move: `dirhp_set_option_origin(loaded_version_id=...)`; remove: `dirhp_remove_graph_reference`.
   - Move only the origin mark (e.g. to a hinge), geometry staying put: `dirhp_set_option_base_point(loaded_version_id,
     point, x_axis?, z_axis?)`. The copy shows Edits until saved; following copies stay in place on Update.
   - Open copy as a nested child: `dirhp_add_as_child(loaded_version_id, parent_loaded_version_id?, slot_loaded_version_id?)`;
     back to top level: `dirhp_remove_from_parent(loaded_version_id)`; several placed copies of one owner
     at once: `dirhp_remove_placed_copies(loaded_version_ids)`. Each stages without saving and is one Rhino Undo step.
   - Saved options as children: `dirhp_add_saved_as_children(version_ids, parent_loaded_version_id?)`. The one
     copy of each option that is no one's child in the file links where it is; otherwise the version loads at
     its original location and links there. Nothing moves. Two such copies refuse all with
     `multiple_loaded_copies`: ask which, then `dirhp_add_as_child`. All or nothing, one Rhino Undo step.
   - One placement inside a saved parent, alone: `dirhp_list_version_occurrences(version_id)`, then
     `dirhp_load_version(occurrence={"parent_version_id": ..., "path": [...]})`. It opens where it sits in the
     project, not linked into the parent.
   - Swap the design a direct placement draws: `dirhp_update_graph_reference(reference_id=..., target_kind="option"|"version", target_id=...)`;
     refused while the placement has local edits. Several placements of one option to one design, as one
     Rhino Undo step and all or nothing: `dirhp_swap_graph_references(reference_ids=[...], target_kind, target_id)`.
   - Swap the design a nested option draws: the same call with `target_kind="option"`; add
     `draw_version_id` to draw an older version as unsaved work, not a pin. One Rhino Undo reverses it,
     nested options included. Refused while anything under the child has unsaved work.
   - Fill an empty nested option: the same swap. Its unused option is deleted, or kept and the reply's
     `placeholder` says why. A parent can't save until the child has a version: save it empty to keep the slot.
     Another copy of that design in the file doesn't matter: each copy is editable and independent.
   - Edited placement: `dirhp_save_version(loaded_version_id=<placement>)` saves a new version of the
     placed design's own option. Rooms pinned to it are re-pinned and rooms following
     the option follow; a room with no other change then reads `needs_save`, not edited. Saving a copy saves the edited copies inside it first; a placement inside a
     placed design can be saved alone, and its holder then reads `needs_save` until saved. Viewers cannot save.
3. Resolve ownership before edits. Composed members are read-only; edit the source.
   Never hand-edit `dirhp:generated_by` geometry. A child cannot compose its parent (`reference_cycle`);
   read parent values with `$global` or a `role="data"` reference. Preserve surviving occurrence keys.
4. For version updates or partial failures, read [updates and recovery](references/updates.md).
5. Verify placement and report source version, occurrence, and saved/unsaved state.
   `dirhp_set_option_datum` stages a datum; `dirhp_update_option_datums` applies it. Apply transforms once.

Discover missing contracts with `dirhp_discover_tools`; gateway calls put `session_id` on `dirhp_invoke`.
Fetch `design-composition` only for missing composition contracts and `datums` for rotation,
alignment, or live bindings. Use `dirhp-options-and-versions` for save receipts.
