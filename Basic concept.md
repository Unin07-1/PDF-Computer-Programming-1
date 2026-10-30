## 1. Fundamentals of Programming: Coding vs. Programming

### 1.1 Basic Concepts & Software Lifecycle

- **Coding:** the process of writing lines of code in a specific programming language to create a computer program (software).

- **Programming:** a complex activity that encompasses coding alongside higher-level tasks such as problem analysis, designing solution logic via algorithms and data structures, and implementation.

- **Phases of the Software Development Lifecycle:**
    - **Planning / Problem Analysis:** understanding the problem and its context
    
    - **Design:** defining and modeling the solution by designing algorithms and data structures (starting from mental schemas or pseudocode).
    
    - **Implementation:** developing the solution through code execution.
    
    - **Testing:** verifying and validating that the developed solution operates correctly and bug-free.
    
    - **Deployment:** delivering the ready-to-use solution.
    
    - **Maintenance:** updating, maintaining, and evolving the software over time.

- **Code Reuse:** avoiding writing software entirely from scratch by reusing existing, proven components.

### 1.2 Comparison: Coding vs. Programming

|**Dimension**|**Coding**|**Programming**|
|---|---|---|
|**Scope**|Converting a set of instructions into a machine-comprehensible language.|Defining requirements, problem-solving, writing pseudocode, algorithmic thinking, optimization, testing, build creation, maintenance.|
|**Required Skill**|Good knowledge of the syntax and semantics of the chosen language.|High-level abstract thinking, context evaluation, and structural problem analysis skills.|
|**Book Analogy**|Writing a single chapter of a book.|Planning and writing the entire book.|

## 2. Programming Languages: Syntax and Semantics

### 2.1 Definition of a Programming Language

A programming language is a rule-based notational system used to describe computation in both human-readable and machine-readable forms, allowing developers to specify algorithms to a computer.
### 2.2 Core Components: Syntax and Semantics

$$\text{Language} = \text{Syntax} + \text{Semantics}$$

- **Syntax:**
    - Describes the composition, structure, and grammatical form of a program.
    - The collection of formal rules specifying how valid statements/code are constructed.
    
- **Semantics:**
    - Describes the meaning and interpretation of a program.
    - The collection of rules defining the meaning associated with words, symbols, and code constructions during execution.

## 3. Language Taxonomies and Programming Paradigms

### 3.1 Classification Criteria for Programming Languages

Programming languages can be classified according to several criteria:

- **Intended Domain:** General-purpose (broad applications) vs. Domain-specific (tailored to specific domains).

- **Paradigm:** Imperative (commands modifying program state) vs. Declarative (declaring logic or truth relations).

- **Abstraction Level:**
    - _High-level:_ close to human natural language and problem abstraction (e.g., C++, Java, Python).
    
    - _Low-level:_ close to machine hardware operations (e.g., Assembly).
    
    - _Executable Machine Code:_ binary sequence directly executed by the CPU.

- **Execution Model:** Compiled (translated to machine code by a compiler), Interpreted (line-by-line translation at runtime), or Just-In-Time (JIT) compiled.

- **Typing System:** Strongly typed (requires explicit conversions) vs. Weakly typed (implicit/automatic type conversion).

### 3.2 Overview of Main Programming Paradigms

- **Procedural Programming:** imperative model structured around procedures/functions modifying state (e.g., C, Pascal).

