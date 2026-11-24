# Overlays

`dirhp_preview(target="geometry", on=True, where={...}, label_keys=[...], color_by="...")` displays
declared representations. It adds no document objects and starts off.

- `where`, `label_keys`, and `color_by` are dirhp parameters. Selector keys and property names must
  match the model exactly. `label_keys` selects label values in order; `color_by` groups colours
  by property value. Do not invent missing properties.
- Preview draws default measurable representations and presentation dimensions. It does not
  duplicate ordinary Rhino geometry. Missing declarations return `refusals`; report them.
- Declare missing geometry with `dirhp_author_representations` only when authoring is requested.
