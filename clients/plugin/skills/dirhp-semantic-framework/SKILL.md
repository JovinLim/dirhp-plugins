---
name: dirhp-semantic-framework
description: Create or edit dirhp intents, properties, relations, and representations. Use to record design meaning and requirements; use dirhp-graph-query to read existing records.
---

# Author design meaning

1. Select the target option and call `orient_me` for live authoring.
   Reuse the session/orientation; ask for the file name or dirhp panel session label only if absent or ambiguous.
   Analysis alone does not authorize writes. Keep requirements, assumptions, and observations distinct.
2. Find existing intents with `dirhp_query` (`nodes.semantic_role = 'intent'`). Reuse explicit IDs
   or exact `(kind,name)`; resolve ambiguous endpoints before writing. Composed members are read-only.
3. Choose the record type: intent = identified concept/claim; property = value on an entity;
   relation = connection; representation = declared display/measurement geometry.
   A kitchen, its target width, and its footprint are separate records. Keep simple values as
   properties, with explicit units and datums. Names and `dirhp:class` do not create intents or capabilities.
4. Call `dirhp_author_intents` for intents and edges; related declarations can share an atomic batch.
   Capabilities: `topological` for membership/scopes, `reference` for datums, `spatial` for extents.
   Edge directions: `member_of` member → group; `contained_in` contained → container;
   `bounded_by` space → boundary. Do not author derived relations.
5. Call `dirhp_author_representations` with `element`, `kind`, `fidelity`, and exactly one of
   `payload`/`source`. At most one representation per element has `default:true`; measurement
   resolves to it. Declarations do not generate geometry; a footprint with `z_band` stays planar.
6. Query the changed records and check representation/measurement results as needed.
   Relations do not propagate edits. For declarations that must update on recipe reruns,
   use `dirhp-recipes` and preserve explicit IDs and step ownership. Report failed semantic refreshes.
   Leave changes staged unless saving was requested or already authorized.

Call exposed tools directly; otherwise use `dirhp_invoke(tool_name=..., session_id=..., arguments={...})`.
Batch missing contracts with `dirhp_discover_tools(names=[...])`; reuse loaded contracts.
Fetch guide `semantic-framework` only for payloads, endpoints, policy bindings, and examples.
