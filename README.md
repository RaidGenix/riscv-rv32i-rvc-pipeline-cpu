# 5-Stage Pipelined RISC-V Processor (RV32I + RVC)

A synthesizable 5-stage pipelined RISC-V CPU core implementing the full **RV32I** base integer instruction set with support for **RVC (compressed 16-bit) instructions**, hazard detection, pipeline stalling, and ALU forwarding.

![Verilog](https://img.shields.io/badge/HDL-Verilog-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![Status](https://img.shields.io/badge/status-active-brightgreen)

---

## 📌 Overview

This project implements a classic **5-stage pipeline** (IF → ID → EX → MEM → WB) RISC-V processor core, designed from scratch in Verilog. It supports mixed execution of 32-bit RV32I instructions and 16-bit RVC compressed instructions through an integrated decompressor, making it more representative of real-world embedded RISC-V cores.

### Key Features
- ✅ Full **RV32I** base instruction set (arithmetic, logical, branch, load/store, jump)
- ✅ **RVC instruction decompressor** — seamless mixed execution of 16-bit and 32-bit instructions
- ✅ **Hazard detection unit** to identify RAW (Read-After-Write) data hazards
- ✅ **Pipeline stalling logic** to safely resolve hazards without incorrect execution
- ✅ **ALU-to-ALU and MEM-to-ALU forwarding** to minimize stall penalties
- ✅ Control-flow hazard handling for branches/jumps
- ✅ Verified with **50+ directed and constrained-random test cases**

---

## 🏗️ Architecture

```
        ┌──────┐   ┌──────┐   ┌──────┐   ┌──────┐   ┌──────┐
        │  IF  │──▶│  ID  │──▶│  EX  │──▶│ MEM  │──▶│  WB  │
        └──────┘   └──────┘   └──────┘   └──────┘   └──────┘
            │           │           │
            │           │           └── ALU Forwarding
            │           └── Hazard Detection / Decompressor
            └── RVC Decompression (16-bit → 32-bit)
```

| Stage | Function |
|-------|----------|
| **IF** (Instruction Fetch) | Fetches instruction from instruction memory; detects 16-bit RVC instructions and routes to decompressor |
| **ID** (Instruction Decode) | Decodes opcode/operands, reads register file, generates control signals, detects hazards |
| **EX** (Execute) | ALU operations, branch resolution, forwarding mux logic |
| **MEM** (Memory Access) | Load/store interaction with data memory |
| **WB** (Write Back) | Writes results back to the register file |

---

## 📂 Repository Structure

```
riscv-pipeline/
├── rtl/
│   ├── core/
│   │   ├── fetch_stage.v
│   │   ├── decode_stage.v
│   │   ├── execute_stage.v
│   │   ├── memory_stage.v
│   │   ├── writeback_stage.v
│   │   ├── pipeline_regs.v
│   │   └── riscv_top.v
│   ├── decompressor/
│   │   └── rvc_decompressor.v
│   ├── hazard/
│   │   ├── hazard_detection_unit.v
│   │   └── forwarding_unit.v
│   └── memory/
│       ├── instr_mem.v
│       └── data_mem.v
├── tb/
│   ├── tb_riscv_top.v
│   ├── directed_tests/
│   └── constrained_random_tests/
├── sim/
│   ├── questasim/
│   └── vivado/
├── docs/
│   ├── architecture.md
│   ├── isa_support.md
│   └── waveforms/
├── scripts/
│   ├── run_sim.sh
│   └── run_synth.tcl
├── LICENSE
└── README.md
```

---

## 🛠️ Tools & Technologies

| Category | Tools |
|----------|-------|
| HDL | Verilog / SystemVerilog |
| Simulation | QuestaSim / ModelSim |
| Synthesis | Xilinx Vivado |
| Verification | Directed + constrained-random testing |

---

## 🚀 Getting Started

### Prerequisites
- Vivado 2020.2+ or QuestaSim/ModelSim
- Make (optional, for build automation)

### Simulation (QuestaSim/ModelSim)
```bash
cd sim/questasim
vlog ../../rtl/**/*.v ../../tb/tb_riscv_top.v
vsim -do "run -all" tb_riscv_top
```

### Synthesis (Vivado)
```bash
vivado -mode batch -source scripts/run_synth.tcl
```

---

## ✅ Verification

The design was validated using **50+ test cases**, including:
- Arithmetic/logical instruction correctness (ADD, SUB, AND, OR, XOR, SLT, etc.)
- Branch and jump instruction control-flow correctness
- Load/store instruction memory interaction
- Compressed (RVC) instruction decode and execution
- Hazard scenarios: back-to-back dependent instructions, load-use hazards, branch hazards


---

## 📊 Results

| Metric | Result |
|--------|--------|
| Instruction set coverage | RV32I (full) + RVC |
| Test cases passed | 50+ / 50+ |
| Hazard types resolved | RAW (data), control-flow |
| Forwarding paths | EX/MEM → EX, MEM/WB → EX |

---

## 🔭 Future Work
- Add support for RV32M (multiply/divide extension)
- Branch prediction (currently static/stall-based)
- AXI-lite memory interface for SoC integration
- FPGA deployment on Basys-3 / Artix-7


## ✍️ Author
**Kartik Sharma**
B.Tech ECE, IIIT Nagpur
[LinkedIn](https://linkedin.com/in/kartik-sharma-59884129a) • [GitHub](https://github.com/RaidGenix)

> 📝 Part of ongoing research on RISC-V Compressed (RVC) instruction decompression latency and pipeline optimization.
