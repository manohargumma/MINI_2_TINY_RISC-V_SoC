# Tiny Secure RISC-V SoC for Embedded Sensor Systems



This repository presents a complete academic System-on-Chip (SoC) design project implemented entirely at **Register Transfer Level (RTL)** with a clean path toward **ASIC implementation (GDSII)**.
The SoC is designed for **low-power embedded sensor applications** and integrates:

* A compact **RV32I single-cycle CPU core**
* A **memory-mapped peripheral subsystem** (UART, SPI, GPIO) 
* A compute-intensive **FIR hardware accelerator**
* A unified **lightweight SoC bus** for communication
* Simulation and verification environments suitable for RTL signoff
![image](https://github.com/manohargumma/MINI_2_TINY_RISC-V_SoC/blob/f1a10bcf64c7df186d8a1da8a95ccd06b8ccff33/image/full_chip_diagram.png)





##  Project Objectives

1. **Design a synthesizable RISC-V CPU core** capable of basic arithmetic, load/store, and control-flow instructions.
2. **Develop essential SoC peripherals** and integrate them using a well-structured memory-mapped architecture.
3. **Implement a hardware FIR accelerator** to demonstrate performance improvement compared to software execution on the CPU.
4. **Integrate, verify, and simulate the full SoC**, preparing it for ASIC flow (synthesis → floorplanning → place & route → GDSII).

---

##  System Overview

The Tiny Secure SoC is composed of the following subsystems:

###  CPU Core 

A simplified **RV32I single-cycle processor** including:

* ALU
* Register File (32 × 32-bit)
* Program Counter
* Instruction decode path
* Instruction and data memory interface


The core executes a reduced subset of RV32I instructions sufficient for embedded workloads and accelerator control.

---

### Peripheral Subsystem

All peripherals are **memory-mapped** and accessible through simple read/write operations.

#### Included peripherals:

* **UART Transmitter**

  * Baud-rate configurable
  * Supports serial debug output
* **SPI Master**

  * Mode-0 operation
  * Basic serial communication for external sensors
* **GPIO**

  * Direction register
  * Input/output data register

---

### FIR Hardware Accelerator (Major Project 1)

A compute-intensive **N-tap FIR filter accelerator** is integrated to demonstrate hardware/software co-design.

Key features:

* Coefficient ROM (fixed during synthesis)
* Sample buffer + MAC (Multiply–Accumulate) datapath
* Memory-mapped control registers:

  * `START`
  * `STATUS`
  * `INPUT_ADDR`
  * `OUTPUT_ADDR`
  * `LENGTH`
* Accelerator autonomously:

  1. Fetches input samples from data memory
  2. Computes FIR outputs
  3. Stores results back to memory

The CPU triggers the accelerator and polls its status — demonstrating offloaded DSP computation.

---

###  SoC Integration 

A unified **SoC Bus** connects:

* CPU Core
* Instruction Memory
* Data Memory
* Peripheral devices
* FIR Accelerator

The top-level module `soc_top.v` integrates all blocks, implements address decoding, and establishes the memory map.

A complete **RTL simulation environment** is provided for functional verification and performance analysis.

---

##  Memory Map 

```
0x0000_0000 – 0x0000_FFFF   Instruction Memory (IMEM)
0x1000_0000 – 0x1000_FFFF   Data Memory (DMEM)

0x2000_0000 – UART TXDATA
0x2000_0004 – UART STATUS/CTRL

0x2000_0100 – SPI CTRL/TXRX

0x2000_0200 – GPIO DIR
0x2000_0204 – GPIO DATA

0x3000_0000 – FIR START / STATUS
0x3000_0004 – FIR INPUT_ADDR
0x3000_0008 – FIR OUTPUT_ADDR
0x3000_000C – FIR LENGTH
```

---

##  RTL-to-GDSII Preparation Flow (Conceptual Overview)

Although physical design is **not implemented in this repository**, the RTL is structured to support standard industrial ASIC flows:

1. **Linting & RTL Review**

   * Clock/reset domain checks
   * Synthesis-friendly coding guidelines

2. **Logic Synthesis**
   Tools: *Yosys / Synopsys Design Compiler / Cadence Genus*

   * Generates gate-level netlist
   * Area and timing reports

3. **Static Timing Analysis (STA)**

   * Multi-corner timing checks
   * Setup/hold validation

4. **Floorplanning & PnR**
   Tools: *OpenROAD / Innovus / ICC2*

   * Placement of CPU, peripherals, and FIR accelerator
   * Power routing
   * Clock-tree synthesis

5. **DRC/LVS Signoff**

   * Ensures layout correctness
   * Confirms layout matches schematic

6. **GDSII Export**
   Final mask-ready file produced by PnR tools.

The project thus forms a full front-end SoC design suitable for back-end implementation.

---



```bash
manohar-g@manohar-g-Lenovo-E41-55:~/rv32$ tree
.
├── rtl
│   ├── accel
│   │   └── fir_accel.v
│   ├── bus
│   │   ├── bus_arbiter.v
│   │   └── simple_bus.v
│   ├── config.tcl
│   ├── cpu
│   │   ├── alu.v
│   │   ├── control.v
│   │   ├── core.v
│   │   ├── dmem_synth.v
│   │   ├── imem_synth.v
│   │   ├── pc.v
│   │   └── regfile.v
│   ├── periph
│   │   ├── gpio.v
│   │   ├── spi_master.v
│   │   └── uart.v
│   └── soc_top.v
├── sw
│   ├── firmware.v
│   └── program.hex
├── synth_netlist.v
├── tb
│   ├── firmware.s
│   ├── tb_core.v
│   ├── tb_peripherals.v
│   └── tb_soc.v
├── tb_core.vvp
├── wave.vcd
└── yosys_short.log

8 directories, 25 files
```

##  **Conclusion**

The Tiny Secure RISC-V SoC project successfully demonstrates the complete design journey of a modern System-on-Chip — from RTL architecture to physical-design readiness.
Through this work, a fully synthesizable **RV32I processor**, a **memory-mapped peripheral subsystem**, and a **hardware-accelerated FIR engine** were designed, verified, and integrated on a unified SoC bus architecture.

The project achieves the following key outcomes:

### ✔ Functional CPU Core

A clean, single-cycle RISC-V core capable of executing essential embedded workloads and controlling peripherals. Its modular RTL design enables easy extension into pipelined or superscalar versions.

### ✔ Peripheral Integration

UART, SPI, and GPIO modules were developed and attached through a well-structured memory map, demonstrating real-world SoC peripheral integration and embedded communication.

### ✔ Hardware Acceleration

The FIR accelerator showcases how compute-intensive tasks can be offloaded from the CPU, leading to significant performance gains. This highlights the relevance of custom accelerators in modern low-power IoT designs.

### ✔ Complete Front-End + Physical-Design Readiness

All modules are written in synthesizable Verilog, lint-clean, simulation-verified, and organized for **OpenLane-based RTL → GDSII** flow, proving the design is suitable for ASIC implementation on the SkyWater 130nm process.

### ✔ Strong Educational & Research Value

The project reflects practical knowledge of:

* Digital architecture
* RTL design methodology
* Verification and testbench creation
* Hardware/software co-design
* ASIC flow concepts (synthesis, STA, P&R, DRC/LVS, GDSII)

This makes the work not only technically successful but also academically rich and industry-relevant.

---



**This project demonstrates that a complete, custom SoC — including CPU, peripherals, accelerator, and physical design flow — can be conceived, designed, and validated from scratch using open-source tools and PDKs.**
It reflects strong competency in digital electronics, VLSI design, and system integration, and provides an excellent foundation for further research, optimization, or real silicon tape-out.

---
## 🌐 Explore More

If you enjoyed this work or found it useful,  
consider showing your support 💖 by **starring ⭐ this repository** or **forking 🔁 it** to explore further!

<p align="center">
  <a href="https://github.com/manohargumma/INTRODUCTION-TO-CIRCUIT-DESIGN-AND-SPICE-SIMUATIONS.git" target="_blank">
    <img src="https://img.shields.io/badge/⭐ Star%20This%20Repo-blue?style=for-the-badge"/>
  </a>
  <a href="https://github.com/manohargumma?tab=repositories" target="_blank">
    <img src="https://img.shields.io/badge/👨‍💻 Explore%20My%20Projects-8A2BE2?style=for-the-badge"/>
  </a>
</p>

---

## 💬 Let’s Connect

<p align="center"> 
  <a href="https://www.linkedin.com/in/manohar-gumma-56920b2b0?lipi=urn%3Ali%3Apage%3Ad_flagship3_profile_view_base_contact_details%3BQDrHZ%2FsISxWSe9PA4xcVSA%3D%3D" target="_blank">
    <img src="https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white"/>
  </a>
  <a href="mailto:manohar.manohargumma13@gmail.com">
    <img src="https://img.shields.io/badge/Email-Contact%20Me-D14836?style=for-the-badge&logo=gmail&logoColor=white"/>
  </a>
  <a href="https://github.com/manohargumma" target="_blank">
    <img src="https://img.shields.io/badge/GitHub-Manohar--Gumma-000000?style=for-the-badge&logo=github"/>
  </a>
</p>

---
 