- **Object-Oriented Programming (OOP):** based on "objects" containing data fields and code procedures; provides abstraction, encapsulation, inheritance, polymorphism, and classes (e.g., C++, Java, Python, C#).

- **Logic Programming:** declarative model based on formal logic using facts, rules, and queries (e.g., Prolog).

- **Functional Programming:** declarative model based on mathematical function composition, recursion, and conditional expressions (e.g., Lisp, ML).

- **Scripting Languages:** interpreted, dynamic environments prioritized for rapid development and data manipulation (e.g., JavaScript, PHP, Python).

- **Command Language:** focused on automating operating system tasks and batch scripts (e.g., bash, sh).

- **Event-Driven Programming:** program flow is determined by external events such as user actions or sensor triggers (e.g., JavaScript).

- **Markup Languages:** text-encoding systems using tags/symbols to specify document layout and rendering (e.g., HTML, XML).

- **Assembly / Low-level:** hardware-dependent code compiled by an assembler, utilized for resource-constrained environments.

- **Mathematical / Statistical:** optimized for matrix calculations, large data arrays, and analytical models (e.g., R, MATLAB).

- **Natural Language Programming:** attempts to synthesize executable tasks directly from human natural language inputs via AI/NLP engines.

> **Note:** Most modern languages are **multi-paradigm**. C++ natively supports procedural, object-oriented, and functional programming paradigms.

## 4. The [[C++]] Language & Development Workflow

### 4.1 Features and Rationale for C++

- Developed at Bell Labs in 1983 by Bjarne Stroustrup as an enhancement of C ("C with Classes").
- Standardized by ISO/IEC (C++98, C++03, C++11, C++14, C++17, C++20, C++23).
    
- **Why study C++ in this course:**
    1. Provides insight into internal computer architecture, memory management, pointers, and compiler operations.
    
    2. Simplifies learning other programming languages subsequently.
    
    3. Enables high-performance, resource-efficient application development.
    
    4. Widely adopted across operating systems, game engines, browsers, embedded devices, and network infrastructure.

### 4.2 C++ Compilation and Linking Flow

The process of turning source files into executable binaries consists of distinct stages:

````
+------------------+         +------------------+
|  Source Code     |         |   Object File    |
|  (e.g., test.cc) | ------> |   (e.g., test.o) |
+------------------+ COMPILER+------------------+
                                      |
                                      | + System Libraries (e.g., cmath)
                                      v LINKER
                             +------------------+
                             | Executable File  |
                             | (e.g., a.out)    |
                             +------------------+
```[cite: 2]

1. **Source Code File (`.cc` / `.cpp`):** plain text file written by the programmer[cite: 2].
2. **Compilation (`g++ -c test.cc`):** the compiler translates source code into machine-oriented *object files* (`test.o`)[cite: 2].
3. **Linking (`g++ test.o -o test`):** the linker combines object files with system libraries to produce an *executable file* (default output: `a.out`, configurable via `-o`)[cite: 2].
4. **Single-step Compilation & Linking:** executed using a direct command (`g++ test.cc`)[cite: 2].

````
### 4.3 Structure (Template) of a C++ Program

```cpp
#include <iostream>   // Include standard Input/Output library stream
using namespace std;  // Use standard namespace identifiers

int main() {          // Main function: execution entry point
    cout << "Hello world \n"; // Output instruction[cite: 2]
    return 0;         // Return exit status 0 to OS (indicates success)
}
````

- `#include <iostream>`: preprocessor directive including standard stream operations (`cout`, `cin`)
    
- `using namespace std;`: grants access to standard library components without needing explicit `std::` prefixes
    
- `int main()`: required entry point function for every C++ program
    
- `return 0;`: signals successful program termination to the host operating system
    

## 5. Basic Syntactic Elements of C++

### 5.1 C++ Character Set (Sequence of Words)

C++ programs are constructed from words made of:

- **Letters:** uppercase (`A-Z`), lowercase (`a-z`), and the underscore `_` (treated syntactically as a letter).
    
- **Digits:** `0 1 2 3 4 5 6 7 8 9`.
    
- **Special Characters:** `! ^ & * ( ) - + = { } | ~ [ ] \ ; ' : " < > ? , . / $`.
    
- **Whitespace:** Space (`" "`), Tab (`"\t"`), Newline (`"\n"`).
    

### 5.2 Comments

Comments document code logic and are completely ignored by the compiler:

- **Single-line:** starting with `//`.
    
- **Multi-line:** enclosed within `/*` and `*/`.
    

C++

```
// Single-line comment

/* Multi-line 
   comment block */[cite: 1]
```

### 5.3 Identifiers and Naming Conventions

- **Identifier:** user-defined name given to variables, functions, or entities.
    
- **Rules:**
    
    - Must begin with a **letter** or an **underscore (`_`)**.
        
    - Cannot start with a digit (e.g., `1var` is **invalid**, `var1` is **valid**).
        
    - Cannot be a reserved language keyword.
        
- **Case Sensitivity:** C++ strictly distinguishes upper and lower case (e.g., `Fact` is different from `fact`).
    
- **Common Naming Styles:**
    
    - _Camel Case:_ `nameSurname` or `NameSurname`.
        
    - _Snake Case:_ `name_surname`.
        

### 5.4 Keywords

Reserved words with predefined, immutable meanings in the language syntax.

_Key C++ Keywords include:_ `asm`, `auto`, `break`, `case`, `catch`, `char`, `class`, `const`, `continue`, `default`, `delete`, `do`, `double`, `else`, `enum`, `extern`, `float`, `for`, `friend`, `goto`, `if`, `inline`, `int`, `long`, `new`, `operator`, `private`, `protected`, `public`, `register`, `return`, `short`, `signed`, `sizeof`, `static`, `struct`, `switch`, `template`, `this`, `throw`, `try`, `typedef`, `union`, `virtual`, `void`, `volatile`, `while`.

## 6. Literal Expressions (Literals)

Literals represent constant values written directly into the source code.

### 6.1 Character Literals & Escape Sequences

Character constants are enclosed in single quotes (e.g., `'n'`).

Control/special characters utilize **escape sequences** starting with a backslash `\`:

| **Sequence** | **Name / Meaning**    | **Description**                        |
| ------------ | --------------------- | -------------------------------------- |
| `\n`         | New Line (LF)         | Line feed / move to next line          |
| `\t`         | Horizontal Tab (HT)   | Add horizontal tab space               |
| `\v`         | Vertical Tab (VT)     | Vertical tab space (printer control)   |
| `\b`         | Backspace (BS)        | Move cursor back one space             |
| `\r`         | Carriage Return (CR)  | Return cursor to start of current line |
| `\f`         | Form Feed (FF)        | Page eject / printer form feed         |
| `\a`         | Acoustic Signal (BEL) | Trigger system alert audio/beep        |
| `\\`         | Backslash             | Literal `\` character                  |
| `\'`         | Single Quote          | Literal `'` character                  |
| `\"`         | Double Quote          | Literal `"` character                  |

### 6.2 String Literals

A sequence of characters enclosed within double quotation marks (e.g., `"Hello!"` or `"Hello World!\n"`).

### 6.3 Integer Representation

Integer literals can be specified in different bases:

- **Decimal (base 10):** default representation; starts with a non-zero digit (`0-9`). E.g., `12`, `-9`, `22`.
    
- **Octal (base 8):** prefixed with `0`; uses digits `0-7`. E.g., `012` (equals $10_{10}$), `010` (equals $8_{10}$).
    
    > **Note:** Writing `08` results in a compilation error because `8` is invalid in octal notation.
    
- **Hexadecimal (base 16):** prefixed with `0x` or `0X`; uses digits `0-9` and letters `A-F`. E.g., `0x12` (equals $18_{10}$), `0x7f`.
    
- **Binary (base 2):** prefixed with `0b`. E.g., `0b10` (equals $2_{10}$), `0b01100` (equals $12_{10}$).
    

### 6.4 Real Numbers (Floating-Point)

Real numbers use a decimal point `.` separating integer and fractional parts.

- **Scientific Notation:** uses `e` or `E` representing powers of 10 ($10^x$).
    
    - E.g., `-12.451e3` evaluates to `-12451`.
        
    - E.g., `-12.451e-3` evaluates to `-0.012451`.
        
- **Division Behavior:**
    
    - `4 / 3` performs integer division and results in `1`.
        
    - `4.0 / 3.0` or `4 / 3.0` performs floating-point division and results in `1.33333`.
        

## 7. Operators and Separators

### 7.1 Operators

Special characters or symbol combinations representing operations performed on values.

_Main C++ Operators:_ `+`, `-`, `*`, `/`, `%`, `=`, `==`, `!=`, `<`, `>`, `<=`, `>=`, `&&`, `||`, `++`, `--`, `+=`, `-=`, `*=`, `/=`, `::`, `[]`, `()`, `?:`, `->`.

**Operator Properties:**

- **Position:**
    
    - _Prefix:_ operator precedes its argument (e.g., `++x`).
        
    - _Postfix:_ operator follows its argument (e.g., `x++`).
        
    - _Infix:_ operator sits between arguments (e.g., `a + b`).
        
- **Arity:** number of required arguments (unary, binary, ternary).
    
- **Precedence:** evaluation priority order (e.g., `1 + 2 * 3` is evaluated as `1 + (2 * 3)` = `7`).
    
- **Associativity:** evaluation direction for operators with identical precedence (left-to-right or right-to-left).
    
### 7.2 Separators

Punctuation marks used to terminate statements, separate list items, or delineate code blocks.

- _Main Separators:_ `()`, `[]`, `{}`, `,`, `;`, `:`.
    
- Brackets must always be paired properly


# 5. Basic Concepts: What Is Programming?

## 5.1 Programming as problem solving

Programming starts from a **problem**, not from code.

A good programming process therefore begins with a conceptual representation of the solution:

```text
Problem statement
      ↓
Understand the context
      ↓
Identify requirements
      ↓
Design the algorithm
      ↓
Choose data structures
      ↓
Describe the logic (e.g. pseudocode)
      ↓
Implement in C++
      ↓
Compile
      ↓
Test and validate
```

The important point is that source code is the **implementation of a solution**, not the solution itself.

---

## 5.2 Algorithm

An **algorithm** is a well-defined procedure for solving a problem.

In practical programming, an algorithm can be viewed as a sequence of computational steps that transforms input information into the desired output.

### Important properties of a useful algorithm

A practical algorithm should be:

- **precise** — each step has a clear meaning;
- **finite** — execution should reach a termination condition when the problem requires it;
- **ordered** — the sequence of operations matters;
- **effective** — individual operations must be executable;
- **correct** — for valid inputs, it should produce the intended result.

The course connects algorithm design with pseudocode because pseudocode allows the programmer to express the logical structure without being tied immediately to C++ syntax.

---

## 5.3 Pseudocode

**Pseudocode** is an informal, structured description of an algorithm.

Example problem: calculate the larger of two numbers.

```text
INPUT a, b
IF a > b
    OUTPUT a
ELSE
    OUTPUT b
END IF
```

This is not C++, but it contains the essential algorithmic logic.

Its main function is to separate:

- **problem-solving logic** from
- **programming-language syntax**.

---

# 6. What Is a Programming Language?

A programming language is a rule-based notation that allows a programmer to describe computation and communicate to a computer what should be executed.

The course uses an analogy with natural languages.

A natural language such as English has:

- words;
- symbols;
- grammatical rules;
- meaning.

A programming language also has:

- symbols;
- words/tokens;
- rules;
- a defined meaning.

The fundamental formula is:

> **Language = Syntax + Semantics**

---

## 6.1 Syntax

**Syntax** describes the structure/form of code.

It specifies:

- how elements may be combined;
- which sequences of tokens are legal;
- the grammatical rules of the language.

Example:

```cpp
int x = 10;
```

This is syntactically well-formed.

By contrast:

```cpp
int = x 10;
```

does not follow C++ declaration syntax.

---

## 6.2 Semantics

**Semantics** describes the meaning of a syntactically valid construct.

For example:

```cpp
x = x + 1;
```

has a syntactic form that is valid, but its semantics are that the current value of `x` is increased by one and stored back into `x`.

### Why the distinction matters

A program may be:

- syntactically incorrect → compiler cannot properly parse/translate it;
- syntactically valid but semantically wrong → it compiles but does not solve the intended problem.

> [!important] Fundamental distinction
> **Syntax asks:** “Is the program written according to the language rules?”
>
> **Semantics asks:** “What does this program mean/do?”

---

# 7. Classification of Programming Languages

The course presents several classification criteria.

## 7.1 By intended domain

### General-purpose languages

Designed for many application domains.

Examples:

- C++
- Java
- Python

### Domain-specific languages

Designed primarily for a particular problem domain.

Examples may target:

- mathematical/statistical processing;
- document formatting;
- hardware description;
- database querying;
- other specialized contexts.

---

## 7.2 By abstraction level

```text
High-level languages
       ↓
Assembly / low-level languages
       ↓
Machine code
```

### High-level

Closer to human reasoning and more abstract from machine details.

Examples:

- C++
- Java
- Python
- JavaScript

### Low-level

Closer to the machine and hardware representation.

Assembly is a typical example.

### Machine code

The executable instructions directly understood by the target processor architecture.

---

## 7.3 By execution model

### Compiled

Source code is translated ahead of execution into lower-level/object/executable representations.

C++ is typically used as a compiled language.

### Interpreted

An interpreter executes instructions through an execution environment rather than producing a traditional native executable in the same way.

### Just-in-time (JIT) compiled

Code segments are compiled at runtime when needed.

---

## 7.4 By typing

The slides distinguish:

- **strongly typed** languages → conversions between incompatible types are more explicitly controlled;
- **weakly typed** languages → more automatic conversions may occur.

For C++, understanding type conversions is particularly important because mixed-type expressions may trigger implicit or explicit conversions.

---

# 8. Programming Paradigms

A **programming paradigm** is a general style or model of programming.

## 8.1 Procedural / imperative programming

Examples in the course:

- C
- Pascal
- Perl

Characteristics:

- procedure/problem-oriented;
- programs are organized as commands/instructions;
- instructions modify program state;
- functions and sequences of instructions are central;
- provides detailed control over computation.

This is the dominant model for the introductory C++ material in the course.

---

## 8.2 Object-oriented programming

Examples:

- C++
- Java
- Python
- C#

The basic concept is the **object**: a unit combining data and operations.

Important concepts include:

- classes;
- abstraction;
- encapsulation;
- inheritance;
- polymorphism;
- code reuse.

The course introduces object-oriented concepts later, but C++ itself supports this paradigm.

---

## 8.3 Logic / declarative programming

Examples given in the slides include Prolog.

Programs are expressed in terms of:

- facts;
- rules;
- questions/queries.

Instead of describing a sequence of machine operations in detail, the programmer describes logical relations and lets the execution mechanism derive consequences.

---

## 8.4 Functional programming

The central abstraction is the **function**.

Important characteristics:

- functions are composed and applied;
- mathematical-function ideas are important;
- recursion is commonly used;
- the paradigm is generally associated with declarative programming.

Functional-style programming can also be used inside multi-paradigm languages such as C++.

---

## 8.5 Scripting languages

Scripting languages are often used for:

- automation;
- data processing;
- web programming;
- rapid development.

---

## 8.6 Command-language programming

Shell languages such as:

- `sh`;
- `csh`;
- `bash`

are used to automate commands and processes.

A shell script is essentially a file containing commands that can be executed in sequence, with control structures and conditions.

---

## 8.7 Event-driven programming

The execution flow is driven by events such as:

- mouse actions;
- key presses;
- sensor events;
- messages;
- events generated by other threads or programs.

A common model is:

```text
Wait for event
     ↓
Event detected
     ↓
Call associated handler / callback
     ↓
Return to waiting state
```

---

## 8.8 Markup languages

Examples:

- HTML;
- XML.

A markup language uses structured symbols to control the structure or presentation of a document.

HTML, for example, describes the structure of web documents interpreted and displayed by browsers.

---

## 8.9 Other paradigms in the slides

The material also mentions:

- assembly programming;
- mathematical programming (e.g. R, MATLAB);
- natural-language programming.

Modern programming languages are often **multi-paradigm**, meaning that they support more than one programming style.
