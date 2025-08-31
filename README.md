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

- Load and Store Instructions  
```
LW R2,124(R8) // R2 = Mem[R8+124]  
SW R5,-10(R25) // Mem[R25-10] = R5  
```
- Arithmetic and Logic Instructions (only register operands)  
```
ADD R1,R2,R3 // R1 = R2 + R3  
ADD R1,R2,R0 // R1 = R2 + 0  
SUB R12,R10,R8 // R12 = R10 – R8  
AND R20,R1,R5 // R20 = R1 & R5  
OR R11,R5,R6 // R11 = R5 | R6  
MUL R5,R6,R7 // R5 = R6 * R7  
SLT R5,R11,R12 // If R11 < R12, R5=1; else R5=0 
```
- Arithmetic and Logic Instructions (immediate operand)  
```
ADDI R1,R2,25 // R1 = R2 + 25  
SUBI R5,R1,150 // R5 = R1 – 150  
SLTI R2,R10,10 // If R10<10, R2=1; else R2=0 
```
- Branch Instructions  
```
BEQZ R1,Loop // Branch to Loop if R1=0  
BNEQZ R5,Label // Branch to Label if R5!=0  
```
- Jump Instruction  
```
J Loop // Branch to Loop unconditionally  
```
- Miscellaneous Instructioon  
```
HLT // Halt execution 
```
### ▫️ Stages of Execution  
The instruction execution cycle in this MIPS32 pipeline follows **five stages**:

1. **IF (Instruction Fetch):** Fetch the instruction from memory.  
2. **ID (Instruction Decode / Register Fetch):** Decode the instruction and read required registers.  
3. **EX (Execution / Effective Address Calculation):** Perform ALU operations or calculate addresses.  
4. **MEM (Memory Access / Branch Completion):** Access data memory or resolve branch instructions.  
5. **WB (Write Back):** Write the result back to the register file.  

> **Note:** Detailed micro-operations are intentionally omitted for simplicity.
## ▫️ Pipelined DataPath  
![MIPS32 Pipelined DataPath](images/pipelined_datapath.png)
