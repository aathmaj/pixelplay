# PixelPlay



<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/2564dd21-5b35-4319-ba27-ffbea5cbc045" />



A custom-built, Pi Zero 2 W handheld for retro gaming and high-fidelity audio, wrapped in a 3D-printed, iPod-inspired case.


---

## About This Project

This project is a complete all-in-one handheld which supports both basic emulation and music playing. The goal was to create something that was a crossover between the Gameboy and the IPod, with a Raspberry Pi Zero 2 W at its core.

It's designed to be an iPod for  music, and a Game Boy for everything up to the GBA/SNES era. This required building a fully custom PCB to integrate a DIY power management circuit, an I2S audio system, trigger buttons, and a decent SPI TFT display.

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
* **Custom Case:** A 3D-printed enclosure designed in blender.

---

## Hardware: The Power Unit

### 1. Core Logic & Display
The Pi Zero 2 W is connected to the display hat, with wiring from the remaining GPIO pins to respective components.

### 2. Custom Power Circuit

<img width="650" height="693" alt="image" src="https://github.com/user-attachments/assets/2193fa74-bc84-4584-95e3-443974a0398e" />


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

## BILL OF MATERIALS

| Index | Manufacturer Part Number                                                  | Manufacturer Name            | Description                      | Quantity | Part No on Site | Unit Price | Extended Price | Link                                                                                                                                                                                                                                                                                                                            |
| ----- | ------------------------------------------------------------------------- | ---------------------------- | -------------------------------- | -------- | --------------- | ---------- | -------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1     | CC0805KKX7R9BB104                                                         | YAGEO                        | CAP CER 0.1UF 250V X7T 0805      | 20       | ST2009SD0086    | 0.023      | $0.45          | [https://sharvielectronics.com/product/0-1uf-100nf-50v-capacitor-%c2%b110-tolerance-cc0805kkx7r9bb104-0805-package/](https://sharvielectronics.com/product/0-1uf-100nf-50v-capacitor-%C2%B110-tolerance-cc0805kkx7r9bb104-0805-package/?srsltid=AfmBOor6LLanBp3zXGE_O8mvBjk9-ufMmL4uXnHnnq624E0vC2Y4Dr8r)                       |
| 2     | CC0805KKX7R8BB106                                                         | YAGEO                        | CAP CER 10UF 25V X5R 0805        | 10       | ST2103SD2184    | 0.034      | $0.34          | [https://sharvielectronics.com/product/10uf-1000nf-25v-capacitor-%c2%b110-tolerance-cc0805kkx7r8bb106-0805-package/](https://sharvielectronics.com/product/10uf-1000nf-25v-capacitor-%C2%B110-tolerance-cc0805kkx7r8bb106-0805-package/?srsltid=AfmBOormWLlRQmPmjs5Fe4hcOEBMGbJ9pqfq2EnwzaC-G6X3pZsj5WuK)                       |
| 3     | 1×11 2.54 Mm Berg Strip                                                   | \-                           | HEADER VERT 1x11 2.54MM          | 1        | ST2136CO8090    | 0.056      | $0.06          | [https://sharvielectronics.com/product/11x1-11pin-male-square-straight-header-connector-11mm-height-2-54mm-pitch/](https://sharvielectronics.com/product/11x1-11pin-male-square-straight-header-connector-11mm-height-2-54mm-pitch/?srsltid=AfmBOood8MSx0vBM3uYkJdJvMlbiKJxqf2YF5XsJdze-fSYdH8IDeWzC)                           |
| 4     | 19-21/BHC-AN1P2/3T                                                        | Everlight                    | LED BLUE CLEAR CHIP SMD          | 10       | ST2009SD0353    | 0.037      | $0.37          | [https://sharvielectronics.com/product/blue-smd-led-19-21-bhc-an1p2-3t-0603-package/](https://sharvielectronics.com/product/blue-smd-led-19-21-bhc-an1p2-3t-0603-package/?srsltid=AfmBOoq8bHmoVEml4Kt3gu-T9Yzwi65OMnhsxSrZsN8cbDNb4lSEQyPE)                                                                                     |
| 5     | RC0603FR-071KL                                                            | YAGEO                        | RES SMD 1K OHM 0.1% 1/10W 0603   | 10       | ST2103SD1187    | 0.015      | $0.15          | [https://sharvielectronics.com/product/1k-ohm-1-10-watt-resistor-1-tolerance-rc0603fr-071kl-0603-package/](https://sharvielectronics.com/product/1k-ohm-1-10-watt-resistor-1-tolerance-rc0603fr-071kl-0603-package/?srsltid=AfmBOoqId5euf5aEXh3O4cwJEbf2kKcuhM3vUA5vKpnr9uu9SYb_K_fJ)                                           |
| 6     | 0603SAF1201T5E                                                            | Royal Ohm                    | RES SMD 1.2K OHM 0.1% 1/10W 0603 | 20       | ST2009SD0388    | 0.0096     | $0.02          | [https://sharvielectronics.com/product/1-2k-ohm-1-10-watt-resistor-1-tolerance-0603saf1201t5e-0603-package/](https://sharvielectronics.com/product/1-2k-ohm-1-10-watt-resistor-1-tolerance-0603saf1201t5e-0603-package/?srsltid=AfmBOor2LiabdGbOo6zNE4_VVilQeU6tE37xizoTIQz6ncK1u2H7MGKX)                                       |
| 7     | RC0402FR-07100RL                                                          | YAGEO                        | RES SMD 100 OHM 0.1% 1/16W 0402  | 20       | ST2103SD0837    | 0.0096     | $0.02          | [https://sharvielectronics.com/product/100-ohm-1-16-watt-smd-resistor-1-tolerance-rc0402fr-07100rl-0402-package/](https://sharvielectronics.com/product/100-ohm-1-16-watt-smd-resistor-1-tolerance-rc0402fr-07100rl-0402-package/?srsltid=AfmBOorOjDg3SNWLEfjSp3uLRn1XN4N2UdtNHn8_x5EpgzByEKbjoTnd)                             |
| 8     | Micro 5.9ZB5.7                                                            | SHOU HAN                     | CONN RCPT USB2.0 MICRO B SMD R/A | 1        | ST2103SD1926    | 0.14       | $0.14          | [https://sharvielectronics.com/product/micro-5-9zb5-7-micro-usb-2-0-b-type-right-angle-female-connector-5pin-smd-package/](https://sharvielectronics.com/product/micro-5-9zb5-7-micro-usb-2-0-b-type-right-angle-female-connector-5pin-smd-package/?srsltid=AfmBOoo7P1bRkdSy3efdi_L1puHaSRWVnwPkLxqZIq8gPUzt1F5x1aCc)           |
| 9     | 19-21/BHC-AN1P2/3T                                                        | Everlight                    | LED YELLOW CLEAR CHIP SMD        | 10       | ST2009SD0353    | 0.037      | $0.37          | [https://sharvielectronics.com/product/blue-smd-led-19-21-bhc-an1p2-3t-0603-package/](https://sharvielectronics.com/product/blue-smd-led-19-21-bhc-an1p2-3t-0603-package/?srsltid=AfmBOorofRiUZZl6scCCM5kZvNMHmjLZiWgdwQTTTJgpgKIh006PftMj)                                                                                     |
| 10    | TAJC226M006RNJ                                                            | KYOCERA AVX                  | CAP TANT POLY 22UF 16V CASEC     | 5        | ST2103SD1014    | 0.12       | $0.60          | [https://sharvielectronics.com/product/22uf-6-3v-tantalum-capacitor-10-tolerance-tajc226m006rnj-case-c/](https://sharvielectronics.com/product/22uf-6-3v-tantalum-capacitor-10-tolerance-tajc226m006rnj-case-c/?srsltid=AfmBOooTDi9DGsvCiMpXFD3NlPxmAUFXxTbQLe0KneixldW_EiwEUNyj)                                               |
| 11    | B340A-E3/61T / B340A-E3/5AT                                               | Vishay General Semiconductor | DIODE SCHOTTKY 40V 3A SMAF       | 5        | ST2103SD1960    | 0.2        | $1.00          | [https://sharvielectronics.com/product/b340a-e3-61t-40v-3a-schottky-diode-do-214ac-package/](https://sharvielectronics.com/product/b340a-e3-61t-40v-3a-schottky-diode-do-214ac-package/?srsltid=AfmBOoreKP0SCcX0MEtYLvBvSI7WVgUwLrA_pXM3saX9JvZbN4LX3ggf)                                                                       |
| 12    | BWVS00505040220M00                                                        | ZXCOMPO                      | FIXED IND 22UH 2A SMD            | 1        | ST2103SD1152    | 1          | $1.00          | [https://sharvielectronics.com/product/nr6045-220-22uh-2-a-smd-inductor/](https://sharvielectronics.com/product/nr6045-220-22uh-2-a-smd-inductor/?srsltid=AfmBOoq0LvJFZnHypITaGi080cHchcLDdLiWq3lnZ9z4Yg3nQQ0olBcZ)                                                                                                             |
| 13    | CRCW060313K3FKEAC                                                         | Vishay                       | RES SMD 13.3K OHM 1% 1/10W 0603  | 20       | ST2103SD2348    | 0.008      | $0.16          | [https://sharvielectronics.com/product/13-3k-ohm-1-10-watt-resistor-1-tolerance-crcw060313k3fkeac-0603-package/](https://sharvielectronics.com/product/13-3k-ohm-1-10-watt-resistor-1-tolerance-crcw060313k3fkeac-0603-package/?srsltid=AfmBOorOas8MxOovoyOHNAOAqr0Lnl0XW1RYuaIXPQrPYMhomo351vG1)                               |
| 14    | 3362P-1-104LF                                                             | Bourns Inc.                  | TRIMMER 100KOHM 0.5W PC PIN SIDE | 1        | ST2136CO6475    | 0.2        | $0.20          | [https://sharvielectronics.com/product/100k-ohm-500mw-single-turn-cermet-trimmer-potentiometer-3362p-1-104-through-hole/](https://sharvielectronics.com/product/100k-ohm-500mw-single-turn-cermet-trimmer-potentiometer-3362p-1-104-through-hole/?srsltid=AfmBOopJ906H1BT0GeAqZ9hjjf9ESl-6Y8kvX76Mbf0Jrl1fFiTI7JDe)             |
| 15    | FS8205A                                                                   | \-                           | MOSFET 20V 6A                    | 5        | ST2103SD0926    | 0.07400    | $0.47          | [https://sharvielectronics.com/product/fs8205a-dual-n-channel-power-mosfet-lithium-battery-protection-sot23-6-package/](https://sharvielectronics.com/product/fs8205a-dual-n-channel-power-mosfet-lithium-battery-protection-sot23-6-package/?srsltid=AfmBOorrxaBsFWEXeW-iLmHAYnD6-wwVqzBMh78Wpj_YWCRvYGGilOUv)                 |
| 16    | TP4056                                                                    | \-                           | LINEAR CHARGER                   | 1        | ST2103SD2342    | 0.07400    | $0.10          | [https://sharvielectronics.com/product/tp4056-4-2v-1a-standalone-linear-li-lon-battery-charger-esop-8-package/](https://sharvielectronics.com/product/tp4056-4-2v-1a-standalone-linear-li-lon-battery-charger-esop-8-package/?srsltid=AfmBOorDYKPd_FWn_uFO9eOhC5LhtOw6yJOVIPPWFAIhez5T-rMxe9WB)                                 |
| 17    | DW01A                                                                     | \-                           | PROTECTION IC                    | 1        | ST2103SD0927    | 0.09900    | $0.10          | [https://sharvielectronics.com/product/dw01a-lithium-battery-protection-ic-sot23-6/](https://sharvielectronics.com/product/dw01a-lithium-battery-protection-ic-sot23-6/?srsltid=AfmBOopDcTAQJ7D0pk37M9fa0qd0hAz32JrUYcUDshgccKVNhsuQOsRD)                                                                                       |
| 18    | MT3608                                                                    | \-                           | STEP UP CONVERTER                | 1        | ST2103SD0818    | 0.15000    | $0.15          | [https://sharvielectronics.com/product/mt3608-high-efficiency-1-2mhz-2a-step-up-converter-sot23-6-package/](https://sharvielectronics.com/product/mt3608-high-efficiency-1-2mhz-2a-step-up-converter-sot23-6-package/?srsltid=AfmBOop7sTmszmY0UhtQ-NhjAvU3tMW8hiD5EqB9X1IEuI0ww6yK0qL8)                                         |
| 19    | Raspberry Pi Zero 2 W                                                     | Raspberry Pi                 | \-                               | 1        | ST2001DV0171    | 18.2300    | $18.23         | [https://sharvielectronics.com/product/raspberry-pi-zero-2-w/](https://sharvielectronics.com/product/raspberry-pi-zero-2-w/?srsltid=AfmBOooUXGk8vHAv72A4m9nw5gRJTtaJhXnz45SK9ZmY2sLfkGmMp-nn)                                                                                                                                   |
| 20    | 20 x 2 PIN HEADER                                                         | \-                           | FEMALE PIN HEADER 2X2            | 1        | ST2136CO7815    | 0.65000    | $0.65          | [https://sharvielectronics.com/product/2x20-pin-23-5mm-long-straight-female-berg-strip-2-54mm-pitch/](https://sharvielectronics.com/product/2x20-pin-23-5mm-long-straight-female-berg-strip-2-54mm-pitch/?srsltid=AfmBOop0gb7MJVVeWzmlwF79JiVizB6d5ao2QOl6GfApTKl6OuxOnujC)                                                     |
| 21    | JST CONNECTOR                                                             | \-                           | JST CONNECTOR 30CM               | 1        | ST2136CO8727    | 0.10000    | $0.10          | [https://sharvielectronics.com/product/jst-ph-2-2-pin-double-sided-female-to-female-connector-with-2mm-pitch-30cm-wire-length/](https://sharvielectronics.com/product/jst-ph-2-2-pin-double-sided-female-to-female-connector-with-2mm-pitch-30cm-wire-length/?srsltid=AfmBOorxdkSUbT3QMziQXPUmxJnOAeKTtmbEsO1DTQycUphCC2isaSrZ) |
| 22    | PAM8403                                                                   | \-                           | STEREO AMPLIFIER                 | 1        | ST2001SM0083    | 0.30000    | $0.30          | [https://sharvielectronics.com/product/pam8403-5v-2-channel-stereo-audio-amplifier-module/](https://sharvielectronics.com/product/pam8403-5v-2-channel-stereo-audio-amplifier-module/?srsltid=AfmBOopvXNTxosaBge4onSOd4NV-A7_9HOMEiask09GE07jN6DYv_GMj)                                                                         |
| 23    | 3.25in TFT                                                                | \-                           | RPI                              | 1        | ST2001SM0288    | 12.5000    | $12.50         | [https://sharvielectronics.com/product/3-5-inch-tft-lcd-touch-display/](https://sharvielectronics.com/product/3-5-inch-tft-lcd-touch-display/)                                                                                                                                                                                  |
| 24    | Single Sided Universal PCB Prototype Board – 20×30 cm                     | \-                           | PERFBOARD PCB PROTOTYPE BOARD    | 1        | ST2106CO4011    | 4.3900     | $4.39          | [https://sharvielectronics.com/product/single-sided-universal-pcb-prototype-board-20x30-cm/](https://sharvielectronics.com/product/single-sided-universal-pcb-prototype-board-20x30-cm/?srsltid=AfmBOoreg4sIWmrVPW3P-26sX7ZaAUL6_ivniM1cH426-vZ1vhM5j1ES)                                                                       |
| 25    | 3.5mm Female Stereo Audio Socket Headphone Jack Connector 5 Pin PCB Mount | \-                           | FEMALE AUDIO JACK SWITCHING      | 1        | ST2006CO0034    | 0.11000    | $0.11          | [https://sharvielectronics.com/product/3-5mm-female-stereo-audio-socket-headphone-jack-connector-5-pin-pcb-mount/](https://sharvielectronics.com/product/3-5mm-female-stereo-audio-socket-headphone-jack-connector-5-pin-pcb-mount/?srsltid=AfmBOorzLp72K540waxZB-G-yQ1aFGqTdi1eLnSlV0hnjlI5Z7bLeuut)                           |
| 26    | 8 Ohm 1 Watt Magnet Speaker 36mm Diameter With 2Pin Connector             | \-                           | SPEAKER 1W TOP PORT 9            | 1        | ST2136CO7965    | 0.68000    | $0.68          | [https://sharvielectronics.com/product/8-ohm-1-watt-magnet-speaker-36mm-diameter-with-2pin-connector/](https://sharvielectronics.com/product/8-ohm-1-watt-magnet-speaker-36mm-diameter-with-2pin-connector/?srsltid=AfmBOooy0T01hbtVSkAsLMTpLKefpwhmW9O3Zm93mtZHqtxOaGcIVCf2)                                                   |
| 27    | Lipo Rechargeable Battery LP88426B                                        | \-                           | Battery-3.7V/3300mAH             | 1        | ST2001BY0284    | 4.75       | $5.00          | [https://sharvielectronics.com/product/lipo-rechargeable-battery-3-7v-3300mah-lp88426b/](https://sharvielectronics.com/product/lipo-rechargeable-battery-3-7v-3300mah-lp88426b/?srsltid=AfmBOoouDsKaV5RgrJ7OqTyJGRB5phka41NrV3YTY31AGB7Kt8LNedfo)                                                                               |
| 28    | SS12F44                                                                   | \-                           | 3 Pin Slide Switch               | 5        | ST2136CO8564    | 0.072      | $0.36          | [https://sharvielectronics.com/product/ss12f44-3pin-spdt-slide-switch-with-single-row-vertical-pcb-mountable-3mm-pitch/](https://sharvielectronics.com/product/ss12f44-3pin-spdt-slide-switch-with-single-row-vertical-pcb-mountable-3mm-pitch/)                                                                                |
| 29    | CJMCU-4344 CS4344                                                         |                              | I2S DAC                          | 1        | ST2010SM1643    | 2.22       | $2.22          | [https://sharvielectronics.com/product/cjmcu-4344-cs4344-digital-to-analog-stereo-audio-conversion-module/](https://sharvielectronics.com/product/cjmcu-4344-cs4344-digital-to-analog-stereo-audio-conversion-module/)                                                                                                          |
|       | GST                                                                       |                              |                                  |          |                 |            | $10.00         |                                                                                                                                                                                                                                                                                                                                 |
|       | PCB                                                                       | LION CIRCUITS                |                                  |          |                 |            | $10.64         |                                                                                                                                                                                                                                                                                                                                 |
|       | 3D PRINT                                                                  | 3DING                        |                                  |          |                 |            | $19.00         |                                                                                                                                                                                                                                                                                                                                 |
|       |                                                                           |                              |                                  |          | TOTAL           |            | $89.88         |                                                                                                                                                                                                                                                                                                                                 |

## CART SCREENSHOTS:
 **Sharvi Electronics**

<img width="1901" height="852" alt="image" src="https://github.com/user-attachments/assets/0608c55b-8f7a-4825-af60-58744200fe86" />
<img width="1900" height="788" alt="image" src="https://github.com/user-attachments/assets/a48a8c2f-b1f7-4b94-a6ad-5a00705d3e30" />
<img width="1902" height="798" alt="image" src="https://github.com/user-attachments/assets/bb3d45b2-d454-400e-830c-708830b9aab9" />
<img width="1898" height="349" alt="image" src="https://github.com/user-attachments/assets/7a1d3fe9-253e-4246-bf41-0c2d0fddc18e" />

**Lion Circuits**

<img width="1903" height="943" alt="image" src="https://github.com/user-attachments/assets/c5e6cd55-6291-422c-aedc-a50242b6f341" />

**3Ding**

<img width="1899" height="645" alt="image" src="https://github.com/user-attachments/assets/91c5349b-eb0d-43d6-b3ec-8f685c73e3ae" />
<img width="1138" height="551" alt="image" src="https://github.com/user-attachments/assets/35952a5c-ed0b-4ac3-b512-0e8b8e4632d1" />


## CASE:

<img width="1920" height="1080" alt="0003" src="https://github.com/user-attachments/assets/99bcd8c6-630c-40b7-b4c1-5fd21c0db0e5" />
<img width="1920" height="1080" alt="0004" src="https://github.com/user-attachments/assets/15b7190b-0edb-4e0f-bc5c-90609bf4cf6f" />
<img width="1920" height="1080" alt="0005" src="https://github.com/user-attachments/assets/8051f705-2f7f-49d1-8024-baa3ffc5d511" />
<img width="1920" height="1080" alt="0006" src="https://github.com/user-attachments/assets/dfb69224-8ff9-4b94-a84f-454da3671a33" />
<img width="1920" height="1080" alt="0007" src="https://github.com/user-attachments/assets/0b0a9980-0840-4b05-b047-e6746a423951" />
<img width="1920" height="1080" alt="0008" src="https://github.com/user-attachments/assets/f15394b1-c2cb-4394-bc39-67f02d9256d3" />


