# Lichee-Jack PCB

This repository contains the open-source extension board system for the **Lichee-Jack** project. It includes multiple PCBs, FPCs, stack-up boards, and extension modules designed to expand the capabilities of the LicheeRV Nano.

![](./doc/LICHEE-JACK_EXT_BOARD.svg)

---

## Overview

The complete hardware system consists of **three main boards**:

---

### 1. `Lichee-Jack_EXT_BOARD`

<img src="./doc/ext_board_v1_2_btm.png" width="90%">
<img src="./doc/ext_board_v1_2_top.png" width="90%">

The primary functional PCB containing:

* Battery management system
* WS2812 RGB LED
* SP3T control switch
* Peripheral support circuits
* PCB thickness: **1.6 mm**

---

### 2. `Lichee-Jack_EXT_BOARD_FPC`

<img src="./doc/ext_board_fpc_3dview.png" width="90%">
<img src="./doc/ext_board_fpc_cadview.png" width="90%">

A flexible cable connecting:

* LicheeRV Nano ETH pads → EXT board ETH pads
* LicheeRV Nano CSI pads → Header board
* Extra signals for extension modules

---

### 3. `Lichee-Jack_HEADER_BOARD`

<img src="./doc/header-board-top.png" width="90%">
<img src="./doc/header-board-btm.png" width="90%">
<img src="./doc/header-board-freecad.png" width="90%">

Adapter board for:

* 1.27 mm 2×10 SMT header
* EXT_BOARD_FPC
* PCB thickness: **0.8 mm**

Used by external extension modules.

---

## Assembly Notes

> **Important:** Follow recommended solder temperatures to avoid damage.

### EXT_BOARD

* Use **high-temperature solder paste** (Sn63/Pb37 or SAC305)

### EXT_BOARD_FPC

* Solder with **standard soldering iron**

### Header Board

* Use **high-temperature solder paste**

### LicheeRV Nano + EXT Board

* Use **low-temperature solder paste** (Sn42/Bi58)

<img src="./doc/PABA_TOP.jpg" width="45%"> <img src="./doc/PCBA_BTM.jpg" width="38.19%">

---

## Battery Information

* Recommended: **Li-Po 3.7V 200mAh**
* Model: **402030**
* Fits the 3D-printed enclosure included in the repo

---

## 5V Power Notes

The LicheeRV Nano shorts **VBUS** ↔ **VSYS** using a 0Ω resistor (marked "5V").

To safely power externally:

> **Remove the 0Ω resistor** on the bottom layer.

---

## Assembly for 3D-Printed Case Compatibility

To fit the Nano + EXT Board inside the enclosure:

### 1. Remove CSI Connector

* Desolder the CSI camera connector on the LicheeRV Nano

### 3. Install Header Board

* Solder SMT connectors based on BOM

### 4. Connect FPC

* Solder EXT_BOARD_FPC between the Header Board and the LicheeRV Nano

### 5. Ethernet Wiring

* Use short 4P or 8P RJ45 cable
* Solder wires to EXT Board ETH pads:

  * WOG, OG, WG, G → pins 1, 2, 3, 6
* Crimp 8P8C connector to cable end

![](./doc/8P8C.jpg)

---

## PCB Assembly via Manufacturer

You may order PCBA for the **EXT_BOARD** using:

* Gerber files
* BOM
* CPL

Manufacturers (JLCPCB / PCBWay / Seeed) will assemble **only the EXT_BOARD**. The LicheeRV Nano must be soldered manually unless custom services are arranged.

![](./doc/JLCPCB1.png)
![](./doc/JLCPCB2.png)
![](./doc/JLCPCB3.png)
![](./doc/JLCPCB4.png)

---

## PCB Version History

Always use the latest revision.

### v1.0 → v1.1

* Added missing Schottky diode between PWR_IN → PWR_OUT
* Fixed boot issue by rerouting SP3T pos1 from GPIO-A16 → GPIO-A15
* See diagram:
  ![](./doc/V1_0_V1_1_CHANGELOG.png)

### v1.1 → v1.0.32 (EXT_BOARD)

* Fixed UART0-RX conflict (GPIO-A17)
* Rerouted pos2 → GPIO-A24
* Added speaker pads
* Replaced old ETH-FPC system with:

  * Stack board
  * Header board
  * New EXT_BOARD_FPC

---

## License

Licensed under **GNU GPLv3**.

You are free to **use**, **modify**, **study**, and **redistribute** this project under the same license.
