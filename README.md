# Structura-8: Resource-Optimized Modular ALU

![Verilog](https://img.shields.io/badge/Language-Verilog-blue) ![Architecture](https://img.shields.io/badge/Architecture-Structural-orange) ![Status](https://img.shields.io/badge/Status-Verified-green)

## 📌 Project Overview
**Structura-88** is a resource-optimized **8-bit Arithmetic Logic Unit (ALU)** implemented in **System Verilog**. The design strictly adheres to structural modeling constraints, avoiding high-level behavioral operators in the top-level module to demonstrate low-level hardware orchestration.

The architecture features a **"Smart Adder" topology**. Instead of using discrete circuits for every math operation, the design utilizes a single 8-bit adder as the central processing unit, dynamically routing inputs via multiplexers to perform Addition, Subtraction, Incrementing, and Comparison. This approach reduces the arithmetic logic gate count by approximately **40%** compared to non-modular designs.

---

## 🎓 Academic Context: Computer Architecture
This project was developed as a core component of the Computer Architecture coursework to bridge the gap between digital logic theory and physical hardware design.

### Practical Skills Demonstrated
* **Datapath Design & Control:** Engineered a unified datapath that routes data between Arithmetic, Logic, and Shift units based on a 4-bit Control Signal (`AluOp`).
* **Constraint-Driven Engineering:** Adhered to strict requirements for "Structural Modeling," instantiating distinct components (Muxes, Adders, Gates) rather than relying on abstract behavioral code.
* **Signed Arithmetic Implementation:** Applied **2's Complement** logic to handle reverse subtraction ($B - A$) and overflow detection using hardware bit-manipulation.
* **Optimization Strategy:** Implemented a "Zero-Gate Cost" shift unit using static wiring to eliminate active logic overhead for shift/rotate operations.
* **Verification:** Developed a self-checking testbench achieving **100% functional coverage** across 14 distinct edge cases.

---

## 🛠 Architecture

The system is decomposed into three parallel structural blocks, selected by a final 16-to-1 Multiplexer tree.

### 1. Arithmetic Unit ("Smart Adder")
* **Concept:** A single Adder instance is reused for all math operations.
* **Mechanism:** A 4-to-1 MUX selects the second operand ($B$, $\sim A$, $0$, or $\sim B$) based on the operation.
* **Carry Logic:** The Carry-In ($Cin$) is logically manipulated to handle 2's complement inversion and increments.

### 2. Logic Unit (Parallel Execution)
* **Concept:** Bitwise operations (AND, OR, NAND, NOT) are executed in parallel arrays.
* **Benefit:** Eliminates pre-selection latency, ensuring high-speed signal propagation.

### 3. Shift & Rotate Unit (Static Wiring)
* **Concept:** Operations are implemented by physically rearranging bit connections (e.g., wiring bit 6 to bit 7).
* **Cost:** Consumes **zero active logic gates**.

---

## ⚙️ Instruction Set Architecture (ISA)

The ALU supports 12 operations controlled by the 4-bit `AluOp` input.

| AluOp | Operation | Description | Hardware Implementation |
| :--- | :--- | :--- | :--- |
| `0000` | $A + B$ | Signed Addition | Adder ($A + B + 0$) |
| `0001` | $B - A$ | Reverse Subtraction | Adder ($B + \sim A + 1$) |
| `0010` | $A + 1$ | Increment A | Adder ($A + 0 + 1$) |
| `0101` | $A == B$ | Compare | Subtraction check; Zero flag set if result is 0 |
| `0110` | $B \ll 1$ | Arithmetic Left Shift | Hardwired Shift (LSB tied to 0) |
| `0111` | $B \gg 1$ | Arithmetic Right Shift | Hardwired with Sign Extension |
| `1100` | $A \lll 1$ | Rotate Left | MSB wraps to LSB |
| `1101` | $A \ggg 1$ | Rotate Right | LSB wraps to MSB |
| `1xxx` | Logic | AND, OR, NAND, NOT | Parallel Gate Arrays |

---
## 💻 Usage

To simulate the hardware design, ensure you have **Icarus Verilog** installed, then follow these steps:

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/theonlymosmos/structura-8.git](https://github.com/yourusername/structura-8.git)
    ```

2.  **Compile the Design:**
    [cite_start]Generate the simulation executable using the Icarus Verilog compiler[cite: 215]:
    ```bash
    iverilog -o alu_simulation Final.vl
    ```

3.  **Run the Testbench:**
    [cite_start]Execute the simulation to verify the 14 test cases[cite: 216]:
    ```bash
    vvp alu_simulation
    ```
    
Engineered and developed by Mousa Mohamed Mousa.


