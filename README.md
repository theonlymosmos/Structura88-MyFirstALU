# Structura-8: Resource-Optimized Modular ALU

![Verilog](https://img.shields.io/badge/Language-Verilog-blue) ![Architecture](https://img.shields.io/badge/Architecture-Structural-orange) ![Status](https://img.shields.io/badge/Status-Verified-green)

## 📌 Project Overview
[cite_start]**Structura-8** is a resource-optimized **8-bit Arithmetic Logic Unit (ALU)** implemented in **System Verilog**[cite: 293]. [cite_start]The design strictly adheres to structural modeling constraints, avoiding high-level behavioral operators in the top-level module to demonstrate low-level hardware orchestration[cite: 296].

[cite_start]The architecture features a **"Smart Adder" topology**[cite: 71]. [cite_start]Instead of using discrete circuits for every math operation, the design utilizes a single 8-bit adder as the central processing unit, dynamically routing inputs via multiplexers to perform Addition, Subtraction, Incrementing, and Comparison[cite: 73, 299]. [cite_start]This approach reduces the arithmetic logic gate count by approximately **40%** compared to non-modular designs[cite: 91].

---

## 🎓 Academic Context: Computer Architecture
[cite_start]This project was developed as a core component of the Computer Architecture coursework[cite: 89]. It demonstrates the practical application of digital logic theory to physical hardware design.

### Practical Skills Demonstrated
* [cite_start]**Datapath Design & Control:** Engineered a unified datapath that routes data between Arithmetic, Logic, and Shift units based on a 4-bit Control Signal (`AluOp`)[cite: 296, 308].
* [cite_start]**Constraint-Driven Engineering:** Adhered to strict requirements for "Structural Modeling," instantiating distinct components (Muxes, Adders, Gates) rather than relying on abstract behavioral code[cite: 95].
* [cite_start]**Signed Arithmetic Implementation:** Applied **2's Complement** logic to handle reverse subtraction ($B - A$) and overflow detection using hardware bit-manipulation[cite: 84, 349].
* [cite_start]**Optimization Strategy:** Implemented a "Zero-Gate Cost" shift unit using static wiring to eliminate active logic overhead for shift/rotate operations[cite: 145, 321].
* [cite_start]**Verification:** Developed a self-checking testbench achieving **100% functional coverage** across 14 distinct edge cases[cite: 364].

---

## 🛠 Architecture

[cite_start]The system is decomposed into three parallel structural blocks, selected by a final 16-to-1 Multiplexer tree[cite: 296].

### 1. Arithmetic Unit ("Smart Adder")
* [cite_start]**Concept:** A single Adder instance is reused for all math operations[cite: 73].
* [cite_start]**Mechanism:** A 4-to-1 MUX selects the second operand ($B$, $\sim A$, $0$, or $\sim B$) based on the operation[cite: 79].
* [cite_start]**Carry Logic:** The Carry-In ($Cin$) is logically manipulated to handle 2's complement inversion and increments[cite: 80].

### 2. Logic Unit (Parallel Execution)
* [cite_start]**Concept:** Bitwise operations (AND, OR, NAND, NOT) are executed in parallel arrays[cite: 317].
* [cite_start]**Benefit:** Eliminates pre-selection latency, ensuring high-speed signal propagation[cite: 153].

### 3. Shift & Rotate Unit (Static Wiring)
* [cite_start]**Concept:** Operations are implemented by physically rearranging bit connections (e.g., wiring bit 6 to bit 7)[cite: 323].
* [cite_start]**Cost:** Consumes **zero active logic gates**[cite: 148].



---

## ⚙️ Instruction Set Architecture (ISA)

[cite_start]The ALU supports 12 operations controlled by the 4-bit `AluOp` input[cite: 377].

| AluOp | Operation | Description | Hardware Implementation |
| :--- | :--- | :--- | :--- |
| `0000` | $A + B$ | Signed Addition | [cite_start]Adder ($A + B + 0$) [cite: 82] |
| `0001` | $B - A$ | Reverse Subtraction | [cite_start]Adder ($B + \sim A + 1$) [cite: 83] |
| `0010` | $A + 1$ | Increment A | [cite_start]Adder ($A + 0 + 1$) [cite: 85] |
| `0101` | $A == B$ | Compare | Subtraction check; [cite_start]Zero flag set if result is 0 [cite: 86] |
| `0110` | $B \ll 1$ | Arithmetic Left Shift | [cite_start]Hardwired Shift (LSB tied to 0) [cite: 136] |
| `0111` | $B \gg 1$ | Arithmetic Right Shift | [cite_start]Hardwired with Sign Extension [cite: 137] |
| `1100` | $A \lll 1$ | Rotate Left | [cite_start]MSB wraps to LSB [cite: 139] |
| `1101` | $A \ggg 1$ | Rotate Right | [cite_start]LSB wraps to MSB [cite: 141] |
| `1xxx` | Logic | AND, OR, NAND, NOT | [cite_start]Parallel Gate Arrays [cite: 126] |

---

Engineered and developed by Mousa Mohamed Mousa.
