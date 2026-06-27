# CPU Architecture Overview

## System Diagram

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

## System Clock

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

## Component Definitions

### Control Unit (CU)
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