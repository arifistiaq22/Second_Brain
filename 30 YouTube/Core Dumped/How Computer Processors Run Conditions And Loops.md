---
tags:
  - digital-electronics
  - electronics
  - logic
  - vlsi
  - youtube
date: 2026-03-19
link: https://youtu.be/Ui6QyzcD3_E?si=hKJ24Pk4QBlix_gO
---
### 1. The Fetch-Decode-Execute Cycle

Before diving into loops, the video recaps how a CPU normally reads code sequentially [[01:22](http://www.youtube.com/watch?v=Ui6QyzcD3_E&t=82)]:

1. **Fetch:** The CPU checks the "Address Register" (Program Counter) to see which memory address holds the next instruction, and reads that instruction from RAM.
2. **Decode:** The CPU figures out what the instruction means (e.g., Load a number, Add two numbers).
3. **Execute:** The CPU performs the math or moves the data.
4. **Increment:** The Address Register automatically increases by 1, pointing the CPU to the very next line of code in memory.

Because the Address Register naturally counts upward (1, 2, 3, 4...), programs naturally run in a strict top-to-bottom sequence.

### 2. The Unconditional Jump (Infinite Loops)

To create a loop, we need the CPU to stop going down the list and instead go back up.

- **The `JUMP` Instruction:** This instruction tells the CPU to explicitly overwrite the value in the Address Register with a specific number [[09:43](http://www.youtube.com/watch?v=Ui6QyzcD3_E&t=583)].
- If you put a `JUMP` instruction at memory address 10 that says "JUMP to Address 5", the CPU will execute lines 5, 6, 7, 8, 9, hit line 10, and immediately go back to line 5.
- This creates a permanent, **Infinite Loop** [[10:08](http://www.youtube.com/watch?v=Ui6QyzcD3_E&t=608)].

### 3. CPU Flags (How the computer "thinks")

To make a loop stop, or to run an `if` statement, the computer needs to make a decision based on a condition (e.g., "Is `x` less than 5?"). The CPU does this using the Arithmetic Logic Unit (ALU) and **Flags** [[10:50](http://www.youtube.com/watch?v=Ui6QyzcD3_E&t=650)].

- A "Flag" is a tiny 1-bit memory cell that turns on (`1`) or off (`0`) based on the result of the _last math equation_ the CPU did.
- **The Zero Flag:** Turns on if the result of an equation was exactly `0`.
- **The Negative Flag:** Turns on if the result of an equation was a negative number.
- **The Overflow Flag:** Turns on if the result was too big to fit in memory.

### 4. Making Decisions (Subtractions and Conditional Jumps)

If a programmer writes `if (a < 5)`, the CPU doesn't conceptually "understand" less-than. Instead, the compiler turns that question into a subtraction problem: **`a - 5`** [[12:45](http://www.youtube.com/watch?v=Ui6QyzcD3_E&t=765)].

By looking at the CPU flags after subtracting `5` from `a`, the computer knows exactly how the numbers relate:

- If `a` is 3: `3 - 5 = -2`. The **Negative Flag** turns on. (Therefore, `a` is less than 5).
- If `a` is 5: `5 - 5 = 0`. The **Zero Flag** turns on. (Therefore, `a` equals 5).
- If `a` is 8: `8 - 5 = 3`. Neither flag turns on. (Therefore, `a` is greater than 5).

**Conditional Jumps:** Once the math is done, the CPU uses special jump instructions that _only_ work if a specific flag is turned on [[11:57](http://www.youtube.com/watch?v=Ui6QyzcD3_E&t=717)].

- **`JUMP_NEGATIVE`:** Only jumps if the Negative Flag is currently on.
- **`JUMP_ZERO`:** Only jumps if the Zero Flag is currently on.

### 5. Putting it together: The `While` Loop

To build a `while (a < 5)` loop, the CPU runs this sequence of assembly instructions [[13:35](http://www.youtube.com/watch?v=Ui6QyzcD3_E&t=815)]:

1. **Subtract:** Calculate `a - 5`.
2. **Conditional Jump:** Use `JUMP_NEGATIVE` to jump into the main body of the loop. If `a` is less than 5, the flag is on, and the jump happens.
3. **Execute Loop:** Run the code inside the loop (e.g., add 1 to `a`).
4. **Unconditional Jump:** At the end of the loop body, use a regular `JUMP` to go all the way back up to Step 1 to do the math again.
5. **Break the Loop:** If `a` eventually reaches 5, `5 - 5 = 0`. The Negative flag does _not_ turn on. The `JUMP_NEGATIVE` instruction fails, and the CPU simply moves down to the next line of code, officially exiting the loop [[14:25](http://www.youtube.com/watch?v=Ui6QyzcD3_E&t=865)].

### 6. The `If` Statement

An `If` statement is identical to a `While` loop at the hardware level, with one minor difference: it lacks the Unconditional Jump at the end [[15:16](http://www.youtube.com/watch?v=Ui6QyzcD3_E&t=916)].

- It does the subtraction.
- It does the Conditional Jump.
- It executes the block of code once.
- Because there is no "jump back to the top" instruction at the end, it just continues running the rest of the program.