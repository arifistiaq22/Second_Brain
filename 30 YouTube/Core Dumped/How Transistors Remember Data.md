---
tags:
  - youtube
  - electronics
  - vlsi
  - digital-electronics
  - logic
date: 2026-03-17
link: https://youtu.be/rM9BjciBLmg?si=3wgtGMesr71WHqP0
---
### The Fundamentals of Remembering Data

The video starts by explaining that traditional logic gates (like AND and OR gates) simply pass signals forward. To create "memory," the output of a circuit must be fed back into its own input.

- **Feedback Loops:** If you take an OR gate and connect its output back to one of its inputs, turning it "on" (1) will cause it to stay on indefinitely, even if the original input is removed [[01:43](http://www.youtube.com/watch?v=rM9BjciBLmg&t=103)]. It essentially "remembers" that it was turned on.
- Similarly, connecting the output of an AND gate back to its input can create a circuit that "remembers" a zero [[02:19](http://www.youtube.com/watch?v=rM9BjciBLmg&t=139)].

### Building a Latch (1-bit Memory)

Individually, these feedback loops aren't very useful because they can only remember one state (either a 1 or a 0). The solution is to combine them.

- **The AND-OR Latch:** By combining an AND gate, an OR gate, and an inverter, we can create a circuit that can be explicitly set to 1 or reset to 0 [[03:53](http://www.youtube.com/watch?v=rM9BjciBLmg&t=233)].
- **The Gated Latch:** To make this practical for a computer, a **Write Enable** line is added [[05:04](http://www.youtube.com/watch?v=rM9BjciBLmg&t=304)].
    - When _Write Enable_ is 0, the latch ignores all incoming data and just holds its current value.
    - When _Write Enable_ is 1, the latch accepts and stores the new data coming in.

### From Latches to Registers

A single gated latch stores exactly 1 bit of information.

- **Registers:** To store useful information (like a byte, which is 8 bits), eight latches are placed side-by-side. Their _Write Enable_ lines are tied together so that all 8 bits can be updated simultaneously [[07:33](http://www.youtube.com/watch?v=rM9BjciBLmg&t=453)]. These are the registers you often hear about inside CPUs.
- **The Scaling Problem:** If you want to store 1 Gigabyte of memory using standard registers, you would need millions of individual wires for inputs and outputs, which is physically impossible to manage [[08:34](http://www.youtube.com/watch?v=rM9BjciBLmg&t=514)].

### Building a Memory Matrix (RAM)

To solve the wiring problem, latches are arranged into a grid or **Matrix**.

- **Row and Column Addresses:** Instead of wiring every single latch directly to the CPU, they are arranged in a grid (e.g., a 4x4 matrix). By activating one specific "Row" wire and one specific "Column" wire, you can pinpoint a single, unique latch [[08:54](http://www.youtube.com/watch?v=rM9BjciBLmg&t=534)]. This intersection point is its **Memory Address**.
- **Decoders:** The computer uses binary decoders to translate a binary memory address into the specific active row and column wires needed to target the correct latch [[09:25](http://www.youtube.com/watch?v=rM9BjciBLmg&t=565)].
- **Shared Data Lines:** Because only one latch in the matrix is ever activated at a time, all the latches can share a single common "Data In" wire and a single "Data Out" wire, drastically reducing the physical wiring required [[11:11](http://www.youtube.com/watch?v=rM9BjciBLmg&t=671)].
- **Read/Write Enable:** Additional logic gates (AND gates) ensure that the _Read_ or _Write_ commands only affect the single latch currently targeted by the Row/Column decoders [[11:46](http://www.youtube.com/watch?v=rM9BjciBLmg&t=706)].

### Scaling to Bytes and Beyond

While the matrix handles single bits, computers operate on Bytes.

- To store bytes, the computer uses **8 parallel matrices**. When a memory address is called, the same address is targeted across all 8 matrices simultaneously, allowing an entire 8-bit byte to be read or written at once [[14:20](http://www.youtube.com/watch?v=rM9BjciBLmg&t=860)].
- **The Address Bus:** The maximum amount of RAM a computer can support is dictated by the width of its Address Bus. A 32-bit address bus can only generate enough unique binary addresses to map out about 4 Billion bytes, which is why older 32-bit computers maxed out at exactly 4GB of RAM [[16:00](http://www.youtube.com/watch?v=rM9BjciBLmg&t=960)].

### Static vs. Dynamic RAM

The video concludes by noting that the matrix of logic gates demonstrated is called **Static RAM (SRAM)**. It is incredibly fast but uses many transistors, making it expensive [[16:11](http://www.youtube.com/watch?v=rM9BjciBLmg&t=971)]. Most modern system memory is Dynamic RAM (DRAM), which uses a single transistor and a capacitor to save space and cost, though it is slightly slower.