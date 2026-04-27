# Nand2Tetris – Project 4: Machine Language

## Overview

Project 4 of the [Nand2Tetris](https://www.nand2tetris.org/) course introduces **machine language programming** using the **Hack assembly language**. The goal is to gain a deep understanding of how low-level programs interact directly with the hardware — the CPU, memory, and I/O devices — that were built in the previous projects.

By writing programs at the assembly level, you bridge the gap between the high-level languages that programmers use daily and the binary instructions that a computer actually executes.

---

## The Hack Computer Architecture

The Hack computer is a simple 16-bit machine composed of:

- **CPU** – Executes A-instructions and C-instructions.
- **Instruction Memory (ROM)** – Stores the program to be executed (read-only).
- **Data Memory (RAM)** – General-purpose memory plus memory-mapped I/O:
  - `RAM[0]`–`RAM[15]` → Virtual registers `R0`–`R15`
  - `RAM[16384]`–`RAM[24575]` → Screen memory map (256 rows × 512 pixels, 1 bit per pixel)
  - `RAM[24576]` → Keyboard memory map (holds the scan code of the currently pressed key)

---

## Hack Assembly Language

Hack programs are written in a simple assembly language with two types of instructions:

### A-Instruction (Address Instruction)
```
@value
```
Loads a constant or symbol into the **A register**. This sets the current memory address for subsequent C-instructions.

**Example:**
```asm
@100   // A = 100
```

### C-Instruction (Compute Instruction)
```
dest=comp;jump
```
Performs a computation, optionally stores the result, and optionally performs a jump.

**Example:**
```asm
D=D+1      // D = D + 1
D;JGT      // if D > 0, jump to address in A
```

### Symbols and Labels
- **Predefined symbols**: `R0`–`R15`, `SCREEN`, `KBD`, `SP`, `LCL`, `ARG`, `THIS`, `THAT`
- **Labels**: `(LOOP)` defines a label pointing to the next instruction
- **Variables**: Any new symbol referenced with `@symbol` is allocated to RAM starting at address 16

---

## Programs Implemented

### 1. `Mult.asm` – Multiplication

**Goal:** Compute `R2 = R0 * R1` using only addition (the Hack ALU has no multiply instruction).

**How it works:**
- Reads the values stored in `R0` and `R1`.
- Adds `R0` to a running sum `R1` times using a loop.
- Stores the final result in `R2`.

**Pseudocode:**
```
R2 = 0
i = R1
LOOP:
  if i == 0 goto END
  R2 = R2 + R0
  i = i - 1
  goto LOOP
END:
```

---

### 2. `Fill.asm` – Screen I/O

**Goal:** Listen to keyboard input and fill or clear the entire screen accordingly.

**How it works:**
- Continuously polls `RAM[24576]` (the keyboard register).
- If a key is pressed (value ≠ 0), it fills every pixel on the screen with black (`-1` / `1111111111111111`).
- If no key is pressed (value = 0), it clears the entire screen to white (`0`).

**Pseudocode:**
```
LOOP:
  if KBD == 0 goto CLEAR
  fill screen with -1
  goto LOOP
CLEAR:
  fill screen with 0
  goto LOOP
```

---

## How to Run

1. Download and open the **CPU Emulator** from the [Nand2Tetris software suite](https://www.nand2tetris.org/software).
2. Load the `.asm` file (e.g., `Mult.asm` or `Fill.asm`) into the emulator.
3. The emulator will automatically translate the assembly into binary and load it into the ROM.
4. Set the desired input values in RAM (e.g., `R0 = 3`, `R1 = 4` for Mult) and run the program.
5. Verify the output (e.g., `R2` should equal `12` for the multiplication example).

---

## Key Concepts Learned

- How machine language instructions map to hardware operations
- Memory-mapped I/O and how programs interact with screens and keyboards at the hardware level
- Writing loops, conditionals, and subroutines in assembly
- The distinction between symbolic assembly code and binary machine code
- How an assembler translates symbols and labels into memory addresses

---

## Project Structure

```
Nand2TetrisProject4/
├── Mult.asm     # Multiplication program (R2 = R0 * R1)
└── Fill.asm     # Screen I/O program (keyboard-controlled fill)
```

---

## References

- [Nand2Tetris Official Website](https://www.nand2tetris.org/)
- [The Elements of Computing Systems](https://mitpress.mit.edu/9780262539807/the-elements-of-computing-systems/) – Nisan & Schocken (MIT Press)
- [Hack Assembly Language Specification](https://www.nand2tetris.org/project04)
