# Clearance and motion checks in SolidCraft (>= 0.28) before issuing layout drawings

Visual 3D and hand calculation missed five real collisions on one crane line; the automatic checks found them.

## Static poses (fast, scriptable)
- Build the static plant, then add the moving machine at every pose it reaches (crane every ~0.8 m of travel,
  pick/drop heights, each tipper angle) with unique tags, and run `inspect.clearance {bodies: moving, against:
  static, clearance: 15-20, min_volume: 1}` per pose. `solidcraft-cli run script.json --results --json-out res.json`
  returns the results; summarise overlaps and the nearest gaps per pair.
- Static-vs-static also matters: belts against plates, braces, base plates, RAT stoppers.
- Contacts that are by design (basket on rollers, pads under lugs) show as 0 mm gaps, not overlaps.

## Mechanisms (continuous)
- Put each moving group in its own component (`component.create {grounded: false}`), ground the frame
  explicitly (the first component created is grounded by default - a moving part grounded by accident moves
  nothing and every study stops at step 1).
- Sliders/revolutes with world points, `flip: true`; motion links for mirrored sides.
- `motion.study` + `motion.interference {study, occurrences: moving, against: frame}` over the whole cycle, and
  `joint.sweep {joint, from, to, step, occurrences, against}` for the real limit angle (e.g. a tipper free to
  -143.9 deg against a 135 deg requirement). `motion.export_animation` gives a GIF for the customer.
- Bodies that stretch (belts, scissors) are left out of the moving parts.
- Barrels as hollow tubes (rod inside) so cylinders do not report as collisions.

## Stainless (SUS304) structure
- Built-in material "Stainless Steel" is 8.0 g/cm3; create "SUS304" (7.93) and assign it, "UHMW-PE" (0.94) for
  wear pads, so masses come out right.
- `frame.member` on a sketch line with a built-in profile (rect_tube, round_tube, angle, channel, bar) gives real
  sections and mass; keep a list of stock SUS sizes (e.g. 25.4/38.1/50.8/76.2/100/150 square tube, 50x100,
  75x150, 100x200 rect tube, 25.4-76.2 pipe, 40/50 angle) and confirm availability with the supplier.
- Simplified solid baskets/products do not give real masses - keep measured tare values.
