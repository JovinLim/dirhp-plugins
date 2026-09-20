---
name: di-measurement
description: >-
  Measure live staging or saved DI graph objects, capability-based references, groups, clearances, extents,
  spacing, and outlines while checking frames, conventions, fidelity, and refusal codes.
---

# DI measurement

Choose the tool: `di_get_extents` for how big one thing (or one set) is, `di_measure` for how far
apart two things are (scalar `a`+`b` for a pair, a list on either side for set minimum),
`di_measure_series` for spacing regularity (`node_ids` or `scope_id`). Mixing operands across
tools is a schema error.

Point, centerline, and plane operands come from the `reference` capability, not from a particular
kind. A concept named `Level` is measurable only because a `plane` representation was declared for
it. Collections with no declared geometry have no extents.

DI produces no representations. An element with a representation marked `default` resolves to it; one
with none resolves to ITSELF and measures its own stored geometry, reporting `representation:
"self"` with `extent_class` naming the structure found. Nothing is extruded from labels, a
`z_band`, or bbox height. A declared footprint is a planar surface with zero vertical span; only an
explicit closed mesh supports volume, containment, or overlap depth. A closed ring owns no
centerline unless one is declared.

DI reads no width key it named itself. Pass `width_keys` (for example `["DI:Thickness"]`) to buffer
an undeclared open curve into a planar footprint for that read only; it never creates a stored
sibling or a volume. `height_keys` is unsupported. Dimensions do not take `width_keys` — declare a
centerline representation with thickness instead.

Declare with `di_author_representations(representations=[{element, kind, payload|source, fidelity,
default}])` — measurable `kind` values are `solid`, `surface`, `footprint`, `centerline`, `wire`,
`point`, `envelope`, and `plane`; `source` names a node whose geometry is copied, which is how a
recipe says the curve it drew a wall from is that wall's centerline. `dimension` is a presentation
kind with `{a,b,through,text?}`. It cannot be default and measurement always refuses it.

For self measurement, read `dimensions` with returned `frame`; `major/median/minor` do not identify
height. Unsupported frames downgrade and report `frame_unavailable`. `bbox` is the axis-aligned
box of the measured payload, not the host node's world box.

For pairs, inspect `relation`, `readings`, `witness`, `convention`, and
`convention_unavailable`. Conventions are `nearest_faces`, `centerline`, `clear`, `overall`, and
`centroid`; `nearest_faces` is the general fallback. Use `di_list_representations` to see which
payloads make the others available — an undeclared element returns an empty list, which is the
honest answer. Negative clearance means overlap or containment, not zero.

`witness` is `{a, b, direction, length}` — the move instruction: `direction x (length - target)`
is the translation. That is why you measure BEFORE writing geometry with a numeric argument. DI
supplies no target distance, no frame, and no "left" — ask rather than assume, and name the `frame`
(`world`/`plan`/`object`/`centerline`/`along`) explicitly when direction matters.

For sets, minimum comparison uses nearest faces and rejects other choices with
`unsupported_set_convention`. For a series, one convention applies across every pair; grids refuse
ambiguous ordering unless `along` names an axis. A two-member scope is not a pair.

For recipe and policy steps, use batched `measure.node_extents@1`, `measure.extents@1`,
`measure.pair@1`, or `measure.series@1`. Caller keys return unchanged beside readings and
refusals. These pure graph functions have no kernel accuracy and no `width_keys`.

Use `di_get_wire` for projected outlines and holes. Check `fidelity` and `reliable`; request kernel
accuracy when a tolerance decision depends on exact authored surfaces. Kernel mode refuses rather
than presenting an approximation as exact.

To SHOW a reading in Rhino rather than return it — a viewport dimension, a tracked one that
follows the geometry, or a baked one for a drawing — use `$di-annotations`.

Fetch guide `measuring-operations` with `di_get_guide` for the full refusal and accuracy matrix.

**Done:** mode, frame/convention, relation, units, reliability, and fidelity are reported with the
number.

## Option datums

A footprint can be a tilted planar region with `plane_transform` and holes. Intrinsic area is invariant under rigid placement; projected area is not. Derive outlines with `di_get_wire`, optionally supplying `projection_datum`. A direction parallel to that plane refuses. See the `datums` guide for accuracy limits.
