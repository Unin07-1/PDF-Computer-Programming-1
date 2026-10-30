## Types of computers

The lecture identifies several categories, based on size, capability, portability, and typical role:

| Category | Description from the lecture | Typical context |
|---|---|---|
| Supercomputer | Designed for very large amounts of data and computation at high speed. | Scientific and large-scale computational tasks. |
| Mainframe | High-performance system for large information-processing workloads; traditionally shared by many users. | Large organizations and centralized data processing. |
| Desktop personal computer | A personal computer with separate peripherals such as a monitor and keyboard; storage may be internal or external. | Individual or office work. |
| Laptop | Portable form of personal computer. | Mobile personal computing. |
| Handheld computer / smartphone | Compact, wireless, touchscreen computer. | Personal mobile computing. |
| Embedded system | Computer integrated into a device, such as a smartwatch. | Dedicated functions inside larger products. |

These categories are not mutually exclusive technical definitions; they are an introductory classification used by the lecture.

## 3. Transistors, processing speed, and Moore's law

A **transistor** is a semiconductor device used in electronic circuits. The lecture quotes Gordon Moore's 1965 observation that the number of transistors that could be integrated on a circuit roughly doubled every two years. It uses this as a historical way to discuss growth in computing capacity.

The lecture also notes that CPU clock frequency did not continue increasing indefinitely because heat dissipation became a significant constraint. Modern systems increasingly use multiple cores, parallel processing, and other architectural techniques rather than relying only on raising the clock frequency.

> [!note] Historical context
> Moore's law is a historical empirical trend, not a guarantee that every future system will double in transistor count on an exact schedule. The slide raises the issue of physical limits; it should not be read as a precise prediction for every contemporary processor.

## 4. Software organization in a computer

The lecture presents the computer system as layers:

1. **Hardware** — physical components of the computer.
2. **Operating system (OS)** — system software operating directly on the machine and providing common services.
3. **Programming environment** — tools such as a compiler and debugger.
4. **Application software** — programs used by people, such as browsers, mail readers, and spreadsheets.

The layers depend on lower-level services but provide higher-level abstractions. A user can use an application without directly managing CPU instructions or physical memory cells.

```text
Application software
Programming environment (compiler, debugger, etc.)
Operating system
Hardware
```

The exact boundaries differ between systems, but the hierarchy is useful for understanding the role of each layer.

## 5. Von Neumann architecture

### 5.1 Main idea

The lecture introduces the **Von Neumann architecture**, associated with John von Neumann's work in the mid-1940s. Its key educational feature is that a general-purpose computer can be programmed for many different tasks instead of being permanently limited to one fixed function.

The simplified model includes:

- **CPU (Central Processing Unit):** executes instructions and performs processing.
- **Primary memory:** holds instructions and data currently being used.
- **Secondary memory:** stores large quantities of data and programs persistently.
- **Input/output devices:** keyboards, monitors, network interfaces, storage devices, and other peripherals.
- **System bus:** allows components to exchange data, addresses, and control information.

```text
                    +----------------+
                    |      CPU       |
                    +-------+--------+
                            |
                        System bus
          +-----------------+------------------+
          |                 |                  |
   +------v------+  +-------v--------+  +------v-------+
   | Primary     |  | Secondary      |  | Input/Output |
   | memory      |  | storage        |  | devices      |
   +-------------+  +----------------+  +--------------+
```

This diagram is conceptual; real systems contain additional controllers, caches, interconnects, and specialized processors.

### 5.2 Modern architectural extensions mentioned in the lecture

- A **GPU (Graphics Processing Unit)** may assist the CPU with highly parallel computational workloads, including graphics and mathematical operations.
- A CPU may contain multiple independent **cores**, allowing work to run in parallel when software and the system support it.
- Processors use local, fast storage such as registers and caches to reduce the cost of repeatedly accessing data. The lecture describes this generally as local memory for frequently accessed data.

## 6. Binary representation of information

### 6.1 Bit and byte

Computers represent instructions and data as sequences of binary digits because digital hardware can distinguish two states. The two symbols are `0` and `1`.

- A **bit** (binary digit) is one binary symbol and is the elementary unit of information.
- A **byte** consists of 8 bits.

For example, the eight-bit sequence `01100001` represents the decimal number 97 when interpreted as an unsigned binary integer.

### 6.2 Representation depends on interpretation

