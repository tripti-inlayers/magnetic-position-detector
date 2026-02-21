# Magnetic Position Detector

**Non-Contact Position Sensing via Hall Effect & Op-Amp Window Comparator**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)  
[![Hardware](https://img.shields.io/badge/Hardware-Analog-orange.svg)]()  
[![PCB](https://img.shields.io/badge/PCB-Single--Layer-green.svg)]()

> A pure analog circuit that replicates the laptop lid detection mechanism. Uses a Hall Effect sensor and window comparator (no microcontroller) to detect magnet proximity with two-state LED feedback.

![PCB Photo](hardware/photos/assembled.jpg)

---

## Overview

Every time you close your laptop and it enters sleep mode, a small magnet in the display bezel triggers a Hall Effect sensor on the base. This project builds that exact mechanism from scratch using analog circuitry.

**Key Features:**
- Non-contact magnetic sensing (zero mechanical wear)
- Window comparator with dual thresholds (V<sub>L</sub> and V<sub>H</sub>)
- Two-state LED indication:
  - Yellow: Magnetic field detected (approaching)
  - Red: Position confirmed (target distance reached)
- Pure analog design (no microcontroller/firmware)
- Hand-fabricated PCB using toner-transfer etching

---

## How It Works

### Detection Principle

The **SS49E Hall Effect sensor** outputs an analog voltage (0–5V) proportional to magnetic flux density. At rest (no magnet), output is ~2.5V.

The **Window Comparator** (built with two LM358N op-amps) defines two thresholds:
- **V<sub>L</sub>** (lower threshold) ≈ 1.67V
- **V<sub>H</sub>** (upper threshold) ≈ 3.33V

### State Table

| Magnet Distance   | V<sub>sensor</sub> | Yellow LED | Red LED | Status              |
|------------------|--------------------|------------|---------|---------------------|
| Far / Absent     | ~2.5V              | OFF        | OFF     | Idle                |
| Approaching      | > V<sub>L</sub>    | ON         | OFF     | Field Detected      |
| Target Distance  | V<sub>L</sub> < V < V<sub>H</sub> | ON         | ON      | Position Confirmed  |

### Block Diagram

```
┌─────────────┐    ┌──────────────┐    ┌────────────────────┐    ┌─────────────┐
│ Power       │───>│ SS49E Hall   │───>│ Window Comparator  │───>│ LED Output  │
│ Supply      │    │ Sensor (U2)  │    │ U5.1 + U6.1/U6.2   │    │ Yellow/Red  │
│ 7805A → 5V  │    │ Analog Out   │    │ (LM358N × 2)       │    │ Indicators  │
└─────────────┘    └──────────────┘    └────────────────────┘    └─────────────┘
```

---

## Circuit Design

### Components

| Component     | Part #   | Qty | Function                       |
|--------------|----------|-----|--------------------------------|
| Hall Sensor   | SS49E    | 1   | Magnetic flux → voltage       |
| Op-Amp        | LM358N   | 2   | Window comparator (U5, U6)    |
| Regulator     | 7805A    | 1   | Stable 5V supply              |
| Resistors     | 1kΩ, 10kΩ | 6   | Current limiting, thresholds  |
| Capacitors    | 100nF    | 3   | Decoupling                    |
| LEDs          | KT-0603R | 3   | Status indicators             |

### Circuit Stages

1. **Power Supply (U1, C1, C2)**
   - 7805A regulates input to stable 5V
   - C1/C2 decouple input/output

2. **Hall Sensor (U2, C3)**
   - SS49E outputs analog voltage proportional to magnetic field
   - C3 decouples sensor supply

3. **Window Comparator (U5.1, U6.1, U6.2)**
   - U5.1: Buffer stage (voltage follower)
   - U6.2: Lower comparator → Yellow LED when V<sub>sensor</sub> > V<sub>L</sub>
   - U6.1: Upper comparator → Red LED when V<sub>L</sub> < V<sub>sensor</sub> < V<sub>H</sub>

4. **LED Output**
   - R1, R3, R4 (1kΩ) limit LED current to ~3mA
   - Direct drive from comparator outputs

---

## Hardware Implementation

### PCB Fabrication (Toner-Transfer Method)

1. Design: PCB layout in EasyEDA
2. Print: Mirrored layout on glossy paper (laser printer)
3. Transfer: Iron toner onto copper-clad board
4. Etch: Immerse in FeCl₃ solution
5. Clean: Remove toner with acetone
6. Drill: 1mm holes for components
7. Solder: Assemble and test continuity

Result: Single-layer PCB with 0.254mm trace width/clearance

---

## Applications

- Laptop lid closure detection
- Industrial non-contact sensing
- Seatbelt buckle detection
- Flip cover sensing for phones/tablets

---

## Test Results

| Test Case             | Expected Behavior         | Result    |
|----------------------|---------------------------|-----------|
| Power on, no magnet  | Green LED only            | ✅ PASS   |
| Magnet approaching   | Yellow LED activates      | ✅ PASS   |
| Magnet at target     | Red LED activates         | ✅ PASS   |
| Magnet withdrawn     | LEDs deactivate in reverse| ✅ PASS   |

**Performance:**
- Detection range: 1–4 cm (neodymium magnet)
- Response time: <10 ms
- Power consumption: ~25 mA @ 5V

---

## Repository Structure

```
├── README.md
├── LICENSE
├── hardware/
│   ├── schematic.png
│   ├── pcb-layout.png
│   ├── BOM.csv
│   └── photos/
│       ├── pcb-etched.jpg
│       └── assembled.jpg
└── docs/
    └── project-report.pdf
```

---

## Future Enhancements

- [ ] Add hysteresis (Schmitt trigger) to prevent LED flicker
- [ ] Integrate ESP32 for IoT monitoring
- [ ] Replace LED output with MOSFET for actuator control
- [ ] Multi-axis tracking with 3D sensor array
- [ ] OLED display for real-time distance

---

## Authors

**Tripti Patel** (24116102) & **Sai Santosh** (24116104)  
B.Tech — Electronics & Communication Engineering  
National Institute of Technology, Raipur  
Session: 2025–26

**Supervisor:** Dr. Mayur Katwe

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- Dr. Mayur Katwe (Project Supervisor), NIT Raipur
- Department of Electronics & Communication Engineering, NIT Raipur
