---
name: di-semantic-framework
description: Author/inspect intents, properties, relations, and representations for spaces, requirements, groups, and spatial claims. Route reactive semantics to policies.
---

# Semantic contracts

Intent = persistent design identity/claim. Properties retain useful values; relations connect
entities; representations declare measurement/display geometry. A surface/class tag is not an intent.
Target values, units, assumptions, and observations need distinct property conventions; these do
not enforce constraints. Simple values need properties, not separate variable entities.

## Author and navigate

| Need | Tool/contract |
|---|---|
| Existing intent | `di_list_intents`, scoped graph queries |
| Related declarations | `di_author_intents`: intents, capabilities, edges, deletes, family replacement |
| Element geometry | `di_author_representations`: explicit payload or source |
| Related context | `di_walk_nodes`; complex joins/state questions: `di_query` |
| Group for action | `di_resolve_scope`: exact model selectors |

Reuse explicit IDs or exact `(kind,name)`; ambiguous endpoints refuse. Related declarations can
share an atomic batch. Direct authoring stages without requiring a recipe/policy or saving.

Example: kitchen intent owns target width and footprint representation; physical partitions are
separate objects connected by supported boundary relations. Changing the footprint can preserve
room identity and requirements. Existing geometry or a proposed intent can be the starting point.

## Capabilities and representations

`kind`/`DI:class` are open vocabulary; no inferred behavior.

- `topological`: membership, including query scopes.
- `reference`: point, centerline, plane, oriented datum.
- `spatial`: footprint, envelope, surface, solid, wire.

Representation: `element` + `kind` + `fidelity` + exactly one of `payload`/`source`.
Element = intent ID or supported host identity. At most one `default:true` per element; bare-ID
measurement resolves to it. Declarations do not generate/extrude geometry. Footprints remain planar
with `z_band`; volume requires suitable closed geometry. `dimension` is presentation-only,
never default, never a measurement input.

## Relations and automation

Membership is nonexclusive. A second purpose can be a property without replacing class.
Query scopes use `definition.selector={"where": {...}}`; matching is exact, without aliases.
Direct intent edges: `member_of`, `contained_in`, `bounded_by`. Other authoring routes have their
own contracts; do not assume they accept the same relations.

Relations do not propagate edits. For requested reusable/reactive behavior, `di-design-logic`
distinguishes run/register/install. `starter.enclosed_space@1` derives a declared space's footprint
from boundaries; it does not create the room identity. Requirements alone need no installed policy.
Missing edges: inspect endpoints, contract, and result before claiming a cause. Report failed
semantic refreshes; preserve requested save state.

Load when needed:
- `semantic-framework`: executable room example, endpoint contracts, policies/starters/reconciliation.
- `measuring-operations`: representation payloads and measurement basis.
- `datums`: oriented reference placement.
