---
name: dirhp-review-views
description: Create plan or section review views, capture images, and isolate dirhp designs for presentation. Use for cameras and visibility, not dimensions or version history.
---

# Review a design visually

1. Select the option/version and requested view. Reuse the live session or call `orient_me`;
   request the intended document's code only if absent or ambiguous. List existing option views.
2. Choose the output:
   - Image: `dirhp_capture_viewport(view=<saved view>, option_id=...)`; capture restores the viewport.
     For current option framing use `frame="active_option"`.
   - Plan/section: `dirhp_create_review_view(view_name, kind="plan"|"section", level=...|section_line=...)`,
     then capture it. The level and default framing follow current graph inputs; do not hand-set a camera.
   - Save/synchronize a camera: read [saved cameras](references/saved-cameras.md).
   - Move the designer's viewport only when requested: `dirhp_show_option_view`.
3. If presentation requires isolation, use `dirhp_isolate_version_view(loaded_version_id=...)`; it also focuses but does not
   move the camera or save. A parent shows with its children; add `also_loaded_version_ids` to show several copies
   (a ×N group). Clear with `dirhp_clear_isolation` after presentation unless asked to retain it.
   Never script raw Rhino Hide/Show. Focus alone hides nothing.
4. Inspect the actual image, not only the capture receipt. Report visual limits; use measurements
   for numeric claims. A review image does not require or authorize a version save.

Discover missing contracts in a batch; gateway calls put `session_id` on `dirhp_invoke`.
Fetch `option-views` only for view contracts not covered here; use `dirhp-annotations` for dimensions.
