```
 █████   █████  ████████   ████████   ████████   ████████  
░░███   ░░███  ███░░░░███ ███░░░░███ ███░░░░███ ███░░░░███ 
 ░███    ░███ ░███   ░███░███   ░███░░░    ░███░███   ░███ 
 ░███    ░███ ░░█████████░░█████████   ██████░ ░░████████  
 ░░███   ███   ░░░░░░░███ ░░░░░░░███  ░░░░░░███ ███░░░░███ 
  ░░░█████░    ███   ░███ ███   ░███ ███   ░███░███   ░███ 
    ░░███     ░░████████ ░░████████ ░░████████ ░░████████  
     ░░░       ░░░░░░░░   ░░░░░░░░   ░░░░░░░░   ░░░░░░░░   
                                                           
                        
```
# Yamaha V9938 VDP - Proteus VSM Model

![Version](https://img.shields.io/badge/version-v1.1-blue) ![Proteus](https://img.shields.io/badge/Proteus-8.x-orange) ![C++](https://img.shields.io/badge/Language-C++-green)

## Overview
This repository contains a custom **Yamaha V9938 Video Display Processor (VDP)** model for the Proteus Design Suite. 

The V9938 is a retro graphics chip (famous for powering the MSX2 computer standard). Because standard Proteus lacks complex bitmapped video generators, this VSM DLL allows you to integrate a fully functioning VDP into your 6502/Z80 SBC simulations. It provides real-time visual output via a native Windows popup, allowing you to test video initialization routines, palette loading, and hardware-accelerated drawing commands directly in Proteus. ie: connect it to my RGB Screen model.

This model was specifically used to add video to the **SYSMON65** SBC project.

## Current Capabilities & Supported Modes
The V9938 is a complex chip.


### ✅ Currently Implemented:
* **Text 1 (T1):** 40 x 24 character legacy text mode (2 colors). 
  * *Note on T1:* Uses a 6x8 pixel character matrix. Custom fonts must be left-aligned, as the VDP hardware strictly renders bits 7 through 2 and ignores bits 1 and 0.


## Core Functions
- **Proteus Graphics Window:** outputs to RGB, where my RGB Display (RGBOUT) model will display it to screen .
- **VRAM Addressing:** Supports up to 128KB of dedicated VRAM, managed completely by the VDP. 
- **Custom Palette Engine:** Full support for the V9938's 9-bit RGB palette (512 possible colors).
- **Hardware Command Engine (Blitter):** Emulates hardware-accelerated VRAM-to-VRAM copy commands, line drawing, and logical operations (AND, OR, XOR).

## Known Limitations & Quirks
Because this is a simulation running inside a larger EDA tool, there are specific limitations to be aware of:

1. **Simulation Speed vs. Real-Time:** The simulation will scale its speed down to maintain cycle accuracy. Your code will execute perfectly, but run in "slow motion."
2. **Window Redraw Decoupling:** To prevent Proteus from freezing, the Windows popup does not redraw on every single pixel change. The screen buffer is updated silently and pushed to the UI during the simulated Vertical Blanking (VBLANK) interval.
3. **Text Mode 1 Font Quirk:** When using the legacy 40-column Text Mode 1, characters are 6x8 pixels, not 8x8. The VDP hardware strictly renders bits 7 through 2 of your pattern data and **ignores bits 1 and 0**. Custom fonts must be shifted to the left to display correctly without clipping.

## Installation & Setup
To make installation as simple as possible, this project is structured to mirror your Proteus installation folders.

1. Copy the contents of this repository's `MODELS` folder (e.g., `V9938.dll`) to your Proteus `MODELS` directory.
2. Copy the contents of this repository's `LIBRARY` folder (e.g., `MOS.LIB` ) to your Proteus `LIBRARY` directory.
3. Restart Proteus; it will automatically generate the required `.IDX` file.
4. Search the Component Picker (P) for `V9938`.

## Roadmap
- [x] Basic I/O Interface & Status Registers
- [x] TMS9918 Legacy Graphics & Text Modes
- [ ] V9938 G4-G7 256-Color Bitmap Modes
- [ ] Hardware Command Engine (BitBlt)
- [ ] Hardware Sprites (Mode 2)

---
Version: 1.1  
Date: April 2026

**Author:** Joe DiMeglio  
**Project Home:** [jdimeglio/65C02-Proteus](https://github.com/jdimeglio/65C02-Proteus/tree/main/9938%20VDP%20Proteus%20Model)  
*(Located in the `V9938 Proteus Model` directory)*

