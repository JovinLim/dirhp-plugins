# Dimensions

A reading and its presentation are separate declarations:

1. Use `dirhp_author_intents` for a dimension query: `kind="dimension"`, capability `topological`,
   `definition.mode="query"`. Pair readings use `from`/`to` selectors, each resolving one node;
   aggregate/series readings use `selector`. Fetch `annotations` for the payload contract.
2. Use `dirhp_author_representations` for its `kind="dimension"` presentation with
   `payload={a,b,through,text?}` and `default=false`.
3. Show it with `dirhp_preview(target="geometry", on=True, where={"kind":"dimension"})`.

`dimension`, the query fields, and representation payload fields are dirhp contracts. Coordinates,
text, selectors, and element IDs come from the design/request. `through` places the line;
measurement does not choose its offset or plane.

- `stamp: {"dirhp:RunLength":"length"}` is an example: `stamp` and `length` are dirhp contracts;
  `dirhp:RunLength` is a chosen model property, not a required field.
- Prefer frame-named readings to magnitude rankings. Report `axes_ambiguous` or
  `dimension_unresolved`; do not present unresolved readings as current.
- Query membership and readings update when the graph assembles. The presentation line does not
  follow automatically. Refresh its declaration after geometry changes before presenting it.
- A dimension representation is presentation-only; measurement never consumes it.
