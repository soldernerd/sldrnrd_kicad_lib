This is a custom library for KiCad (https://www.kicad.org/).



Remote repository is https://github.com/soldernerd/sldrnrd\_kicad\_lib



Be concise in your answers and answer only the question asked. I will tell you if I need more information or background.



## Library Conventions

### Folder structure

| Folder | Layout | Description |
|--------|--------|-------------|
| `symbols/` | By category subfolder | One `.kicad_sym` file per component |
| `sldrnrd.pretty/` | Flat | One `.kicad_mod` file per footprint |
| `3dmodels/` | Flat | One `.step` file per footprint |
| `datasheets/` | By category subfolder | One PDF per component or package family |

### Symbols
- One `.kicad_sym` file per component, placed in the appropriate category subfolder.
- Create the category folder if it doesn't exist yet.
- Existing category placeholders (ADC, DAC, Diode, Display, Inductor, OpAmp, RotaryEncoder, Switch, …) are kept even when empty.

### Footprints
- Flat inside `sldrnrd.pretty/` — no subfolders.
- Generic package footprints (e.g. `SOT-23-5`, `TSSOP-8`) are shared across multiple components.

### 3D models
- `.step` files only — `.wrl` files are not used.
- Flat inside `3dmodels/`.
- Filename must match the corresponding footprint name exactly (e.g. `SOT-23-5.step` for `SOT-23-5.kicad_mod`).

### Datasheets
- Flat inside the appropriate category subfolder under `datasheets/`.
- Use a short, recognisable filename (full part number, without variant suffix if the datasheet covers the whole family).

### Naming
- Specific ICs: use the manufacturer part number (e.g. `STM32G0B1RET6`, `RN4871-I_RM128`).
- Generic packages: use the standard package name (e.g. `SOT-23-5`, `TSSOP-14`, `LQFP-64`).

