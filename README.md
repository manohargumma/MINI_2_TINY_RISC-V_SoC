# Tiny Secure RISC-V SoC for Embedded Sensor Systems



This repository presents a complete academic System-on-Chip (SoC) design project implemented entirely at **Register Transfer Level (RTL)** with a clean path toward **ASIC implementation (GDSII)**.
The SoC is designed for **low-power embedded sensor applications** and integrates:

* A compact **RV32I single-cycle CPU core**
* A **memory-mapped peripheral subsystem** (UART, SPI, GPIO)
* A compute-intensive **FIR hardware accelerator**
* A unified **lightweight SoC bus** for communication
* Simulation and verification environments suitable for RTL signoff
![image](https://github.com/manohargumma/MINI_2_TINY_RISC-V_SoC/blob/f1a10bcf64c7df186d8a1da8a95ccd06b8ccff33/image/full_chip_diagram.png)



This project addresses the four required sub-deliverables:
**Mini Project 1**, **Mini Project 2**, **Major Project 1**, and **Major Project 2** — combined into a single, coherent SoC architecture.

---

## 2. Project Objectives

1. **Design a synthesizable RISC-V CPU core** capable of basic arithmetic, load/store, and control-flow instructions.
2. **Develop essential SoC peripherals** and integrate them using a well-structured memory-mapped architecture.
3. **Implement a hardware FIR accelerator** to demonstrate performance improvement compared to software execution on the CPU.
4. **Integrate, verify, and simulate the full SoC**, preparing it for ASIC flow (synthesis → floorplanning → place & route → GDSII).

---

## 3. System Overview

The Tiny Secure SoC is composed of the following subsystems:

### 3.1 CPU Core (Mini Project 1)

A simplified **RV32I single-cycle processor** including:

* ALU
* Register File (32 × 32-bit)
* Program Counter
* Instruction decode path
* Instruction and data memory interface

The core executes a reduced subset of RV32I instructions sufficient for embedded workloads and accelerator control.

---

### 3.2 Peripheral Subsystem (Mini Project 2)

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

### 3.3 FIR Hardware Accelerator (Major Project 1)

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

### 3.4 SoC Integration (Major Project 2)

A unified **SoC Bus** connects:

* CPU Core
* Instruction Memory
* Data Memory
* Peripheral devices
* FIR Accelerator

The top-level module `soc_top.v` integrates all blocks, implements address decoding, and establishes the memory map.

A complete **RTL simulation environment** is provided for functional verification and performance analysis.

---

## 4. Memory Map (Example)

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

## 5. RTL-to-GDSII Preparation Flow (Conceptual Overview)

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

## 6. Verification Methodology

### 6.1 Unit-Level Verification

Each module includes a targeted testbench:

* ALU functional tests
* Register file write/read behavior
* UART waveform validation
* SPI shift operation
* FIR accelerator test using small deterministic vectors

### 6.2 Integration Verification

The SoC testbench (`tb_soc.v`) validates:

* Instruction fetch
* Data memory access
* Peripheral register writes
* FIR accelerator handshake

### 6.3 MATLAB Reference Model (For FIR Output Validation)

MATLAB-generated golden vectors verify FIR output correctness:

* Input stimulus
* Expected FIR response
* RTL output comparison

### 6.4 Waveform Inspection

`core.vcd` and `soc.vcd` allow visual inspection using GTKWave.

---

## 7. Directory Structure

```
rtl/         → Synthesizable Verilog source code  
tb/          → Testbenches  
sw/          → Firmware, IMEM hex images  
doc/         → Block diagrams, reference documentation  
scripts/     → Yosys-based synthesis examples  
constraints/ → Placeholder for ASIC/PD constraints  
Makefile     → Build automation for simulation  
README.md    → Project documentation  
```

---

## 8. Example Results to Report to Guide

You are expected to demonstrate:

### ✔ Functional CPU operation

Running small programs (e.g., arithmetic loop, memory operations).

### ✔ Correct peripheral behavior

Waveforms showing UART transmission, SPI shifting, GPIO toggling.

### ✔ FIR accelerator correctness

Matching RTL output with MATLAB golden model.

### ✔ Performance improvement

Hardware FIR is significantly faster than software FIR on the CPU.

### ✔ Clean RTL architecture

Documented modules, memory map, and top-level integration.

### ✔ ASIC-readiness

RTL is synthesizable, modular, bus-clean, and has well-defined boundaries.

---

## 9. Professional Summary for Project Guide

This project demonstrates the **complete front-end design** of a RISC-V SoC, from RTL specification through simulation and accelerator integration.
It reflects industry-standard SoC development practices:

* Modular, synthesizable Verilog
* Memory-mapped bus and peripherals
* Hardware accelerator integration
* Testbench-driven verification
* MATLAB-based golden reference comparison
* ASIC-ready design methodology

The FIR accelerator showcases how hardware acceleration significantly enhances system performance for embedded signal-processing tasks.

---

## 10. How to Build, Run & Simulate

### To run CPU tests:

```bash
make sim_core
```

### To run full SoC simulation:

```bash
make sim_soc
```

View results in GTKWave:

```bash
gtkwave soc.vcd &
```

---

## 11. Conclusion

This repository provides a high-quality educational SoC platform that unifies all four project deliverables into a single coherent design. It offers a realistic introduction to:

* CPU micro-architecture
* Peripheral and bus design
* Hardware acceleration
* RTL verification
* ASIC-oriented design methodology

It is suitable for academic submission, viva demonstration, and as a foundation for future ASIC or advanced RISC-V projects.

---

## 12. Acknowledgments / Authors

*Add names here*

---

## 13. License (Optional)

MIT License or institution-specific license.

---

If you want, I can also generate:

✅ A **PDF report** for submission
✅ A **clean full-chip architecture diagram** for the README
✅ A **presentation PPT** for your guide
✅ A **Gantt chart / workflow diagram**
✅ A **MATLAB FIR verification script**

Just tell me what you want next.

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


