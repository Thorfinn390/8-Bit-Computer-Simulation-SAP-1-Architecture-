# 8-Bit Computer Simulation (SAP-1 Architecture)

<img width="1118" height="931" alt="image" src="https://github.com/user-attachments/assets/7bbac871-1212-4b2c-9f00-30a825389f26" />

Welcome to the repository of this fully functional **8-bit custom microprocessor simulation**. Heavily inspired by classic SAP (Simple As Possible) computer architectures and breadboard computer concepts, this project implements a complete Von Neumann computer architecture within a digital logic simulator environment.

The design features a centralized 8-bit data bus, a dedicated ALU with conditional flags, an automated control unit driven by microcode ROMs, and a clean decimal output display system.

---

## System Schematic

Below is the complete blueprint of the architecture, showing the interconnectivity of the registers, control lines, and the central data bus:

---

## Hardware Architecture Breakdown

The computer consists of several modular components interacting via a central **8-bit Data Bus**:

### 1. Core Execution Units

* **Register A (Accumulator):** The primary register involved in all arithmetic operations. It can latch data from the bus (`A_IN`) or output its current state to the bus (`A_OUT`).
* **Register B:** A secondary operand register dedicated to holding the second argument for the ALU (`B_IN`).
* **Arithmetic Logic Unit (ALU):** Computes the sum or difference of Register A and Register B. It supports subtraction via the `SUB` line and outputs the result to the bus (`ALU_OUT`).
* **Flags Register:** Latches conditional flags (`ZERO` and `COUT` / Carry Out) from the ALU on the `FLAGS_IN` signal to enable conditional branching.

### 2. Memory & Program Control

* **Program Counter (PC):** Tracks the memory address of the current instruction. It can increment automatically (`COUNTER_ENB`), output to the bus (`COUNTER_OUT`), or be overwritten for branching operations (`JUMP`).
* **Memory Address Register (MAR):** Latches the 4-bit memory address from the bus (`MAR_IN`) to index the RAM.
* **RAM Module:** Houses program code and data. Includes manual toggle switches and multiplexers for a **Program Mode** to directly write code into memory before execution.
* **Instruction Register (IR):** Splits the fetched byte into an operational code (Opcode) sent to the Control Unit, and an address/immediate value that can be output back onto the bus (`INR_OUT`).

### 3. Output & Control

* **Output Register & BCD Display:** Catches the final calculated values from the bus (`OUTPUT_REG_IN`) and translates binary data into a human-readable 3-digit signed decimal output (Hundreds, Tens, Ones).
* **Control Unit (Microcode ROMs):** The brain of the computer. It uses a step counter (`STEP CTN`) and two `2K x 8 ROM` matrices to decode instructions and systematically assert control flags across the entire system.

---

## Microcode Control Signals Matrix

The system operates deterministically based on the status of its control matrix. The control lines active during any given clock cycle determine the flow of data:

| Signal Name | Type | Description |
| --- | --- | --- |
| `HALT` | Output | Stops the system clock simulation. |
| `MAR_IN` | Input | Latches the current bus value into the Memory Address Register. |
| `RAM_IN` / `RAM_OUT` | I/O | Writes the bus value to RAM / Outputs the active RAM contents to the bus. |
| `INR_IN` / `INR_OUT` | I/O | Latches the bus value into the Instruction Register / Outputs the IR operand to the bus. |
| `A_IN` / `A_OUT` | I/O | Latches data from the bus into Register A / Registers A onto the bus. |
| `B_IN` | Input | Latches data from the bus into Register B. |
| `ALU_OUT` | Output | Pushes the computed ALU result onto the data bus. |
| `SUB` | Control | Switches the ALU configuration from Addition (LOW) to Subtraction (HIGH). |
| `COUNTER_ENB` | Control | Increments the Program Counter on the next clock pulse. |
| `COUNTER_OUT` | Output | Pushes the Program Counter value onto the bus. |
| `JUMP` | Control | Forces the Program Counter to latch the address currently on the bus. |
| `FLAGS_IN` | Input | Saves the current ALU status flags (`Carry` and `Zero`). |

---

## Getting Started & Simulation

To load and run this architecture locally, follow these steps:

1. **Download the Simulator:** Ensure you have a compatible digital logic simulator installed (Logisim Evolution).
2. **Open the Circuit:** Load the main circuit file.
3. **Program the RAM:**
* Toggle the `PROGRAM_MODE` switch to `1`.
* Use the DIP switches to select an address and input your custom binary opcodes.
* Toggle the write switch to save your instructions.

4. **Execute:** Switch `PROGRAM_MODE` back to `0`, trigger `RESET` to clear the registers, and toggle `CLK_ENB` to start executing your code!

> **Note:** For debugging individual microcode cycles, turn off `CLK_ENB` and use the `MANUAL_TICK` button to step through execution phases line-by-line.
