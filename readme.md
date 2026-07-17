# Rainfall - Binary Exploitation Challenge Series

This is a progressive binary exploitation challenge series designed to teach fundamental concepts of memory vulnerabilities and exploitation techniques.

## Overview

The Rainfall challenges are organized into levels (0-9) and bonus levels (0-3), each introducing progressively more complex exploitation concepts. Each level contains:

- **source** - The vulnerable C source code
- **flag** - The solution/flag for that level
- **walkthrough** - Detailed explanation of the vulnerability and how to exploit it
- **Ressources/commands.md** - Additional commands and resources

## Level Progression & Dependencies

```
START
  ↓
[Level 0] - Basic Integer Comparison & Command-line Arguments
  ↓
[Level 1] - Buffer Overflow & Return-to-libc Exploitation
  ↓
[Level 2] - Buffer Overflow with Shellcode Injection
  ↓
[Level 3] - Format String Vulnerabilities
  ↓
[Level 4] - Advanced Format String Exploitation
  ↓
[Level 5] - Function Pointer Overwriting
  ↓
[Level 6] - Global Variable Manipulation via Format Strings
  ↓
[Level 7] - Executable Stack & Direct Shellcode Execution
  ↓
[Level 8] - Memory Management Exploitation
  ↓
[Level 9] - Advanced Exploitation Techniques
  ↓
[Bonus 0] - Buffer Overflow with String Concatenation
  ↓
[Bonus 1] - Additional Exploitation Challenge
  ↓
[Bonus 2] - Advanced Techniques
  ↓
[Bonus 3] - Final Challenge
```

## Quick Start

To begin solving the challenges:

1. Start with **Level 0** - Read the `source` file to understand the vulnerability
2. Read the `walkthrough` for hints and methodology
3. Check `Ressources/commands.md` for useful tools and commands
4. Capture the flag by exploiting the vulnerability
5. Progress to the next level once you've solved the current one

## Key Concepts by Level

| Level | Primary Concept | Difficulty |
|-------|-----------------|-----------|
| 0 | Argument parsing, number conversion | ⭐ |
| 1 | Buffer overflow, return address override | ⭐⭐ |
| 2 | Shellcode injection, overflow chains | ⭐⭐ |
| 3 | Format string vulnerabilities | ⭐⭐⭐ |
| 4 | Format string advanced | ⭐⭐⭐ |
| 5 | Function pointer hijacking | ⭐⭐⭐ |
| 6 | Global variable corruption | ⭐⭐⭐⭐ |
| 7 | Direct code execution | ⭐⭐⭐⭐ |
| 8 | Memory management | ⭐⭐⭐⭐ |
| 9 | Complex exploitation chains | ⭐⭐⭐⭐⭐ |
| Bonus 0-3 | Advanced techniques & combinations | ⭐⭐⭐⭐⭐ |

## Prerequisites

- Understanding of x86 assembly language
- Knowledge of memory layout (stack, heap, code segment)
- Familiarity with tools: GDB, objdump, strings, python
- Linux command-line experience
- C programming basics

## Tools You'll Need

- **gdb** - GNU Debugger for binary analysis
- **objdump** - Disassembler and binary tool
- **strings** - Extract readable strings from binaries
- **python/python3** - Exploit payload generation
- **ltrace/strace** - System call tracing
- **xxd/hexdump** - Hexadecimal visualization

## Recommended Learning Path

1. Complete levels 0-2 to understand buffer overflows
2. Complete levels 3-4 to master format strings
3. Complete levels 5-7 to learn advanced hijacking techniques
4. Complete levels 8-9 for complex exploitation chains
5. Complete bonus levels for additional challenges

## Resources

Each level directory contains:
- Vulnerable binary (executable)
- Source code analysis in `source` file
- Step-by-step walkthrough in `walkthrough` file
- Useful commands in `Ressources/commands.md`

## Notes

- Always read the walkthrough after attempting a challenge
- Use GDB to understand the exact memory layout
- Test exploits locally before attempting on the actual binary
- Understand WHY an exploit works, not just HOW to run it

---

## CPU Architecture Overview

### System Diagram

```
┌────────────────────────────────────────────────────────────────────────┐
│                              CPU (Processor)                           │
│                                                                        │
│   ┌────────────────────────────────────────────────────────────────┐   │
│   │                      Control Unit (CU)                         │   │
│   │     Fetches & decodes instructions from RAM/Cache              │   │
│   └────────────────────────────────────────────────────────────────┘   │
│              │ (Sends signals)                ▲ (Reads data)           │
│              ▼                                │                        │
│   ┌──────────────────────┐        ┌────────────────────────────────┐   │
│   │  Arithmetic Logic    │        │          Registers             │   │
│   │     Unit (ALU)       │ <────> │  Ultra-fast local storage:     │   │
│   │                      │        │  PC, IR, etc.                  │   │
│   └──────────────────────┘        └────────────────────────────────┘   │
│              ▲                                    ▲                    │
│              └──────────────┬─────────────────────┘                    │
│                             ▼                                          │
│   ┌────────────────────────────────────────────────────────────────┐   │
│   │              Internal Cache Memory (L1 / L2)                   │   │
│   └────────────────────────────────────────────────────────────────┘   │
└─────────────────────────────┬──────────────────────────────────────────┘
                              ▼ (System Bus)
┌────────────────────────────────────────────────────────────────────────┐
│                        Main Memory (RAM)                               │
└────────────────────────────────────────────────────────────────────────┘
```

### System Clock

```
            ┌──────────────────────────────┐
            │    System Clock (Oscillator) │
            └──────────────────────────────┘
                    │         │         │
                    ▼         ▼         ▼
                  CU        ALU     Registers
            (Synchronized pulses)
```

---

### Component Definitions

#### Control Unit (CU)
Acts as the processor's supervisor. It:
- Fetches instructions from the computer's memory
- Decodes what they mean
- Directs the flow of data to other internal components

### Arithmetic Logic Unit (ALU)
The computational engine of the CPU. It:
- Performs basic mathematical calculations (addition, subtraction, etc.)
- Performs logic comparisons (AND, OR, NOT operations)

### Registers
Microscopic, ultra-fast temporary storage slots located inside the CPU. They:
- Hold data that the ALU is actively working on
- Store the exact memory location of the next line of code to execute

### System Clock
An internal crystal oscillator that:
- Sends out steady electrical pulses
- Synchronizes all movement of bits through the CPU gates
- Speed is measured in Gigahertz (GHz)

### ============== STACK DIAGRAM ==============

```text
               TOP (Lower Addresses)
                     │
                     ▼
+----------------------------------------------+
| C()                                          |
+----------------------------------------------+
| B()                                          |
+----------------------------------------------+
| A()                                          |
+----------------------------------------------+
|              MAIN STACK FRAME                |
+----------------------------------------------+
| 4. Local Variables                           |
|    (int a, char buffer[64], ...)             |
+----------------------------------------------+
| 3. Saved EBP                                 |
+----------------------------------------------+
| 2. Return Address                            |
+----------------------------------------------+
| 1. Function Arguments                        |
+----------------------------------------------+
| MAIN()                                       |
+----------------------------------------------+
                     ▲
                     │
             BOTTOM (Higher Addresses)
```