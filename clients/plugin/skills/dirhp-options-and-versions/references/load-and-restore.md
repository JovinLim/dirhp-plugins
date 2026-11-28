# Load or restore a version

`dirhp_load_version` modes:
- `editable` (default): restore exact saved geometry inside this workspace as a copy of the
  version's option, editable for editors (viewers' copies are not), without changing focus. Its
  nested options are editable copies too, pinned ones still pinned. Every
  call adds another copy, at the version's original location (where it sat when saved) or at
  `placement`; it never refreshes an existing one. A version saved before dirhp recorded its
  location loads at the document origin with `original_location: "missing"`: tell the designer
  `original_location_message`. The reply's `placement` says where it went: `original`,
  `document_origin`, `picked` (your `placement`) or `occurrence` (`occurrence={parent_version_id, path}`
  opens one placement inside a saved parent alone, where it sits in the project; list them with
  `dirhp_list_version_occurrences`). A save from a copy placed elsewhere keeps the original. Moving a
  copy at its original location is an edit: saving it records the new original with unchanged
  geometry; moving it back clears the edit. A new base point (`dirhp_set_option_base_point`)
  moves the original on any copy, and Update keeps following copies where they are across it
  (`base_point_differs` refuses a combine whose sides moved it differently). Loading restores stored geometry, including
  managed outputs; it does not run a recipe.
- `option_history`: cross-workspace only; copies the version and ancestors with remapped parents
  into a new editable option. Same-workspace refuses `use_view_and_focus`: load it without a mode. Find the source with `dirhp_get_workspace`, import, then edit.
  A name clash refuses `option_name_exists`.

`dirhp_unload_version(loaded_version_id)` removes one independent loaded copy and its nested
placements and nested-option copies from the document. It leaves saved versions, options and history intact. Unsaved work
is refused unless the designer explicitly authorizes `discard_changes=true` (direct tool, or
`dirhp_invoke_destructive`; `dirhp_invoke` refuses it). A nested copy must
be removed through its owning composition. Pass `remove_from_parent=true` to also remove a nested
option from its parent in the same step; Rhino Undo restores both. Unfocus only clears the authoring target; isolation
changes visibility; update replaces a copy's content; permanent deletion removes saved history.

If the reply is lost, the operation stays pending under its operation id. Reconnecting does not
delete geometry by itself. A document that still matches the recorded set cancels the pending
unload. A document that no longer contains any of that set drops tracking and keeps the saved
snapshot. Mixed, edited, legacy, or unverified contents stay pending until
`dirhp_review_unload_recovery(operation_id)` and an explicit
`dirhp_resolve_unload_recovery` choice: `keep_current` retains the geometry and its drafts;
`complete_unload` removes only the members in that review and records a new undoable operation.
Pass the review token from the same review. `review_changed` means inspect again; do not retry
the delete. Another copy of the same file is not reconciled while the originating Rhino instance
is still connected. Save, focus, and unload of an affected copy return `unload_recovery_pending`
until that operation is resolved.

