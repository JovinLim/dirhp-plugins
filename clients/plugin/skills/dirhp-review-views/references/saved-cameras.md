# Save and synchronize cameras

Use `dirhp_save_option_view(view_name)` and `dirhp_list_option_views`. Cameras are design-relative.
`sync_rhino_view=True` also writes a Rhino named view. If `rhino_view` is `differs` or `missing`,
ask which camera to retain before `dirhp_override_option_view(view, keep="rhino"|"dirhp")`.
Saving over a review view's name replaces it with a fixed camera that no longer follows the level.
Plan review cuts are 1.2 m above the level; level/default framing update on each capture/Show
through `view_inputs`. Constants change only when the review view is recreated.
