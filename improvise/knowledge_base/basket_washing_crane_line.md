# Basket washing line with an overhead gang crane (soak / drain / dump)

Field-proven pattern (2026) for washing a wet product (seaweed, vegetables) in ~200 kg perforated SUS baskets
in existing soak tanks, with one operator. Adjust every number to the site.

## 1. Process and recipe
Fill basket to a net weight -> soak 30 min -> lift and drain 5 min -> dip again, x3 rounds -> lift out, weigh ->
convey to the next room -> tip into the mixer -> empty basket returns to the fill point. One basket loop, one way,
no crossing traffic. Tanks with 3 compartments in a row = 3 baskets per crane lift (gang gripper, pitch = compartment
pitch). Stagger tank starts (~36 min for 3 tanks) so lift times never coincide.

## 2. Layout rules (low room, ceiling ~3.0 m)
- Crane runways INSIDE the building-column line, hung on wall and column brackets; no floor posts. A rail on the
  column centre line means the bridge cannot pass the column.
- Carried-basket path: basket bottom about 100 mm above the highest soaking basket (+1.12 -> travel +1.20 m).
  Anything that stays at a tank must sit below it when idle.
- Drain rests as LIFT-BEAMS: posts only at the tank ends, one beam per long side lifted ~720 mm by guided air
  cylinders, pads pushed 50 mm under the lug-bar ends. Idle beam top below the travel path. Swing arms or posts
  between baskets block the crane.
- Gripper heads above the basket rim (heads beside the lug bars collide with the rest pads); no drip trays at
  lug height; anti-sway scissors in the plane along the gripper beam (across the travel they hit the end wall).
- A lane of waiting full baskets that the crane crosses when unloading can only be fed while a tank waits for
  its load; park filled baskets outside the crane path (RAT / fill spur). Costs a few % of output.
- Manual fill: RAT pushes the empty basket onto a forward/reverse roller-drive spur on 4 load cells, operator on a
  +0.30 m platform, net = gross - RFID tare, release at target, spur reverses into the lane (~5 min / basket).
- Tipper: clamp the lug bars, lift ~0.65 m, tilt ~135 deg about the front-bottom edge. Check basket AND arms
  against plate and belts at every angle. Plate under the sweep -> cross belt under the plate's low edge in front of
  the tipper columns -> Z-type corrugated sidewall belt that stays outside the receiving machine and runs level
  over its inlet. Keep braces and drives out of the belt path and out of the cradle sweep.

## 3. Capacity (typical simulation result)
3 tanks x 3 baskets, 30/5 x3 recipe: ~1,000 kg/h net, crane ~13 % busy, 12 baskets circulating (buy +50 % spare).
Model every conveyor as an accumulating queue (one basket per position) and check basket overlaps in the
animation automatically before showing it.

## 4. Timing control and traceability
- Times from sensor-confirmed events (at soak level, on the rest), never from the plan clock. Never under-soak in
  auto; manual early lift is recorded and goes to QA.
- Overrun per round: green <= 60 s, yellow <= 5 min, red > 5 min = QA hold at the tipper (customer QA confirms).
- Compensation modes: A = every round >= 30:00 (default); B = total 90:00, the overrun of one round is taken off
  the next one, never below a minimum (e.g. 25:00).
- Downstream full -> stop feeding upstream so finished baskets wait on the rests, out of the water.
- RFID tag + tare per basket; read and weigh at fill, crane pick, every lift, lift-out, room exit and tipper.
  Record per basket cycle: net in, tank/compartment, real soak/drain per round, net after washing, dump time, mixer
  batch. Power loss: brakes hold, PLC/HMI on UPS, resume from timestamps.
