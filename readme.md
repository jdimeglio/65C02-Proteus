```
  █████████  ███████████ █████  █████ ███████████ ███████████
 ███░░░░░███░█░░░███░░░█░░███  ░░███ ░░███░░░░░░█░░███░░░░░░█
░███    ░░░ ░   ░███  ░  ░███   ░███  ░███   █ ░  ░███   █ ░ 
░░█████████     ░███     ░███   ░███  ░███████    ░███████   
 ░░░░░░░░███    ░███     ░███   ░███  ░███░░░█    ░███░░░█   
 ███    ░███    ░███     ░███   ░███  ░███  ░     ░███  ░    
░░█████████     █████    ░░████████   █████       █████      
 ░░░░░░░░░     ░░░░░      ░░░░░░░░   ░░░░░       ░░░░░       
```
# SYSMON65 - Custom Proteus VSM Models

![Proteus](https://img.shields.io/badge/Proteus-8.x-orange) ![C++](https://img.shields.io/badge/Language-C++-green) ![Assembly](https://img.shields.io/badge/Language-6502_Assembly-red)

## Overview
Welcome to the master repository for my **SYSMON65 Single Board Computer** simulation components. 

This repository contains a collection of custom-built Virtual System Modelling (VSM) components for the Proteus Design Suite. 
Because standard Proteus lacks complex models for certain retro hardware, I developed some of these C++ DLLs from scratch to accurately simulate the peripherals, video pipelines, and interfaces required for a complete 65C02-based architecture.

##  Repository Structure

Each folder in this repository contains the C++ source code, compiled `.dll` models, and testing environments for a specific hardware component:

| Directory | Description |
| :--- | :--- |
| **`6522 Proteus Model`** | **MOS 6522 VIA:** My Custom C++ model of the Versatile Interface Adapter. Handles parallel I/O and hardware timers essential for the 6502 ecosystem. |
| **`65C02 Proteus Model`** | **CPU & Architecture Support:** Contains files, memory decoding logic, and simulation support structures for the core WDC 65C02 processor architecture. |
| **`9938 VDP Proteus Model`** | **Yamaha V9938 VDP:** Custom model of the MSX2 Video Display Processor. Handles dedicated VRAM, hardware graphics rendering, and outputs discrete video timing signals. |
| **`Protues LIB`** | **Schematic Symbols:** Contains the Proteus `.LIB` files. These are the visual schematic symbols and pin definitions that link to the compiled C++ DLLs. *(Note: Matches Proteus directory structure).* |
| **`RGB Screen`** | **RGBOUT Monitor Simulator:** My custom VSM display module that acts as a physical CRT. It reads raw `PCLK`, `HSYNC`, `VSYNC`, and 3-bit Digital RGB signals from the VDP and paints them to a Windows UI popup with 2x2 integer scaling. |
| **`i8255 Proteus Model`** | **Intel 8255 PPI:** Custom C++ model of the Programmable Peripheral Interface, providing additional parallel I/O expansion capabilities. |

## ⚙️ Global Installation Instructions

To use these components in your own Proteus workspace, you need to copy the compiled models and libraries into your local Proteus installation directories:

1. Download or clone this repository.
2. Navigate to your local Proteus installation (typically `C:\Program Files (x86)\Labcenter Electronics\Proteus 8 Professional\`).
3. Copy the `.LIB` files from the **`Protues LIB`** folder into your Proteus **`LIBRARY`** folder.
4. Copy the compiled `.dll` and `.bin` files from the individual model folders into your Proteus **`MODELS`** folder.
5. Restart Proteus. The new components will now be fully indexed and available in your Component Picker (P).

---
**Author:** Joe DiMeglio  
**Project Home:** [jdimeglio/65C02-Proteus](https://github.com/jdimeglio/65C02-Proteus)