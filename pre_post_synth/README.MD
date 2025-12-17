# SoC Pre-Synthesis & Post-Synthesis Verification


## 🔹 Pre-Synthesis Simulation

### **Definition**

Pre-synthesis simulation is the process of verifying the **functional correctness of a design at the RTL (Register Transfer Level)** before any logic synthesis is performed.

At this stage, the design is described using **behavioral and structural Verilog constructs** such as `always` blocks, `if-else`, `case` statements, and registers.

---

### **Purpose of Pre-Synthesis Simulation**

* Validate the **intended functionality** of the design
* Ensure correct **clock and reset behavior**
* Detect logical errors early in the design cycle
* Verify module integration before synthesis

---

### **Characteristics**

* Uses **RTL source files**
* No technology-specific cells are involved
* Timing delays are ideal (zero delay)
* Simulation focuses purely on **functionality**

---

### **Tools Used**

* Icarus Verilog (iverilog)
* GTKWave (waveform viewing)

---



### **In This Project**

In this project, pre-synthesis simulation was performed on the **Tiny RISC-V SoC RTL** to verify:

* Proper reset and clock operation
* Stable signal behavior without unknown states
* Correct integration of CPU, memory, and peripherals

---
### Direcotory structure
```bash
manohar-g@manohar-g-Lenovo-E41-55:~/pre_post_synth$ tree
.
├── sim
│   └── WEEK_2
│       ├── post_synth
│       │   ├── post_synth.vcd
│       │   ├── soc_post
│       │   ├── soc_top.v
│       │   └── tb_soc_post.v
│       └── pre_synth
│           ├── rtl.vcd
│           ├── soc_rtl
│           └── tb_soc_rtl.v
└── src
    ├── alu.v
    ├── bus_arbiter.v
    ├── control.v
    ├── core.v
    ├── dmem_synth.v
    ├── fir_accel.v
    ├── gpio.v
    ├── imem_synth.v
    ├── pc.v
    ├── regfile.v
    ├── simple_bus.v
    ├── soc_top.v
    ├── spi_master.v
    └── uart.v

6 directories, 21 files
manohar-g@manohar-g-Lenovo-E41-55:~/pre_post_synth$ 
```

### pre_synth Simulation
 ***step1:Compile RTL***
