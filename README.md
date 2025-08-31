# Design of a 5-Stage Pipelined MIPS32 RISC Processor

This repository documents the design and Verilog implementation of a **MIPS32 Instruction Set Architecture (ISA)-based RISC processor**.  
The processor is built with a **classic 5-stage pipeline**, making it efficient for instruction execution and suitable for academic learning and simulation.

---

## 📑 Table of Contents
- [MIPS32 Overview](#-mips32-overview)
- [Addressing Modes](#-addressing-modes)
- [Supported Instructions](#-supported-instructions)
- [Instruction Encoding](#-instruction-encoding)
- [Execution Stages](#-execution-stages)
- [Non-Pipelined Datapath](#-non-pipelined-datapath)
- [Pipelined Datapath](#-pipelined-datapath)
- [Verilog Design Code](#-verilog-design-code)
- [Testbench and Example Program](#-testbench-and-example-program)
- [Run on EDAPlayground](#-run-on-edaplayground)
- [Known Limitations](#-known-limitations)
- [References](#-references)

---

## 🖥 MIPS32 Overview
- **32 × 32-bit General Purpose Registers (GPRs)**: R0 to R31  
- **Register R0** is hardwired to `0`  
- **32-bit Program Counter (PC)**  
- **No status flags** like carry, zero, or sign  
- **Few addressing modes** for simplicity  
- **Memory is word-addressable** with a 32-bit word size  
- Only **load** and **store** instructions can access memory  

---

## 🧾 Addressing Modes
| Addressing Mode          | Example Instruction     |
|-------------------------|-------------------------|
| Register Addressing     | `ADD R1, R2, R3`        |
| Immediate Addressing    | `ADDI R1, R2, 200`      |
| Base Addressing         | `LW R5, 150(R7)`        |
| PC-Relative Addressing  | `BEQZ R3, LABEL`        |
| Pseudo-Direct Addressing| `J LABEL`               |

---

## 🛠 Supported Instructions
Only a **subset of MIPS32 instructions** is implemented for simplicity.

### **Load/Store**
```asm
LW R2,124(R8)   # R2 = Mem[R8+124]
SW R5,-10(R25)  # Mem[R25-10] = R5
