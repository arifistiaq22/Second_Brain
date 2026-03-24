---
tags:
  - digital-electronics
  - electronics
  - logic
  - vlsi
  - youtube
date: 2026-03-19
link: https://youtu.be/PVNAPWUxZ0g?si=0Swd77e3lVkZa-Gu
---
### 1. The Clock Signal and Edge Detection

The video starts by defining the clock signal not as a magical metronome, but as a simple oscillating electrical signal (a square wave) that alternates between 0 and 1.

- **The "Edges":** The computer doesn't care about the periods where the signal is steadily 1 or 0 (these are "dead zones"). The critical moments are the **Rising Edge** (the exact moment the signal flips from 0 to 1) and the **Falling Edge** (when it flips from 1 to 0) [[01:30](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=90)].
- **Edge Detectors:** By using an AND gate and an inverter, engineers can build a circuit that takes advantage of tiny, nanosecond physical delays in the wiring. This circuit outputs a tiny "pulse" _only_ at the exact moment of a Rising Edge [[02:02](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=122)].

### 2. From Latches to Flip-Flops

In previous videos, the creator explained **Latches** (memory cells that store a 1 or a 0). However, standard latches are "Level Triggered"—meaning they can be overwritten at any time as long as the "Enable" wire is set to 1.

- **The Flip-Flop:** By placing an Edge Detector between the clock and a latch's Enable wire, the latch becomes **Edge Triggered** [[06:28](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=388)]. It will _only_ accept new data at the precise nanosecond the clock pulses from 0 to 1. This new component is called a Flip-Flop.

### 3. The JK Flip-Flop (Toggling)

A specific type of flip-flop, the **JK Flip-Flop**, solves a problem with older latches where sending a "Set" and "Reset" signal at the same time caused an error [[07:36](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=456)].

- If both the J (Set) and K (Reset) inputs are activated simultaneously during a Rising Edge, the JK Flip-Flop simply **toggles** its current state (if it was 1, it becomes 0; if it was 0, it becomes 1) [[08:16](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=496)].

### 4. Creating a Binary Counter

If you permanently wire the J and K inputs to "ON" (e.g., 5 Volts) and connect the clock to it, the JK Flip-Flop will automatically toggle its output on every single clock pulse. Because it takes two clock pulses to complete a full 0-to-1-to-0 cycle, the flip-flop's output oscillates at exactly **half the speed** of the main clock [[08:52](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=532)].

- **Chaining Flip-Flops:** If you take the output of that first flip-flop and use it as the clock input for a _second_ flip-flop, the second one oscillates at a quarter of the speed. You can chain as many together as you want [[11:09](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=669)].
- **Counting in Binary:** If you look at a chain of four flip-flops acting together, their outputs (`0000`, `0001`, `0010`, `0011`...) form a perfect binary counting sequence that increments by 1 on every pulse of the main clock. This circuit is called a **Binary Counter** [[12:38](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=758)].

### 5. Orchestrating the CPU Stages

Finally, the video pieces these concepts together to answer the main question: _How does the CPU know which step to execute?_

- Assume a CPU takes four steps to run code: Fetch, Decode, Execute, Increment.
- To cycle between four options, the CPU needs a 2-bit binary number (`00`, `01`, `10`, `11`).
- The CPU uses a 2-bit Binary Counter (made of two JK Flip-Flops) connected directly to the system clock [[13:19](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=799)].
- The output of this counter is fed into a **Binary Decoder**.
- **The Result:** On the first clock pulse, the counter outputs `00`, and the decoder activates all the physical wires needed for the **Fetch** stage [[09:40](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=580)]. On the next pulse, the counter clicks to `01`, turning off the Fetch wires and activating the wires for the **Decode** stage. This continues sequentially, looping back to `00` after the fourth step [[13:26](http://www.youtube.com/watch?v=PVNAPWUxZ0g&t=806)].

By utilizing this counter and decoder system, the incredibly fast oscillation of a quartz crystal clock physically forces the CPU to march step-by-step through its instructions without skipping a beat.