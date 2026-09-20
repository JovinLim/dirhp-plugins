# Project conventions

Record only conventions needed by the current task. Use existing project information before
proposing new names or properties. A short working brief can hold:

| Field | Record |
|---|---|
| Scope | Site, building, system, or component included in this decision |
| State | Exact snapshot IDs, or identified staging options |
| Frame | Document units, project axes, level datum, or façade-local axes |
| Criterion | Target, tolerance, source, and scope of application |
| Evidence | Chosen geometry or representation, method, and fidelity |
| Assumed context | Proposed extents, margins, and their basis; separate from confirmed conditions |
| Unknowns | Missing model content or brief decisions that affect the answer |

Targets and observations are different data. For example, a brief's maximum envelope height and
a measured height need separate fields and a shared datum. Do not overwrite a target with a result.
An assumed north direction is not a surveyed bearing. An informal setback target is not proof of
the applicable planning rule.

## Choose graph structure by purpose

- Use an authored topological intent for a useful selection scope, such as a façade system or
  renovation package. Membership can overlap.
- Use a query-mode scope when membership follows existing authored properties. A frozen ID list
  records a selection at one point in time; it does not maintain a computed spatial rule.
- Use a separate option when a component needs independent history, reuse, or regeneration.
  A selection group alone does not require an option.
- Choose representations for the question: solid for volume, an explicit footprint for plan
  extent, a centreline for spacing, or a datum for orientation. Do not infer one from the label.

Keep keys and units consistent with the project. If a new convention is required, explain its
meaning and use only the needed fields. Do not impose an IFC vocabulary, a classification system,
or a full building hierarchy on an unrelated task.
