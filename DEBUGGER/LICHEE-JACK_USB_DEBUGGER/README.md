# Lichee-Jack USB Debugger

This sub-directory contains the **USB 2.0 hub + UART converter board** designed specifically for the **Lichee-Jack** project.
It is intended to simplify development, debugging, and flashing by combining **USB OTG**, **UART console**, and **power control** into a single compact board.

![](../../doc/LICHEE-JACK_USB_DEBUGGER.svg)

---

## Overview

The **LicheeRV Nano** features a **USB 2.0 Type-C OTG port implemented directly by the SoC PHY**.
This design is slightly different from a typical USB 2.0 Type-C implementation:

* **SUB1 / SUB2** pins are internally multiplexed as **SoC UART0**

  * Used for early boot logs, kernel `printk`, and Linux login console
* **D+ / D-** pins are used as a standard **USB OTG interface**

  * Used for USB gadget mode (RNDIS, ECM, HID, Mass Storage, etc.)

Because UART and USB share the same physical Type-C connector, direct connection to a PC is inconvenient during development.

> [!IMPORTANT]
> This board is **not a generic USB-C hub**.
>
> It is designed specifically around the **LicheeRV Nano Type-C PHY behavior**,
> where **USB OTG and UART share the same connector**.
> Using it with unrelated USB-C devices may lead to undefined behavior.

To solve this, the **Lichee-Jack USB Debugger** acts as a **2-port Type-C converter**:

* **Port 1 (Upstream)** → Connects to the host PC
* **Port 2 (Downstream)** → Connects to the Lichee-Jack device

The board cleanly separates:

* USB OTG data path
* UART debug console
* Power and connection detection

---

## Architecture & Design

### USB Hub

The board uses a **Microchip USB2422/MJ** USB 2.0 hub controller:

* **1 upstream port** (PC)
* **2 downstream ports**

  * One for Lichee-Jack USB OTG
  * One internally connected to the USB-UART bridge

This allows the Lichee-Jack USB gadget functions to work normally **while UART is simultaneously available** on the same PC connection.

> [!NOTE]
> The hub architecture allows **USB gadget functions and UART console**
> to operate **simultaneously over a single PC connection**.
> No cable swapping or re-plugging is required during development.

### USB-UART Bridge

UART is provided by **CP2102-GMR**:

* Connected to Lichee-Jack **UART0 (SUB1 / SUB2)**
* Exposes a standard USB CDC-ACM serial device on the host
* Suitable for:

  * U-Boot console
  * Linux kernel logs
  * Root login shell

### Type-C CC & Power Control

The debugger board implements **CC-based power detection and control**:

* **PMOS power switch** controlled by CC logic
* Behavior:

  * If **Lichee-Jack is not connected**, PMOS remains **OFF**
  * When Lichee-Jack is connected:

    * CC pins are pulled down by the device
    * PMOS gate is driven
    * Power path is enabled automatically

This prevents:

* Back-powering
* Floating VBUS issues
* Accidental short or unstable power states

> [!NOTE]
> Power delivery on this board is **fully hardware-controlled via CC detection**.
> No firmware or software control is involved.

---

## Features

* USB 2.0 Hub (1-up / 2-down)
* Integrated USB-UART console
* Native support for LicheeRV Nano Type-C pin multiplexing
* Automatic CC-based power switching
* Works seamlessly with USB gadget modes
* No firmware required (fully hardware-driven)

---

## Assembly

* **PCB**: 2-layer board, thickness 1.6 mm
* **Assembly side**: Top side only
* **Solder paste**:

  * Sn63 / Pb37 **or**
  * SAC305

> [!CAUTION]
> The USB hub uses a **QFN package** and requires a **proper reflow profile**.
>
> Insufficient temperature or uneven heating may cause:
>
> * Hidden cold joints
> * Unreliable USB enumeration
>
> A controlled **high-temperature reflow process** is strongly recommended.

---

## PCBA

* All components placed on **top layer only**
* Suitable for low-cost SMT assembly

The following files are provided in the **EXT-Board** latest release:

* BOM (Bill of Materials)
* CPL / Pick-and-Place
* Gerber files

---

## Notes

> [!WARNING]
> **Type-C cable selection is critical.**
>
> Many USB 3.x Type-C cables are **SuperSpeed-only** and **do not carry USB-2 / SBU signals**.
> If such a cable is used, **SUB1 / SUB2 (UART0)** may be physically disconnected,
> resulting in **no UART console output** even when the hardware is correct.
>
> Always use a **Thunderbolt / USB4 Type-C cable**, which is required by specification
> to include **full USB-2 wiring**.

> [!TIP]
> **No UART console? Try flipping the Type-C connector on the Lichee-Jack side.**
>
> Unlike USB D+ / D-, **SUB1 / SUB2 (UART0) are not orientation-symmetric**.
> Depending on plug orientation, the UART pins may not be connected.
>
> Simply unplug and **reverse the Type-C connector** to restore UART functionality.

* Designed specifically for **Lichee-Jack / LicheeRV Nano** Type-C behavior
* Not intended as a generic USB-C hub
* Recommended for development, debugging, and factory flashing workflows

