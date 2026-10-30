# 47. Integrated Development Environments (IDEs)

## 47.1 What is an IDE?

An **Integrated Development Environment** combines multiple development tools in one environment.

Examples mentioned in the course include:

- Eclipse;
- Visual Studio Code;
- NetBeans;
- SonarLint;
- JetBrains tools.

An IDE can provide:

- code editor;
- compiler/build configuration;
- debugging tools;
- automatic execution;
- code-completion support;
- project/workspace management.

---

## 47.2 Language-specific vs general IDEs

Some IDEs are strongly specialized for a language.

Others are generic and must be configured by choosing:

- the programming language;
- compiler;
- extensions;
- debugger;
- other environment settings.

---

# 48. Visual Studio Code in the Course

The course uses:

```text
Windows
   +
WSL
   +
Linux g++ compiler
   +
Visual Studio Code
```

The intended architecture separates the Windows graphical editor from the Linux development environment used to compile the code.

---

## 48.1 Required setup

The course's example assumes:

- Windows OS;
- WSL already installed;
- `g++` already installed in WSL.

The setup sequence is:

1. install Visual Studio Code;
2. install the C++ extension;
3. install the Remote-WSL extension;
4. connect VS Code to WSL;
5. select the correct workspace directory;
6. write C++ code;
7. compile using WSL's `g++`;
8. run the executable;
9. debug when necessary.

---

## 48.2 C++ extension

Inside VS Code:

```text
Extensions
   ↓
Search: C++
   ↓
Install C++ extension
```

The C++ extension provides programming-language support, including editing and development features.

---

## 48.3 Remote WSL extension

The Remote-WSL extension allows VS Code on Windows to interact with the Linux environment provided by WSL.

Conceptually:

```text
Windows VS Code
      │
      │ Remote connection
      ▼
WSL Linux environment
      │
      ▼
     g++
```

---

## 48.4 Workspace

The course stresses selecting the correct directory—the **workspace** in which source files are located and from which compilation takes place.

A common source of confusion is editing one copy of a file while compiling another copy located in a different directory.

### Good practice

Before compiling, verify:

```text
Where is the source file?
Which directory am I in?
Which file is VS Code editing?
Which file is g++ compiling?
```

---

## 48.5 IntelliSense

VS Code's **IntelliSense** helps by:

- suggesting available constructs;
- completing code automatically;
- showing instruction/function information.

It is a productivity tool, not a substitute for understanding syntax and semantics.

---

## 48.6 Compilation in VS Code

The course shows both:

- graphical compilation controls;
- compilation directly in the terminal.

For example:

```bash
g++ hello-world2.cc
```

The terminal remains important even when an IDE is used because it exposes the underlying compiler workflow directly.

---

# 49. WSL — Windows Subsystem for Linux

## 49.1 Purpose

WSL provides an environment for running Linux inside Windows.

For this course, its practical purpose is to provide a Linux-like environment containing the tools needed for C++ development, particularly `g++`.

---

## 49.2 Course installation example

The laboratory slides show an example workflow:

1. start PowerShell as administrator;
2. list available Linux distributions;
3. install an Ubuntu distribution;
4. restart the PC;
5. create the Ubuntu username/password;
6. install the development tools/compiler.

The slide example uses commands similar to:

```powershell
wsl --list --online
wsl --install --d Ubuntu-20.04
```

and then, in Linux:

```bash
sudo apt-get install build-essential
```

> [!warning] Version note
> These commands are the course's example workflow. WSL/Ubuntu installation commands can change over time; for an actual installation, use the current official Microsoft/Ubuntu documentation as well as the course's specific instructions.

---

# 50. Linux and Ubuntu for the Laboratory

## 50.1 Physical laboratory setup

The physical laboratory machines use a **dual-boot** configuration:

- Windows;
- Linux.

The course laboratory uses Linux and the `g++` compiler.

Students log in using university credentials.

---

## 50.2 Ubuntu

Ubuntu is a Linux-based operating system/distribution.

A typical Linux distribution contains components such as:

- Linux kernel;
- GNU tools and libraries;
- additional software;
- documentation;
- window system;
- window manager;
- desktop environment;
- drivers and, where applicable, proprietary software.

Other Linux distributions mentioned in the slides include:

- Mint;
- Fedora;
- openSUSE;
- Debian;
- Arch;
- Mageia;
- OpenWrt;
- Rocks Cluster.

---

# 51. Ways to Obtain a Linux Environment

The course presents several possibilities.

## 51.1 Dual boot

Linux is installed alongside Windows on the physical computer.

At startup, the user selects which OS to boot.

### Advantages

- full native Linux environment;
- direct access to hardware.

