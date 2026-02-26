# Fabrication & Assembly Guide

## Parts List

| Part | Value | Package | LCSC # | Qty | ~Cost |
|------|-------|---------|--------|-----|-------|
| U1 | NXP NT3H2111W0FHKH | SOT-886 | C710403 | 1 | $0.50 |
| C1 | 47pF C0G 0402 | 0402 | C1552 | 1 | $0.01 |

## Step 1: Export Gerbers

1. Open `business_card.kicad_pcb` in KiCad 9
2. File > Fabrication Outputs > Gerbers (.gbr)
3. Select all copper + mask + silkscreen + Edge.Cuts layers
4. Check "Use Protel filename extensions" for JLCPCB compatibility
5. Generate drill files (Excellon format) in the same output directory
6. Zip all `.gbr` + `.drl` files together

## Step 2: Order PCBs from JLCPCB

1. Go to [jlcpcb.com](https://www.jlcpcb.com) > Order Now
2. Upload the Gerber zip
3. Settings:
   - **Base Material**: FR-4 (prototype) or Flex (FPC) for transparent
   - **Layers**: 2
   - **PCB Thickness**: 0.8mm (FR-4) or 0.2mm (FPC)
   - **Surface Finish**: ENIG (gold traces)
   - **Solder Mask**: None or transparent (for the clear look)
   - **Silkscreen**: White
   - **PCB Color**: N/A if no mask, otherwise clear/transparent
4. For transparent cards specifically, select "Transparent FPC" if available
5. Quantity: 5 minimum (they're cheap — order 10+)

## Step 3: Order SMT Assembly (Recommended)

Hand-soldering a SOT-886 (1mm flip-chip) is brutal. Let JLCPCB do it.

1. On the same JLCPCB order, toggle "SMT Assembly"
2. Select **Bottom Side** (components are on B.Cu)
3. Upload BOM:
   ```
   Comment,Designator,Footprint,LCSC
   NT3H2111W0FHKH,U1,SOT-886,C710403
   47pF,C1,0402,C1552
   ```
4. Upload pick-and-place (CPL) file — export from KiCad:
   File > Fabrication Outputs > Component Placement (.pos)
5. Confirm placements in JLCPCB's visual preview
6. Review & order

## Step 4: Program the NFC Tag

Once boards arrive with components soldered:

1. Install **NFC Tools** on your phone
   - [Android (Play Store)](https://play.google.com/store/apps/details?id=com.wakdev.wdnfc)
   - [iPhone (App Store)](https://apps.apple.com/app/nfc-tools/id1252962749)
2. Open NFC Tools > **Write** tab
3. **Add a record** > URL > `https://mattcoburn.ai/card`
4. Tap **Write**
5. Hold your phone flat against the card (center of card, over the antenna area)
6. Wait for confirmation buzz/notification
7. Done — the URL is permanently stored until you overwrite it

## Step 5: Test

1. Lock your phone, then tap the card against it
2. Phone should show a notification to open `mattcoburn.ai/card`
3. Test with both Android and iPhone if possible
4. Test reading distance — should work through a phone case at 1-3cm

## Troubleshooting

**Phone doesn't detect the card:**
- Make sure NFC is enabled on the phone
- Try different positions — the phone's NFC reader is usually near the camera
- The antenna may need tuning — try a different value cap (39pF or 56pF)

**Read range is very short (<1cm):**
- Antenna inductance may be off — the 47pF cap assumes ~1.42uH
- Try swapping C1 to 39pF (higher inductance tolerance) or 56pF (lower)
- FR-4 substrate has higher dielectric losses than PET — range will improve on transparent FPC

**NFC Tools can't write:**
- NT3H2111 may have write protection enabled — use NFC Tools to check/reset
- Make sure you're writing NDEF format, not raw memory

## Cost Estimate (10 cards)

| Item | Cost |
|------|------|
| PCBs (10x, FR-4, ENIG) | ~$15 |
| SMT Assembly | ~$8 setup + $1/board |
| Components (NT3H2111 + cap) | ~$5 total |
| Shipping | ~$10 |
| **Total** | **~$40 for 10 cards** |

Transparent FPC is more expensive (~$50-80 for the boards alone), so prototype on FR-4 first.
