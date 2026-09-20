---
name: di-architecture
description: Interpret architectural briefs through DI requirements, spaces, relations, and representations. Route systems, evaluation, and alternatives as needed.
---

# Architectural judgement

Base `di-getting-started` supplies DI concepts/state/navigation. This skill supplies domain
judgement. Without DI evidence, continue only independent work; do not invent measurements.

## Interpret and represent

- Extract decision, scope, spaces, objects, dimensions, relations. Retain useful information in DI;
  distinguish requirements, proposed assumptions, and observed conditions.
- Space identity/function: intent; class/layer alone does not prove function.
- Dimensions: document units + datum + target/assumed/measured status. Measurement also needs
  geometry and live/saved state. Floor top elevation, thickness, and height above it differ.
- Relations: navigate relevant members, containers, boundaries; check supported contracts.
- Representation: a footprint can express proposed extent without a physical floor. Construction
  or clearance questions may require faces/solids. Existing walls or proposed intents can be the
  starting evidence; there is no universal authoring order.
- Clarify material unknowns without a reasonable working basis; state reversible concept assumptions.
  Local edits need no full site setup. Complex briefs/new conventions:
  [project conventions](references/project-conventions.md).

## Scope and action size

Furniture-only/diagram scope stays limited. Each object needs a purpose; a room name does not
require a room assembly. Keep independently editable objects separate. Guide `design-context`
covers spatial assumptions and representation limits.

Split multi-part work where uncertain results affect later decisions. Verify an unfamiliar
partition/opening operation before wider use; batch predictable repetitions. Avoid one script for
the entire floor plan. DI identities/relations support local repair without rebuilding valid work.

## Routes

| Need | Skill |
|---|---|
| Geometry/adoption | Base `di-rhino-authoring` |
| Intents/representations/groups | Base `di-semantic-framework` |
| Components/dependencies/regeneration | `di-architecture-systems` |
| Clearance/spacing/area/dimensions | `di-architecture-evaluation` |
| Alternatives/revisions/handoff | `di-architecture-options` |

Load relevant mechanics only. Saving/repetition need no recipe; automation choices:
base `di-design-logic`. Direct space intents do not imply automatic room lifecycle.

## Evidence

Actual images support form/orientation/access; measurements support dimensions. Capture receipts
are not image review. Explain architectural trade-offs; do not invent a single score.
Report unknowns and failures separately from suspected causes. Check endpoints/contracts for
missing relations. Study/comparison alone does not authorize model changes.

Save when authorized; report `recipe_proof` and warnings. For baseline + unsaved edit, compare
named snapshot with live evidence; do not save again merely to inspect the edit.
