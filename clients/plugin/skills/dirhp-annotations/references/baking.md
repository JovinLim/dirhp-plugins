# Baking

`annotate.dimension@1` creates Rhino dimensions from the same `{a,b,through,text?}` declarations.
Refresh the reading and presentation before baking. Model units set sizing; `scale` is the drawing
scale denominator.

Baked dimensions are not dirhp-tagged or graph-owned and do not update themselves. Rerun the utility
with `replace=True` to replace its baked output. These differ from recipe-managed auxiliary marks.

Apply option datum transforms to anchors and planes; guide `datums`. Annotation text stays
reader-facing under reflection; modelled text geometry reflects with the model.