```bash  
cd sim/WEEK_2/pre_synth

iverilog -g2012 -o soc_rtl \
  tb_soc_rtl.v \
  ../../../src/soc_top.v \
  ../../../src/core.v \
  ../../../src/alu.v \
  ../../../src/control.v \
  ../../../src/regfile.v \
  ../../../src/pc.v \
  ../../../src/imem_synth.v \
  ../../../src/dmem_synth.v \
  ../../../src/simple_bus.v \
  ../../../src/bus_arbiter.v \
  ../../../src/uart.v \
  ../../../src/spi_master.v \
  ../../../src/gpio.v \
  ../../../src/fir_accel.v
```
***step2:Run Simulation***
```bash
vvp soc_rtl
gtkwave rtl.vcd
```
### Wavforms
![image](https://github.com/manohargumma/MINI_2_TINY_RISC-V_SoC/blob/2429690ca82974319b65e6ba941f1a364976b759/pre_post_synth/images/Screenshot%20from%202025-12-17%2014-02-50.png)
### **Conclusion (Pre-Synthesis)**

Pre-synthesis simulation ensures that the RTL design is **functionally correct and ready for synthesis**, reducing the risk of errors propagating into later stages of the ASIC flow.

---
## Logic Synthesis using OpenLane
***Step 1: Go to OpenLane***
```bash
cd openlane
make mount
```
***Step 2: Run Only Synthesis***
```bash
prep -design design_op
run_synthesis
```
***Step 3: Find Synthesized Netlist***
```bash
openlane/designs/design_op/runs/RUN_*/results/synthesis/
├── soc_top.v          # Synthesized netlist
├── stat.rpt
├── yosys.log
```
### stats
aftter simulation in that youu see
```bash
~/OpenLane/designs/design_op/runs/RUN_2025.12.16_15.36.43/reports/synthesis$ tree
.
├── 1-synthesis.AREA_0.chk.rpt
├── 1-synthesis.AREA_0.stat.rpt
├── 1-synthesis_dff.stat
├── 1-synthesis_pre.stat
└── 1-synthesis_pre_synth.chk.rpt

1 directory, 5 files
```
***pre_synth stats***
```bash
60. Printing statistics.

=== soc_top ===

   Number of wires:                 19
   Number of wire bits:             59
   Number of public wires:           9
   Number of public wire bits:      43
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                 18
     $_ANDNOT_                       3
     $_AND_                          3
     $_DFFE_PN0P_                    5
     $_DFF_PN0_                      1
     $_NAND_                         1
     $_NOT_                          2
     $_XOR_                          3

```
***post_synth stats***
```bash
74. Printing statistics.

=== soc_top ===

   Number of wires:                 29
   Number of wire bits:             60
   Number of public wires:          12
   Number of public wire bits:      43
   Number of memories:               0
   Number of memory bits:            0
   Number of processes:              0
   Number of cells:                 58
     sky130_fd_sc_hd__a31o_2         1
     sky130_fd_sc_hd__and2_2         2
     sky130_fd_sc_hd__and2b_2        1
     sky130_fd_sc_hd__and4_2         1
     sky130_fd_sc_hd__buf_1          3
     sky130_fd_sc_hd__conb_1        35
     sky130_fd_sc_hd__dfrtp_2        6
     sky130_fd_sc_hd__inv_2          1
     sky130_fd_sc_hd__nand2_2        2
     sky130_fd_sc_hd__nand3_2        1
     sky130_fd_sc_hd__o31a_2         1
     sky130_fd_sc_hd__or2_2          2
     sky130_fd_sc_hd__xnor2_2        2

   Chip area for module '\soc_top': 424.156800
```
## 🔹 Post-Synthesis Simulation

### **Definition**

Post-synthesis simulation is the process of verifying the design **after logic synthesis**, using the **gate-level netlist** generated by the synthesis tool.

The RTL is replaced by a network of **technology-specific standard cells**, making this stage closer to the actual hardware implementation.

---

### **Purpose of Post-Synthesis Simulation**

* Ensure synthesis did not alter the design’s functionality
* Verify correct mapping of RTL logic to standard cells
* Detect synthesis-related issues such as missing resets or unintended logic changes
* Confirm equivalence between RTL and synthesized design

---

### **Characteristics**

* Uses **gate-level netlist**
* Includes **standard cell models** from the PDK
* Timing delays may be ignored (functional simulation)
* Focuses on **structural and functional correctness**

---

### **Tools Used**

* Icarus Verilog (functional gate-level simulation)
* Sky130 standard cell Verilog models
* GTKWave (waveform viewing)

---

### **In This Project**

Post-synthesis simulation was performed using the **synthesized netlist generated by OpenLane** and **Sky130 standard cell libraries**.
The observed behavior matched the pre-synthesis RTL simulation, confirming that the synthesis process preserved functional correctness.

---
### post_synth Simulation
```bash
iverilog -g2012 -o soc_post \
  tb_soc_post.v \
  soc_top.v \
  $SKY130A/libs.ref/sky130_fd_sc_hd/verilog/primitives.v \
  $SKY130A/libs.ref/sky130_fd_sc_hd/verilog/sky130_fd_sc_hd.v

vvp soc_post
gtkwave post_synth.vcd
```

### **Note on Timing Warnings**

During post-synthesis simulation, timing-related warnings may appear due to limited timing support in Icarus Verilog. These warnings originate from standard cell timing specifications and do not affect functional verification.

---

### **Conclusion (Post-Synthesis)**

Post-synthesis simulation confirms that the synthesized gate-level design behaves identically to the original RTL, ensuring readiness for physical design stages.

---

## 🔹 Key Difference Summary

| Aspect         | Pre-Synthesis      | Post-Synthesis               |
| -------------- | ------------------ | ---------------------------- |
| Design Level   | RTL                | Gate-level                   |
| Input Files    | RTL Verilog        | Synthesized netlist          |
| Standard Cells | Not used           | Used                         |
| Timing         | Ideal (zero delay) | Functional (delays ignored)  |
| Main Goal      | Verify logic       | Verify synthesis correctness |

---

## 🔹 Overall Importance

> Performing both pre-synthesis and post-synthesis simulations ensures a robust and reliable ASIC design flow by validating functionality before and after synthesis. This methodology follows industry best practices and academic tapeout programs.

---
