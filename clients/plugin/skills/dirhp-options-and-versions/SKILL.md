---
name: dirhp-options-and-versions
description: Create and focus dirhp options; save, load, tag, restore, and compare design states. Use for stored state and history.
---

# Manage options and versions

Model: every loaded copy is editable for workspace editors (viewers' copies are not); focus selects
the default authoring target. Unsaved edits stay until Update or a save. Any document can edit an option at any version. First save wins.

1. Find the workspace/option with `dirhp_get_workspace` and `dirhp_find_options`; reuse existing records.
   Saved-only browsing needs no Rhino. For live changes reuse the session or call `orient_me`;
   ask for the file name or dirhp panel session label only if absent or ambiguous.
2. Choose the requested action:
   - Continue/edit a saved version: `dirhp_load_version(version_id)` adds a copy without changing
     focus. Use `dirhp_focus_version(loaded_version_id)` to select the default authoring target.
     `dirhp_unfocus` clears focus and preserves all copies and edits. Focus hides nothing.
   - Requested alternative: `dirhp_create_option(name, from_version_id=...)` forks any workspace
     version, a teammate's included; preserve the source.
   - Unrelated design: `dirhp_create_option(name)` (empty). To seed from a selection, then
     `dirhp_adopt_objects` and `dirhp_save_version`. Names are unique per workspace (per parent when nested) -> `option_name_exists`.
     Keep drafts in the current option; do not create alternatives unless asked.
   - Load/restore, or Update loaded copies to the latest save: read [load and restore](references/load-and-restore.md).
   - Unload one independent copy: use `dirhp_unload_version(loaded_version_id)` with its exact copy ID. It refuses unsaved work unless discard was explicitly authorized. Saved history remains.
   - Interrupted unload: `dirhp_list_unload_recoveries` then `dirhp_review_unload_recovery(operation_id)`. Choose `keep_current` or `complete_unload` only after the designer authorizes it, and pass that review's token to `dirhp_resolve_unload_recovery`. Neither choice is automatic. Keep preserves current geometry; complete removes only the reviewed current members and can be undone in Rhino. A changed document returns `review_changed`; review again. Do not repeat a delete after a lost reply — retry the same operation id.
   - Compare state/history: `dirhp_compare_design_states`; identify exact versions or staging.
   - List history / see who has it loaded: `dirhp_list_versions` (option or design, tagged or not); `dirhp_list_loaded_versions` (where, unsaved?, connected?; not what changed). Both session-free, read-only.
   - Generator behind a version's objects: `dirhp_list_version_generators`, then `dirhp_open_version_generator(version_id, artifact_sha)` (opens in Grasshopper with the solver locked; the person confirms in Rhino).
   - Grasshopper definition (its own versioned part, not an option): `dirhp_read_grasshopper_canvas`, `dirhp_save_definition`, `dirhp_list_definition_versions`, `dirhp_compare_definition_versions` (category `grasshopper`); guide `grasshopper-definitions`. `dirhp_save_definition` takes a required `message` too; a first save also needs `new_definition_name`, the name the designer types (never the file name; `name_required`). Definition names are unique per workspace and copy names across the workspace's definitions, ignoring case (`definition_name_exists`, `option_name_exists`); rename with `dirhp_rename_definition` / `dirhp_rename_definition_option` only when asked. `new_option_name` saves the document as a new copy: an ordinary save (`dirhp_invoke`), only when asked; only `force` is destructive. `definition_advanced`: the designer chooses.
   - Merge one option into another of the same design: `dirhp_review_option_merge(source_option_id, target_option_id)`, show the items with both options' names, then `dirhp_apply_option_merge` with the designer's `target`/`source` choices through `dirhp_invoke_destructive`. It needs a copy of the target loaded, at its latest save, with no unsaved edits; the result is unsaved in that copy (one Ctrl+Z) until `dirhp_save_version`, only when asked. Guide `options-and-versions`, "Merge one option into another".
   - Save/tag: read [save and tags](references/save-and-tags.md), then `dirhp_save_version`
     or `dirhp_tag_version` only when requested or already authorized. `option_advanced`: the
     designer chooses (never pick `combine` resolutions, keep one side of an overlap, `force` or `replace` yourself).
   - Every save carries a note: pass `message` to `dirhp_save_version` saying what changed and why; a blank one is refused (`message_required`).
   - Save exactly a set of objects as a copy's next version ("save only these"): `dirhp_save_only_these(object_ids, message, loaded_version_id?)`. Objects in no option or another copy are adopted in, the copy's other objects leave it and stay in the model untracked, and only that copy is saved; one Ctrl+Z puts every object back. It saves, so only when asked; it drops no geometry and no unsaved edit. `nothing_leaves` means use `dirhp_save_version`. Guide `object-history`, "Save only these".
   - Rename a version (its name is its note): `dirhp_rename_version(version_id, message)`, only when asked; the note cannot be blank; no new version, old note kept in the activity log.
   - Naming versions: tell the designer a version's note and short id (first 8 characters of `version_id`), e.g. “Moved the canopy 2 m north” (a3f9c2e1), never "v13"; pass the full `version_id` to tools. A short id from the designer: `dirhp_list_versions(workspace, version_id_prefix=...)` returns every match; ask when there are two.
   - Risky calls (`dirhp_delete_option`, `dirhp_delete_version`; `force`, `discard_changes`, a `replace` mode,
     `resolutions`, `child_targets` that move a child) go only through
     `dirhp_invoke_destructive(tool_name, session_id, arguments)`, and only on the designer's say-so: their app asks
     them to approve it. Other tools refuse them with `destructive_tool_required` and the exact `call` to make.
   - Save an edited placed design (e.g. a chair placed in a room): `dirhp_save_version(loaded_version_id=<the placement>)` saves it as a new version of the chair's option; rooms holding it then show as edited. A save also saves every edited copy inside the saved one, deepest first, never its parents; a copy placed inside a placed design saves alone the same way.
3. Check the returned state and diagnostics. Authoring and recipe runs stage changes;
   completion alone does not authorize saving. Preserve requested unsaved work.
4. Report the option/version IDs, saved or staged state, and any `recipe_proof`/`blockers`.

Use `dirhp-design-composition` for nested components and occurrences; `dirhp-review-views` for images
and isolation; `dirhp-recipes` for regeneration. Call exposed tools directly; otherwise put
`session_id` on `dirhp_invoke`. Batch missing contracts with `dirhp_discover_tools`.
Fetch `options-and-versions` only for missing import/history contracts, and `freshness`
for state refusals. Do not load all guides before a simple action.
