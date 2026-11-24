# Managed Marks

For recipe-owned labels, leaders, witness curves, or construction marks, declare the output group
`auxiliary`. It may return a bare array and must not declare `semantics`.

- Reruns delete and rebuild managed marks, including hand-edited ones. Change the step or its inputs.
- Auxiliary objects are queryable host nodes but make no semantic declarations.
- Use `created` with `semantics` only when marks need design properties or semantic membership.
  Created-output relations target `$output` paths, not intents; do not assert derived relations.
  Fetch `annotations` for membership authoring.
- Use `dirhp_annotation_style().text_height`. Use `view.assign_layer@1` when a separate layer is needed.
- Reuse the existing option recipe where appropriate; guide `recipe-authoring` covers steps/outputs.
  For installed annotation recipes, `alias_prefix` resolves `recipe_alias_conflict` and permits
  multiple instances. Cleanup removes only that instance's owned steps/objects.
