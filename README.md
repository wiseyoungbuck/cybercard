# NFC Business Card PCB

Transparent NFC business card for GTC. Credit-card sized (85.6mm x 53.98mm) PCB with an NFC antenna, IC, and decorative copper/silkscreen artwork.

## Viewing the Design

### Open in KiCad 9
1. Open KiCad 9
2. File > Open > select `business_card.kicad_pcb`
3. The file opens directly as a standalone PCB (no schematic/project needed)

### Viewing Layers
- **Front copper (F.Cu)**: Antenna spiral, "The Matthew Effect", "MATT COBURN", neural network motif
- **Front silkscreen (F.SilkS)**: "Applied AI x Security"
- **Back copper (B.Cu)**: IC pads, cap pads, routing traces from antenna vias
- **Back silkscreen (B.SilkS)**: CTA text, NFC icon, contact info, QR placeholder
- Toggle layers on/off in the right sidebar Layers panel

### 3D Viewer
- View > 3D Viewer (or Alt+3) to see a realistic rendering of both sides
- Use middle-mouse to rotate, scroll to zoom
- Press `Z` to flip the board and inspect the back side

### CLI Preview (no GUI needed)
```bash
# Render front side to PNG
kicad-cli pcb export svg business_card.kicad_pcb -o front.svg --layers "F.Cu,F.SilkS,Edge.Cuts"
rsvg-convert front.svg -o front.png -w 1200

# Render back side to PNG (mirrored for correct reading)
kicad-cli pcb export svg business_card.kicad_pcb -o back.svg --layers "B.Cu,B.SilkS,Edge.Cuts" --mirror
rsvg-convert back.svg -o back.png -w 1200

# Run DRC check
kicad-cli pcb drc business_card.kicad_pcb -o drc_report.json --severity-all
```

## Design Overview

### Front Side (F.Cu + F.SilkS)
- **Antenna**: 3-turn rectangular spiral on F.Cu (0.5mm trace, 0.5mm gap, ~1.42uH)
- **Text**: "The Matthew Effect" and "MATT COBURN" in copper, "Applied AI x Security" in silkscreen
- **Neural network motif**: Decorative node-edge graph in copper flanking the text
- **Logo zone**: 10mm x 10mm courtyard placeholder at center-bottom

### Back Side (B.Cu + B.SilkS)
- **IC**: NT3H2111 NFC tag IC in SOT-886 package (U1)
- **Capacitor**: 47pF 0402 tuning cap (C1)
- **CTA**: "TAP YOUR PHONE TO TALK TO MY AI" with NFC contactless icon
- **Contact info**: Name, title, email in silkscreen
- **QR code**: Placeholder — replace with actual QR for `https://mattcoburn.ai/card`

### Antenna Design
- Target inductance: ~1.42uH (13.56MHz resonance with ~97pF total capacitance)
- Modified Wheeler formula: 3 turns, 0.5mm trace, 0.5mm gap
- 3mm inset from board edge
- Two vias connect antenna ends to IC on back side

## DRC Status

3 warnings (all non-critical):
- Silkscreen clipped by solder mask (U1 reference label — cosmetic)
- Missing footprint libraries `NFC` and `Passive` (custom footprints defined inline)

0 errors. 0 unconnected pads.

## TODO Before Fabrication

1. **QR Code** ([THE-183](https://linear.app/thedolist/issue/THE-183/add-qr-code-to-nfc-business-card-pcb)): Replace the placeholder rectangle with actual QR code modules.
2. **Logo**: Add Tangible AI logo in the courtyard zone on F.Cu or F.SilkS.
3. **Visual review**: Open in KiCad 3D viewer to inspect both sides.
4. **Verify antenna**: Consider NFC simulation or test with prototype.
5. **Export Gerbers**: File > Fabrication Outputs > Gerbers for JLCPCB upload.

## Manufacturing

Target: JLCPCB Transparent FPC (prototype on FR-4)

| Parameter | Value |
|-----------|-------|
| Board size | 85.6mm x 53.98mm |
| Layers | 2 (F.Cu + B.Cu) |
| Trace width | 0.5mm (antenna), 0.3mm (routing), 0.2mm (motif) |
| Via | 0.6mm pad / 0.3mm drill |
| Corner radius | 3.18mm |

## Components

| Ref | Value | Package | Description |
|-----|-------|---------|-------------|
| U1 | NT3H2111 | SOT-886 | NFC tag IC |
| C1 | 47pF | 0402 | Tuning capacitor |
