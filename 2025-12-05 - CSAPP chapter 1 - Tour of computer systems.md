---
tags:
  - books
  - comp-architecture
  - low-level
---
# CSAPP chapter 1 - Tour of computer systems

Programs start their lives as text files, which contain sequences of bytes, each representing an integer that maps to an ASCII character. E.g. 97 --> "a".

All files are either text (all ASCII) or binary. A file's content is interpreted based on context. E.g. A text editor tries to interpret bytes as ASCII characters.

A program's source file (text) is compiled into an _executable object file, AKA executable object program_ (binary machine instructions).

**The compilation process** (`gcc -o hello hello.c`):
1. **Preprocessing phase**
    - `hello.c --[preprocessor (cpp)]--> hello.i (modified source file`
    - Preprocessor modifies `hello.c` according to `#` directives. E.g. `#include <stdio.h>` tells it to include the contents of `stdio.h`.
2. **Compilation phase**
    - `hello.i --[compiler (cci)]--> hello.s (assembly program)`
    - Compiler translates `hello.i` into assembly language (textual representation of machine instructions).
    - Note: `hello.s` is still a text file.
3. **Assembly phase**
    - `hello.s --[assembler (as)]--> hello.o (object file, binary)`
    - Assembler translates `hello.s` into a _relocatable object program (aka object file)_, `hello.o`, consisting of binary machine instructions.
4. **Linking phase**
    - `hello.o --[linker (ld)]--> hello (executable object program/file)`
    - Linker locates object files for any libraries (e.g. `printf.o`) and merges them with `hello.o` to produce the final executable program.

Note: A shell is a special type of program that interprets command lines. It allows us to run executable files.

**Hardware organisation**
![[csapp_hardware_organisation.png]]
1. **Buses** - Carry bytes of data between system components. Number of bytes transferred per operation is called the _word size_ - typically 4 or 8 bytes (32 or 64 bits) in modern systems.
2. **I/O devices** - Receive/display input/output from the physical world. E.g. Keyboard, mouse, monitor. Connected to the I/O bus via either a _controller (chip set built into the device, or on the motherboard)_ or an _adapter (card that is plugged into a motherboard slot)_.
3. **Main memory** - Temporary storage device made up of DRAM chips. Memory is logically arranged as a linear array of bytes, each with its own memory address. Stores machine instructions and variable values during program execution.
4. **Processor (CPU)** - Processing engine of the computer. Consists of the program counter (stores the memory address of the next instruction to execute), the register file (a collection of registers that store temporary values), and the Arithmetic/Logic Unit (ALU, performs simple computations). Typical CPU operation:
    1. Read the next instruction from main memory based on the address stored in the PC.
    2. Interpret and execute the instruction. May involve (non-exhaustive)
        1. **Load** - copy a value from main memory to a register, overwriting the previous value.
        2. **Store** - copy a value from a register to main memory, overwriting the previous value.
        3. **Operate** - copy the contents of two registers into the ALU, perform some arithmetic operation, then copy the results into a new register.
        4. **Jump** - Extract and copy a word from the instruction into the PC to change the next instruction's location.
    3. Update the PC to point to the next instruction.

Note: A CPU's _instruction set architecture (ISA)_ describes the available instructions and their effects. In a sense, a CPU is an implementation of its ISA. This contrasts with its _microarchitecture_, which describes HOW the CPU executes certain instructions (usually tuned for performance). **Implication: Multiple CPUs can implement the same ISA using different architectures, leading to different performance characteristics.**

Bookmark: PDF page 40