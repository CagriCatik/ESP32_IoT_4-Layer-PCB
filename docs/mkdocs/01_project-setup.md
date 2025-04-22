# Project Setup

This part provides a step-by-step, expert-level overview of designing a PCB in KiCad 9 based on an established workflow. It covers installation, configuration, project setup, library management, plugin use, and the initial schematic capture process.

---

## 1. Introduction

KiCad 9 introduces new features and improved performance over KiCad 8, while maintaining backward compatibility. This guide demonstrates how to:

- Install KiCad 9 alongside KiCad 8
- Configure global and per-project settings
- Manage symbol and footprint libraries
- Install and leverage essential plugins
- Organize and capture a multi‑page schematic

Whether you’re upgrading or starting fresh, this workflow ensures consistency, control, and high‑quality output.

---

## 2. System Requirements & Installation

1. **Platform:** Linux (tested on Debian/Ubuntu), Windows, or macOS.
2. **Dependencies:** Standard C++ toolchain, Python (for scripting), and Qt 6 libraries.

**Installation Steps:**

1. **Download KiCad 9 RC**
   - Obtain the latest `.deb` (or `.exe`/`.dmg`) from the official KiCad site or PPA repository.
   - Example (Debian/Ubuntu):
     ```bash
     sudo add-apt-repository ppa:kicad/kicad-9.0-releases
     sudo apt update
     sudo apt install kicad
     ```
2. **Parallel Installation**
   - KiCad 9 installs to `/usr/bin/kicad9` (or equivalent), leaving KiCad 8 at `/usr/bin/kicad` untouched.
   - You may create desktop or menu shortcuts labeled “KiCad 9 RC.”
3. **Verify**
   - Launch both versions:
     - `kicad` → KiCad 8
     - `kicad9` → KiCad 9 RC

---

## 3. Creating a New Project

1. **Directory Structure**
   - Create a dedicated folder for your project (e.g., `esp32_sensor_board`).
   - Decide whether to let KiCad create a subfolder or use an existing one.
2. **Project Initialization**
   - Launch KiCad 9 RC and select **File → New Project**.
   - Point to `esp32_sensor_board` and **uncheck** "Create new subdirectory."
   - Name the project file (e.g., `esp32_sensor_board.pro`).

This ensures all schematic (`.sch`), PCB (`.kicad_pcb`), and library files reside in one place.

---

## 4. Global Preferences & Editor Settings

Navigate **Preferences → Preferences…** to adjust:

### A. General
- Enable **High‑Quality Antialiasing** for sharper text and lines.

### B. Schematic Editor
- **Display Options:** Defaults are usually sufficient.
- **Grid Settings:**
  - Standard grids: 150 mil, 25 mil, 10 mil
  - Fast‑switch grids: 50 mil, 25 mil
  - Customize grid override rules for component placement.
- **Crosshairs:** Four‑window crosshair style for precise alignment.
- **Annotation & Net Label Options:** Accept defaults or adjust to your naming conventions.

### C. PCB Editor
- **Display & Grid:**
  - Match grid settings to schematic.
  - Four‑window crosshairs mirror the schematic editor.
- **Origin Axis:** Show and lock origin for multi‑page coordination.
- **Editing Modes:** Verify unit (mil/mm), layer visibility, and hotkeys.

Click **OK** to save preferences.

---

## 5. Plugin & Content Manager

KiCad 9 supports a variety of user‑contributed plugins. Recommended minimal set:

- **FreeRouting Plugin** – For semi‑automatic autorouting.
- **HQ-DFM** – High‑Quality Design-for-Manufacturing checks.
- **Interactive HTML BOM** – Generates clickable BOMs.
- **PCB Action Tools** – Advanced layout utilities.
- **Round Tracks** – Beautify track corners.

**Installation:**

1. **Open** the Plugin & Content Manager (top‑right button).
2. **Browse** and **Install** desired plugins.
3. **Restart** KiCad to enable them.

---

## 6. Library Management

Use **Preferences → Manage Symbol Libraries** and **Manage Footprint Libraries** to add project‑specific libraries.

1. **Scope Choice:**
   - **Global**: For reusable corporate or personal libraries.
   - **Project‑Specific**: Preferred for isolated control.
2. **Adding Footprint Libraries:**
   - Click **Add**, choose **Project‑Specific**.
   - Point to `./libraries/footprints` in your project folder.
   - Give it a clear nickname (e.g., `esp32_proj_fp`).
3. **Adding Symbol Libraries:**
   - Repeat for `./libraries/symbols`.
   - Multi‑select `.lib` files via Shift‑Click.
   - Assign a logical nickname (e.g., `esp32_proj_sym`).
