---
name: di-architecture-evaluation
description: >-
  Evaluate architectural geometry in DI against stated criteria: clearances, member spacing,
  envelopes, and area definitions. Report measurement basis, fidelity, and unresolved evidence.
---

# Architectural evaluation

Use the base DI connection and measurement skill. Fetch the measurement guide from `di_list_guides`
and `di_get_guide` for exact conventions. Evaluate the requested state; do not save merely to make
a read convenient. Named snapshots support historical review; staging requires live evidence.

## Define what the criterion measures

| Question | Establish before evaluating |
|---|---|
| Building height | Base datum, direction, and included roof or plant geometry |
| Setback | Authoritative boundary geometry, projection plane, and nearest included extent |
| Clear opening | Physical faces or explicit thickness; centreline spacing is a different metric |
| Repeated member spacing | Member reference, axis, ordering, and tolerance |
| Floor or site area | Included surfaces, holes, overlap treatment, and intrinsic vs projected area |
| Volume | Closed geometry; an envelope or footprint is not sufficient evidence |

Distinguish proposed context from confirmed physical geometry. A distance to an assumed space
outline is a proposed boundary distance, not a verified wall clearance or compliance result.

Do not report bounding-box area as floor area. Do not sum repeated occurrences or overlapping
surfaces without stating the counting rule. Use the same basis for the target and the measurement.

## Obtain evidence

Start with a scope census. Narrow candidates through authored properties and relations. Inspect
available representations, then choose the geometry that answers the criterion. Use extents for
dimensions, pair measurements for separation, and series measurements for repeated spacing.
Use a supported geometry query for area; discover its contract rather than inventing an area mode
on an unrelated tool.

Keep method, frame, units, and fidelity with each result. Bounding-box or approximate evidence may
support an early study, but does not establish a tight tolerance decision. Request kernel evidence
when required and available. Preserve refusals instead of replacing missing geometry with zero.

Compare the result to the supplied criterion. State meets, does not meet, or unresolved. This is a
verdict on that criterion and scope, not certification of the building. If a rule's source or
applicability is missing, report the measured fact and the missing premise.

## Use automation only where the contract supports it

A backend measurement can produce readings without producing a semantic patch. Before installing
a policy that selects objects by measured results, fetch `design-logic` and inspect the registered
output contracts. Do not bind readings as edges or install a frozen measured ID list as a live rule.
Use an interactive evaluation and one-time reviewed patch when no supported transform exists.

Read [the evidence format](references/evidence.md) when presenting several criteria or preparing
a comparison. Visual captures can show form and context, but do not establish numeric clearance.
