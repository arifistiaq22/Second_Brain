---
tags:
  - os
  - software
  - youtube
  - micro-architecture
date: 2026-03-24
link: https://youtu.be/M9HHWFp84f0?si=qxLNyzoo1NTuf54z
---
### **1. The Basics of Processes and Concurrency**

- **Concurrency:** In systems with a single CPU and multiple running processes, the operating system rapidly alternates CPU access between them. This happens so quickly that it creates the illusion of simultaneous execution [[00:35](http://www.youtube.com/watch?v=M9HHWFp84f0&t=35)].
- **Maximizing CPU Usage:** Concurrency isn't just for multitasking; it's designed to fill idle gaps. For instance, if a process is waiting for a slow I/O resource (like reading a file from a disk), it cannot execute instructions. Instead of leaving the CPU idle, the OS assigns the CPU to another process that is ready to execute [[01:26](http://www.youtube.com/watch?v=M9HHWFp84f0&t=86)].
- **The Limitation of Traditional Processes:** Traditionally, a process has a single "program counter." This means it can only keep track of one interrupted location at a time. Therefore, two tasks _within the same process_ (like two functions) cannot run concurrently under a strict, single-process model [[02:31](http://www.youtube.com/watch?v=M9HHWFp84f0&t=151)].

### **2. The "Blocking" Problem in Software**

The video uses a web server that returns profile images as an example of why intra-process concurrency is needed:

- When a client requests an image, the server must search and load the image from the disk. This I/O operation is highly costly and slow compared to the CPU's processing speed [[03:47](http://www.youtube.com/watch?v=M9HHWFp84f0&t=227)].
- If the server handles requests sequentially, a fast "accept" step is followed by a very slow "read" step. If thousands of requests arrive simultaneously, later requests are forced to wait in a bottleneck [[04:17](http://www.youtube.com/watch?v=M9HHWFp84f0&t=257)].
- This creates a **blocking effect**, resulting in wasted computational time where the CPU just sits idle waiting for the disk [[05:00](http://www.youtube.com/watch?v=M9HHWFp84f0&t=300)].

### **3. The Traditional Multi-Process Solution**

Historically, developers solved this by utilizing a main "Listener" process.

- Every time a request arrives, the listener doesn't handle it directly. Instead, it spawns an entirely new child process dedicated solely to serving that client [[05:19](http://www.youtube.com/watch?v=M9HHWFp84f0&t=319)].
- **The Downside:** Processes are heavy. Each process is a self-contained context with its own isolated address space. Creating a new process for thousands of clients consumes massive amounts of memory [[05:54](http://www.youtube.com/watch?v=M9HHWFp84f0&t=354)]. Additionally, creating processes is computationally expensive, and if these processes need to share a global state, it requires complex Inter-Process Communication (IPC) [[06:23](http://www.youtube.com/watch?v=M9HHWFp84f0&t=383)].

### **4. Enter Threads: The Multi-Threading Solution**

To solve the blocking problem efficiently, modern systems use threads, which allow concurrency _within_ the executable code of a single process [[07:47](http://www.youtube.com/watch?v=M9HHWFp84f0&t=467)].

- **Shared Memory:** Instead of creating a new process for every task, we create new threads. All threads within a process share the exact same address space and resources (like open files and the Heap) [[08:59](http://www.youtube.com/watch?v=M9HHWFp84f0&t=539)].
- **Isolated State:** While they share memory, threads **cannot** share a CPU state. Because the OS must interrupt threads to allocate the CPU elsewhere, each thread must have its own program counter, register set, and crucially, its own **stack pointer** [[09:18](http://www.youtube.com/watch?v=M9HHWFp84f0&t=558)].
- **The Stack vs. The Heap:** Having separate stacks prevents concurrent threads from accidentally overwriting each other's local variables [[09:35](http://www.youtube.com/watch?v=M9HHWFp84f0&t=575)]. If threads need to communicate, they should use the shared Heap, though this requires careful hardware-level synchronization to avoid data corruption [[10:22](http://www.youtube.com/watch?v=M9HHWFp84f0&t=622)].

### **5. What a Thread Is (and Isn't)**

- **Implementation:** In operating systems like Linux, processes and threads are beautifully abstracted into a single structure called a `task` [[11:28](http://www.youtube.com/watch?v=M9HHWFp84f0&t=688)]. Every process automatically starts with at least one "main thread" [[12:08](http://www.youtube.com/watch?v=M9HHWFp84f0&t=728)].
- **Not a Function:** A thread is **not** a function. It does not contain code. Instead, its program counter simply _points_ to executable code residing in the read-only text section of the process's memory [[13:57](http://www.youtube.com/watch?v=M9HHWFp84f0&t=837)].
- **Safe Concurrent Execution:** Because executable code and constants reside in read-only memory, multiple threads can safely point to and execute the exact same piece of code simultaneously without conflict [[14:45](http://www.youtube.com/watch?v=M9HHWFp84f0&t=885)].

By using threads, a single-core processor can handle thousands of network requests concurrently, sidestepping I/O blocking effects while maintaining an incredibly lightweight memory footprint compared to spawning traditional processes.