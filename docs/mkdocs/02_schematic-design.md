# Schematic Design

## 1. Project & Sheet Organization

- **Multi‑sheet structure**  
  - **Page 1**: USB & power circuitry  
  - **Page 2**: ESP32 module, USB‑to‑TTL converter, status LEDs  
  - **Page 3**: Sensors  
  - **Page 4**: User interface  
  - _Why it matters:_ logical separation keeps each domain self‑contained, makes ERC/DRC reviews easier, and aids team navigation.

- **Title‑block setup**  
  - Use **Sheet Settings** (right‑click → Properties → Sheet) to fill Title, Date, Revision, and Notes.  
  - Propagate these automatically into your PCB fabrication drawings.

- **Hierarchical Sheets**  
  - Each major function lives in its own `.kicad_sch` file, linked via a **hierarchical sheet symbol**.  
  - Name the sheet symbol and its source file clearly (`ESP32_C302.kicad_sch`) so file‑system and schematic navigator stay in sync.

---

## 2. Component Placement & Grouping

- **Logical grouping**  
  - Cluster USB receptacle + polyfuse + input filter cap, then the LiPo charger IC + its support passives, then LDO + its caps, then battery connector + power‑LED.  
  - Surround each cluster with a graphic **box** and label it (“USB Power Block”, “LiPo Charger”, “3.3 V LDO”, etc.) for visual clarity.

- **Symbol selection**  
  - Use footprint‑linked symbols from vetted libraries (e.g. SnapEDA imports).  
  - When symbols don’t exist, import or create a custom one—keep pin ordering logical (e.g. power pins on top/bottom).

- **Duplication & placement shortcuts**  
  - **D** to duplicate, **M** to move, **G** to drag (preserves wires), **R** to rotate.  
  - Minimize re‑wiring by dragging rather than moving.

---

## 3. Annotation & Value Assignment

- **Designators & values**  
  - After placing a generic resistor symbol (R), immediately set its **value** (10 kΩ, 470 Ω, 5 k1) and confirm its **designator** (R1, R2…).  
  - For resistor arrays or special values, use unambiguous notation (`5k1` instead of `5.1 k`) to avoid mis‑reads.

- **Controlled designator assignment**  
  - When importing many passive parts whose order matters, pre‑assign designators (R5, C6, etc.) rather than letting KiCad auto‑increment.

---

## 4. Nets & Wiring Practices

- **Wire drawing**  
  - Click pin → drag → click to finish; **Esc** cancels.  
  - Use **G (Drag)** to reposition symbols without breaking existing nets.

- **Net labels**  
  - Place **named net labels** (`VBAT`, `5V_USB`, `USB_DP`, `USB_DN`) to connect wires across sheet regions and sheets.  
  - For USB differential pairs, suffix net names with `P/N` (e.g. `USB_DP`, `USB_DN`) to leverage KiCad’s differential‑pair routing support.

- **Global vs local**  
  - Use **global labels** sparingly (power rails), and use **hierarchical labels** or sheet connectors for page‑to‑page signals to avoid accidental shorting.

---

## 5. Power & No‑Connect Flags

- **Power flags**  
  - Whenever a power pin is driven only by a net label (no explicit “power source” symbol), drop a **PWR_FLAG** on that net to suppress ERC “power-pin not driven” errors.
  - Example: tying MCP73871 VSS pins to GND via a power flag ensures ERC won’t complain.

- **No‑connect flags**  
  - Any unused device pin should get a **No_Connect** (NC) marker; this silences ERC warnings about floating pins.

- **Ground symbols**  
  - Use the GND symbol from the **power symbols** library for all chassis/common‑return.  
  - Do **not** hand‑draw ground wires; place ground‑symbol pins directly on each component pin to ensure clear connectivity.

---

## 6. Reference to Datasheets & Application Circuits

- **Follow manufacturer app‑notes**  
  - For the MCP73871 LiPo charger, replicate the “Typical Application” schematic exactly: pull‑down resistors on PROG pins, indicator LED resistors, decoupling caps, etc.
  - For the LM1117 LDO, use the recommended output and input capacitors (0.1 µF ceramic + 10 µF tantalum) placed logically on the schematic as they’ll map to critical PCB placement.

