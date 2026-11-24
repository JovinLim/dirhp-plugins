---
name: dirhp-geometry-authoring
description: Create, edit, or adopt Rhino geometry in a dirhp option. Use for direct geometry changes; use dirhp-recipes for parameter-driven regeneration.
---

# Author Rhino geometry

1. Call `orient_me`; reuse the session and returned orientation.
   If the document is absent or ambiguous, ask the user for its file name or the session label shown in the dirhp panel.
   Confirm the target option, units, and datum. Creation/edit requests authorize only their scope.
2. Query existing objects with `dirhp_query`; use current `nodes.object_id` for Rhino scripts,
   not `node_id`. Intents and nested `@...` nodes have no object GUID. Composed content is read-only.
   Use direct scripts for given values and local edits, including loops and arrays.
   For requested parameters, regeneration, or variants, use `dirhp-recipes` before creating geometry.
3. Run `dirhp_run_rhino_python` with injected `__rhino_doc__`, never `ActiveDoc`.
   `scriptcontext` and `rhinoscriptsyntax` use the session document only during execution.
   Replace tracked geometry with `__rhino_doc__.Objects.Replace(object_id, new_geometry)`.
   For attributes, duplicate, edit, then `ModifyAttributes`; in-place User Text edits omit dirhp events.
   Use `dirhp:class` for roles and `dirhp:name` for labels. Never write lifecycle tags or hand-edit
   `dirhp:generated_by` objects; change their recipe inputs/code instead.
4. Adopt new geometry in the same Undo record with `adopt_created_geometry=True` and the
   target `option_id` (omit for focus). Otherwise use `dirhp_adopt_objects` with the run's
   `history.operation_id`; check `resolved`. Pass `loaded_version_id` to select an exact editable
   copy without changing focus (exclusive with `option_id` and `version_id`).
   Recipe `created` outputs are already owned.
   To take objects out of a copy without deleting them, use `dirhp_remove_from_option(loaded_version_id,
   object_ids)`: they stay in the model untracked, as one Undo step; it does not save.
   Object history records changes, not ownership.
5. Check measurements for numeric claims and inspect an actual viewport image for visual claims.
   Inspect intermediate results before dependent edits. After failure, inspect partial effects
   and repair locally; direct scripts have no recipe rollback. Leave changes staged unless saving
   was requested or already authorized; use `dirhp-options-and-versions` for that save.

Blocks: copies in a document share one definition when content and name match. Editing a
definition (even a nested one) changes and flags every option using it; a rename is saved. Explode
keeps the pieces in the instance's option. Don't create duplicate definitions to isolate a copy;
no freeze exists.

Apply datum transforms once with `dirhp_datum_point`, `dirhp_datum_vector`, `dirhp_datum_plane`, or
`dirhp_datum_geometry`. Floor elevation, thickness, and height above floor are separate inputs.

Call exposed tools directly; otherwise use `dirhp_invoke(tool_name=..., session_id=..., arguments={...})`.
Batch missing contracts with `dirhp_discover_tools(names=[...])`; reuse loaded contracts.
Fetch only needed guides: `metadata` for attribute examples, `datums` for transforms,
`object-history` for other adoption handles, `graph-sql` for queries.
