# PCB Design Workflow

## Table of Contents

1. [Introduction](#introduction)  
2. [Phase I: Concept & Preparation](#phase-i-concept--preparation)  
   1. [Requirements Gathering](#requirements-gathering)  
   2. [System Architecture & Block Diagrams](#system-architecture--block-diagrams)  
   3. [Risk Analysis & Feasibility](#risk-analysis--feasibility)  
3. [Phase II: Component Research & BOM Management](#phase-ii-component-research--bom-management)  
   1. [Electrical Specifications](#electrical-specifications)  
   2. [Mechanical & Thermal Constraints](#mechanical--thermal-constraints)  
   3. [Supply‑Chain & Lifecycle](#supply-chain--lifecycle)  
   4. [BOM Structuring & Revision Control](#bom-structuring--revision-control)  
4. [Phase III: Library Preparation](#phase-iii-library-preparation)  
   1. [Schematic Symbols](#schematic-symbols)  
   2. [PCB Footprints & Land‑Pattern Validation](#pcb-footprints--land-pattern-validation)  
   3. [3D Models & STEP Imports](#3d-models--step-imports)  
   4. [Library Governance & Versioning](#library-governance--versioning)  
5. [Phase IV: Schematic Capture](#phase-iv-schematic-capture)  
   1. [Sheet Organization & Hierarchy](#sheet-organization--hierarchy)  
   2. [Net‑Naming Conventions & Buses](#net-naming-conventions--buses)  
   3. [Power Distribution Networks (PDN)](#power-distribution-networks-pdn)  
   4. [Design Rule Constraints & Electrical Rule Checks](#design-rule-constraints--electrical-rule-checks)  
6. [Phase V: Symbol‑to‑Footprint Linking](#phase-v-symbol-to-footprint-linking)  
   1. [Automated Assignment Tools](#automated-assignment-tools)  
   2. [Pin‑Mapping & Gate Swapping](#pin-mapping--gate-swapping)  
   3. [Footprint Fallbacks & Alts](#footprint-fallbacks--alts)  
7. [Phase VI: PCB Stack‑Up & Rules Setup](#phase-vi-pcb-stack-up--rules-setup)  
   1. [Layer Count & Materials](#layer-count--materials)  
   2. [Impedance Control & Transmission Lines](#impedance-control--transmission-lines)  
   3. [Design Rule Matrix & Classes](#design-rule-matrix--classes)  
8. [Phase VII: Component Placement](#phase-vii-component-placement)  
   1. [Functional Block Clustering](#functional-block-clustering)  
   2. [Connector & Test‑Point Positioning](#connector--test-point-positioning)  
   3. [Thermal Relief & Heatsinking](#thermal-relief--heatsinking)  
   4. [Mechanical & EMC Keep‑Outs](#mechanical--emc-keep-outs)  
9. [Phase VIII: Routing & Signal Integrity](#phase-viii-routing--signal-integrity)  
   1. [Trace Topology & Layer Usage](#trace-topology--layer-usage)  
   2. [High‑Speed & Differential Pair Routing](#high-speed--differential-pair-routing)  
   3. [Return Paths & Plane Coupling](#return-paths--plane-coupling)  
   4. [Length Tuning & Matching](#length-tuning--matching)  
10. [Phase IX: Power Integrity & Decoupling](#phase-ix-power-integrity--decoupling)  
    1. [Placement of Bulk & Local Caps](#placement-of-bulk--local-caps)  
    2. [PDN Simulation (Optional)](#pdn-simulation-optional)  
    3. [Ferrite Beads & Filters](#ferrite-beads--filters)  
11. [Phase X: Thermal Analysis](#phase-x-thermal-analysis)  
    1. [Steady‑State & Transient](#steady-state--transient)  
    2. [Copper Thieves & Thermal Vias](#copper-thieves--thermal-vias)  
    3. [Enclosure Interaction](#enclosure-interaction)  
12. [Phase XI: 3D Validation & Mechanical Check](#phase-xi-3d-validation--mechanical-check)  
13. [Phase XII: DRC, ERC & Advanced Checks](#phase-xii-drc-erc--advanced-checks)  
    1. [Standard DRC/ERC](#standard-drcerc)  
    2. [Design for Manufacturability (DFM)](#design-for-manufacturability-dfm)  
    3. [Design for Test (DFT)](#design-for-test-dft)  
    4. [EMC/EMI Pre‑Compliance](#ecmemi-pre-compliance)  
14. [Phase XIII: Gerber, Drill & Fabrication Data](#phase-xiii-gerber-drill--fabrication-data)  
    1. [Gerber RS‑274X & Attributes](#gerber-rs-274x--attributes)  
    2. [Excellon Drill & Tool Tables](#excellon-drill--tool-tables)  
    3. [ReadMe & Factory Notes](#readme--factory-notes)  
15. [Phase XIV: Assembly Data Prep](#phase-xiv-assembly-data-prep)  
    1. [Pick‑and‑Place (Centroid) Files](#pick-and-place-centroid-files)  
    2. [Assembly Drawings & Stencils](#assembly-drawings--stencils)  
    3. [Finalized BOM & Certificates](#finalized-bom--certificates)  
16. [Phase XV: Release & Handoff](#phase-xv-release--handoff)  
    1. [Version Control & Archiving](#version-control--archiving)  
    2. [Change Notice & ECOs](#change-notice--ecos)  
    3. [Supplier Communication](#supplier-communication)  
17. [Conclusion & Best Practices](#conclusion--best-practices)  

---

## 1. Introduction

A rigorous, multi‑phase PCB workflow drastically reduces rework and time‑to‑market. Each phase here builds on the last, with checks and balances (DRC, ERC, DFM, DFT, EMC) to catch errors early. While tools vary (Altium, OrCAD, KiCad, Allegro), the principles remain constant.

---

## 2. Phase I: Concept & Preparation

### Requirements Gathering  
- **Functional Specs:** Voltage rails, I/O counts, performance metrics (e.g., ADC sampling rate).  
- **Environmental:** Temperature range, shock/vibration, IP ratings.  
- **Regulatory:** UL, CE, FCC, RoHS directives; pre‑qualification for safety.

### System Architecture & Block Diagrams  
- Break the system into power, analog front‑end, digital logic, communications, and user interface blocks.  
- Define interfaces (e.g., SPI, I²C, USB, CAN) and isolation/level‑shifting needs.

### Risk Analysis & Feasibility  
- **FMEA:** Identify failure modes (e.g., power‑rail over‑voltage).  
- **Feasibility Studies:** Simulate critical blocks (e.g., high‑speed SERDES eye diagrams, DC/DC converter stability).

---

## 3. Phase II: Component Research & BOM Management

### Electrical Specifications  
- Cross‑reference voltage, current, switching speed, noise, and reliability parameters.

### Mechanical & Thermal Constraints  
- Package height, thermal pad footprint, power dissipation, required heatsinks.

### Supply‑Chain & Lifecycle  
- Check manufacturer lifecycle status (e.g., “not recommended for new designs”).  
- Identify primary and alternate sources; note MOQ and pricing tiers.

### BOM Structuring & Revision Control  
- Columns: Designator, QTY, Ref‑Des, Manufacturer PN, MPN, Value/C Rating, Package, Alt PN, Cost, Stock.  
- Use PLM or Git‑backed YAML/CSV with release tags.

---

## 4. Phase III: Library Preparation

### Schematic Symbols  
- Conform to IEEE/IEC standards for symbol shapes (op‑amps, gates).  
- Include pin‑types (Power, Passive, I/O) for ERC.

### PCB Footprints & Land‑Pattern Validation  
- Follow IPC‑7351 Class B or better.  
- Inspect courtyard, assembly, and fabrication layers.  
- Verify with datasheet mechanical drawings; measure pads, pitch, slivers.

### 3D Models & STEP Imports  
- Import 3D STEP files for critical connectors, heat sinks, enclosures.  
- Align origin/pivot points with footprint.

### Library Governance & Versioning  
- Central library server or shared Git repo.  
- Enforce review before symbol/footprint merges.  
- Tag stable releases.

---

## 5. Phase IV: Schematic Capture

### Sheet Organization & Hierarchy  
- Top‑level sheet with power and global nets.  
- Sub‑sheets per functional block; pass parameters via harness connectors.

### Net‑Naming Conventions & Buses  
- Use unambiguous names: +3V3, GND, CAN_H, CAN_L.  
- Define buses (ADDR[7:0], DATA[15:0]) and harness labels.

### Power Distribution Networks (PDN)  
- Annotate each power pin with source type (LDO1, SWITCHER_B).  
- Insert test‑points and measurement points.

### Design Rule Constraints & Electrical Rule Checks  
- Set up ERC for pin‑type mismatches, floating pins, power‑to‑power loops.  
- Define custom ERC rules for your organization (e.g., all analog grounds must tie to AGND).

---

## 6. Phase V: Symbol‑to‑Footprint Linking

### Automated Assignment Tools  
- Use centralized database: query by MPN to retrieve footprint name.  
- Batch‑assign by filter (e.g., all 0603 passives).

### Pin‑Mapping & Gate Swapping  
- For multi‑gate ICs, ensure gate‑to‑footprint gate mapping matches pin numbering.  
- Handle unused gates (tie inputs to valid logic level, leave outputs unconnected).

### Footprint Fallbacks & Alts  
- Define secondary footprints for alternate suppliers.  
- Mark in BOM and CAD so a PnP machine can switch if primary is out of stock.

---

## 7. Phase VI: PCB Stack‑Up & Rules Setup

### Layer Count & Materials  
- Select core/prepreg materials (FR‑4, Rogers, PTFE).  
- Specify dielectric constants for impedance simulation.

### Impedance Control & Transmission Lines  
- Define target impedances: 50 Ω single‑ended, 90 Ω differential.  
- Assign stripline vs. microstrip layers.

### Design Rule Matrix & Classes  
- Net classes: Power (wide traces), High‑speed (tighter clearance), Control (standard).  
- Clearance rules per class, via definitions (thru‑via, microvia, blind/buried).

---

## 8. Phase VII: Component Placement

### Functional Block Clustering  
- Place power upstream (near connectors/incoming power).  
- Group RF front‑end away from noisy digital regions.

### Connector & Test‑Point Positioning  
- Align connectors to mechanical cutouts early.  
- Distribute test‑points for ATS (bed‑of‑nails) and manual probing.

### Thermal Relief & Heatsinking  
- For thermal pads, define via‑in‑pad or thermal‑via arrays.  
- Place copper pours under hot chips, connect via thermal spokes.

### Mechanical & EMC Keep‑Outs  
- Mark board outlines, keep‑out for screws/holders.  
- Reserve guard‑ring or stitching‑via region for sensitive analog.

---

## 9. Phase VIII: Routing & Signal Integrity

### Trace Topology & Layer Usage  
- Assign specific layers for direction (e.g., X routing on L2, Y routing on L4) to minimize crosstalk.

### High‑Speed & Differential Pair Routing  
- Set differential pair rules: max skew, max gap, min/ max spacing.  
- Use controlled impedance tuning in CAD.

### Return Paths & Plane Coupling  
- Ensure continuous return plane below.  
- Avoid splitting planes under wide high‑speed nets.

### Length Tuning & Matching  
- Use meanders or serpentine to match bus lengths (USB SuperSpeed, LVDS pairs).

---

## 10. Phase IX: Power Integrity & Decoupling

### Placement of Bulk & Local Caps  
- Bulk caps near regulator outputs; smaller ceramics (<100 nF) close to IC pins.  
- Route caps on same layer as IC power pins for minimal loop area.

### PDN Simulation (Optional)  
- Use tools like Ansys SIwave, Cadence Sigrity to simulate impedance vs. frequency.

### Ferrite Beads & Filters  
- Place EMI filters on sensitive lines (USB, audio).  
- Observe manufacturer’s recommended high‑frequency bypass.

---

## 11. Phase X: Thermal Analysis

### Steady‑State & Transient  
- Model worst‑case ambient and power dissipation.  
- Verify via hand‑calculations or CFD simulation for heat sinks.

### Copper Thieves & Thermal Vias  
- If routers “steal” copper, place dummy copper to balance copper distribution.  
- Use thermal‑via arrays under power ICs.

### Enclosure Interaction  
- Check airflow patterns, mounting bosses, and metal standoffs.

---

## 12. Phase XI: 3D Validation & Mechanical Check

- Generate full 3D assembly; export as STEP/Parasolid for enclosure CAD.  
- Cross‑section to inspect silk overlaps, solder‑mask dams, and keep‑outs.

---

## 13. Phase XII: DRC, ERC & Advanced Checks

### Standard DRC/ERC  
- Iteratively fix violations: clearances, annular rings, un‑routed nets.

### Design for Manufacturability (DFM)  
- Use IPC‑2581/ODB++ exports.  
- Check solder‑mask slivers, paste‑mask overlaps, panelization guidelines.

### Design for Test (DFT)  
- Ensure test‑point coverage ≥ 90%.  
- Add boundary‑scan targets, JTAG headers.

### EMC/EMI Pre‑Compliance  
- Place EMI filters, ferrites, common‑mode chokes.  
- Run near‑field scans or simulation (CST, HFSS) if available.

---

## 14. Phase XIII: Gerber, Drill & Fabrication Data

### Gerber RS‑274X & Attributes  
- One file per layer: Top Cu, Bottom Cu, Solder‑mask, Silk, Paste.  
- Include layer attributes (G04 comments) for clarity.

### Excellon Drill & Tool Tables  
- Declare tool numbers, drill spans (microvia, via, slot).  
- Ensure proper units (inch/mm) with header.

### ReadMe & Factory Notes  
- Document stack‑up, impedance targets, material call‑outs, finishing (ENIG/HASL).  
- Note special requirements: gold fingers, controlled-depth drills.

---

## 15. Phase XIV: Assembly Data Prep

### Pick‑and‑Place (Centroid) Files  
- CSV or XLS with columns: Designator, X (mm), Y (mm), Rotation (°), Side.  
- Verify coordinate origin and units match fab house spec.

### Assembly Drawings & Stencils  
- Provide top‑ and bottom‑layer drawings with reference designators.  
- Include gerber for solder‑paste stencil.

### Finalized BOM & Certificates  
- BOM must match pick‑and‑place file — no orphan parts.  
- Include RoHS/REACH declarations, shelf‑life info for electrolytics.

---

## 16. Phase XV: Release & Handoff

### Version Control & Archiving  
- Tag release (v1.0) in Git or PLM system.  
- Archive all files (CAD, docs, outputs) in a release folder.

### Change Notice & ECOs  
- If post‑release modifications arise, issue ECO with affected nets/components, date, reason.

### Supplier Communication  
- Send package to PCB fab/ass’y: include FTP/portal upload, confirm receipt.  
- Track in your ERP/MRP to align with procurement.

---

## 17. Conclusion & Best Practices

- **Iterate Early:** Run DRC/DRM after each major step.  
- **Automate Where Possible:** Script BOM exports, ERC rule checks, batch‑generate outputs.  
- **Cross‑Team Reviews:** Hold schematic, layout, and DFM reviews with mechanical, manufacturing, and test engineering.  
- **Maintain Documentation:** Keep datasheets, test reports, and change logs centralized.

By following this granular workflow—and tailoring specific sub‑steps to your organization’s tools and standards—you’ll maximize first‑pass yield, accelerate time‑to‑prototype, and ensure a smooth transition into volume production.