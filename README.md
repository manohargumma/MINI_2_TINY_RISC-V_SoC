# MINI_2_TINY_RISC-V_SoC
![image](https://github.com/manohargumma/MINI_2_TINY_RISC-V_SoC/blob/f1a10bcf64c7df186d8a1da8a95ccd06b8ccff33/image/full_chip_diagram.png)
manohar-g@manohar-g-Lenovo-E41-55:~/chatgptrv$ tree
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
│   ├── design_op
│   │   ├── alu.v
│   │   ├── bus_arbiter.v
│   │   ├── control.v
│   │   ├── core.v
│   │   ├── dmem_synth.v
│   │   ├── fir_accel.v
│   │   ├── gpio.v
│   │   ├── imem_synth.v
│   │   ├── pc.v
│   │   ├── regfile.v
│   │   ├── simple_bus.v
│   │   ├── soc_top.v
│   │   ├── spi_master.v
│   │   └── uart.v
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

9 directories, 39 files
manohar-g@manohar-g-Lenovo-E41-55:~/chatgptrv$ 
