# fx-WahWahWah — Thomas Organ Cry-Baby (95-910511) reproduction PCB

An open-hardware KiCad recreation of the 1970s Thomas Organ Cry-Baby wah,
model **95-910511**, board **25-5500-2**.

The goal is a modern two-layer board that drops into an original Thomas Organ
shell and accepts either vintage or modern parts — including the original TDK
5103 "cube" inductor, the earlier round "stack of dimes" inductor, or a current
production replacement.

> **Status: not yet fabricated.** The design passes electrical and clearance
> checks (see [Verification](#verification)) but no board has been made or
> tested. Dimensions carry the tolerances documented below. If you build one,
> please open an issue and say how it went.

---

## Design goals

1. **Drop into an original shell.** Mounting features match the original board
   so it fits a period Thomas Organ chassis.
2. **Take any inductor.** One land pattern accepts modern two-lead wah
   inductors *and* both vintage four-post types.
3. **Vintage or modern passives.** Every resistor and capacitor has two
   footprints — one sized for 1970s carbon-comp and film parts, one for modern
   components — so builders choose the look they want.
4. **Hand-built.** Through-hole throughout, no assembly service required.

---

## Board

| | |
|---|---|
| Size | 3.000 × 2.0625 in (76.20 × 52.39 mm) |
| Layers | 2 |
| Mounting | 2 round holes + 1 edge slot |

### Mounting geometry

Origin at the **top-left corner**, component side up, X right / Y down —
the orientation of the original etch artwork.

| Feature | X (mm) | Y (mm) | Tolerance |
|---|---|---|---|
| Edge slot (left) | 3.05 | 9.97 | ±1.0 mm |
| Right hole | 70.36 | 24.87 | ±0.5 mm |
| Bottom hole | 50.22 | 44.82 | ±0.6 mm |

The left feature is cut as an **open slot in `Edge.Cuts`**, not a drilled hole.
On original boards this position is a hole that has broken out through the
board edge — the remaining web is only ~1.3 mm and cracks under a torqued
screw. Cutting it open avoids reproducing that failure, and because the slot
is open the ±1.0 mm X tolerance stops mattering.

Holes are oversized relative to stock to absorb the tolerances above. Screws
are #6 pan head (~6.4 mm head), which is the limit on how far they can be
opened up.

---

## Circuit

Standard Cry-Baby topology, transcribed from the period schematic:

- **Q1** — common-emitter gain stage, biased by an R6/R5 (470K/100K) divider
  tapped off its own collector, fed back to the base through the inductor
  network and R3 (1.5K).
- **Wah pot** takes Q1's collector through C4 and feeds its wiper to **Q2**.
- **Q2** — emitter follower driving the L/C tank through C3.
- **Tank** — L1 (500 mH) in parallel with R4 (33K) for damping; the resonant
  peak sweeps with the pot, and the loop closes back at Q1's base.

Bias is tolerant of transistor gain: across hFE 235 → 1200, Q1's collector
current moves only ~6% and Vc stays near mid-rail. **hFE is a voicing choice,
not a bias constraint** — it sets loop gain, so it controls how sharp and vocal
the peak is. The original specified 235–470 hFE; higher-gain parts give a
peakier, more aggressive wah.

---

## Bill of materials

Resistors `R1`–`R10` and capacitors `C1`–`C5` are the **modern** footprints.
`R21`–`R30` and `C21`–`C25` are the **vintage-sized** alternates at the same
positions, marked DNP.

| Ref | Value | Ref | Value |
|---|---|---|---|
| R1 | 68K | C1 | 10n |
| R2 | 470R | C2 | 4.7µ (see note) |
| R3 | 1.5K | C3 | 10n |
| R4 | 33K | C4 | 220n |
| R5 | 100K | C5 | 220n |
| R6 | 470K | L1 | 500 mH |
| R7 | 22K | Q1 | 2N5088 |
| R8 | 470K | Q2 | 2N5089 |
| R9 | 10K | RV1 | 100K log (off-board) |
| R10 | 1K | SW1 | SPDT (off-board) |

**Populate one variant per position, not both.** The footprints deliberately
overlap.

*C2 note:* the original is 3.9 µF, which is effectively unobtainable today.
The modern position is 4.7 µF; the vintage alternate (C22) keeps 3.9 µF. 
Several alternate footprints are added here - The 2mm lead offset
footprint fits modern electrolytics, there's a 5mm offset footprint that
will fit most tantalum caps, and the original ~15mm offset to fit a radial
cap or as on many older boards, a tantalum with the leads bent out.

In addition, there is an alternate landing for C2 in the upper right of
the board that can be populated by 2 2.2uF film box capacitors. These
retain the audio transparency of tantalum caps while still being
in production today (despite being a bit expensive).

Only populate one of these landings - either one of the footprints
below the inductor, or the pair of film caps in the upper right side
of the board.

### Passive lead pitches

Vintage footprints use the original board's spacings:

| Span | Pitch | Used by |
|---|---|---|
| 2× | 0.53 in / 13.5 mm | carbon-comp resistors |
| 3× | 0.80 in / 20.3 mm | 0.01 µF film caps |
| 4× | 1.06 in / 27.0 mm | 0.22 µF film caps |

The original was hand-taped on a ~0.265 × 0.1665 in grid — **not** a clean
decimal grid. Don't try to force these to 0.1 in increments.

### Transistors

The original board's silkscreen reads **E-C-B for Q1 and B-C-E for Q2** —
mirrored, and matching no modern part. This board uses **E-B-C** throughout and
silkscreens the pin letters rather than relying on the package outline, so any
part goes in whichever way puts its emitter on the E pad.

BC549/BC550 (C-B-E) also drop straight in — base is centre on both pinouts, so
they simply insert rotated 180°. If you want the stock voicing, **BC549B**
(hFE 200–450) brackets the original 235–470 spec more closely than either
2N5088 (300–900) or 2N5089 (400–1200).

---

## Inductor

`L1` uses a custom universal land pattern (`Wah_Inductor_Universal`, in
`fx-WahWahWah.pretty/`):

- **Four radial runways, three holes each.** Opposing pairs give lead spacings
  of **10.8 / 12.6 / 14.4 / 16.2 / 18.0 mm**. The 14.4 mm option matches the
  [stompboxparts ME-6](https://stompboxparts.com/transformers-inductors/wah-inductor-me-6/)
  (570 mH, 40 Ω max, Allied ARM6-4102-A) exactly.
- **A 14.85 × 12.29 mm four-post rectangle** for the original TDK 5103 cube and
  the earlier stack-of-dimes. Both vintage inductors share this pattern.

On the vintage rectangle, only the **left pair is electrical**; the right pair
is mechanical and carries no net. This matches the original artwork, where only
the left two posts had traces — if the 5103's right-hand posts are shield or
case tabs, netting them would short the coil to the can.

All same-numbered holes are tied together in copper, so whichever pair a
builder populates is live.

---

## Off-board wiring

The board carries no output pad — this is correct and matches the original.
The output jack is fed from the switch entirely off-board, and the wah signal
reaches the switch via the LUG 3 node.

| Pin | Net | Goes to |
|---|---|---|
| 1 | POT_LUG_2 | Wah pot, lug 2 (wiper) |
| 2, 3 | IN_JACK_TIP | Input jack tip; SPDT switch |
| 4 | POT_LUG_3 | Wah pot, lug 3 |
| 5, 6 | BATT_NEG | Battery −; input jack ring |
| 7 | +9V | Battery + |
| 8 | GND | Input jack sleeve |
| 9, 10 | GND | Spare grounds |

Battery negative returns through the input jack ring, so inserting a plug
powers the circuit — the stock arrangement.

### Grounding notes

- **Do not ground the mounting screws.** The shell is already at circuit ground
  through the jack bushings; bonding it again through the screws adds redundant
  paths that carry no return current and invites ground loops.
- **Do run a wire to pot lug 1.** It's the bottom of the divider that sets loop
  gain, and in the stock wiring its only return is through the enclosure
  casting. Use one of the spare ground pads (J1.9/J1.10) rather than relying on
  50-year-old bushing continuity — a corroded joint there makes the sweep
  scratchy and weak.

---

## Verification

Checked against JLCPCB's published constraints:

| Check | Result | Limit |
|---|---|---|
| Hole-to-hole, 92 through-holes | 0 violations | 0.5 mm |
| Different-net copper | min 0.800 mm | — |
| Copper to board edge / cutouts | min 0.500 mm | 0.2 mm |
| Non-plated slot width | 4.5 mm | 1.0 mm |

**DRC shows courtyard overlaps.** These are expected and intentional: the
vintage and modern footprints occupy the same positions by design, and only one
is ever populated. Courtyards are a pick-and-place abstraction and carry no
fabrication or electrical meaning on a hand-built board. Every check that maps
to a physical constraint passes.

---

## Provenance and tolerances

No original board was available to measure directly. The dimensions here were
derived photogrammetrically from published references and cross-validated
between independent sources:

1. A hobbyist home-etching layout sheet (dimensional reference only).
2. A photograph of a populated original board, perspective-corrected via
   homography against the stated 3.000 × 2.0625 in outline.
3. A photograph of a second unit of a different revision, used to confirm that
   hole positions are constant across revisions.

Sources 1 and 2 agree on hole-to-hole vectors to **0.47–1.57 mm**, with a
best-fit scale ratio of 0.9925. Sources 2 and 3 agree to within **1.04 mm** —
the noise floor of the photographs — confirming that the hole pattern did not
change between board revisions.

**If you have an original board and calipers, measurements would be very
welcome.** The two round mounting holes and the inductor rectangle are the
numbers most worth confirming.

---

## Credits

- **[Vintage Cry-Baby thread on the PedalPCB forum](https://forum.pedalpcb.com/threads/vintage-cry-baby.29962/)**
  — the discussion that started this, and the source of the reference material.
- **Andrew F. Shorty** — the schematic transcription and home-etching layout
  that served as the dimensional reference.
- **Forum members** who photographed original hardware, without which none of
  the measurements would have been possible.
- **PedalPCB** — the [Tear Jerker Wah](https://docs.pedalpcb.com/project/TearJerker.pdf)
  build documentation, whose universal inductor land pattern informed the
  runway geometry here.

Reference documents are **not** redistributed in this repository; they remain
the property of their respective authors. Follow the links above for the
originals.

---

## License

Licensed under the **CERN Open Hardware Licence Version 2 – Permissive**
(CERN-OHL-P v2). See [LICENSE](LICENSE) and [NOTICE](NOTICE).

You may make, modify, and sell boards from these files, with no obligation to
publish your changes. The underlying circuit dates from the 1970s and its
patents have long since expired.