- **Clear annotation**  
  - Add a comment or PDF excerpt in your project documentation folder pointing back to the exact datasheet figure you implemented, for traceability.

---

## 7. Visual Aesthetics & Readability

- **Color‑coding sheets**  
  - Tint each hierarchical sheet symbol (e.g. light blue fill) so you immediately know which function you’re editing.

- **Functional blocks**  
  - Surround each related set of components with a **drawn rectangle** and label (“Charger”, “Regulator”, “USB Connector”).  
  - This helps reviewers and yourself jump to the right area quickly.

- **Consistent orientation**  
  - Align all input‑facing pins on one edge, outputs on the opposite.  
  - Keeps wires straight, reduces crossings, and improves readability.

---

## 8. ERC & Design‑Rule Checking

- **Run ERC frequently**  
  - After finishing each functional block, run the **Electrical Rules Check** to catch floating pins, missing power sources, or net conflicts before moving on.

- **Folder backups & autosave**  
  - Enable **Project Backup** (e.g. every 5 min, keep 10 copies) under **Preferences → PCB Editor → General** (or Schematic Editor equivalent) to avoid data loss, especially on release‑candidate versions.

---

### Summary

This transcript shows a very methodical, standards‑compliant approach to KiCad schematic design:

1. **Structured via hierarchical sheets** for clear separation of concerns.  
2. **Consistent component grouping** and annotation to mirror datasheet examples.  
3. **Rigorous net labeling** (including differential pairs) to feed into PCB‑layout routing tools.  
4. **Use of PWR_FLAGS and NC symbols** to maintain clean ERC results.  
5. **Aesthetic touches**—boxes, tinting, blocks—that make hand‑off and peer review much smoother.

Below is a step‑by‑-step, KiCad 9‑focused walkthrough of your transcript’s schematic‑capture process, emphasizing clear explanations for beginners. 

In summary, you begin by planning a four‑page schematic structure in KiCad 9 (“USB & Power”, “ESP32 + USB‑TTL & LEDs”, “Sensors”, “User Interface”), then configure your sheet settings (title, date), and place key power‑related components (USB‑C receptacle, Li‑Ion charger IC, LDO regulator, battery connector). Next, you add pull‑down resistors and indicator LEDs—always matching datasheet examples—while using hotkeys (A, M, G, D) to speed placement. You then draw wires (W), assign net labels (L) for rails and signals, and suppress ERC errors with PWR_FLAG and No_Connect symbols. You group each functional block with graphic outlines and labels, annotate designators/values, and finally create a hierarchical‑sheet symbol linking to your next page. Throughout, you run ERC frequently and back up automatically, ensuring a robust, readable schematic ready for layout.

---

## 1. Plan Your Multi‑Page Structure  
Before touching KiCad, sketch out your logical pages:  
- **Page 1**: USB & power components  
- **Page 2**: ESP32 module + USB‑TTL converter + status LEDs  
- **Page 3**: Sensors  
- **Page 4**: User interface  
Keeping each domain on its own sheet prevents clutter and simplifies reviews citeturn0search1.

---

## 2. Configure Grid & Sheet Settings  
1. **Grid Setup**: Press **Ctrl + Shift + G** to toggle grid visibility, or open **View ▶ Grid Properties** and set a 50 mil grid for symbols/wires and finer grids for text/graphics citeturn0search2.  
2. **Sheet Settings**: Right‑click whitespace ▶ **Properties ▶ Sheet**. Enter **Title** (“ESP32 Sensor Board”), **Date**, and optional revisions—this metadata appears in your title block.

---

## 3. Place & Configure Components  
1. **Open Symbol Chooser**: Click **Place Symbol** (op‑amp icon) or press **A**.  
2. **Search & Place**:  
   - **USB‑C receptacle**: search `SS-52400`, double‑click → click to place.  
   - **Li‑Ion charger IC**: search `MCP73871`, place it near the USB block.  
   - **LDO regulator**: search `LM1117`, place beside charger.  
   - **Battery connector**: search `JST_T`, place below charger/regulator.  