The same bit pattern can mean different things depending on the data type or instruction interpreting it. A computer does not inherently see a sequence as a letter, a signed integer, a floating-point number, or an instruction; those meanings depend on the representation and context.

### 6.3 Characters

The lecture uses an ASCII-style table to illustrate character encoding. Its example is:

\[
(97)_{10} = (01100001)_2
\]

The binary byte `01100001` corresponds to the character `a` in ASCII.

This is a teaching example, not a complete discussion of modern character encoding. Real software may use encodings such as UTF-8, in which a character is not necessarily stored in one byte.

## 7. Memory and addresses

### 7.1 Addressable memory

Primary memory can be viewed as a sequence of storage locations. Each location has an **address**, which the processor can use to access the contents at that location. This direct addressing distinguishes memory from a purely sequential medium such as a tape.

The lecture uses the terms **word** and **memory location**:

- A memory location is a directly addressable place in memory.
- A **word** is a group of bits processed as a unit by a particular machine. The word size depends on the architecture; the lecture gives historical examples of 32-bit and 64-bit systems.

Do not confuse a word with a byte. A byte is defined here as 8 bits; a machine word can contain several bytes.

### 7.2 Data and instructions in memory

Both data and program instructions are represented in binary. In the simplified model, the computer distinguishes them through their placement and the rules used to interpret the contents. The source depicts memory as holding a program area and a data area.

### 7.3 RAM

**RAM (Random Access Memory)** is commonly used as primary working memory.

- Its access time is modelled as independent of the address being accessed.
- It is **volatile**: its contents are lost when power is removed.
- The lecture distinguishes **DRAM** and **SRAM**. DRAM requires periodic refreshing and can offer higher storage density; SRAM does not require that same refresh process but is typically used where speed is important.

The source emphasizes RAM's role as the main working area for active programs and data.

### 7.4 ROM

**ROM (Read-Only Memory)** is presented as non-volatile memory used to retain information needed when the computer starts, before the operating system is loaded. The lecture gives the BIOS as an example.

> [!note] Terminology clarification
> “ROM” is used broadly in introductory explanations. Modern firmware is often stored in rewritable non-volatile memory, so “ROM is writable only once” is an oversimplification of current hardware. The important course distinction is that firmware storage is persistent and available at startup.

### 7.5 Secondary memory / mass storage

Secondary storage provides persistent storage for programs and large amounts of data. It can be written repeatedly and usually has more capacity but higher access latency than primary RAM. Examples include solid-state drives and hard disk drives.

| Feature | RAM | Startup firmware storage / ROM concept | Secondary storage |
|---|---|---|---|
| Main role | Active programs and data | Startup instructions and firmware | Long-term files and programs |
| Volatility | Volatile | Non-volatile | Non-volatile |
| Repeated writes | Yes | Depends on technology | Yes |
| Typical access | Fast working memory | Fast access to small startup code | Persistent storage, generally slower than RAM |

The comparison is qualitative; precise speed and write characteristics depend on the actual technology.

## 8. CPU: components and responsibilities

The **CPU** executes machine instructions. The lecture's simplified CPU model includes the following components and registers.

| Component | Abbreviation | Role in the lecture's model |
|---|---|---|
| Program counter | PC | Holds the address of the next instruction to execute. |
| Current instruction register | CIR | Holds the instruction currently being interpreted/executed. |
| State register | SR | Holds status information or flags describing CPU results/state. |
| Interrupt register | INTR | Represents interrupt-related information. |
| Arithmetic Logic Unit | ALU | Performs arithmetic and logical operations. |
| Control unit | — | Coordinates the fetch/decode/execute sequence and control signals. |
| General-purpose / operand registers | A, B, etc. in the diagram | Hold operands and intermediate values used by instructions. |
| Address register | AR | Holds the memory address involved in a transfer. |
| Data register | DR | Holds data being read from or written to memory. |
| Clock / synchronizer | — | Coordinates CPU actions in time. |

The exact names and organization of registers differ across processor architectures; the lecture's diagram is a conceptual model.

### 8.1 Status flags

The source diagram names flags such as `C`, `Z`, `S`, and `V`:

- `C`: carry-related flag.
- `Z`: zero-result flag.
- `S`: sign-related flag.
- `V`: overflow-related flag.

The precise semantics depend on a processor's instruction set, but such flags allow later instructions to branch or make decisions based on earlier results.

