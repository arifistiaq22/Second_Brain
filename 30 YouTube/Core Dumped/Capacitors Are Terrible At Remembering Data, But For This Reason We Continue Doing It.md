---
tags:
  - digital-electronics
  - electronics
  - logic
  - youtube
  - vlsi
date: 2026-03-19
link: https://youtu.be/7WnbIeMgWYA?si=VGiH6bQ9g-Ott_Mi
---
### 1. The Dynamic Memory Cell (Mosfet + Capacitor)

In a previous video, the creator explained that Static RAM (SRAM) uses multiple transistors arranged in logic gates to trap a signal. Dynamic RAM uses a much simpler approach:

- **The Capacitor:** Acts like a tiny battery that stores an electrical charge. A charged capacitor represents a binary `1`, and a discharged capacitor represents a binary `0` [[01:45](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=105)].
- **The Mosfet (Transistor):** Acts as a gatekeeper. By applying voltage to the mosfet's "gate," it opens up, allowing electricity to flow in to charge the capacitor, or flow out to discharge it [[01:24](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=84)].
- **The Advantage:** Because a DRAM cell only requires one transistor and one tiny capacitor, you can pack millions more of them into the same physical area compared to SRAM. This makes DRAM highly cost-effective and capable of massive capacities (like the 16GB or 32GB sticks of RAM in your PC) [[02:49](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=169)].

### 2. The Big Problem: Destructive Reading

The massive drawback to using capacitors is how they are read.

- To check if a capacitor is storing a `1` (charged), the computer has to open the mosfet gate to see if electricity flows out.
- **Destructive Read:** The very act of checking the capacitor causes it to lose its charge [[03:17](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=197)]. Therefore, reading a bit of data in DRAM inherently destroys that data.

### 3. How DRAM Reads Without Losing Data

Because reading is destructive, DRAM requires complex circuitry to immediately save and rewrite the data back into the cell the moment it is read.

- **The Matrix:** Memory cells are arranged in a massive grid of rows (**Word Lines**) and columns (**Bit Lines**) [[03:53](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=233)].
- **Pre-charging:** Before reading, the bit lines are charged to exactly half the working voltage (e.g., 0.5 volts if the system runs on 1 volt) [[06:38](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=398)].
    
- **Sense Amplifiers:** When a row is activated, the capacitors interact with the 0.5V bit lines.
    
    - If a capacitor holds a `1` (1V), it leaks charge _into_ the bit line, slightly raising the bit line's voltage.
    - If a capacitor holds a `0` (0V), it pulls charge _from_ the bit line, slightly lowering the bit line's voltage [[07:00](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=420)].
- The **Sense Amplifier** detects this tiny voltage shift and instantly saves a solid `1` or `0` into a temporary SRAM latch [[07:21](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=441)].
    
- **The Rewrite:** Before the operation finishes, the sense amplifier forces that saved value back down the bit line, restoring the capacitor to its original charged or discharged state [[08:49](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=529)].
    

### 4. Writing Data to DRAM

Writing data uses the same underlying circuitry but in reverse, utilizing a **Demultiplexer (Demux)** [[09:46](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=586)].

- The bit lines are pre-charged, and the row is opened, causing all the capacitors in that row to dump their data into the sense amplifiers [[10:15](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=615)].
- The Demultiplexer overwrites _only_ the specific target latch inside the sense amplifier with the new data from the CPU [[10:43](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=643)].
- Finally, the sense amplifiers push all the values (the untouched original data _plus_ the newly overwritten bit) back into the capacitors, safely storing the new data without corrupting the rest of the row [[10:50](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=650)].

### 5. The Second Big Problem: Data Leakage (Refreshing)

Even when the mosfet gate is closed, capacitors are imperfect and slowly leak their charge over time. If left alone, all the `1`s would eventually drain and become `0`s, corrupting the computer's memory [[12:31](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=751)].

- **The Refresh Cycle:** To prevent data loss, the RAM must constantly "refresh" itself. It systematically opens every single row, lets the sense amplifiers read the fading charges, and forces a full 1V or 0V back into the capacitors to top them up [[13:20](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=800)].
- **Latency:** This refresh cycle must happen to every row every few milliseconds. If the CPU tries to read or write to the RAM while a refresh cycle is happening, it has to wait, which introduces latency [[14:26](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=866)].

### Summary: SRAM vs. DRAM

The video concludes by comparing the two memory types:

- **SRAM (CPU Cache):** Uses complex logic gates. It is incredibly fast because reading it doesn't destroy the data, and it never needs to be refreshed. However, it takes up a lot of physical space and is very expensive [[17:26](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=1046)].
- **DRAM (System Memory):** Uses simple capacitors. It is slower because reading requires destructive extraction, immediate rewriting, and constant refreshing. However, its tiny footprint allows for massive, cheap data storage [[17:09](http://www.youtube.com/watch?v=7WnbIeMgWYA&t=1029)].