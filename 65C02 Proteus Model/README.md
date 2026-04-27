```
  ████████  ██████████   █████████     █████     ████████      █████████  ███████████    █████████ 
 ███░░░░███░███░░░░░░█  ███░░░░░███  ███░░░███  ███░░░░███    ███░░░░░███░░███░░░░░███  ███░░░░░███
░███   ░░░ ░███     ░  ███     ░░░  ███   ░░███░░░    ░███   ░███    ░░░  ░███    ░███ ███     ░░░ 
░█████████ ░█████████ ░███         ░███    ░███   ███████    ░░█████████  ░██████████ ░███         
░███░░░░███░░░░░░░░███░███         ░███    ░███  ███░░░░      ░░░░░░░░███ ░███░░░░░███░███         
░███   ░███ ███   ░███░░███     ███░░███   ███  ███      █    ███    ░███ ░███    ░███░░███     ███
░░████████ ░░████████  ░░█████████  ░░░█████░  ░██████████   ░░█████████  ███████████  ░░█████████ 
 ░░░░░░░░   ░░░░░░░░    ░░░░░░░░░     ░░░░░░   ░░░░░░░░░░     ░░░░░░░░░  ░░░░░░░░░░░    ░░░░░░░░░  
                                                                                                   
```

## Hardware Specifications

### Core System
* **CPU:** WDC 65C02 Microprocessor
* **Clock Speed:** 1.9216 MHz Oscillator
* **ROM:** 8KB EPROM (`2764`)
* **RAM:** 1MB Static RAM (`IS62C10248AL`) accessed via a banked memory window.

### Memory Map (Work in Progress)
Address decoding is driven by a primary `74LS138` dividing the 64KB space into 8KB blocks, with a secondary `74LS138` sub-decoding the I/O block.

*Note: The current memory map is a simulation prototype. The lower RAM banking is temporary to test Proteus logic and will be moved to a higher expansion space to protect the Zero Page and Stack in the final revision.*

* **`$0000 - $1FFF`**: Base System RAM Window (8KB) *(Currently Banked - See WIP Note)*
* **`$2000 - $9FFF`**: Reserved / Expansion Space
* **`$A000 - $BFFF`**: I/O Peripheral Space
* **`$C000 - $DFFF`**: Reserved / Expansion Space
* **`$E000 - $FFFF`**: System ROM (8KB)

### I/O & Peripherals (Custom Proteus Models)
* **`$A800` - MOS 6522 VIA:** Fully working custom Proteus model. Provides Versatile Interface Adapter capabilities for parallel I/O and hardware timers.
* **`$A900` - Yamaha V9938 VDP:** Custom Proteus model equipped with 64KB of dedicated Video RAM (`62256` SRAMs).
* **`$AF00` - MC6850 ACIA:** Asynchronous Communications Interface Adapter configured for 57,600 Baud serial communication.

### Custom Architectural Features

#### 1. Custom RGB Video & ANSI Serial Pipeline
* **RGB Monitor (`RGBOUT`):** The V9938 outputs raw `PCLK`, `SYNC_H`, `SYNC_V`, and discrete RGB signals directly into a custom-built C++ Proteus monitor model. This provides a real-time, hardware-scaled (2x2) CRT simulation natively within a Windows popup.
* **ANSI Serial Terminal (`ANSI_TERM`):** The MC6850 ACIA interfaces directly with a custom C++ terminal model, enabling full ANSI escape sequence rendering (colored text, cursor control, screen clearing) over the simulated serial line.

#### 2. "NOP" Instruction Bank Switching (Work in Progress)

To break the 64KB address limit of the 65C02, the SYSMON65 uses a completely custom hardware trapping mechanism. 
By monitoring the CPU's `SYNC` pin and Data Bus, the circuit intercepts specific `NOP` opcodes to trigger a `74LS377` latch. 

This latch captures the upper nibble (`ED4 - ED7`) of the executing NOP instruction and uses it to drive the extended address lines (`MA16 - MA19`) of the 1MB RAM. 
This allows the software to seamlessly swap the active RAM window on the fly using a single 1-cycle instruction.

** Architecture Note:** *The current implementation banks the `$0000 - $1FFF` region purely to validate the Proteus simulation and latch logic. 
The final hardware revision will lock the lower RAM as fixed and move this banked window to a higher, safer memory expansion area.*

| Executed Opcode | Latch Value (`MA19-16`) | Active RAM Bank | Physical 1MB RAM Range Accessed |
| :---: | :---: | :---: | :--- |
| **`$02`** | `%0000` | Bank 0 | `0x00000 - 0x01FFF` |
| **`$12`** | `%0001` | Bank 1 | `0x10000 - 0x11FFF` |
| **`$22`** | `%0010` | Bank 2 | `0x20000 - 0x21FFF` |
| **`$32`** | `%0011` | Bank 3 | `0x30000 - 0x31FFF` |
| **`$42`** | `%0100` | Bank 4 | `0x40000 - 0x41FFF` |
| **`$52`** | `%0101` | Bank 5 | `0x50000 - 0x51FFF` |
| **`$62`** | `%0110` | Bank 6 | `0x60000 - 0x61FFF` |
| **`$72`** | `%0111` | Bank 7 | `0x70000 - 0x71FFF` |
| **`$82`** | `%1000` | Bank 8 | `0x80000 - 0x81FFF` |
| **`$92`** | `%1001` | Bank 9 | `0x90000 - 0x91FFF` |
| **`$A2`** | `%1010` | Bank 10 | `0xA0000 - 0xA1FFF` |
| **`$B2`** | `%1011` | Bank 11 | `0xB0000 - 0xB1FFF` |
| **`$C2`** | `%1100` | Bank 12 | `0xC0000 - 0xC1FFF` |
| **`$D2`** | `%1101` | Bank 13 | `0xD0000 - 0xD1FFF` |
| **`$E2`** | `%1110` | Bank 14 | `0xE0000 - 0xE1FFF` |
| **`$F2`** | `%1111` | Bank 15 | `0xF0000 - 0xF1FFF` |

*(Note: Software execution simply calls the corresponding NOP instruction just before reading/writing to the banked window).*

Version: 1.0  
Date: April 2026

**Author:** Joe DiMeglio  
**Project Home:** [https://github.com/jdimeglio/MOS-6522-Proteus](https://github.com/jdimeglio/65C02-Proteus/tree/main/65C02%20Proteus%20Model) 