## 9. Clock and execution timing

The lecture models CPU and device operations as coordinated by a clock. A clock provides regular timing events that synchronize internal operations. Increasing clock frequency can increase the number of timing cycles per second, but does not by itself guarantee a proportional increase in overall program speed. Performance also depends on architecture, memory behaviour, instruction work, parallelism, and other system constraints.

The slides connect the limits of clock-frequency growth to heat dissipation and introduce multicore processors as one response to performance requirements.

## 10. Machine instructions and assembly

The CPU understands a limited set of machine instructions, each represented by a binary encoding. **Assembly language** gives human-readable names and operands to machine-level operations.

The lecture shows illustrative instructions such as loading a value into a register and writing it to an output device. Assembly syntax varies by processor; the sample is intended to show the idea, not to be portable across machines.

### 10.1 From C++ to machine execution

A high-level language such as C++ abstracts away most details of the actual hardware. For example:

```cpp
int main() {
    int a = 1;
    int b = 2;
    int c = a + b;
}
```

The source code is translated by a compiler into instructions the processor can execute. One C++ statement may result in several machine instructions. When the executable runs, its instructions and the data it uses are loaded into memory as required by the system.

The lecture's main conceptual mapping is:

- C++ **variables** abstract storage locations and values.
- C++ **statements** abstract sequences of machine instructions.
- The execution model still updates values as instructions are executed and advances through the program, unless control-flow instructions change the path.

## 11. The instruction cycle: fetch, decode, execute

The simplified execution cycle is:

1. **Fetch:** the CPU uses the program counter (PC) to locate and read the next instruction.
2. **Decode:** the control unit interprets the instruction and determines what operation and operands are required.
3. **Execute:** the CPU performs the operation, possibly using the ALU, registers, memory, or an I/O device.
4. **Update PC:** the PC normally advances to the next instruction, unless a jump, branch, call, interrupt, or other control-flow change redirects execution.

This cycle is repeated while the program runs.

### 11.1 What happens in a memory read (simplified)

The source's fetch-phase drawings illustrate a sequence in which an address is placed in an address register, a read operation is requested, data is returned through a data register, and the instruction/data is then used by the CPU. The exact internal timing of modern processors is more elaborate, but the roles of address, data, and control signals remain useful at a conceptual level.

## 12. The system bus

The **system bus** connects processor, memory, and I/O components so they can exchange information. In the lecture, a transfer is described as a connection between units, where one transmits and another receives, with the processor directing the operation in the model.

The bus is divided into three conceptual groups:

- **Data bus:** carries the data being transferred.
- **Address bus:** identifies the memory location or device endpoint involved.
- **Control lines:** communicate the type and timing of the operation, such as read or write.

A memory write, for example, needs the target address, the data to write, and control information specifying that the operation is a write.

## 13. Loading a program and boot sequence

The lecture asks how a program comes to reside in memory. Its simplified account is:

1. Startup firmware is available in non-volatile memory.
2. A bootloader begins executing at startup.
3. The bootloader loads the operating system (or a subsequent boot stage).
4. The operating system initializes the environment and coordinates the execution of user programs.

The details vary across firmware, bootloaders, operating systems, and hardware platforms, but the key principle is that a computer executes startup instructions before a normal user application is running.

## 14. One CPU and multiple programs: the operating system's role

A real computer needs to run many programs and respond to events such as keyboard input, network activity, and screen updates. The lecture uses a simplified single-CPU model to motivate **context switching** and operating-system coordination.

- A process runs for some interval.
- The OS may suspend it and give another process a turn.
- An interrupt can signal that an event requiring attention has occurred.
- The OS coordinates resource use and helps make concurrent activity appear orderly.

On modern computers, there may also be multiple CPU cores, so some tasks can truly run in parallel. Context switching and scheduling remain relevant because there are often more runnable threads than available execution units.

## 15. Operating system: definition and layered architecture

An **operating system (OS)** is system software that operates directly on the physical machine and provides higher-level functionality while abstracting hardware-specific details.

The lecture depicts the OS as a layered system. Each layer provides services to layers above it and uses services from layers below it. This separation distinguishes an interface from its implementation.

The main responsibilities shown in the source are:

- Process management.
- Memory management.
- Peripheral/device management.
- File-system management.
- Command interpretation and activation of programs.

These responsibilities are organized around the OS kernel and supporting system programs and libraries.

