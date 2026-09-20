---
name: di-classification
description: >-
  Find geometrically congruent objects from one exemplar, review only ambiguous matches, and seal
  the accepted objects as an open-vocabulary topological grouping. Use only when membership
  must be inferred from shape, not to assign a class to known objects.
---

# DI geometric classification

Use only when membership is unknown and shape congruence is relevant. For an exact set known
from creation, user selection, or metadata, write `DI:class` User Text directly. Shape does not
establish an object’s role. Classification writes membership, not class tags.

1. Call `di_classify_by_exemplar(label=...)`. Pass an explicit exemplar when known; otherwise DI
   uses exactly one cached selection, then asks for one Rhino pick. Several selected objects refuse
   with `exemplar_ambiguous` rather than guess.
2. Read the proposal. `accepted` is congruent, `ambiguous` needs judgment, and `rejected` is out.
   Proposal tint is scratch state: it does not dirty the graph or enter history.
3. If amber objects exist, call `di_resolve_classification_exceptions(proposal_id)` once. Select
   exceptions; Enter accepts all. Never call this per object.
4. Call `di_apply_classification(proposal_id, kind=...)`, then `di_save_snapshot()`.
   Use `di_abandon_classification` instead when the proposal is wrong.

`kind` is the designer's wording. Applying creates or updates one topological-capable intent and
explicit `member_of` relations atomically. It does not create a spatial realization or special
seal path. Use `$di-semantic-framework` when the grouping also needs guides. Use
`$di-design-logic` when the grouping should become reusable policy or recipe logic.

Use `band` to tune tolerance and `scope_id` to limit the sweep. `di_get_shape_signature` inspects
one object; `di_match_signature` sweeps without proposing. Congruence ignores translation and
orientation but not scale; mirror images remain distinct unless explicitly allowed.

The proposal changed nothing durable and does not make the graph stale. Pass through
`signature_too_loose`: adjudicating more objects than the classifier accepted defeats
the review model. Tighten tolerance or choose a better exemplar. Manual verdicts survive a
recompute because proposal overrides replay over new buckets.

Members must be `di:<DI:id>` or intent ids, never Rhino GUIDs. The label stays verbatim and
approval writes MEMBERSHIP ONLY — no object tags. DI knows no classes to map a label onto.

**Done:** no ambiguous candidates remain, the grouping is staged as one delta, and the approved
snapshot is saved.
