```
███████████     █████████  ███████████     ███████    █████  █████ ███████████
░░███░░░░░███   ███░░░░░███░░███░░░░░███  ███░░░░░███ ░░███  ░░███ ░█░░░███░░░█
 ░███    ░███  ███     ░░░  ░███    ░███ ███     ░░███ ░███   ░███ ░   ░███  ░ 
 ░██████████  ░███          ░██████████ ░███      ░███ ░███   ░███     ░███    
 ░███░░░░░███ ░███    █████ ░███░░░░░███░███      ░███ ░███   ░███     ░███    
 ░███    ░███ ░░███  ░░███  ░███    ░███░░███     ███  ░███   ░███     ░███    
 █████   █████ ░░█████████  ███████████  ░░░███████░   ░░████████      █████   
░░░░░   ░░░░░   ░░░░░░░░░  ░░░░░░░░░░░     ░░░░░░░      ░░░░░░░░      ░░░░░    
```

# RGB Monitor Simulator (RGBOUT) - Proteus VSM Model

![Version](https://img.shields.io/badge/version-v1.1-blue) ![Proteus](https://img.shields.io/badge/Proteus-8.x-orange) ![C++](https://img.shields.io/badge/Language-C++-green)

## Overview
This is my custom **RGB Monitor Simulator (RGBOUT)**.  

Unlike memory-mapped video chips, this VSM DLL acts like a true physical CRT/LCD display. It does not read from a CPU address bus. It listens to raw hardware display timing signals (Pixel Clock, H-Sync, V-Sync, and Video Data) and paints the resulting image line-by-line to a Windows popup screen. 

I created this model to test the interface of my 65C02 to the V9938 in my **SYSMON65** SBC. 

It could also be used for model testing custom FPGA VGA controllers, discrete logic video cards, or interfacing with complex Video Display Processors.

## Key Technical Features
- **Raw Signal Rendering:** Interprets discrete `HSYNC`, `VSYNC`, and `PCLK` (Pixel Clock) signals to position the virtual electron beam on the screen.
- **High-Speed Execution:** Decouples the Proteus event-driven electrical simulation from the Windows UI refresh rate (`BitBlt`), ensuring the popup window renders smoothly without freezing.
- **Hardware Integer Scaling (2x2):** Retro video chips often output low resolutions. This monitor automatically applies a 2x2 pixel multiplier on the fly, rendering a crisp, scaled image inside the 920x640 display window.
- **8-Color Digital RGB Palette:** Fully decodes discrete R, G, and B input pins to render the standard 8-color digital palette (Black, Blue, Green, Cyan, Red, Magenta, Yellow, White).

## Pinout & Interface
The monitor model exposes the following pins to the Proteus schematic:

| Pin Name | Direction | Description |
| :--- | :--- | :--- |
| `PCLK` | Input | **Pixel Clock:** Triggers the sampling of the video data pins. |
| `SYNC_H` | Input | **Horizontal Sync:** Active-low pulse. Returns the beam to the left edge and advances down the screen. |
| `SYNC_V` | Input | **Vertical Sync:** Active-low pulse. Returns the beam to the top-left corner (Frame Reset). |
| `R` | Input | **Video Data (Red):** The digital red pixel data. |
| `G` | Input | **Video Data (Green):** The digital green pixel data. |
| `B` | Input | **Video Data (Blue):** The digital blue pixel data. |

## Installation & Setup
To make installation as simple as possible, this project is structured to mirror your Proteus installation folders.

1. Copy the contents of this repository's `MODELS` folder (e.g., `RGBOUT.dll`) to your Proteus `MODELS` directory.
2. Copy the contents of this repository's `LIBRARY` folder (e.g., `MOS.LIB`) to your Proteus `LIBRARY` directory.
3. Restart Proteus; it will automatically generate the required `.IDX` file.
4. Search the Component Picker (P) for `RGBOUT` or `RGB Monitor`.

## 🛠 Roadmap
- [x] Discrete PCLK, HSYNC, and VSYNC Tracking
- [x] 2x2 Hardware Pixel Scaling
- [x] Windows GDI Popup Rendering
- [x] 8-Color (3-bit) Digital RGB Input
- [ ] Dynamic Window Resizing based on active H/V Sync timings

---
Version: 1.1  
Date: April 2026

**Author:** Joe DiMeglio  
**Project Home:** [jdimeglio/65C02-Proteus](https://github.com/jdimeglio/65C02-Proteus/tree/main/RGB%20Screen)  
*(Located in the `RGBOUT Proteus Model` directory)*