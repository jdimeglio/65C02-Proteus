```
  ████████  ██████████  ████████   ████████ 
 ███░░░░███░███░░░░░░█ ███░░░░███ ███░░░░███
░███   ░░░ ░███     ░ ░░░    ░███░░░    ░███
░█████████ ░█████████    ███████    ███████ 
░███░░░░███░░░░░░░░███  ███░░░░    ███░░░░  
░███   ░███ ███   ░███ ███      █ ███      █
░░████████ ░░████████ ░██████████░██████████
 ░░░░░░░░   ░░░░░░░░  ░░░░░░░░░░ ░░░░░░░░░░ 
               
                                 
```
# MOS 6522 VIA - Proteus VSM Model

![Version](https://img.shields.io/badge/version-v1.0-blue) ![Proteus](https://img.shields.io/badge/Proteus-8.x-orange) ![C++](https://img.shields.io/badge/Language-C++-green)

## Overview
This is my cycle-accurate **MOS 6522 Versatile Interface Adapter (VIA)** model for the Proteus Design Suite. 

The standard Proteus library lacks comprehensive support for many 6502-family peripheral chips. This bridges that gap, allowing me to test 6502 assembly code, hardware interrupts, and parallel I/O routines within the Proteus simulation environment before deploying to real physical hardware. 

This model was specifically developed to my **SYSMON65** SBC (Single Board Computer) project.

## Key Technical Features
- **Cycle-Accurate Timing:** Evaluates internal timers and register latches strictly on the falling edge of the `PHI2` system clock.
- **Bi-Directional Parallel Ports:** Fully supports `DDRA` and `DDRB` (Data Direction Registers) for precise pin-by-pin input/output control on Port A and Port B.
- **Hardware Timers:** Supports 16-bit decrementing timers (T1 and T2). Timer 1 fully implements both **One-Shot** and **Free-Running (Continuous)** modes via the Auxiliary Control Register (ACR).
- **Interrupt Handling:** Complete implementation of the Interrupt Flag Register (IFR) and Interrupt Enable Register (IER), properly pulling the `$IRQ$` pin low when enabled conditions are met.

## Register Memory Map
The 6522 occupies 16 bytes of memory. The specific base address depends on the hardware decoding (e.g., `$A600`, `$AE00`).

| Offset | Register | Description |
| :--- | :--- | :--- |
| `+ $00` | **ORB/IRB** | Port B Output/Input Register |
| `+ $01` | **ORA/IRA** | Port A Output/Input Register |
| `+ $02` | **DDRB** | Data Direction Register B |
| `+ $03` | **DDRA** | Data Direction Register A |
| `+ $04` | **T1C-L** | Timer 1 Counter (Low) / Latch Write |
| `+ $05` | **T1C-H** | Timer 1 Counter (High) |
| `+ $06` | **T1L-L** | Timer 1 Latch (Low) |
| `+ $07` | **T1L-H** | Timer 1 Latch (High) |
| `+ $08` | **T2C-L** | Timer 2 Counter (Low) / Latch Write |
| `+ $09` | **T2C-H** | Timer 2 Counter (High) |
| `+ $0A` | **SR** | Shift Register *(See Roadmap)* |
| `+ $0B` | **ACR** | Auxiliary Control Register |
| `+ $0C` | **PCR** | Peripheral Control Register *(See Roadmap)* |
| `+ $0D` | **IFR** | Interrupt Flag Register |
| `+ $0E` | **IER** | Interrupt Enable Register |
| `+ $0F` | **ORA/IRA** | Same as `$01` without handshake clearing |

## Installation & Setup
Ive copied all my MOS family custom components into a single `MOS.LIB`.

1. Copy the contents of this repository's `MODELS` folder (e.g., `6522.dll`) to your Proteus `MODELS` directory.
2. Copy the contents of this repository's `LIBRARY` folder (`MOS.LIB`) to your Proteus `LIBRARY` directory.
3. Restart Proteus; so it will automatically generate the required `.IDX` file for the new library.
4. Search the Component Picker (P) for `6522` and look for the component associated with the `MOS` library.

## Hardware Wiring Notes
For the model to successfully read and write to your virtual CPU bus, ensure the following pins are routed correctly:
* **PHI2:** Must be connected to your system's clock oscillator.
* **RW:** Must be tied to the 6502 R/W line.
* **Chip Selects:** The model evaluates selection as: `CS1 == HIGH` AND `$CS2$ == LOW`. Ensure your address decoding logic (e.g., 74LS138) matches these polarities.
* **RS0 - RS3:** Must be connected to the CPU's Address lines `A0` through `A3`.

## Roadmap
- [x] Port A / Port B Parallel I/O
- [x] Timer 1 & Timer 2 Execution
- [x] IFR/IER Interrupt Generation
- [ ] Shift Register (SR) Implementation
- [ ] CB1/CB2 and CA1/CA2 Edge-Triggered Handshaking (PCR)

---

Version: 1.0  
Date: April 2026

**Author:** Joe DiMeglio  
**Project Home:** [https://github.com/jdimeglio/MOS-6522-Proteus](https://github.com/jdimeglio/MOS-6522-Proteus) 
