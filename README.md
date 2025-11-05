# PixelPlay

A custom-built, Pi Zero 2 W handheld for retro gaming and high-fidelity audio, wrapped in a 3D-printed, iPod-inspired case.


---

## About This Project

This project is a complete all-in-one handheld which supports both basic emulation and music playing. The goal was to create something that was a crossover between the Gameboy and the IPod, with a Raspberry Pi Zero 2 W at its core.

It's designed to be an "iPod" for lossless music, and a "Game Boy" for everything up to the GBA/SNES era. This required building a fully custom PCB to integrate a DIY power management circuit, an I2S audio system, trigger buttons, and a decent SPI TFT display.

---

##  Key Features

* **Core Logic:** Raspberry Pi Zero 2 W (Quad-Core ARM Cortex-A53).
* **Operating System:** RetroPie, providing a seamless front-end for emulation and media.
* **Display:** 3.5" TFT Display connected via the high-speed SPI interface GBA/SNES games.
* **High-Fidelity Audio:** A 4-wire I2S audio system based on the CS4344 DAC for clear, lossless music playback.
* **Dual Audio Output:** Features both a 3.5mm headphone jack and an internal speaker, with a 5-pin switching jack to automatically mute the speaker when headphones are inserted.
* **Integrated Power:** A fully custom, component-level power management system:
    * **TP4056** for USB-C LiPo charging.
    * **DW01A + FS8205A** for battery protection (over-discharge, short-circuit).
    * **MT3608** boost converter to generate a stable 5V rail.
* **Full Controls:** Includes D-Pad, A/B/X/Y, Start/Select, and two shoulder (trigger) buttons.
* **Custom Case:** A bespoke, 3D-printed enclosure designed in blender.

---

## Hardware: The "Motherboard"

The core of this project is a single, custom-designed PCB that serves as a motherboard for all components.

### 1. Core Logic & Display
The Pi Zero 2 W is connected to the display hat, with wiring from the remaining GPIO pins to respective components.

### 2. Custom Power Circuit
Instead of an expensive module, the power system is built from individual ICs on the PCB.
* **Path:** USB-C 5V $\rightarrow$ **TP4056** $\rightarrow$ Charges LiPo Battery.
* The LiPo Battery is protected by the **DW01A/FS8205A** circuit.
* The protected battery voltage feeds the **MT3608** boost converter.
* The **MT3608** outputs a stable 5V, which powers the Raspberry Pi.

### 3. Audio System
This is a high-fidelity 4-wire audio chain.
* **DAC:** The Pi's I2S pins are routed to the **CJMCU-4344 (CS4344)** module.
* **Output:** The CS4344 outputs a clean, line-level analog signal.
* **Switching:** This signal goes to the `TIP` and `RING` inputs of the switching headphone jack.
* **Amplifier:** The jack's `TIP-SWITCH` and `RING-SWITCH` pins are wired to the input of a **PAM8403** stereo amplifier.
* **Result:** When headphones are unplugged, the signal is routed to the amplifier, which drives the speaker. When headphones are plugged in, the connection to the amp is broken, and the speaker becomes silent.

### 4. Controls
* All 12 buttons are wired to a GPIO pin on one side and a common ground on the other. This is facilitated by the dpad and start-select buttons which have conductive strips on the bottom, which press on to a perfboard, completing a circuit to ground. The res of the buttons will be external hardware. The Pi's internal pull-up resistors are used.

---

