---
tags:
  - youtube
  - digital-electronics
  - electronics
  - vlsi
  - logic
date: 2026-03-19
link: https://youtu.be/GYlNoAMBY6o?si=X07T9DC_vvXnaU3t
---
### 1. Optimizing Wires with a Data Bus

The video begins by addressing the problem of too many wires. If every register (memory cell) in a CPU had separate input and output wires connected to every other component, the physical wiring would be impossible.

- **The Data Bus:** To solve this, the inputs and outputs of all registers share a single set of shared wires called a "Data Bus" [[01:38](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=98)].
- **Enable Signals:** Because all registers share the same bus, the CPU uses **Read Enable** and **Write Enable** signals. When data is on the bus, only the register with its "Write Enable" activated will accept and store that data. Conversely, only a register with its "Read Enable" activated will output its stored data onto the bus [[01:51](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=111)].

### 2. Executing Basic Instructions (Hardware Level)

The video explains how Assembly code instructions—specifically **Load** and **Store**—actually work at the physical hardware level using these enable signals.

- **The LOAD Instruction:** (Moving data from RAM into a CPU Register) [[03:05](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=185)]
    
    1. The CPU sends a specific memory address to the RAM via the Address Bus.
    2. The CPU activates the RAM's "Read Enable" signal, placing the data from that address onto the shared Data Bus.
    3. Simultaneously, the CPU activates the "Write Enable" signal of a specific target Register (e.g., Register 2).
    4. The data flows from the RAM, across the bus, and is saved into the register.
- **The STORE Instruction:** (Moving data from a CPU Register into RAM) [[04:20](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=260)]
    
    1. The CPU activates the "Read Enable" of a specific Register (e.g., Register 1), placing its data onto the Data Bus.
    2. The CPU sends the target memory address to the RAM via the Address Bus.
    3. The CPU activates the RAM's "Write Enable" signal, forcing the RAM to save the data from the bus into that specific address.

### 3. Decoding the Instructions

How does the hardware know _which_ enable wires to turn on? The answer is the binary instruction itself.

- When code is compiled into machine code (1s and 0s), those bits act as direct inputs to **Binary Decoders** [[06:25](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=385)].
    
- For example, an 8-bit instruction might be structured like this: `01 10 1111`
    
    - **Bits 1-2 (`01`):** Might mean "LOAD". This tells the main decoder to activate the memory read signals.
    - **Bits 3-4 (`10`):** Indicates the target Register (Register 2). This activates Register 2's Write Enable.
    - **Bits 5-8 (`1111`):** The binary memory address (Address 15) sent to the RAM [[07:05](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=425)].

### 4. The Arithmetic Logic Unit (ALU) & Multiplexers

When executing math (like addition), the CPU uses the Arithmetic Logic Unit (ALU).

- Because the ALU needs two numbers (operands) to do math, the CPU must read from two registers at the same time. This bypasses the general Data Bus.
- **Multiplexers:** The CPU uses multiplexers (which contain decoders) to select exactly which two registers will connect directly to the ALU's inputs for that specific calculation [[12:51](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=771)].
- The result is temporarily saved in a hidden internal register before being written back to the target general-purpose register [[13:59](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=839)].

### 5. The Fetch-Decode-Execute Cycle

The most critical part of the video explains the "Control Unit" and the fundamental loop that makes a computer run: The Fetch-Decode-Execute cycle [[15:02](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=902)].

- **The Address Register (Program Counter):** This internal register keeps track of where the CPU is in the program. When the computer turns on, it points to address `0` [[15:59](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=959)].
- **Stage 1: FETCH.** The CPU uses the Address Register to read the next instruction from RAM and loads it into a special **Instruction Register** [[16:28](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=988)].
- **Stage 2: DECODE.** The bits inside the Instruction Register are fed into the hardware decoders. This sets up all the correct "Read" and "Write" enable signals across the entire CPU [[16:36](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=996)].
- **Stage 3: EXECUTE.** The actual data is moved, or the math is performed, based on the open pathways created in the Decode stage [[16:49](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=1009)].
- **Increment:** Finally, the Address Register increments by 1 (pointing to the next line of code), and the cycle repeats endlessly [[17:12](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=1032)].

By running this cycle continuously, the CPU can read variables from memory, perform math on them, and store the results—which is exactly what happens when you write a simple line of high-level code like `int a = 5 + 3;` [[18:59](http://www.youtube.com/watch?v=GYlNoAMBY6o&t=1139)].