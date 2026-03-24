---
tags:
  - os
  - software
  - micro-architecture
  - microprocessor
date: 2026-03-23
link: https://youtu.be/O2tV9q6784k?si=OeqPpDJFk_P_v-ru
---
### **1. Core Terminology & Concepts**

Before diving into the algorithms, the video establishes how a CPU actually interacts with running code:

- **Processes vs. Programs** [[01:42](http://www.youtube.com/watch?v=O2tV9q6784k&t=102)]: A program is just an executable file. When launched, it becomes a _process_—a running context managed by the OS using a Process Control Block (PCB).
- **CPU & I/O Bursts** [[08:09](http://www.youtube.com/watch?v=O2tV9q6784k&t=489)]: Programs do not use the CPU continuously. They alternate between **CPU Bursts** (actively computing data) and **I/O Bursts** (waiting for a network request, disk read, or user input). Most processes have many short CPU bursts and very few long ones.
- **Process States** [[10:29](http://www.youtube.com/watch?v=O2tV9q6784k&t=629)]: Over their lifetime, processes cycle through distinct states: _New_, _Ready_ (waiting for the CPU), _Running_ (using the CPU), _Waiting_ (waiting for I/O), and _Terminated_.
- **The Dispatcher** [[13:05](http://www.youtube.com/watch?v=O2tV9q6784k&t=785)]: The scheduler is the "brain" that decides _which_ process goes next. The dispatcher is the mechanism that physically swaps the processes out—a highly optimized action called a "context switch."

### **2. The Evolution of Scheduling Algorithms**

The video walks through the progression of scheduling logic, highlighting why early algorithms failed and how they were improved.

**First Come First Served (FCFS)** [[01:30](http://www.youtube.com/watch?v=O2tV9q6784k&t=90)]

- **How it works:** The simplest policy. Processes are placed in a basic First-In, First-Out (FIFO) queue and run in the order they arrive.
- **The Flaw (The Convoy Effect):** If a massive, CPU-heavy process gets to the front of the line, all the quick, lightweight processes get stuck waiting behind it [[18:00](http://www.youtube.com/watch?v=O2tV9q6784k&t=1080)]. This causes I/O devices to sit idle and ruins system responsiveness.

**Shortest Job First (SJF)** [[18:20](http://www.youtube.com/watch?v=O2tV9q6784k&t=1100)]

- **How it works:** Assigns the CPU to the process with the shortest _next_ CPU burst. This mathematically guarantees the absolute minimum average waiting time across the system.
- **The Flaw:** It requires seeing into the future. Since the OS cannot perfectly predict how long a process will take, it has to guess using an exponential average formula based on the process's past behavior [[20:24](http://www.youtube.com/watch?v=O2tV9q6784k&t=1224)].

**Round Robin (RR) & Preemption** [[27:59](http://www.youtube.com/watch?v=O2tV9q6784k&t=1679)]

- **How it works:** Introduces **Preemption** [[24:45](http://www.youtube.com/watch?v=O2tV9q6784k&t=1485)], meaning the OS can forcibly interrupt a running process. Every process is given a fixed slice of time (a "time quantum"). If the process doesn't finish its work within that slice, it is paused and sent to the back of the queue.
- **The Flaw:** Choosing the right time quantum is incredibly difficult. If the time slice is too large, the system behaves just like FCFS. If the slice is too tiny, the CPU spends all of its time executing context switches rather than doing actual work [[30:33](http://www.youtube.com/watch?v=O2tV9q6784k&t=1833)].

**Priority Scheduling & Multi-Level Queues** [[34:14](http://www.youtube.com/watch?v=O2tV9q6784k&t=2054)]

- **How it works:** To maintain smoothness during heavy multitasking, the OS assigns priorities (e.g., interactive UI apps get a higher priority than background email syncing). Processes are divided into different queues based on these priorities [[37:35](http://www.youtube.com/watch?v=O2tV9q6784k&t=2255)].
- **The Flaw (Starvation):** A steady stream of high-priority tasks can prevent low-priority tasks from ever executing. Operating systems solve this via "Aging"—gradually increasing a process's priority the longer it sits waiting in a queue [[36:14](http://www.youtube.com/watch?v=O2tV9q6784k&t=2174)].

### **3. The Modern Solution: Multi-Level Feedback Queue**

The video culminates with the **Multi-Level Feedback Queue (MLFQ)**, a dynamic scheduling system that adapts to how processes behave in real-time [[39:11](http://www.youtube.com/watch?v=O2tV9q6784k&t=2351)].

Here is how it automatically sorts tasks:

1. Every new process starts in the highest-priority queue, which has a very short time quantum.
2. If the process uses its entire time slice without stopping, the OS assumes it is a heavy "CPU-bound" task and demotes it to a lower-priority queue with a larger time slice.
3. If a process yields the CPU early (e.g., waiting for you to click a button), the OS recognizes it as an interactive "I/O-bound" task and keeps it in the high-priority queue.

**The Result:** Interactive apps like web browsers naturally bubble to the top of the priority list for instant response times. Meanwhile, heavy rendering or benchmark software naturally sinks to the bottom, safely churning through computations using whatever CPU cycles are left over [[42:34](http://www.youtube.com/watch?v=O2tV9q6784k&t=2554)].

**A Final Note on Threads** [[43:37](http://www.youtube.com/watch?v=O2tV9q6784k&t=2617)] Modern operating systems (like Windows and Linux) take this a step further by scheduling **threads** rather than entire processes. This is why a heavy application can keep its UI perfectly smooth (running on a high-priority thread) while doing intense background math (running on a lower-priority thread).