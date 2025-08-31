# MIPS32-Pipeline
This repository contains the details and the code for the MIPS32 ISA based RISC Processor, which is implemented in 5 stage pipelined configuration.
# MIPS32-Pipeline
This repository contains the details and the code for the MIPS32 ISA based RISC Processor, which is implemented in 5 stage pipelined configuration.

## Table of Contents
- [MIPS32](#mips32)
- [Addressing Modes](#addressing-modes)
- [Instructions Considered](#instructions-considered)
- [Instruction Encoding](#instruction-encoding)
- [Stages of Execution](#stages-of-execution)
- [Non Pipelined DataPath](#non-pipelined-datapath)
- [Pipelined DataPath](#pipelined-datapath)
- [Verilog Design Code](#verilog-design-code)
- [Example Program Testbench Code](#example-program-testbench-code)
- [EDA Playground Link](#edaplayground-link)
- [Known Issues and Problems](#known-issues-and-problems)
- [References](#references)
  ## MIPS32
- 32 × 32-bit General Purpose Registers (GPRs) `[R0 to R31]`
- `R0` is hardwired to logic `0`
- 32-bit Program Counter (PC)
- No flag registers (carry, zero, sign, etc.)
- Few addressing modes
- Only **Load** and **Store** instructions can access memory
- Memory is word-addressable with a **32-bit word size**

---

## Addressing Modes

| **Addressing Mode**       | **Example Instruction** |
|--------------------------|--------------------------|
| Register addressing       | `ADD R1,R2,R3`          |
| Immediate addressing      | `ADDI R1,R2,200`        |
| Base addressing           | `LW R5,150(R7)`         |
| PC-relative addressing    | `BEQZ R3,Label`         |
| Pseudo-direct addressing  | `J Label`               |

---

## Instructions Considered
Not all instructions of MIPS32 are implemented. For simplicity, only a subset of instructions is considered:

### **Load and Store Instructions**
- `LW R2,124(R8)`  → `R2 = Mem[R8+124]`  
- `SW R5,-10(R25)` → `Mem[R25-10] = R5`

### **Arithmetic and Logic Instructions (Register Operands)**
- `ADD R1,R2,R3`   → `R1 = R2 + R3`
- `ADD R1,R2,R0`   → `R1 = R2 + 0`
- `SUB R12,R10,R8` → `R12 = R10 - R8`
- `AND R20,R1,R5`  → `R20 = R1 & R5`
- `OR R11,R5,R6`   → `R11 = R5 | R6`
- `MUL R5,R6,R7`   → `R5 = R6 * R7`
- `SLT R5,R11,R12` → `If R11 < R12, R5=1; else R5=0`

### **Arithmetic and Logic Instructions (Immediate Operand)**
- `ADDI R1,R2,25`  → `R1 = R2 + 25`
- `SUBI R5,R1,150` → `R5 = R1 - 150`
- `SLTI R2,R10,10` → `If R10 < 10, R2=1; else R2=0`

### **Branch Instructions**
- `BEQZ R1,Loop`   → Branch to `Loop` if `R1=0`
- `BNEQZ R5,Label` → Branch to `Label` if `R5!=0`

### **Jump Instruction**
- `J Loop` → Unconditional branch to `Loop`

### **Miscellaneous Instruction**
- `HLT` → Halt execution
## Instruction Encoding
- `shamt`: Shift amount  
- `funct`: Opcode extension for additional functions
