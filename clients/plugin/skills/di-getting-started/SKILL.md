---
name: di-getting-started
description: Understand DI's design records, state, and navigation. Read when starting a DI task; route specialist operations as needed.
---

# DI system model

DI is shared design memory connected to Rhino. Retain useful requirements and relations; retrieve
them in the intended design state instead of reconstructing context from earlier scripts.

- **Intent:** independently identified concept/claim, e.g. kitchen.
- **Property:** entity value/assertion, e.g. target width, unit, purpose, assumption.
- **Relation:** supported connection, e.g. membership, containment, boundary.
- **Representation:** geometry declared to display/measure an element.
- **Rhino object:** document geometry; ownership requires adoption or declared recipe output.
- **Option / staging / snapshot:** design context / editable state / exact saved state.
- **Cache:** bounded session record of captured objects; neither ownership nor saved memory.

Kitchen intent + target-width property + footprint representation + related partitions are
separate records. The kitchen identity can survive a changed footprint. A surface named Kitchen
or tagged `IfcSpace` does not create that intent. Capabilities enable behavior; names do not.
Relations record connections; recipes/policies provide explicit automation, not automatic enforcement.

## Decision principles

- Interpret the brief as requirements, entities, values, relations, uncertainty, and possible actions.
  Keep requirements, assumptions, and observations distinct; target width is not measured width.
- Reuse relevant records. Simple shared values belong on entities; not every variable needs a node.
  Document units/datums establish geometric meaning. Retain information useful to later actions/chats.
- Clarify material uncertainty unresolved by brief/model; state reversible concept assumptions.
  A study alone does not authorize model writes.
- Split multi-part designs at dependencies or uncertain results. Avoid one Python script for a whole
  floor plan. Read results before dependent work; batch related records or predictable repetitions.
  A local edit may need one call. Inspect partial effects after failure; repair locally, preserve valid work.

## State and navigation

Live: `di_connect` connects and orients; reuse `session_id` or auto-activate the sole present document.
Absent/ambiguous document: ask for its `DI_Connect` code; use `di_connect(code=...)`.
Reuse returned orientation; `orient_me` refreshes it when needed.

Saved: `di_list_workspaces`, `di_get_workspace`, `di_find_options`; `scope.snapshot_id` selects exact
saved evidence without live Rhino. Default graph/measurement reads use live focus, unsaved edits,
and composed references. `stage_unavailable` never means fallback to an older snapshot.
Explicit `scope={"option_id": ..., "state":"staging"}` excludes composed references.

| Question | Tool |
|---|---|
| Unknown context or vocabulary | `di_inspect_graph`: scoped counts/keys/containment |
| Match entities | `di_filter_nodes`: exact keys/values; bounded detail |
| Known identity | `di_get_node`; skip redundant discovery |
| Related context | `di_walk_nodes`: locality/containment; check relation contracts |
| Authored meaning/geometry | `di_list_intents` / `di_list_representations` |
| Complex relations/state comparison | `di_query`; guide `graph-sql` |

Matching is exact and case-sensitive; `name` differs from `DI:name`. Retrieve relevant records,
not the whole graph. Persist DI identities, not raw Rhino GUIDs.

## Load conditions

Read only needed skills/references; a route is not a requirement to load both. Fetch known guides
with `di_get_guide`; list unknown topics with `di_list_guides`. Reuse loaded guidance.
Discover missing contracts with `di_discover_tools`; call through `di_invoke` when required.

| Need | Route |
|---|---|
| Intents/relations/representations | `$di-semantic-framework` → `semantic-framework` |
| Geometry/adoption | `$di-rhino-authoring` → `metadata`, `object-cache` |
| Architectural judgement | Optional installed `di-architecture`; `design-context` |
| Automation choice/snippets | `$di-design-logic` → `design-logic`; `recipe-authoring` for snippets |
| Measurement/labels | `$di-measurement`, `$di-annotations` → `measuring-operations`, `annotations` |
| Components/state/placement | `$di-options-and-snapshots` → `options-and-snapshots`, `datums` |
| Infer unknown membership from shape | `$di-classification`; known roles use properties |
| Existing-object edit | `di_get_playbook("modify")` |

Evidence: measurements support dimensions; inspect actual images for visual claims, not capture
receipts. Save only when requested/already authorized; report `recipe_proof` and `blockers`.
Recipe success is not required to save. Preserve requested unsaved state and read-only ownership;
do not save just to inspect current work.
