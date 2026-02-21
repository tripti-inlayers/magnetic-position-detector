# 🧲 Magnetic Position Detector

**Non-Contact Magnetic Sensing via Hall Effect and Window Comparator**

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Hardware](https://img.shields.io/badge/Hardware-Analog-orange.svg)]()
[![PCB](https://img.shields.io/badge/PCB-Single--Layer-green.svg)]()

A pure analog circuit that emulates the laptop lid detection mechanism. Utilizes a Hall Effect sensor and op-amp-based window comparator to detect magnetic proximity with two-level LED indication—without any microcontroller.

![PCB Photo](hardware/photos/assembled.jpg)

---

## Overview

When you close a modern laptop, a hidden magnet in the lid activates a Hall Effect sensor near the base, putting the system to sleep. This project replicates that core concept using only analog components and discrete circuit design.

**Highlights:**
- No-contact magnetic detection—no moving parts
- Dual-threshold voltage detection (via window comparator)
- Two LED indicators:
  - Yellow: Field approaching
  - Red: Target position reached
- Fully analog: no microcontroller or firmware
- Hand-fabricated single-layer PCB

---

## Circuit Principle

The analog voltage output from the SS49E Hall Effect sensor varies with magnetic field intensity. Two op-amp comparators are used to define a voltage window, such that:
- **Yellow LED** lights up when field is approaching
- **Red LED** confirms that magnetic proximity has crossed the final threshold

A buffer stage is inserted between the Hall sensor and the comparator inputs to ensure accurate, low-impedance signal propagation.

---

## Block Diagram

```
┌─────────────┐    ┌──────────────┐    ┌────────────┐    ┌────────────────────┐    ┌─────────────┐
│ Power       │───>│ 7805 Regulator│───>│ SS49E Sensor│───>│ Comparator Logic   │───>│ LED Outputs │
│ Supply      │    │     (5V)     │    │ + Buffer    │    │ (LM358N × 2)       │    │ Yellow/Red  │
└─────────────┘    └──────────────┘    └────────────┘    └────────────────────┘    └─────────────┘
```

---

## Circuit Design

The circuit is built around these key stages:

1. **Power Regulation**:
   - A 7805 voltage regulator delivers a stable 5V supply.

2. **Sensor Stage**:
   - The SS49E Hall Effect sensor outputs ~2.5V at rest.
   - This signal is buffered using one op-amp in voltage follower mode.

3. **Window Comparator**:
   - The buffered voltage is compared against two thresholds (V_L and V_H)
   - One comparator activates the Yellow LED (V > V_L)
   - Another comparator activates the Red LED (V within V_L and V_H range)

4. **Output Indicators**:
   - Two LEDs provide clear, real-time status of magnetic proximity.

---

## PCB Implementation

The circuit is fabricated on a single-layer PCB using the toner-transfer method:

- Designed in EasyEDA
- Layout printed on glossy sheet and transferred with heat
- Etched using ferric chloride (FeCl₃)
- Holes drilled manually and components soldered

Minimum trace width and clearance: 0.254 mm

---

## Performance Summary

- **Detection Range:** 1–4 cm (neodymium magnet)
- **Response Time:** < 10 ms
- **Power Consumption:** ~25 mA at 5V
- **Reliability:** Successfully tested for approach, detection, and withdrawal scenarios

---

## Applications

- **Laptop lid proximity detection** (replicated from real use-case)
- **Industrial sensing:** piston/cylinder position tracking
- **Consumer devices:** flip cover detection in phones/tablets
- **Automotive sensing:** seat-belt latching, gear position monitoring

---

## Future Scope

- Integrate hysteresis for flicker-free switching
- Expand to multi-axis detection with orthogonal Hall sensors
- Add ESP32 for Wi-Fi-based position reporting
- Replace LEDs with control output (e.g., MOSFET driving relay)
- Display magnetic field strength on OLED module

---

## Authors

**Tripti Patel** (24116102)  
**Sai Santosh** (24116104)  
B.Tech in Electronics & Communication Engineering  
National Institute of Technology, Raipur (2025–26)

**Supervisor:** Dr. Mayur Katwe

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for full terms.

- Dr. Mayur Katwe (Project Supervisor), NIT Raipur
- Department of Electronics & Communication Engineering, NIT Raipur
