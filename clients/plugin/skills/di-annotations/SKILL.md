---
name: di-annotations
description: >-
  Attach dimensions, labels, text dots, leaders, and witness curves to a design so they regenerate
  with it, choosing between viewport dimensions, lifecycle-owned marks, and queryable annotation.
---

# DI annotations and dimensions

Annotation belongs to the thing that regenerates geometry — the recipe or query intent — not to a
one-off gesture. Pick the route by what the annotation IS.

**A reading that belongs to the design.** Author a `dimension` intent with `di_author_intents`
(`kind="dimension"`, `topological`, `definition.mode="query"`). Membership is the ordinary Select
grammar: `from`/`to` (each exactly one node) for a pair, or `selector` for `aggregate` / `series`.
Nothing enters the document. Declare its visible line separately with
`di_author_representations(kind="dimension", payload={a,b,through,text?}, default=false)`, then use
the generic overlay: `di_preview(target="geometry", on=True, where={"kind":"dimension"})`. The
overlay starts OFF.
`member_of` edges are rebuilt on every assemble, so "which dimensions track this wall" is
an ordinary `di_get_edges` query. Stamp only keys you name (`stamp: {"DI:RunLength": "length"}`);
frame-named length, width, and height stay stable while magnitude ranking can swap. A tie refuses
with `axes_ambiguous`; a
query that stops resolving reports `dimension_unresolved` rather than leaving a stale number looking
current.

**A dimension a policy keeps current after every hand edit.** The intent re-measures itself on
every assemble, but its presentation line does not — install a policy (`rerun_on_dirty=true`) that
rewrites it. `graph.pairs@1` builds the keyed pairs to measure from graph logic (self-join,
cross-join, an edge relation, or bbox proximity) or from two selected `DI:id`s, carrying a stable
`handles` map since a dirty pass mints every graph id fresh. `annotate.dimension_records@1` turns
`measure.pair@1` readings plus those handles into one dimension intent and line per pair, placed by
an explicit `normal`/`offset`. See `$di-design-logic` and guide slug `annotations` for the full
chain.

**Geometry a node declares but Rhino does not hold.** A space's boundary, a datum plane, an
element's plan footprint. `di_preview(target="geometry", on=True, where={...},
label_keys=[...], color_by="...")` draws it as a non-document overlay. `where` is the ordinary
selector `di_filter_nodes` takes (find exact spellings with `di_query` over `node_keys`); `label_keys` names the properties whose values become the tag, in
your order, under their exact spellings (DI merges no keys and invents none); `color_by` names one
property whose VALUE picks the colour, so equal values share a hue with no palette to configure.
What draws is the node's default measurable representation plus any presentation dimensions.
Measurement never reads a dimension representation. It draws DECLARED geometry only: Rhino is already drawing ordinary host
objects, so a node declaring nothing comes back in `refusals`, never silently missing. Declare a
shape with `di_author_representations` (`footprint`, `centerline`, `point`, `plane`, `envelope`,
`solid`, `surface`, `wire`, or presentation `dimension`). Overlay starts OFF and adds no objects.

**Labels, leaders, witness curves, construction marks.** Author them in a recipe step and declare
the group `auxiliary`. Lifecycle-owned: deleted and rebuilt on every rerun, spared when hand
edited. Auxiliary carries no class and no name, so it makes no semantic claim — it still becomes an
untyped host node, which is what lets it be found and cleaned up, but it says nothing. Auxiliary
groups may be a bare array, so throwaway marks need no invented keys. Put them on their own layer
with `view.assign_layer@1`. For text labels, call `di_annotation_style()` and use its
`text_height` rather than inventing a model-unit height.

**Annotation that must be queryable.** Declare the group `created` with a `semantics` block; the
objects stay ordinary host objects. Then add a `policy` step giving them `member_of` on a
topological intent. Created-output `relations` target only `$output` paths, never intents, and DI
refuses computed relations such as `represents`.

**Dimensions baked for a drawing.** `annotate.dimension@1` takes the declared
`{a,b,through,text?}` payloads and bakes them as real Rhino dimensions. The overlay and bake
render one declaration. A recipe or policy must rewrite that declaration when its measurement
changes. DI sizes them from the live model units (override the drawing denominator with `scale`).
Baked dimensions are deliberately untagged and never re-read themselves, so the overlay
stays the working view.

Recipes stack, so an annotating recipe installs onto the option that already builds the design.
A recipe whose aliases are taken is refused with `recipe_alias_conflict`; pass `alias_prefix` to
move it aside, which is also how one annotating recipe covers two parts of a model. Removing it
later takes only its own steps and objects.

Save with `di_save_snapshot` once the result is approved. See `$di-measurement` for choosing
conventions and frames, `$di-rhino-authoring` for writing the step, and guide slug `annotations`
via `di_get_guide` for the full contract.

**Done:** every annotation is owned by a step or query that rebuilds it, dimensions report their
members and fidelity, nothing untracked was left in the document, and the approved snapshot is saved.

## Option datums

Transform dimension anchors and planes with the option datum. Annotation text remains reader-facing under reflection; modelled text geometry reflects as model geometry. Use full-matrix geometry helpers for points, vectors, and geometry. See the `datums` guide.
