# hfssGdsStackup example: grounded coplanar waveguide

A small test case for [hfssGdsStackup_v1p2.html](../hfssGdsStackup_v1p2.html): a 2-layer board with a grounded
coplanar waveguide (GCPW) and ground vias.

| File | Content |
|------|---------|
| `gcpw_test.gds` | Layout, units 1 nm database / 1 µm user. Layer 1: bottom ground; 2: top metal (signal + 2 coplanar grounds); 3: 10 vias; 4: substrate outline. |
| `gcpw_test_stackup_config.json` | Stackup for the tool: GND (copper, 35 µm), SUB (FR4_epoxy, 200 µm), VIA (copper, through SUB), TOP (copper, 35 µm). |

## How to run

1. Open `hfssGdsStackup_v1p2.html` in a browser and click **Load config**. Pick `gcpw_test_stackup_config.json`, or drag and drop it onto the page.
2. Click **Export script** and save `import_gcpw_test.py` **into this folder**. The GDS path `gcpw_test.gds` is resolved relative to the script's folder.
3. In AEDT, open (or insert) an HFSS design, then run **Automation > Run PyAEDT Script** and select the script.
   This needs CPython with PyAEDT (`ansys-aedt-core`) 0.18 or newer. It was tested with AEDT 2025.2.

You should see a copper ground plane, an FR4 substrate, a top layer with the signal strip between two ground planes,
and two rows of vias from the top grounds down to the bottom ground. Each layer is in its own group (GND, SUB, VIA,
TOP), and the variables `GND_thickness`, `GND_lowerElevation`, ... are created in the design. Change one of these
variables in HFSS and the layers follow.
