---
name: dirhp-annotations
description: Add or preview dimensions, labels, leaders, and witness curves in dirhp. Use for annotation output, not numeric measurement alone.
---

# Annotate a design

1. Identify the requested output and obtain current readings with `dirhp-measurement` as needed.
2. Read only the procedure for that output:

   | Output | Procedure |
   |---|---|
   | Preview existing declarations without Rhino objects | [Overlays](references/overlays.md) |
   | Store a dimension reading and presentation line | [Dimensions](references/dimensions.md) |
   | Labels, leaders, or marks that regenerate | [Managed marks](references/managed-marks.md) |
   | Rhino dimensions for a drawing | [Baking](references/baking.md) |

3. Apply that procedure within the authorized scope. Use confirmed model keys and selectors.
   Reading and presentation are separate; refresh both before presenting changed geometry.
   Do not install automatic updates unless requested. Dimension representations are presentation
   only and cannot be used as measurement operands.
4. Inspect the visible output. For regenerable marks, check a rerun updates without duplicates.
   Report unresolved readings, missing declarations, and whether output is an overlay, managed
   geometry, or a bake. State how it updates.
5. Leave work unsaved unless saving was requested or already authorized.

Discover only missing contracts. For automatic updates use
`dirhp_get_guide("annotations", sections=["Dimensions a policy keeps current"])` (guide `annotations`); other
payloads: `query=`. Not a prerequisite for every annotation task.
