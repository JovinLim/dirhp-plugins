# Resolve measurement geometry

- dirhp produces none. A `default` representation is measured; otherwise the element measures ITSELF (`representation: "self"`, `extent_class` names the structure). `bbox` is the measured payload's box.
- Nothing is extruded from labels, a `z_band`, or bbox height. A footprint has zero vertical span; only an explicit closed mesh supports volume, containment, or overlap depth. A closed ring owns no centerline unless declared.
- Point, centerline, and plane operands need the `reference` capability and a declared representation (`dirhp_author_representations`; kinds and `source` in the guide). A `dimension` is presentation only: never default, always refused by measurement.
- `width_keys` names model-defined properties; `["dirhp:Thickness"]` is an example, not a guaranteed
  field or inferred thickness. Use keys confirmed in the model. It buffers an undeclared curve into a planar band for that read only; without it a curve, closed or open, measures as a curve, never as the region it encloses. `height_keys` is unsupported; dimensions take no `width_keys`.

Fetch `measuring-operations` only for missing representation payload contracts.
