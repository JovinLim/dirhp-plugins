---
name: di-rhino-authoring
description: Create/edit Rhino geometry, commit observable attributes, and adopt objects into DI. Covers direct scripts, selections, and geometry-producing recipes.
---

# Rhino authoring contracts

Geometry creation does not establish intent or option ownership. Space semantics:
`di-semantic-framework`; representation/context judgement: guide `design-context` or optional
`di-architecture`. Use relevant DI properties, identities, and relations as inputs.

## Action boundaries

- Multi-part design: separate calls where results inform later work; no whole-floor-plan script.
- Known repeated inputs: a loop/batch is appropriate. Unfamiliar opening operation: test a small
  relevant part before expanding. No fixed object count or screenshot cadence.
- Return only useful identities/operation handles, counts, warnings, measurements.
- Failure: inspect partial effects, repair affected work, retain valid results. Direct scripts
  do not inherit recipe rollback. Use actual images for visual review and measurements for dimensions.

## Direct edits

- Fixed geometry: `di_run_rhino_python`. Known roles: `DI:class`; labels: `DI:name`.
  Reuse vocabulary and exact identities; classification is unnecessary for known roles.
- Existing attributes: duplicate `Attributes`, edit the duplicate, commit `ModifyAttributes`.
  In-place `rs.SetUserText` / `SetUserString` lacks the event DI needs. Example: guide `metadata`.
- Capture creations with `cache=True`; check `cache.complete`, not just execution status.
- Entire retained batch intended: `di_adopt_cached_objects(operation_id=...)`; no search needed.
  Subset: `di_find_cached_objects`. User identifies objects: `di_cache_rhino_selection`.
  Adoption/incomplete capture contract: guide `object-cache`.
- Adoption stages without saving. Do not set lifecycle tags or modify read-only composed members.
  Recipe outputs declared `created` already have ownership; skip adoption. Persist DI identities,
  not raw Rhino GUIDs.

## Execution choice

| Need | Mechanism |
|---|---|
| Fixed geometry/local edit | Direct Python; adopt unowned results |
| Reusable generation on request | Existing recipe, or register + run; installation is separate |
| Regeneration with option | Add/update snippet or install recipe |
| Semantic records only | Intent/representation tools; no Rhino snippet |

Saving, repetition, and loops do not require recipes. Use `di-design-logic` for timing; search
catalog logic before writing more. Snippet authors load `recipe-authoring` for context, outputs,
compilation, and recovery. Reactive semantic-only behavior uses policies.

Placement: respect actual units and datum. `di_datum_point`, `di_datum_vector`, `di_datum_plane`,
`di_datum_geometry` apply the full transform once; guide `datums` covers execution contexts.
Floor top elevation, thickness, and height above floor are distinct inputs.

Verify the intended state; save when authorized. Report `recipe_proof`/`blockers`; recipe success
is not required to save. Preserve requested unsaved edits.
