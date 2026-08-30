# µT-Kernel 3.0 (mtk3) Ports & SMP Extensions

⭐️ *If you find these ports or SMP extensions helpful for your own development or research, please consider starring this repository! It helps track community interest and project impact.*

This repository serves as the central landing page and documentation hub for a series of independent ports of the TRON Forum's [µT-Kernel 3.0](https://github.com/tron-forum/mtkernel_3) to various modern microcontrollers. 

The goal of this project is to expand µT-Kernel 3.0 support across Espressif's ESP32 ecosystem and extend the official Raspberry Pi Pico port to support Symmetric Multiprocessing (SMP).

## Supported Architectures Matrix

| Target Board | Architecture | Core Type | RTOS Variant | Key Focus |
| :--- | :--- | :--- | :--- | :--- |
| **ESP32-C3** | RISC-V | Single-Core | Standard | Baseline RISC-V implementation. |
| **ESP32-C6** | RISC-V | Single-Core | Standard | Custom interrupt controller handling distinct from C3. |
| **ESP32-S3** | Xtensa | Dual-Core | **SMP** | Symmetric Multiprocessing adaptation for dual-core Xtensa. |
| **Pi Pico (RP2040)** | ARM Cortex-M0+ | Dual-Core | **SMP** | SMP extension of the official TRON Forum port. |
| **Pi Pico 2 (RP2350)** | ARM Cortex-M33 | Dual-Core | **SMP** | SMP extension of the official TRON Forum port (WIP). |
| **Pi Pico 2 (RP2350)** | RISC-V Hazard | Dual-Core | **SMP** | SMP extension of the official TRON Forum port (NEXT). |

[Note] **Pi Pico (RP2040)** that uses ARM Cortex-M0+ has the official TRON Forum port (https://github.com/tron-forum/mtk3_bsp) for single-core. **This was not done by me.**

---

## 📂 Repository Directory

Choose a target platform below to navigate to its specific repository, source code, and build instructions.

### 1. [ESP32-S3 SMP Port - Coming Soon](link-to-your-s3-repo)
**Status:** Active | **Variant:** SMP
Created a Symmetric Multiprocessing (SMP) variation of the RTOS specifically tailored for the ESP32-S3's dual-core Xtensa architecture. 
* **Current Capabilities:** 
  * [Add 2-3 bullet points here, e.g., Core affinity scheduling, inter-processor interrupts (IPI), spinlock implementations.]
  * Builds via [ESP-IDF / specific toolchain].

### 2. [ESP32-C6 Port](https://github.com/sirfonzie/esp32c6-mtk3.git)
**Status:** Active | **Variant:** Standard (Single-Core)
A port targeting the ESP32-C6 RISC-V core. While similar to the C3, this port implements the necessary modifications for the C6's distinct interrupt handling mechanism.
* **Current Capabilities:** 
  * Boards — M5Stack M5NanoC6 (native USB-Serial-JTAG) and Waveshare ESP32-C6-LCD-1.3. The M5NanoC6 is the validated reference board; the Waveshare port builds and is documented but has no hardware capture yet.
  * Kernel — full preemptive µT-Kernel 3.0 (IEEE 2050-2018). Tasks, priorities, semaphores, event flags, mutexes, mailboxes, message buffers, fixed/variable memory pools, cyclic + alarm handlers, physical timers. 1 ms tick. Boot smoke suite passes 8/8 on every start.
  * Peripherals — DEV_SER (UART1, routed to the M5NanoC6 Grove port) through the device manager; all ESP-IDF drivers remain available to applications. No DEV_ADC or I²C driver on this target.
  * Radios — WiFi, lwIP, ESP-MESH, and BLE (NimBLE) run on µT-Kernel via a FreeRTOS API shim that passes 97/97 of its own hardware validation checks. Adds an IEEE 802.15.4 promiscuous listener — all three share the C6's single 2.4 GHz front end through the software coexistence arbiter. Concurrent WiFi+BLE works but is marked experimental.
  * Examples — 10 standalone IDF apps, from a two-task template to a two-board BLE + ESP-MESH bridge and a full LCD/TF/RGB board-scanner.

### 3. [ESP32-C3 Port](https://github.com/sirfonzie/esp32c3-mtk3.git)
**Status:** Stable | **Variant:** Standard (Single-Core)
The baseline RISC-V port for the ESP32-C3, establishing the foundational architecture for Espressif RISC-V targets.
* **Current Capabilities:**
  * Boards — ESP32-C3 Super Mini (native USB-JTAG) and M5StampC3 (CH9102 UART0).
  * Kernel — full preemptive µT-Kernel 3.0 (IEEE 2050-2018). Tasks, priorities, semaphores, event flags, mutexes with priority inheritance, mailboxes, message buffers, fixed/variable memory pools, cyclic + alarm handlers, physical timers. 1 ms tick.
  * Peripherals — DEV_SER (UART1) and DEV_ADC (ADC1 ch0–4) through the device manager; all ESP-IDF drivers remain available to applications.
  * Radios — WiFi, lwIP, ESP-NOW, ESP-MESH, and BLE (NimBLE) run on µT-Kernel via a FreeRTOS API shim. BLE and WiFi each solid; concurrent WiFi+BLE works but is marked experimental.
  * Examples — 12 standalone IDF apps, from a two-task template to a two-board BLE + ESP-MESH bridge.

### 4. [Raspberry Pi Pico (RP2040) with SMP Extension](https://github.com/sirfonzie/mtk3smp-rp2040.git)
**Status:** Active | **Variant:** SMP
This repository branches from the official TRON Forum Raspberry Pi Pico port and extends it to become fully SMP compliant, utilizing both ARM Cortex-M0+ cores.
* **Current Capabilities:**
  * True SMP scheduling - global ready queue with per-core dispatch, static task affinity, cross-core wake, inter-processor interrupts, and remote task management, all qualified on hardware
  * Full preemptive kernel - tasks, priorities, semaphores, event flags, mutexes, mailboxes, message buffers, fixed and variable memory pools, cyclic and alarm handlers with per-processor assignment
  * Measured 1.799x speedup - the same 384-job workload runs 44.4% faster with one worker per core than with both pinned to one
  * Two qualified console profiles - UART0 (the baseline) and an optional SMP USB-CDC profile that survives a dual-core print storm with zero unintended drops
  * CYW43439 radio bring-up - firmware boot, station enable, OTP MAC and active scan pass a 72/72 hardware gate as a development profile

---

## Getting Started

If you want to pull all ports simultaneously, this repository uses Git Submodules.

```bash
# Clone this umbrella repository and all associated ports
git clone --recursive [https://github.com/yourusername/mtk3-ports.git](https://github.com/yourusername/mtk3-ports.git)