`dirhp_refresh_loaded_versions` is "Update": it moves loaded copies and following instances, placed ones inside placed designs too, to the head
of their own option (a fork never takes its parent's head); optional `option_id` limits it to one option. Pinned copies are left alone (a nested child: see below).
A clean copy moves. A copy with unsaved edits is skipped as `dirty` (unless its live content already equals the target).
Ask the designer which mode, per copy, and pass it in `modes`: `replace` drops the edits; `combine` keeps them on top of the latest
save. Never pick a mode yourself; `force=true` is `replace` for every such copy. A `replace` mode, `force` or `resolutions`
makes the call risky: send it through `dirhp_invoke_destructive` with the same arguments.
Combine is two calls. The first returns `combine_review` and applies nothing: per copy `items` (kind `both_changed`, shown as
"Both changed", or `invalid_merge`), `refused`, `counts` and `dropped`, plus one `review_token`. Show the items to the designer.
Then call again with the same `modes`, that `review_token` and `resolutions` = `{loaded_version_id: {item_id: "mine" | "theirs"}}`
for every item, exactly as the designer chose. Items of kind `overlap` (both sides' changed objects touching or within the
workspace's `overlap_tolerance`) are warnings: show them; left out they keep both, or the designer keeps one side
(`"mine"` / `"theirs"` in `resolutions`). Never resolve an item or pick an overlap side yourself. `review_stale`: review again.
`recipe_changed_both_sides`: combine cannot keep both; offer `replace` or saving first. `units_differ`: the latest save is in other units than the copy (saved before copies were converted); nothing changed. A combined copy stays unsaved (`save_required`)
on top of the latest save. The whole Update is one Rhino undo step (Ctrl+Z undoes every copy it moved), like the panel's Update. Before a combine, tell the designer its limits
(whole-object conflicts, the overlap check's cap, possible duplicates after explode/join): `dirhp_get_guide(slug="options-and-versions", sections=["Combine: known limits"])`.
Result has `pulled`, `skipped` (left alone, with reason) and `failed`. All or nothing: an error or stop puts every copy back (`state: rolled_back`, others `failed` `rolled_back`; rerun moves all). `rollback_unconfirmed`: designer checks the model first (one Ctrl+Z puts it back); do not rerun.
A nested child (inside a parent copy) goes to what its parent's version saved for it (`parent_saved`), not to its option's latest.
When any child could take a newer save, or one with unsaved edits would move without a mode, the call returns
`status: "choices_required"` and applies nothing: show `child_choices`
(per child: loaded, `parent_saved_version_id` and `latest_version_id` with their `_message` notes, `pinned`, `edits`, `choosable`/`reason`; name each by note and short id) to the
designer and ask which take their latest. Call again with the same arguments, that `choice_token` and `child_targets` =
`{loaded_version_id: "latest" | "parent_saved" | "keep"}`; unanswered is `parent_saved`; `keep` leaves a child with unsaved edits
as it is (one with `choice_required: false` takes only `keep`); a child with edits that moves needs its `modes` entry. Never pick `latest` yourself unless the designer asked (e.g. "update everything to latest"). A pinned child set to
`latest` is re-pinned there. A parent whose children now draw other versions than it saved is not edited:
the result's `needs_save` lists it; tell the designer to save that parent to record them. The panel shows the same question and the first answer wins:
`already_answered` means it ran (often from the panel): report its `operation_id` and `results`, do not call again.
`choice_cancelled`: the designer said stop; change nothing. `review_stale`: ask again with the fresh `choices`.

`dirhp_update_loaded_version` replaces one occurrence in place along the same linear option history,
and repairs one whose geometry is absent. Its Rhino object ids change.
It creates no second copy. A placement or viewer's copy stays clean; a pinned hand placement moves and stays pinned (to the target); a copy at its option's latest save
retains it and stages the restored content for a later child version; a copy behind it moves to
the target and stays clean. The copy need not be focused. Recipe-owned occurrences must be
repointed through their step and rerun. `force=true` overwrites drafts, not lineage or
document checks. Use it only when that overwrite is authorized, through `dirhp_invoke_destructive`; so are
`child_targets` of `parent_saved` or `latest`, since a moved child loses its unsaved edits.
Copies inside it follow too: when one differs from what the target version saved, the call replaces nothing and
returns `choices_required` (`child_choices`, `choice_token`); the panel shows the same question, first answer wins.
Answer with `child_targets`: `parent_saved` (as the target saved it; drops that child's edits), `latest`, or `keep`.
Unanswered: `parent_saved` for a clean child, `keep` for one with edits. The reply lists `children`
(moved / kept / failed) and `needs_save`; the parent and its children are one Rhino undo step.

`hash_diagnostics` on a receipt: the load succeeded but equivalence is unproven. Guide `freshness`.

Restore uses saved bytes. Regenerate only when recomputation is intended; it can produce a different
result with current inputs. Unsaved work has no saved bytes. Never substitute regeneration silently.