3. **Hotkeys for efficiency**:  
   - **R**: rotate  
   - **M**: move (wires reflow)  
   - **G**: drag (preserves wiring)  
   - **D**: duplicate symbols  
4. **Set Values & Footprints**: Double‑click each symbol, fill **Value** (e.g. `10 kΩ`, `LM1117`), confirm **Footprint**. Accurate values drive your BOM export citeturn0search6.

---

## 4. Add Pull‑Down Resistors & LEDs  
1. **Resistors**: Place generic `R`, double‑click to set values (`10 kΩ`, `5k1`, `2 kΩ`, `20 kΩ`) and designators (R1…R6). Use `5k1` notation to avoid misreading decimal points citeturn0search11.  
2. **Wiring**: Press **W**, click a resistor pin, drag to its target (e.g. CC1/CC2 pad), click to finish, **Esc** to cancel.  
3. **Indicator LEDs**: Protect LEDs with `470 Ω` resistors (R7–R9), then place R/G/B LED symbols (e.g. `LTST‐red`, `LTST‐green`, `LTST‐blue`) and wire their cathodes together to the net you’ll later name `5V_USB`.

---

## 5. Wire Nets & Apply Labels  
1. **Net Labels**: Press **L**, click a wire, type names such as `VBUS`, `VBAT`, `5V_USB`, or signal names (`STAT1`, `STAT2`) to clarify purpose.  
2. **Hierarchical Labels**: For sheet‑to‑sheet connections, use **Place Hierarchical Label** (right toolbar) and matching **Hierarchical Pin** on the parent sheet to define interfaces citeturn0search12.

---

## 6. Suppress ERC Errors with Power & No‑Connect Flags  
- **Power‑Flag (`PWR_FLAG`)**: Place on nets like `VBUS` and `5V_USB` so ERC knows these rails are driven, eliminating “power pin not driven” errors citeturn0search3.  
- **No‑Connect Flag**: Place `No_Connect` symbols on unused IC or connector pins to suppress floating‑pin warnings citeturn0search14.  
- **Run ERC often**: Click the ERC (green‑check) button after completing each block to catch wiring mistakes early citeturn0search3.

---

## 7. Group Functional Blocks Visually  
1. **Draw boxes**: Use **Place Graphic Line** to encircle related parts (USB jack + polyfuse + input cap, charger circuit, LDO + caps, battery connector + power LED).  
2. **Label boxes**: Add Text (Place ▶ Text) such as “USB Power Block” or “LiPo Charger Section” for quick scanning citeturn0search7.

---

## 8. Define Hierarchical Sheets  
1. **Place Hierarchical Sheet**: Click the sheet‑icon, draw its outline, double‑click to name (`ESP32_Core`) and set its file (`ESP32_Core.kicad_sch`) citeturn0search1.  
2. **Expose Pins**: On the new sheet symbol, add **Hierarchical Pins** named to match signals inside (e.g. `5V_USB`, `GND`, `STAT1`, etc.).  
3. **Navigate**: Double‑click the sheet or select it in the **Schematic Navigator** to jump pages.

---

## 9. Annotate & Finalize  
- **Annotate**: Tools ▶ Annotate Schematic to auto‑number any remaining parts.  
- **Backups**: In **Preferences ▶ Schematic Editor ▶ Project Auto‑Save**, enable periodic saves every 5 min (keep 10 copies).  
- **Final ERC & Review**: Ensure zero errors/warnings, clean up any overlapping text, and export a PDF schematic for documentation citeturn0search0.

Below is a compact, task‑oriented rewrite of your KiCad schematic workflow.  It walks from project setup through final ERC, emphasising **hierarchy, clear net naming, and disciplined rule‑checking** while weaving in the hot‑keys and symbols you already use.  Citations point to KiCad documentation and community best‑practice threads for deeper context.

---

## 1  Project Preparation

### 1.1  Plan the Page Stack
| Sheet | Purpose |
|-------|---------|
| 1 | USB power entry, Li‑Ion charge & regulation |
| 2 | ESP32 module, USB‑‑TTL bridge, status LEDs |
| 3 | Sensors |
| 4 | User‑interface devices |

