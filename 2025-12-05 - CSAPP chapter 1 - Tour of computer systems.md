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

**Caches and why they matter**
- Cache memory:
    - Small and fast data storage devices that live in the processor.
    - Temporarily stores data that could be reused by the processor in the near future (alternative to main memory).
    - Implemented with special hardware called _Static Random Access Memory (SRAM)_.
- Why caches?
    - Running programs require data transfer between system components (e.g. disk --> main memory --> CPU registers).
    - Larger storage devices = slower data transfer speeds = more non-computational overhead. E.g. Transferring a word from disk to CPU takes 10,000,000 times longer than from main memory.
        - _Processor-memory gap_: Difference between processor's computation speed and speed of data transfer to/from main memory. Gap has been increasing because processor performance improves much faster than memory performance.
- Why do caches work?
    - They exploit the concept of _data locality_: The tendency for programs to repeatedly access data and code in the same region.
    - Caches allow machines to simulate speedy AND large memory.
- Modern machines have multiple levels of caches - L1, L2, and L3 - with increasing size but decreasing access speed.
    - Note: Although access speed decreases along the cache hierarchy, it's still faster than accessing data from main memory.

**More general idea behind caching: The Memory Hierarchy.**
- Idea: Different types of memory in a computer can be organised in a hierarchy, where each level serves as a "cache" for the level below it. E.g. CPU registers (L0) cache data/instructions from L1 cache (L1), and so on.
- Full hierarchy:
    - 0: CPU registers (aka the register file)
    - 1: L1 cache
    - 2: L2 cache
    - 3: L3 cache
    - 4: Main memory
    - 5: Local disk
    - 6: Remote disk (e.g. network file servers)

![[csapp_example_memory_hierarchy.png]]

**Note: Programs don't directly access system components like main memory or I/O devices. Access is mediated by the OS.**

Primary purposes of the OS:
1. Prevent misbehaving applications from hogging system resources.
2. Provide a uniform interface to access system resources (because different hardware vendors may provide different interfaces).

Core abstractions provided by the OS
1. Files - represent I/O devices such as disks and network cards.
2. Virtual memory - represent main memory and disk I/O devices.
3. Processes - represent running applications (processor + main memory + I/O devices).

The "process" abstraction
- OS creates a _process_ for each running application.
- Gives the illusion that each application has exclusive use of the hardware.
- The "process" abstraction allows a single CPU to run multiple processes _concurrently_ - i.e. interleave their instructions.
- OS switches between applications via _context switching_ - i.e. save all data and instructions relevant to a process (its _context_) and hand over control to another process.
    - Note: Context switching often happens to maximise CPU usage. E.g. Switch to another process when one process is waiting for I/O.
- Process A switches to process B by means of a _system call_, which passes control to the OS.
- Context switching is managed by the OS _kernel_, which is a part of the OS that always resides in memory.
    - Note: **The kernel is not a separate process.** It consists of code and data structures that the OS uses to manage processes.

![[csapp_context_switching.png]]

Bookmark: Pg 48, 1.7.2 Threads.