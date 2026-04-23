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
- Every symbol must have exactly one footprint assigned. Multiple symbols may reference the same footprint file.
- Do not create new folders or subfolders without explicit user approval.
- Category folder names are singular (e.g. `Connector`, not `Connectors`).
- Pin placement must mirror the physical pinout of the component (e.g. pin 1 top-left, pin order following the datasheet package diagram).

The canonical folder structure is:

```
symbols/
  ADC/
  Audio/
  Capacitor/
  Connector/
    100Mil/
    FFC/
    USB/
  Crystal/
  DAC/
  Diode/
  Display/
  Inductor/
  InputDevice/
    PushButton/
    RotaryEncoder/
  LED/
  Logic/
    74/
  Memory/
    EEPROM/
    Flash/
  Microcontroller/
    PIC/
    STM/
  OpAmp/
  Power/
    BatteryManagement/
    ChargePump/
    LinearRegulator/
  Resistor/
  Sensor/
  Transistor/
    MosfetN/
    MosfetP/
    NPN/
    PNP/
  Wireless/
    Bluetooth/
```

### Footprints
- Flat inside `sldrnrd.pretty/` — no subfolders.
- Generic package footprints (e.g. `SOT-23-5`, `TSSOP-8`) are shared across multiple components.

### 3D models
- `.step` files only — `.wrl` files are not used.
- Flat inside `3dmodels/`.
- Filename must match the corresponding footprint name exactly (e.g. `SOT-23-5.step` for `SOT-23-5.kicad_mod`).

### Datasheets
- Required for complex components (ICs, displays, modules, etc.); omitted for simple passives (resistors, capacitors, generic diodes, etc.).
- Stored as PDF in `datasheets/<category>/` — mirroring the `symbols/` folder structure.
- Use a short, recognisable filename (full part number, without variant suffix if the datasheet covers the whole family).
- Claude downloads the datasheet from the manufacturer's website or a trusted distributor (Mouser, Farnell, Digikey, Octopart, or similar).
- The symbol's `Datasheet` field must contain the path relative to the library root: `${SLDRNRD_LIB}/datasheets/<category>/<filename>.pdf`.

### Naming
- No spaces or special characters; use `_` as separator and `-` where it is part of a standard name.
- **Simple passives:** generic descriptor (e.g. `Resistor`, `Capacitor_100n`, `LED_Red`).
- **Discrete semiconductors:** part number or generic type (e.g. `1N4148`, `NPN_Generic`).
- **Complex ICs and modules:** full manufacturer part number (e.g. `PIC16F18446`, `RN4871-I_RM128`).
- **Datasheets:** broadest part number covered by the datasheet (e.g. `PIC16F184xx.pdf`, `RN4871.pdf`).
- **Footprints:** standard package name (e.g. `SOIC-8`, `SOT-23-5`, `TSSOP-14`).

### Master symbol library

`symbols/sldrnrd.kicad_sym` is the single combined library file that KiCad projects reference. It must always be kept in sync with the individual per-component `.kicad_sym` files.

- Every symbol that exists as an individual file under `symbols/<category>/` must also appear in `sldrnrd.kicad_sym`.
- Symbol names inside `sldrnrd.kicad_sym` are prefixed with their category path: `"Category/SubCategory/SymbolName"` (e.g. `"Power/LinearRegulator/LP5907MFX-3.3_NOPB"`).
- Whenever a new symbol is added or an existing one is modified, update `sldrnrd.kicad_sym` in the same commit.
- When adding to `sldrnrd.kicad_sym`: copy the symbol body verbatim from the individual file, change only the symbol name to add the category prefix, and append it before the final closing `)` of the file.

### Commit and push workflow
Before committing, Claude must:

1. **Check spec adherence** — verify all symbols, footprints, datasheets, and 3D models conform to the conventions in this file. Fix any violations automatically.
2. **Check footprint existence** — every footprint referenced by a symbol must exist in `sldrnrd.pretty/`. Create any missing footprint automatically.
3. **Check datasheet existence** — every complex component symbol must have a datasheet. Download any missing datasheet automatically from the manufacturer's website or a trusted distributor (Mouser, Farnell, Digikey, Octopart, or similar).
4. **Check 3D model existence** — every footprint must have a corresponding `.step` file in `3dmodels/`. Source and save any missing 3D model automatically from a trustworthy source (GrabCAD, SnapEDA, manufacturer, or similar).
5. **Update sldrnrd.kicad_sym** — add or update every new/changed symbol in the master library file (see above).
6. **Renames and moves** — renaming or moving a component that has already been pushed to GitHub breaks existing designs and requires explicit user confirmation before proceeding. Renames and moves of components not yet pushed to GitHub may be done automatically.
7. Once all checks pass (or fixes are applied), commit to master locally. **Do not push to GitHub** — ask the user first before running `git push`.

### JLCPCB / LCSC part numbers
- If a component is available on JLCPCB, include its LCSC part number as a symbol field named `LCSC` (e.g. `C123456`).
- Omit the field entirely for components not available on JLCPCB.