Isolating domains on separate sheets keeps ERC scopes tight and helps reviewers focus on one subsystem at a time. citeturn0search0

### 1.2  Fill the Title Block
* **Right‑click ▶ Properties ▶ Sheet** → enter Title, Date, Rev, Notes.  
* These fields propagate automatically to fabrication drawings.

---

## 2  Hierarchical Structure

| Step | Action |
|------|--------|
|1 | **Place Hierarchical Sheet** icon → drag a frame |
|2 | Name frame & file (e.g. `ESP32_Core.kicad_sch`) |
|3 | **Import Sheet Pins** to expose only the nets you need |

Hierarchical pins act like ports, keeping global labels to a minimum and preventing accidental shorts across pages. citeturn0search0turn0search9

---

## 3  Component Placement

### 3.1  Logical Clustering
1. **USB Power Block**: receptacle + polyfuse + input cap  
2. **Li‑Ion Charger**: MCP73871 + passives  
3. **3  V3 LDO**: LM1117 + decoupling  
4. **Battery Block**: JST connector + power LED  

Surround each cluster with a thin rectangle and a label to speed visual scanning during peer review.

### 3.2  Symbol & Footprint Hygiene
* Prefer library symbols that already carry footprints; import from SnapEDA or create custom when needed.  
* Keep power pins at top/bottom, signals left/right for tidy routing.

### 3.3  Hot‑Keys That Matter
| Key | Effect |
|-----|--------|
| **A** | Place symbol |
| **R** | Rotate |
| **D** | Duplicate |
| **M / G** | Move with / without re‑wiring |

---

## 4  Annotation & Values

* Assign the **value** the moment you drop a passive (`10 k`, `470 Ω`, `5k1`).  
* Pre‑set designators for arrays or ordered parts to avoid KiCad’s alphabetical auto‑increment later.

---

## 5  Net Naming & Wiring

| Best‑practice | Why |
|---------------|-----|
|Use short, descriptive labels (`VBAT`, `USB_DP`) instead of drawn wires across the page | boosts readability |
|Add `P`/`N` suffixes for differential pairs (`USB_DP`, `USB_DN`) so KiCad recognises them during PCB routing citeturn0search3 | feeds diff‑pair router |
|Reserve global labels for power rails only; route sheet‑to‑sheet signals through hierarchical pins | avoids hidden shorts |

---

## 6  Power Flags & No‑Connects

Symbol | Purpose | When to place
|-------|---------|-------------
`PWR_FLAG` | Marks a net as actively driven; silences “power‑pin not driven” ERC error | every rail fed only by a label, e.g. `5V_USB` citeturn0search1turn0search7
`No_Connect` (X) | Declares an unused pin intentionally floating | any unused IC or connector pin citeturn0search2turn0search8

---

## 7  Visual Aids

* **Tint sheet frames** or use a light background colour per function (Power = blue, MCU = green).  
* Align inputs on the left edge of a block and outputs on the right; this keeps wires straight and crossings rare.

---

## 8  Verification & Backup

| Action | Timing | Tool |
|--------|--------|------|
|Run **ERC** | after finishing each block | ▸ green‑check icon citeturn0search4turn0search10 |
|Enable **Autosave / Project Back‑up** | once per project | Preferences ▶ Schematic Editor ▶ Auto‑Save (5 min, ≥10 copies) citeturn0search5turn0search11 |

---

## 9  Final Checklist

1. Sheets named, numbered, and linked.  
2. Every cluster boxed and labelled.  
3. All passives carry values and correct designators.  
4. Power nets have **PWR_FLAG**, unused pins have **NC**.  
5. ERC returns *zero* errors, only justified warnings.  
6. Project PDF exported, revision stamped, and backed up.

---

### Quick‑Reference Hot‑Key Card

| Task | Hot‑Key |
|------|---------|
|Place symbol | **A** |
|Wire | **W** |
|Net label | **L** |
|Drag item | **G** |
|Duplicate | **D** |
|ERC | **Ctrl + Shift + E** |

---

Following this checklist‑driven structure keeps your schematic clean, navigable, and fabrication‑ready without last‑minute ERC surprises.