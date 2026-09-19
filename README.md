# Midi_Fighter_Spectra

An open-source firmware for the **DJ TechTools Midi Fighter Spectra**, ported and reverse-engineered from the open-source **Midi Fighter 64** firmware base (`wunnation/Midi_Fighter_64`).

---

## 1. Project Purpose

The original Midi Fighter 64 prototype was made by bridging four Midi Fighter Spectra boards into a single chassis. 
In [this](https://djtechtools.com/2015/04/21/shawn-wasabi-64-button-midi-fighter-special-edition/) blog post featuring Shawn Wasabi's Marble Soda hit, it's actually pretty fascinating and also [hilarious](https://s11234.pcdn.co/wp-content/uploads/2015/04/mf64-behind-the-dev.jpg.optimal.jpg).
As I searched through the (now obsolete) production MF64 firmware repository, it looks like it preserved much of the underlying Spectra firmware architecture.
The LUFA USB stack, SysEx handling, debounce logic, and LED animation pipelines, which are all scaled up to an 8×8 grid for the 64.

Now, my goal of this project is to:
* **Downscale the MF64 firmware** back to native Spectra hardware specifications (4×4 grid and 3 + 3 bank side buttons).
* **Preserve compatibility** with the official DJ TechTools Midi Fighter Utility via authentic USB Descriptors and SysEx handshakes.
* **Re-implement the Spectra's Lights** because... well.. I don't know. I just wanna..? ¯\_(ツ)_/¯
* **Create a base for Spectra Custom Firmwares** if someone (or, me. I guess) wants to add [Apollo Studio](https://github.com/mat1jaczyyy/apollo-studio) support to a 4×4, lmao.
* **Also make this an academic research** cause I'm learning Embedded systems in my 3rd year classes (at the time of writing) so i can gaslight myself into giving myself an edge over everyone else in my class xdxdxd
 
---

## 2. A small research of the Midi Fighter Spectra's Hardware Specifications

So, it appears that the Midi Fighter Spectra was just a stripped down Midi Fighter 3D, which explains the silent discontinuation of the Midi Fighter 3D. (sad, wish I can get my hands on one :/)
Doesn't say a lot given that the 3D is cheaper than the Spectra, despite losing the gyro and extra 4 bank buttons. 
So, the MCU *may* just be a lot more capable, because it's basically a diet 3D.

| Component | Specification | Notes |
| :--- | :--- | :--- |
| **Microcontroller** | **Atmel / Microchip ATmega32U4** | 8-bit AVR RISC @ 16 MHz, 5V logic |
| **Flash Memory** | 32 KB | ~4 KB reserved for DFU bootloader; **~28 KB available for use** |
| **SRAM** | 2.5 KB (2560 bytes) | The budget for buffers, stack, and dynamic allocations |
| **EEPROM** | 1 KB (1024 bytes) | Stores 4 banks and probably more stuff |
| **Input Scanning** | **MC74HC165A** PISO Shift Registers | Serialized parallel inputs for the 16 Sanwa buttons + 6 side switches |
| **LED Drivers** | **TI TLC5946 / TLC59461** | Multichannel 16-channel constant-current sink PWM drivers daisy-chained |
| **Grid Layout** | 16 Sanwa OBSF-24 arcade switches | Arranged in a 4×4 physical matrix with individual RGB ring backlights |
| **Auxiliary I/O** | 6 tactile side push-buttons | Dedicated to bank switching, utility toggles, and SysEx triggers |
| **Bootloader** | Atmel DFU Bootloader (FLIP compatible) | USB VID: `0x03EB` / PID: `0x2FF4` |

---

> [!CAUTION]
> This project is AI-assisted, but mostly in the searching and a bit of the writing aspect. Everything else may be amateurishly done by a clueless guy.

> [!NOTE]
> If you'd like to contribute, let me know through Discord! (@conc1erge)