### Disadvantages

- requires repartitioning/install management;
- only one OS is active at a time.

---

## 51.2 Live USB

Linux can be booted from a USB device.

The slide notes that a live-USB environment is generally **not persistent** unless configured otherwise.

---

## 51.3 Virtual Machine

A virtual machine emulates/abstracts computer hardware sufficiently to run a guest operating system inside a host operating system.

Key components:

- **host** → physical machine/environment;
- **hypervisor** → software layer that creates and runs VMs;
- **guest** → operating system or execution environment inside the VM.

Possible tools mentioned include:

- VMware Workstation Player;
- Oracle VirtualBox.

---

## 51.4 WSL

WSL provides a Linux environment integrated with Windows without requiring a traditional full virtual-machine workflow.

For this course, WSL is especially relevant because the IDE can remain on Windows while compilation occurs with Linux tools.

---

# 52. Linux Shell

The **shell** is the interface between the user and the operating system.

Two broad interface styles are:

- **CLI** — Command Line Interface;
- **GUI** — Graphical User Interface.

The shell is especially important for programming because it provides direct control over:

- commands;
- files;
- directories;
- programs;
- environment variables;
- scripts.

The slides mention several shells, especially:

- Bash;
- C Shell (`csh`);
- `tcsh`;
- Korn shell (`ksh`).

---

## 52.1 Shell scripting

A shell can also execute scripts.

A shell script is a file containing commands and possibly:

- conditions;
- loops;
- variables;
- functions;
- other shell-language constructs.

This makes the shell useful not only for manually issuing commands, but also for automation.

---

# 53. Linux Filesystem

Linux organizes files into a directory tree.

The root is represented by:

```text
/
```

Example of an absolute path from the course:

```text
/home/ale/Docs
```

Conceptually:

```text
/
└── home
    └── ale
        └── Docs
```

---

## 53.1 Absolute vs relative paths

### Absolute path

Starts from the root:

```text
/home/user/Docs
```

### Relative path

Interpreted from the current working directory:

```text
Docs
```

Knowing the current directory is essential when compiling source files.

---

# 54. Essential Linux Shell Commands

The first laboratory introduces several commands.

| Command | Meaning |
|---|---|
| `pwd` | print working directory |
| `ls` | list files/directories |
| `cd name` | enter directory `name` |
| `mkdir name` | create directory |
| `rm file` | remove a file |
| `cat file` | display/concatenate file contents |
| `clear` | clear terminal |
| `man command` | show command manual |
| `cp file1 file2` | copy file |
| `mv file1 file2` | move/rename file |
| `touch file` | create an empty file |
| `exit` | close shell |

### Typical sequence

```bash
pwd
ls
cd Desktop
ls
mkdir test
cd test
touch example.txt
ls
cd ..
ls
rm -r test
```

The exact deletion command should be used carefully; removing directories is a destructive operation.

---

# 55. Text Editors

The laboratory introduces the distinction between a simple text editor and an IDE.

Examples mentioned:

- Text Editor / GUI editor;
- `vi`;
- `emacs`;
- `nano`;
- `gedit`.

A text editor simply writes/modifies the source text. It does not necessarily provide integrated compilation and debugging.

---

## 55.1 `vi` basics

The course gives a short `vi` tutorial.

Open a file:

```bash
vi filename
```

The two key modes are:

- command mode;
- insert mode.

Switch:

```text
command → insert     i
insert  → command    ESC
```

Useful commands in command mode:

```text
:q!   quit without saving
:w    save
:wq   save and quit
```

---

# 56. [[Laboratory]] Software Development Workflow

A complete beginner workflow is:

```text
1. Open terminal
2. Navigate to workspace
3. Create/edit source file
4. Save source file
5. Compile with g++
6. Read compiler output
7. Fix errors
8. Recompile
9. Execute program
10. Test with different inputs
11. Debug if necessary
```

---

## 56.1 Source → object → executable

A source file such as:

```text
test.cc
```

can become:

```text
test.o
```

through compilation, and then:

```text
test
```

through linking.

The final executable can contain machine-level instructions and linked library references that are not intended for direct human reading.

---

# 57. IDE vs Text Editor vs Compiler

These tools should not be confused.

| Tool | Main role |
|---|---|
| Text editor | Write/edit source code |
| Compiler (`g++`) | Translate source code into lower-level/object/executable forms |
| Linker | Combine object code with libraries into an executable |
| Debugger | Inspect and control execution to diagnose problems |
| IDE | Integrate several development tools into one environment |
| Shell | Execute commands and interact directly with the OS |

An IDE is therefore an **integration layer**, not a replacement for the underlying compiler/runtime environment.

---
