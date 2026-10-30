# 1. Course Introduction

## 1.1 Course identity and organization

The course is **Computer Programming 1 (ENG)** at the **University of Trento**, Department of Information Engineering and Computer Science (DISI), within the English-taught undergraduate curricula.

According to the course introduction:

- academic year: **2026–2027**;
- semester: **first semester**;
- lessons: **September–December 2026**;
- examination periods: **January/February 2027, June/July 2027, September 2027**;
- teaching modality: **in person**;
- workload: approximately **45% lectures and 55% laboratory**;
- credits: **12 ECTS/CFU**;
- one CFU corresponds to 25 hours of total student workload, so 12 CFU correspond to approximately **300 hours**.

The official course infrastructure is divided mainly between:

- **Esse3** → administrative and examination information, including examination registration;
- **Moodle** → course news, communications, slides, exercises, source code, and other teaching material.

The strong laboratory component is important: this is not a course in which theoretical study can be separated completely from practical programming.

---

## 1.2 Informatics vs Computer Science

The slides distinguish between **Informatics** and **Computer Science**, while acknowledging that the terms are often informally used as synonyms.

### Informatics

Informatics is presented as the study of:

- computation;
- information;
- automation;
- representation of information;
- processing of information;
- communication of information in natural and engineered systems.

It therefore covers scientific and technical aspects of collecting, representing, processing, and communicating information.

### Computer Science

Computer Science is associated more directly with:

- theoretical subjects such as algorithms and information theory;
- practical subjects such as software design and implementation;
- applications in areas such as engineering, science, medicine, law, etc.;
- computer programming as a central practical activity.

### Example: software engineering

The course uses a management/customer-care system as an example.

**Informatics-oriented questions** might concern:

- how information flows through the system;
- which users interact with the system;
- which roles and responsibilities exist;
- how the information flow can be optimized.

**Computer-Science-oriented questions** might concern:

- implementation of the software platform;
- selection/design of an efficient sorting algorithm;
- compiler optimization;
- reduction of computational time and complexity.

> [!note] Core idea
> Computer Programming 1 sits strongly on the Computer Science side of this distinction because it teaches how to formulate algorithms and implement them as executable software.

---

# 2. Course Objectives and Learning Outcomes

## 2.1 Main objective

The course aims to provide basic knowledge of four closely connected areas:

1. **Computers**
2. **Algorithms**
3. **Programming languages**
4. **Programming**

It is explicitly an **introductory programming course**, not a course on advanced application development.

The course is not primarily about developing web, mobile, or network applications. Its central purpose is to establish the conceptual and practical foundations required to model, solve, and implement relatively simple problems.

---

## 2.2 Expected learning outcomes

By the end of the course, a student should be able to:

- explain fundamental notions of **imperative programming**, especially in C++;
- analyze problems of simple to average difficulty;
- identify an algorithmic solution to a problem;
- implement that solution in C++;
- work with fundamental data types and program structures;
- understand the organization and execution of a basic C++ program;
- begin to understand object-oriented concepts;
- use programming tools and a compiler in a controlled laboratory environment.

The course therefore develops both **conceptual competence** and **operational competence**.

---

## 2.3 Coding is not programming

One of the fundamental distinctions of the course is:

> **Coding is a part of programming, not a synonym for programming.**

### Coding

Coding is the activity of writing source code in a programming language so that a computer can execute the specified instructions.

A coder needs, above all:

- knowledge of the selected language;
- knowledge of its syntax;
- knowledge of its semantics;
- ability to express an already-defined solution in code.

### Programming

Programming is broader. It includes:

1. understanding the problem;
2. defining requirements;
3. analyzing the problem;
4. designing a solution;
5. defining an algorithm;
6. selecting suitable data structures;
7. implementing the algorithm;
8. compiling/building the program;
9. testing and debugging;
10. deploying the solution;
11. maintaining and evolving the software.

A useful mental model is:

```text
Problem
  ↓
Analysis
  ↓
Algorithm + Data Structures
  ↓
Pseudocode / design
  ↓
Implementation (C++)
  ↓
Compilation + Linking
  ↓
Testing / Debugging
  ↓
Executable program
  ↓
Maintenance / Evolution
```

> [!tip] Exam concept
> If asked to distinguish **coding** from **programming**, emphasize that coding is primarily implementation, while programming includes the entire problem-solving and software-development process.

---

# 3. Course Content / Roadmap

The course content is presented as a set of related topics rather than as a rigid linear sequence.

## 3.1 Basic programming concepts

- algorithms;
- programming phases;
- programming languages;
- computer architecture concepts relevant to programming;
- introduction to imperative C++.

## 3.2 Variables and predefined types

- representation;
- variables and constants;
- integers;
- booleans;
- real numbers;
- characters;
- enumerations.

## 3.3 Control structures

- simple instructions;
- expressions;
- assignments;
- sequences;
- conditions;
- cycles.

## 3.4 Input / Output

- standard input/output;
- file input/output;
- command-line arguments (`argc`, `argv`).

## 3.5 Program organization

- multiple source files;
- functions;
- parameter passing;
- recursive functions;
- introductory classes.

## 3.6 Advanced data types and memory management

The course outline also includes:

- arrays;
- ordered arrays;
- multidimensional arrays / matrices;
- strings and texts;
- pointers and references;
- `struct`;
- dynamic memory allocation;
- dynamic arrays and dynamically allocated structures;
- lists;
- stacks;
- queues;
- binary trees;
- graphs.

## 3.7 Advanced topics

Depending on available time, the course may also address:

- object-oriented programming;
- testing;
- debugging;
- generative programming and related topics.

> [!note] Scope of these notes
> The present document focuses on the material explicitly requested: course introduction, concepts and notions, basic concepts, data types, streams, programming/instructions, IDE/WSL, laboratory introduction, and the initial laboratory exercises.

---

# 4. Teaching, Learning, and Assessment

## 4.1 Teaching method

The course combines three complementary activities.

### Lectures

Lectures provide:

- theoretical notions;
- principles;
- tools;
- examples of problems;
- examples of programs.

### Laboratory

Laboratory activities use **learning by doing**. The primary purpose is practical training in C++ programming.

### Tutoring

Tutoring sessions take place in the laboratory and can involve:

- tutoring by teachers;
- tutoring by other students.

The objective is to support the learning process while students work on practical programming problems.

---

## 4.2 Assessment

The examination is divided into two parts:

| Part | Weight |
|---|---:|
| Theory | 40% |
| Laboratory | 60% |

The final grade is expressed in **/30** and is computed as the weighted average of the two components.

Both parts must be passed with at least **18/30**.

The exam contains:

- closed and open questions for theoretical competence;
- programming exercises for practical C++ competence.

The exam is performed in the laboratory using the laboratory PC and tools; the course material specifies that other material is not admitted.

Each examination session lasts approximately **2 hours**.

---

## 4.3 Recommended study method

The course material recommends a strongly practice-oriented strategy:

1. attend lectures and laboratories;
2. study progressively rather than accumulating material;
3. repeat the examples presented during class;
4. execute the exercises yourself;
5. practice interpreting compiler and debugger messages;
6. write clear, readable, indented code;
7. ask questions when something is unclear.

### Programming practice principle

> **Experience is a fundamental part of learning programming.**

Knowing a syntax rule is not enough. The student must learn to recognize problems, implement solutions, compile, interpret errors, test, and revise the code.

---