## 16. Programs and processes

A **program** is a static entity: executable code stored on a device. A **process** is a dynamic entity created when a program is running, together with its current execution state.

The lecture models a process as a pair:

\[
P = (E, S)
\]

where:

- `E` is the executable program code.
- `S` is the process state, including values stored in memory and CPU state such as the program counter.

The relationship between programs and processes is not one-to-one. The same program can be started multiple times, creating different processes with separate states.

## 17. Process states and scheduling

The lecture identifies several high-level process states:

- **Ready:** able to run but waiting for CPU time.
- **Running:** currently executing on a processor.
- **Waiting/blocked:** waiting for an external event or an I/O operation.
- **Terminated:** execution has ended, possibly normally or because of an error.

### 17.1 Time-sharing

In the lecture's single-processor model, only one process can execute at a given instant. The OS assigns a time interval, or **time slice**, to a process, and then may switch to another process. This is known as **time-sharing**.

### 17.2 Scheduling and round-robin

The OS kernel chooses which ready process should execute. This decision is called **scheduling**. The lecture names **round-robin** as a typical approach: processes receive turns in a cycle. A running process may give up the CPU earlier if it waits for I/O or another event.

This is a simplified introduction rather than a claim that every operating system uses pure round-robin scheduling for every kind of task.

## 18. Memory management and virtual memory

The memory manager:

- Allocates primary memory to processes.
- Separates process memory areas to support protection and isolation.
- Manages memory use when processes need more space than can comfortably remain in RAM.

### 18.1 Swap and virtual memory

The lecture describes **swap space** as an area on secondary storage that can be used to hold some memory contents when a process is suspended or RAM is under pressure. Moving data between RAM and storage helps the OS present processes with a virtual memory space larger than the currently available physical RAM.

Virtual memory is an abstraction: a process uses virtual addresses, while the operating system and hardware translate them to physical memory locations or handle cases where pages are not currently resident in RAM.

The slide's example of using a portion of disk space is illustrative; there is no universal fixed percentage of storage that must be used as swap.

## 19. Device management

The device manager exposes higher-level operations for accessing peripherals and hides many hardware-specific details from application programs.

The source distinguishes these components:

- **Physical drivers:** interact more directly with the device; may use polling or interrupts and may operate as part of the kernel.
- **Logical drivers:** provide higher-level primitives for peripheral access.
- **Special files:** offer file-like access to certain devices in systems that use this design.
- **Spooling system:** coordinates queued access to devices such as printers.

This abstraction makes it possible for applications to request an operation without directly controlling the electrical or protocol details of the hardware.

## 20. Command interpreter: shell and user interface

The **command interpreter** manages commands issued directly by the user and acts as an interface to the operating system. The lecture contrasts this with graphical interfaces based on windows and icons, while noting that text-based command interpreters remain useful and sometimes necessary.

A **shell** is a common kind of command interpreter. It reads a command, interprets its structure, and launches a program or performs a supported operation. The shell is not the same thing as the operating-system kernel: it is an interface program that requests services from the OS.

The supplied slides introduce the role of the command interpreter but do not provide a detailed command reference. Specific shell syntax therefore belongs in a separate practical note if it is part of a later lab.

## 21. File-system concepts

The file system manages persistent files and directories and provides operations to locate, read, write, and protect them.

### 21.1 File metadata and descriptors

The lecture describes a file as being associated with a descriptor containing characteristic information such as:

- Name.
- Size.
- Starting location on storage (in the simplified model).
- Owner.
- Access permissions.

Actual modern file-system metadata varies by operating system and file-system implementation; the list is a conceptual introduction.

### 21.2 Directory tree and pathname

A folder/directory contains entries referring to files and possibly other directories, forming a tree. A **pathname** identifies a file through its position in that structure.

Example from the style of the lecture:

```text
C:\alpha\beta\gamma.txt
```

The file-system manager provides higher-level operations, coordinates access, can manage concurrent access to the same file, and hides characteristics of the underlying storage device.

## 22. Putting the pieces together: executing a C++ program

The following sequence summarizes how the lecture's topics fit together:

1. A programmer writes C++ source code.
2. The compiler translates the source into an executable representation suitable for the target platform.
3. The executable is stored on secondary storage.
4. The OS creates a process and arranges for its code and required data to be mapped into memory.
5. The CPU fetches and executes machine instructions, using registers and the ALU and accessing memory as required.
6. The program may request input/output, file operations, or other services through the OS and its libraries.
7. The OS schedules processes, handles events and interrupts, manages memory, and coordinates device access.
8. When execution ends, the OS reclaims process resources according to the system's rules.

