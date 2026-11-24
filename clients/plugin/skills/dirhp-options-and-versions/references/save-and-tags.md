# Save receipts and version tags

- Saving never runs the recipe; an invalid chain or missing run proof does not block it. Report `recipe_proof` (`verified`, `unverified`, `invalid`, `none`, `unknown`) and `blockers`.
  `none` means the option has no recipe. `unknown` with `proof_target_unresolved` means dirhp could
  not judge it; it is not "no recipe".
- `version_not_ready`: a staged datum is not current. Retry `dirhp_update_option_datums`, then save.
- A save always lands on top of the option's latest save: a copy behind it is combined onto it
  (edits kept) and saved in the same call; the web Commit button and Save all do the same for every copy they save, and skip only the copies that need a decision (and the copies holding them): the reply's `refused` lists them with the choices below, `saved` the rest. `option_advanced` comes only when the combine needs the
  designer. Show `head_saved_by`/`head_saved_at`, the `items` (`both_changed`, `invalid_merge`, `overlap`) and
  `dropped`, then run only the choice they pick:
  - `combine`: `dirhp_save_version(review_token=..., resolutions={item_id: "mine"|"theirs"})` through `dirhp_invoke_destructive` (with `resolutions` it is a risky call), one resolution per item, as the designer decided; each `overlap` item is a warning: left out it keeps both, or `"mine"`/`"theirs"` keeps one side, only as the designer says. `review_stale`: show the new review.
  - `force`: the choice's call, `dirhp_invoke_destructive(tool_name="dirhp_save_version", arguments={..., "force": true})`; seals the copy exactly as it is on top of the latest save. Report `dropped_changes`: the latest save's changes the new version lacks (still in history).
  - `replace` (only when combine is refused): the choice's call, `dirhp_refresh_loaded_versions(modes={id: "replace"})` through `dirhp_invoke_destructive`; drops unsaved edits.
  Never force, resolve, pick an overlap side or replace without the designer's explicit instruction. Saving as a new option (`new_option_name`) stays available on request. Known limits: guide `options-and-versions`, "Combine: known limits".
- `message` is required: one note for the whole save, what changed and why (the designer's words, or what you changed; never "update"). It goes on every version the save makes, the copies inside included, and names them. A blank note is refused `message_required`. To give copies different notes, save them one at a time, inside first.
- A save covers the copy and every edited copy inside it, deepest first; the receipt's `saved` lists each (`loaded_version_id`, `version_id`, `option_id`, `created`). It never saves the copy's parents: a nested copy saved alone leaves its holder showing Edits. A refusal partway carries `saved` (those stay saved); report both, fix the refused copy, then save again.
- Stuck save (`invalid_version`, `missing_geometry`, `geometry_upload_failed`, `invalid_semantic_manifest`, `invalid_layer_manifest`): ask the designer, then `dirhp_update_loaded_version(loaded_version_id, version_id=<its saved version>, force=true)` through `dirhp_invoke_destructive`; it drops unsaved changes, brings back saved hide/show and puts each member back on its saved layer, and is one Rhino undo step (Ctrl+Z brings them back).
- `geometry_not_savable`: do not revert. Tell the designer which object Rhino cannot store (the message names it); removing or converting it lets the save through.
- Dropped connection: `dirhp_get_recipe_run_status`. An unknown outcome blocks nothing; replace the loaded copy with `dirhp_update_loaded_version` if needed.

Version tags are designer-named labels on versions (not `dirhp:*` object tags). Tag only when asked; `move=true` reassigns a name within an option. To find one, pass the designer's words as `tag_hint`, check for an existing tag first, confirm when the best hit is not `exact`, then use its `version_id`.
