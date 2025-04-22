# Custom IoT PCB with ESP32‑C3‑02 & TP4056 Charger

**Portable • Wi‑Fi & BLE • Li‑ion Charging • Modular**

---

## 📋 Table of Contents
1. [Overview](#overview)
2. [Features](#features)
3. [Design Highlights](#design-highlights)
4. [Applications](#applications)
5. [Getting Started](#getting-started)
6. [Repository Structure](#repository-structure)
7. [License](#license)

---

## 🌟 Overview
A compact, battery‑powered IoT development board built around the ESP32‑C3‑02 SoC. It integrates a TP4056 Li‑ion charger for safe single‑cell battery management and provides Wi‑Fi and BLE connectivity, environmental sensing, and expandable storage—all on a custom PCB optimized for low power consumption.

## 🚀 Features
- **Microcontroller:** ESP32‑C3‑02 (32‑bit RISC‑V core with Wi‑Fi & BLE)
- **Power Management:**
  - TP4056 charger IC with thermal regulation, undervoltage lockout, and trickle charging
  - USB‑C input (5 V) and Li‑ion battery connector (3.7 V nominal)
  - On‑board 3.3 V regulator for stable MCU/peripherals power
- **Environmental Sensors:**
  - BME280 (temperature, humidity, pressure)
  - Ambient light sensor
  - Sound level sensor
- **Storage:**
  - Micro SD card slot for data logging
  - SPI flash memory for firmware and data
- **Display:** 0.96" I2C OLED for real‑time metrics
- **Indicators:** Dual‑color LEDs (red = charging, green = full)
- **Expansion:** Breakouts for I2C, UART, SPI, and GPIO headers

## 🛠️ Design Highlights
1. **Power Integration**
   - USB‑C powers both system operation and Li‑ion battery charging
   - Charge current adjustable via resistor
   - Protection against over‑charge, over‑discharge, and thermal events
2. **PCB Layout Optimization**
   - Wide power traces minimize voltage drop
   - Thermal vias and copper pours under TP4056 for heat dissipation
3. **Modular Architecture**
   - Unified I2C bus for adding more sensors/peripherals
   - Standard headers simplify prototyping and debugging

## 🌐 Applications
- **Environmental Monitoring:** Deploy edge nodes for temperature, humidity, and pressure logging.
- **Portable Data Logging:** Battery‑powered capture of light, sound, and sensor metrics.
- **Rapid Prototyping:** Baseboard for ESP32‑C3 projects requiring power management and connectivity.

## 🏁 Getting Started
### 1. Download Source Files
```bash
git clone https://cadlab.io/project/28685/main/files.git
cd files
```

### 2. Review Hardware
- Open `hardware/esp32_c3_tp4056.kicad_pcb` in KiCad

### 3. Build & Flash Firmware
```bash
cd firmware
idf.py set-target esp32c3
idf.py build flash monitor
```

### 4. Power & Test
- Connect via USB‑C (5 V) or attach a Li‑ion cell
- Verify red LED lights during charging and green when full
- Monitor serial output for sensor readings and status

## 📂 Repository Structure
```
files/
├── hardware/        # KiCad schematics & PCB layout
├── firmware/        # ESP‑IDF project source code
├── docs/            # Component datasheets & assembly guides
└── LICENSE          # MIT License
```

## 📄 License
This project is licensed under the MIT License. See [LICENSE](LICENSE) for details.

