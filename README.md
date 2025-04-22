# Custom IoT PCB with ESP32‑C3‑02 & TP4056 Li‑ion Charger

A versatile, battery‑powered IoT development board built around the ESP32‑C3‑02 SoC, featuring integrated Li‑ion charging, environmental sensing, and expandable storage.

---

## Table of Contents
1. [Overview](#overview)
2. [Key Features](#key-features)
3. [Hardware Design](#hardware-design)
4. [Firmware](#firmware)
5. [Applications](#applications)
6. [Getting Started](#getting-started)
7. [Repository Structure](#repository-structure)

---

## Overview

This project delivers a custom PCB optimized for low‑power IoT deployments. At its heart is the ESP32‑C3‑02 SoC, providing both Wi‑Fi and BLE connectivity. Power management is handled by the TP4056 charger IC, allowing safe, efficient charging of a single‑cell Li‑ion battery via USB‑C.

## Key Features

- **SoC:** ESP32‑C3‑02 (32‑bit RISC‑V, Wi‑Fi, BLE)
- **Battery Charging:** TP4056 single‑cell Li‑ion charger with thermal regulation, undervoltage lockout, and trickle charge support
- **Power Inputs:**
  - USB‑C (5 V) for charging and power
  - Li‑ion battery connector (3.7 V nominal)
- **Sensors:**
  - BME280 (temperature, humidity, pressure)
  - Ambient light sensor
  - Sound level sensor
- **Storage:**
  - Micro SD card slot
  - External SPI flash memory
- **Display:** 0.96" I2C OLED for live data feedback
- **Indicators:** Dual‑color LEDs for charging status (charging/full)

## Hardware Design

1. **Power Management**
   - USB‑C input streams 5 V to the TP4056; charge rate adjustable via resistor.
   - On‑board protection guards against over‑charge, over‑discharge, and thermal overload.
   - Step‑down regulator delivers stable 3.3 V to the ESP32‑C3‑02 and peripherals.

2. **Modularity**
   - I2C bus breakout for easy sensor and peripheral expansion.
   - Standard headers for UART, SPI, and GPIO access.

3. **PCB Layout**
   - Optimized trace widths for power efficiency.
   - Thermal vias and copper pours for TP4056 heat dissipation.

## Firmware

- **Language:** C/C++ using ESP‑IDF
- **Features:**
  - Sensor polling and data aggregation
  - SD card logging with file rotation
  - OLED dashboard for real‑time metrics
  - OTA update support over Wi‑Fi

## Applications

- **Environmental Monitoring:** Deploy edge devices for temperature, humidity, and pressure logging.
- **Portable Data Loggers:** Battery‑powered logging of ambient light, sound, and sensor data.
- **Prototype Platform:** Rapid development of ESP32‑based IoT solutions.

## Getting Started

1. **Download the Source Files**  
   Visit the Cadlab project page and download the files:  
   https://cadlab.io/project/28685/main/files

2. **Explore the Hardware Design**  
   - Open `hardware/esp32_c3_tp4056.kicad_pcb` in KiCad.

3. **Build & Flash Firmware**  
   ```bash
   cd firmware
   idf.py set-target esp32c3
   idf.py build flash monitor
   ```

4. **Power Up**  
   - Connect USB‑C to a 5 V supply or attach a Li‑ion battery.  
   - Observe LEDs: red (charging), green (charged).

## Repository Structure

```
├── hardware/            # KiCad schematics & PCB files
├── firmware/            # ESP‑IDF project sources
├── docs/                # Datasheets, assembly guides, test reports
└── LICENSE              # MIT License
```
