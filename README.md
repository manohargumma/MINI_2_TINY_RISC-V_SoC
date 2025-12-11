# MINI_2_TINY_RISC-V_SoC
![image](https://github.com/manohargumma/MINI_2_TINY_RISC-V_SoC/blob/f1a10bcf64c7df186d8a1da8a95ccd06b8ccff33/image/full_chip_diagram.png)
his project implements a compact SoC intended for low-power embedded sensors. The SoC integrates:

A simple RV32I single-cycle CPU (minimal instruction set needed for firmware),

A simple memory-mapped bus to connect CPU to peripherals and memory,

Peripherals: UART (TX/RX) and GPIO,

A hardware FIR accelerator (N-tap, fixed point) accessible through memory-mapped registers,

Testbenches and firmware that exercise the accelerator to demonstrate performance gains vs. software.

The design is written in synthesizable Verilog and organized to support simulation (Icarus/ModelSim), FPGA prototyping, and physical implementation with OpenLane (sky130) if desired.
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


