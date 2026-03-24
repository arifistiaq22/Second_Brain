---
tags:
  - os
  - software
  - youtube
  - microprocessor
  - micro-architecture
date: 2026-03-19
link: https://youtu.be/3X93PnKRNUo?si=ECnzB-Cl9vZYCvFN
---
### The Historical Context

- **The Mainframe Era:** Early computers were massive, extremely expensive, and could only be used by one person at a time [[01:00](http://www.youtube.com/watch?v=3X93PnKRNUo&t=60)].
- **The Setup Problem:** Running a program involved a tedious process of manually mounting magnetic tapes for compilers, assemblers, and source code. During this long setup time, the CPU simply sat idle, which was highly inefficient for such an expensive machine [[03:01](http://www.youtube.com/watch?v=3X93PnKRNUo&t=181)].
- **Early Multitasking:** To maximize return on investment, operating systems were developed to allow multiple users to connect to a single computer simultaneously via "dumb terminals" (a screen and keyboard with no independent processing power) [[03:48](http://www.youtube.com/watch?v=3X93PnKRNUo&t=228)].

### How Concurrency Works

- **The Illusion of Simultaneity:** If programs were executed sequentially, users would have to wait a long time for their turn. Instead, programs are broken down into smaller segments and interleaved [[05:23](http://www.youtube.com/watch?v=3X93PnKRNUo&t=323)]. Because computers perform calculations millions of times faster than humans can perceive, this rapid switching creates the illusion that everything is running at the same time.
- **CPU Mechanics:** To understand how switching works, you have to look inside the CPU. The CPU relies on specific registers (like the address register and instruction register) to constantly fetch, decode, and execute instructions step-by-step [[08:29](http://www.youtube.com/watch?v=3X93PnKRNUo&t=509)]. The operating system makes the CPU switch tasks by simply changing the value in the address register to point to a different program.

### How the Operating System Manages the CPU

- **Hardware Interruptions:** The operating system is itself a piece of software that needs CPU time. When a user program needs to read a file or allocate memory, it interacts with the OS through a hardware "interruption" [[11:04](http://www.youtube.com/watch?v=3X93PnKRNUo&t=664)].
- **Context Switching:** When an interruption occurs, the CPU pauses the current program, saves its exact state, and jumps to the operating system's code. The OS handles the request, schedules another process from its queue, and allocates the CPU to the new task [[11:29](http://www.youtube.com/watch?v=3X93PnKRNUo&t=689)].

### The Evolution of Scheduling

- **Cooperative (Non-Preemptive) Scheduling:** Initially, the OS relied on programs to voluntarily release control of the CPU (usually when asking for an I/O operation). However, this is a massive security risk: a malicious program or a simple infinite loop could monopolize the CPU forever, freezing the entire system [[13:29](http://www.youtube.com/watch?v=3X93PnKRNUo&t=809)].
- **Preemptive Scheduling:** To fix this vulnerability, modern systems use a hardware timer. Before the OS hands the CPU over to a user program, it starts a countdown. If the program doesn't voluntarily yield control before the timer expires, the timer triggers an interruption, forcing control back to the OS [[13:59](http://www.youtube.com/watch?v=3X93PnKRNUo&t=839)].

### Concurrency vs. Parallelism

- **The Multicore Solution:** Rapidly switching between processes creates lag if there are too many programs running. Adding multiple processing units (cores) to a single chip allowed for true simultaneous execution [[15:59](http://www.youtube.com/watch?v=3X93PnKRNUo&t=959)].
- **The Key Distinction:** The video concludes with an important quote: _"Concurrency is about dealing with lots of things at once, but parallelism is about doing lots of things at once"_ [[16:24](http://www.youtube.com/watch?v=3X93PnKRNUo&t=984)]. Concurrency is the rapid juggling of tasks, while parallelism involves multiple cores physically executing different tasks at the exact same time.