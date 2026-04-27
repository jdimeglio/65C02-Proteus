```

 █████       █████ ███████████ 
░░███       ░░███ ░░███░░░░░███
 ░███        ░███  ░███    ░███
 ░███        ░███  ░██████████ 
 ░███        ░███  ░███░░░░░███
 ░███      █ ░███  ░███    ░███
 ███████████ █████ ███████████ 
░░░░░░░░░░░ ░░░░░ ░░░░░░░░░░░  
```
# Custom Proteus Models & Libraries

![Proteus](https://img.shields.io/badge/Proteus-8.x-orange) ![C++](https://img.shields.io/badge/Language-C++-green) ![Assembly](https://img.shields.io/badge/Language-6502_Assembly-red)

## Overview
Welcome to the core simulation files for the **SYSMON65** Single Board Computer and related custom architectures. 

Because the standard Proteus Design Suite lacks comprehensive simulation models for several retro computer chips and specific video/serial outputs, this directory contains all the custom C++ Virtual System Modelling (VSM) DLLs, schematic libraries, and binary data ROMs required to accurately simulate the hardware.

## 📁 Directory Structure
This folder is structured to mirror the installation directory of Proteus.
* **`LIBRARY/`**: Contains the custom Proteus `.LIB` schematic files. These hold the visual symbols and pinouts you drop onto your circuit canvas.
* **`MODELS/`**: Contains the compiled C++ `.dll` simulation engines and `.bin` lookup ROMs that bring the schematic symbols to life.

## Contents of `MODELS/`

### VSM Simulation Engines (`.dll`)
These are the compiled C++ models that execute the hardware logic during a Proteus simulation:

| File Name | Description |
| :--- | :--- |
| **`6522.dll`** | MOS 6522 Versatile Interface Adapter (VIA). Cycle-accurate timers and parallel I/O. |
| **`ansiterm.dll`** | Custom Serial Terminal. Parses ANSI escape sequences for colored text and cursor control. |
| **`RGBdisplay.dll`** | RGB Monitor Simulator. Renders raw PCLK, HSYNC, VSYNC, and 3-bit RGB to a Windows popup. |
| **`v9938.dll`** | Yamaha V9938 Video Display Processor. Currently supports Text 1 and Graphic 1 MSX modes. |
| **`i8255.dll`** | Intel 8255 Programmable Peripheral Interface (PPI) model. |
| **`x80_v12.dll`** | Custom x80 architecture CPU / peripheral model module (v12). |


## Installation Instructions
To use these custom components in your own Proteus projects:

1. Close Proteus if it is currently open.
2. Open your Proteus installation directory (usually `C:\Program Files (x86)\Labcenter Electronics\Proteus 8 Professional\`).
3. Copy the contents of this repository's **`LIBRARY`** folder into the Proteus **`LIBRARY`** folder.
4. Copy the contents of this repository's **`MODELS`** folder into the Proteus **`MODELS`** folder.
5. Launch Proteus. It will automatically re-index the libraries and the new parts will be available in the Component Picker.

---
**Author:** Joe DiMeglio  
**Project Home:** [jdimeglio/65C02-Proteus](https://github.com/jdimeglio/65C02-Proteus/tree/main/Protues%20LIB)