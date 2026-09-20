---
name: di-architecture-systems
description: >-
  Design reusable architectural components and systems with DI: façade bays, grids, floor
  components, datums, snapshot occurrences, and recipe or policy timing.
---

# Architectural systems

Use the base DI connection. Fetch `design-logic` before attaching automation and `datums` before
authoring placement. Fetch `options-and-snapshots` for component ownership and occurrence updates.
Discover current operation contracts before constructing calls.

## Choose a component boundary

Separate a component when its design should change independently or be reused in several places.
Keep an ordinary group in the current option when it only needs selection. For a repeated bay,
separate the bay design from the placement pattern and from project data that drives its dimensions.

Use local coordinates and an oriented datum for reusable geometry. Establish which axis follows
the façade, which points outward, and which is vertical. Read the returned axes before placement.
Apply the full datum transform once; an origin offset alone does not handle rotation or reflection.

## Separate generation, evaluation, and declaration

| Architectural job | Mechanism |
|---|---|
| Create fixed members or a placement pattern once | Direct Rhino creation and adoption |
| Generate members or placements that need regeneration | Rhino recipe |
| Read geometry and evaluate dimensions | DI queries or registered backend functions |
| State confirmed groups or representations once | Direct semantic authoring |
| Maintain selector groups or supported representations | Backend policy |

A fixed grid can use direct geometry; repeated objects and script loops do not require a recipe.
Before writing reusable logic, search catalog recipes, policies, and saved steps. Choose once, reusable on
demand, or installed regeneration from the task. Do not install a recipe merely because it is reusable.
Enable dirty reruns only when the requested semantic behavior should follow edits.

Use explicit `$output` bindings for component outputs. Pin external `$global` inputs to their
source manifest; use a data reference when only project parameters are needed. Plan graph reads
before host mutation, or use a supported explicit save boundary. A dirty policy is not a general
geometry solver and does not make an invalid mixed pipeline safe.

## Compose and revise

Save the source component before placing its snapshot. Use stable occurrence keys for physical
locations, such as an established bay identifier; do not renumber surviving bays because one was
removed. Resolve ownership before changing geometry selected through a composed graph.

For a source revision, distinguish loading an exact saved artifact from regenerating it with
current project inputs. Inspect pinned-input drift, update the source when requested, review it,
save it within scope, then repoint consumers and rerun. Saved assemblies keep their old pins.
For live datum bindings, update affected placements and resolve missing sources or cycles before
saving. Independent placements do not follow a source datum automatically.

Read [the bay example](references/repeated-bay.md) when separating component design from placement.

Done: component ownership, driving inputs, placement axes, and rerun timing are explicit; surviving
occurrences retain identity; unresolved dependencies are reported; unrelated geometry is preserved.
