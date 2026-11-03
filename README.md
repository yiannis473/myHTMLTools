# myHTMLTools
General HTML Tools for EE

Each tool is a single self-contained HTML file: open it in a browser, no installation or network access needed.

| Tool | Description |
|------|-------------|
| [touchstoneViewer_v1p2.html](touchstoneViewer_v1p2.html) | Touchstone (S-parameter) file viewer. |
| [hfssGdsStackup_v1p2.html](hfssGdsStackup_v1p2.html) | HFSS GDS stackup generator: define the layer stackup in a table and export a PyAEDT script that imports a GDS file into the active HFSS design as parametric 3D layers. [Example](hfssGdsStackup_example/) |

## HFSS GDS Stackup Generator

Use it to define how each GDS layer becomes a 3D layer in HFSS, then export a Python script that builds the layers.

- **Table:** one row per layer, with name, type (layer / via, used only for the drawing), GDS layer number, z bottom,
  thickness and material. Values can be numbers, in the selected *Units for table values*, or expressions with HFSS
  variables, e.g. `GND_lowerElevation + GND_thickness`. Optional helper variables can also be defined.
- **Checks and preview:** the tool checks the stackup (missing fields, overlaps, unknown variables), draws a
  cross-section and shows the generated script.
- **Config files:** Save config / Load config store the whole setup as JSON. Undo / Redo are available.

**Running the script in AEDT:** open the HFSS design, then run **Automation > Run PyAEDT Script** and select the
exported script. The script:

- works in the active project and design, and does not change the solution type, units, setups or existing objects;
- for each layer `NAME`, uses the design variables `NAME_lowerElevation` and `NAME_thickness`, creating them with the
  table values only if they don't exist yet. An existing variable is reused as it is;
- imports each GDS layer as flat sheets, sweeps them by `NAME_thickness` and moves them by `NAME_lowerElevation`,
  so the geometry follows the variables;
- turns zero-thickness layers into sheets with a Perfect E boundary instead of a material;
- assigns the materials, which must already exist in the library;
- puts each layer in its own group: `NAME`, or `NAME_2`, `NAME_3`, ... if that name is taken. Objects are named
  `<group>_obj1`, `<group>_obj2`, ...;
- doesn't save or close anything.

It needs CPython with PyAEDT (`ansys-aedt-core`) 0.18 or newer, and was tested with AEDT 2025.2 and PyAEDT 0.18.
A ready-to-run test case (GDS + config) is in [hfssGdsStackup_example](hfssGdsStackup_example/).

## License

MIT. The full license text is embedded in each tool: click the **License** button in the tool.
There is no separate LICENSE file. ANSYS, AEDT, HFSS and Electronics Desktop are trademarks of ANSYS, Inc. or its subsidiaries;
these tools are not affiliated with or endorsed by Ansys.