This is a conceptual workflow rather than an exhaustive description of linking, loading, virtual memory, runtime initialization, or hardware-level execution.

## 23. Key terms

| Term | Definition |
|---|---|
| Bit | A binary digit, `0` or `1`. |
| Byte | A sequence of 8 bits. |
| Word | A machine-dependent group of bits processed as a unit. |
| Address | Identifier used to select a memory location or other addressable resource. |
| RAM | Volatile primary working memory. |
| ROM / firmware storage | Non-volatile storage used for startup code in the lecture's simplified model. |
| Secondary storage | Persistent storage for files, programs, and data. |
| CPU | Processor that executes machine instructions. |
| ALU | Component that performs arithmetic and logical operations. |
| PC | Register containing the address of the next instruction in the simplified model. |
| CIR | Register holding the current instruction in the lecture's CPU model. |
| Control unit | Coordinates instruction execution and control signals. |
| System bus | Interconnect through which components communicate. |
| Fetch/decode/execute | Simplified cycle for instruction processing. |
| Operating system | System software that manages hardware and provides services to programs. |
| Process | Running instance of a program with its execution state. |
| Context switch | Transition from one process/thread's execution context to another. |
| Scheduler | OS component/policy that selects a process/thread to run. |
| Virtual memory | Abstraction that separates process-visible addresses from physical RAM locations. |
| Driver | Software component that enables the OS to control or use a device. |
| Shell | Command interpreter that provides a text-based user interface. |
| File system | System for organizing, identifying, storing, and managing files and directories. |

## 24. Common misconceptions

| Misconception | Correct interpretation |
|---|---|
| A computer only processes numbers. | Data and instructions are bit patterns interpreted according to context. |
| A byte and a word are the same. | A byte is 8 bits in this course; word size depends on the architecture. |
| RAM is permanent storage. | RAM is volatile working memory. |
| Secondary storage is just a slower kind of RAM. | It is a different storage layer with different persistence, access, and management characteristics. |
| A C++ statement corresponds to one machine instruction. | One high-level statement may compile into several machine instructions. |
| The PC stores the current data value. | In the lecture's model, it tracks the address of the next instruction. |
| A program and a process are identical. | A program is static code; a process includes a running state. |
| The OS is only the graphical desktop. | The OS also manages processes, memory, devices, files, and other system services. |
| A shell is the kernel. | A shell is a user-facing command interpreter; it asks the OS to perform operations. |
| Virtual memory means the computer has extra physical RAM. | It is an abstraction supported by address translation and, when used, secondary storage. |

## 25. Practice questions

1. What are the main components in the simplified Von Neumann model?
2. What is the difference between a bit, a byte, and a word?
3. Why is RAM described as volatile?
4. What are the roles of the PC, CIR, ALU, and control unit?
5. What information is carried by the data bus, address bus, and control lines?
6. Summarize the fetch/decode/execute cycle.
7. Why might one C++ statement require several machine instructions?
8. What is the difference between a program and a process?
9. What problem does time-sharing address?
10. What is the role of the process scheduler?
11. Why does an OS need memory management and process isolation?
12. What is swap space used for in the lecture's model?
13. What do device drivers and the device manager abstract away?
14. What does a shell do, and how does it differ from a kernel?
15. What information can file metadata contain, and why is a pathname useful?

## 26. Quick revision sheet

- **Von Neumann model:** CPU + primary memory + secondary storage + I/O + system bus.
- **Bit/byte:** bit = binary digit; byte = 8 bits.
- **RAM:** volatile working memory; **secondary storage:** persistent files/programs.
- **CPU components:** PC, CIR, status/interrupt registers, general registers, ALU, control unit, AR, DR, clock.
- **Instruction cycle:** fetch → decode → execute → update control flow/PC.
- **Bus:** data lines + address lines + control lines.
- **OS responsibilities:** processes, memory, devices, file system, command interface.
- **Program/process:** static executable code versus running code plus state.
- **Scheduling:** chooses which ready process/thread gets execution time.
- **Virtual memory:** process-visible address space managed by OS/hardware; swap may use secondary storage.
- **Shell:** text command interpreter, not the kernel.
