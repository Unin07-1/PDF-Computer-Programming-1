# 58. Laboratory Rules and Workflow

One important course rule is:

> **While completing an exercise, use only the C++ concepts and instructions that have already been covered at that point.**

This prevents students from solving an introductory exercise using advanced constructs that bypass the intended learning objective.

Example:

If the exercise only covered:

- variables;
- `cout`;
- arithmetic;

then the solution should not suddenly introduce:

- advanced classes;
- external libraries;
- complex abstractions;
- unnecessary helper frameworks.

---

# 59. Laboratory Exercise 1 — Terminal / Shell

## Task 01: Navigate the filesystem

Required operations:

1. open a terminal;
2. navigate until the `Desktop` directory is reached;
3. list the elements of the current directory;
4. close the terminal.

Typical command sequence:

```bash
pwd
ls
cd Desktop
ls
exit
```

The exact path to `Desktop` depends on the starting directory.

### Skills tested

- terminal interaction;
- navigation;
- understanding current working directory;
- listing files/directories.

---

# 60. Laboratory Exercise 2 — Create and Remove a Directory

## Task 02

Required operations:

1. open terminal;
2. create a directory named `test`;
3. enter the directory;
4. create an empty file;
5. return to the parent directory;
6. verify that `test` exists;
7. remove `test`;
8. verify that `test` no longer exists;
9. close terminal.

Typical workflow:

```bash
mkdir test
cd test
touch example.txt
cd ..
ls
```

After verifying the directory contents, remove the directory using an appropriate directory-removal command.

### Skills tested

- directory creation;
- navigation with `cd`;
- file creation with `touch`;
- parent-directory navigation;
- checking filesystem state;
- safe deletion.

---

# 61. Laboratory Exercise 3 — First C++ Program

## Task

Write the simplest valid C++ program containing an empty `main` function.

```cpp
int main()
{
    return 0;
}
```

Then:

1. save the source file as `example.cpp`;
2. compile it:

```bash
g++ example.cpp
```

3. execute the result:

```bash
./a.out
```

### What is learned?

This tiny exercise establishes the complete source-to-executable workflow:

```text
source file
   ↓
compiler
   ↓
a.out
   ↓
execution
```

The exercise is deliberately performed using only a text editor rather than an IDE, so that the student understands the underlying development process.

---

# 62. Laboratory Exercise 4 — Variables

## Task 04a

Write a C++ program that:

1. defines an integer variable;
2. defines a `char` variable;
3. initializes them with `12` and `'A'`;
4. prints their values.

A corresponding implementation is:

```cpp
#include <iostream>
using namespace std;

int main()
{
    int x = 12;
    char y = 'A';

    cout << "Print the value of x: " << x << endl;
    cout << "Print the value of y: " << y << endl;

    return 0;
}
```

### Conceptual goals

- variable definition;
- initialization;
- `int` vs `char`;
- output stream;
- compilation and execution.

---

## Task 04b — Modify variables

Repeat the previous exercise but modify the variables after the first output:

```cpp
x = 14;
y = 'B';
```

Then print again.

This introduces the core property of a variable:

> **A variable stores a value that can change during execution.**

---

## Task 04c — Recompile after changing initialization

A further variant changes the initialization and requires recompilation and execution.

This demonstrates a crucial distinction:

```text
source code change
      ↓
new compilation
      ↓
new executable behavior
```

Editing a source file does not automatically modify the already compiled executable unless the program is rebuilt.

---

# 63. Laboratory Exercise 5 — Reading a Character

## Task

Write a program that:

1. reads one character from the keyboard;
2. stores it in a variable;
3. prints the stored character.

Example:

```cpp
#include <iostream>
using namespace std;

int main()
{
    char character;

    cout << "Please, insert a character: ";
    cin >> character;

    cout << "The inserted character is: " << character << endl;

    return 0;
}
```

### Conceptual flow

```text
Keyboard
   ↓
cin
   ↓
char variable
   ↓
cout
   ↓
Screen
```

This is the first laboratory exercise that explicitly combines **input** and **output** streams.

---

# 64. Laboratory Exercises on Basic Concepts — Without Input

The second exercise sheet introduces computational problems using variables and arithmetic.

---

## 64.1 Exercise: Social Distance / Circle

### Task

Write a program that:

1. assigns a radius `r` expressed as a real number;
2. calculates the area of the circle;
3. calculates the circumference;
4. prints the radius, area, and circumference.

Constraint from the sheet:

> use `float` as the radius data type.

The standard mathematical formulas are:

```text
A = πr²
C = 2πr
```

A source-friendly implementation using concepts already introduced is:

```cpp
#include <iostream>
using namespace std;

int main()
{
    float r = 2.0;
    float area = 3.14159f * r * r;
    float circumference = 2.0f * 3.14159f * r;

    cout << "Radius = " << r << endl;
    cout << "Area = " << area << endl;
    cout << "Circumference = " << circumference << endl;

    return 0;
}
```

### Concepts tested

- `float`;
- variable initialization;
- arithmetic expressions;
- multiplication;
- output with `cout`.

---

## 64.2 Exercise: American Thermometer

### Problem

A thermometer provides temperature in Fahrenheit, but the required output is Celsius.

Use the standard conversion:

```text
C = (F - 32) × 5 / 9
```

Example:

```cpp
#include <iostream>
using namespace std;

int main()
{
    float fahrenheit = 86.0f;
    float celsius = (fahrenheit - 32.0f) * 5.0f / 9.0f;

    cout << "Fahrenheit = " << fahrenheit << endl;
    cout << "Celsius = " << celsius << endl;

    return 0;
}
```

### Concepts tested

- `float` arithmetic;
- operator precedence;
- use of parentheses;
- arithmetic conversion.

---

# 65. Exercise: Uppercase → Lowercase

## Problem

Given alphanumeric characters stored in separate variables, convert uppercase letters to their lowercase equivalents.

Constraints:

- do not use a dedicated library/function for automatic conversion;
- use character representation principles and the ASCII table as suggested by the exercise.

For the standard ASCII alphabet, uppercase and lowercase letters are separated by a fixed offset.

Conceptually:

```text
'A' → 'a'
'B' → 'b'
...
'Z' → 'z'
```

A character conversion can therefore be based on the numerical distance between corresponding uppercase and lowercase characters.

### Important caution

The exercise is intended to teach character representation. The code should follow the exact concepts covered by the course at that point.

---

# 66. Exercise: Seconds to Midnight

## 66.1 Forward conversion

Given:

- hours `h`;
- minutes `m`;
- seconds `s`;

calculate how many seconds remain until midnight.

There are:

```text
24 × 60 × 60 = 86400 seconds
```

in a complete day.

The current time in seconds from midnight is:

```text
h × 3600 + m × 60 + s
```

Therefore:

```text
secondsToMidnight = 86400 - (h × 3600 + m × 60 + s)
```

Example from the exercise:

```text
23:59:59 → 1 second
```

Another example:

```text
22:15:02 → 6298 seconds
```

A basic implementation is:

```cpp
#include <iostream>
using namespace std;

int main()
{
    int hours = 22;
    int minutes = 15;
    int seconds = 2;

    int secondsToMidnight =
        24 * 60 * 60 - (hours * 60 * 60 + minutes * 60 + seconds);

    cout << "hours = " << hours << endl;
    cout << "minutes = " << minutes << endl;
    cout << "seconds = " << seconds << endl;
    cout << "seconds to midnight = " << secondsToMidnight << endl;

    return 0;
}
```

---

## 66.2 Reverse conversion

Given the number of seconds remaining until midnight, reconstruct:

- hours;
- minutes;
- seconds.

The essential tools are integer division `/` and remainder `%`.

For a number `t`:

```text
hours = t / 3600
remainder = t % 3600
minutes = remainder / 60
seconds = remainder % 60
```

Example:

```text
6298 seconds
```

gives:

```text
hours   = 6298 / 3600 = 1
remainder = 6298 % 3600 = 2698
minutes = 2698 / 60 = 44
seconds = 2698 % 60 = 58
```

so:

```text
1 h 44 min 58 s
```

### Why this exercise is important

It combines several fundamental concepts at once:

- integer variables;
- arithmetic expressions;
- integer division;
- remainder `%`;
- decomposition of a quantity into units.

---

# 67. Additional Laboratory Variants

## Variant 1 — Sphere volume

Given a radius as input, calculate the volume of the corresponding sphere.

Standard formula:

```text
V = 4/3 × π × r³
```

The exercise is useful for reinforcing real-number arithmetic and powers represented by repeated multiplication.

For example:

```cpp
double volume = (4.0 / 3.0) * pi * r * r * r;
```

Note the use of `4.0 / 3.0` rather than `4 / 3`, because the latter is integer division.

---

## Variant 2 — Quotient and remainder

Given:

- dividend;
- divisor;

compute:

- quotient;
- remainder.

Use:

```cpp
quotient = dividend / divisor;
remainder = dividend % divisor;
```

Example:

```text
17 / 5 = 3
17 % 5 = 2
```

This exercise explicitly tests the difference between **division** and **remainder**.

---

# 68. Important Connections Between Concepts

## 68.1 Type → operator → result

The type of an operand affects the meaning of an operation.

Example:

```cpp
4 / 3
```

→ integer division.

Whereas:

```cpp
4.0 / 3.0
```

→ floating-point division.

This is why data types are not merely storage labels: they influence program semantics.

---

## 68.2 Stream → conversion → variable

Input is not simply “copy text into a variable.”

Conceptually:

```text
characters in stream
       ↓
interpretation/conversion
       ↓
value of requested type
       ↓
variable
```

An input error occurs when that conversion cannot be performed as required.

---

## 68.3 Algorithm → instructions

The algorithm is language-independent logic.

C++ instructions implement that logic.

Example:

```text
Algorithm:
If x is greater than zero, print x.
```

becomes:

```cpp
if (x > 0)
    cout << x << endl;
```

---

## 68.4 Scope → lifetime/visibility

A block defines a region of visibility for local objects.

This is why:

```cpp
{
    int x = 10;
}

cout << x;   // invalid: x is outside its scope
```

The name `x` is only available within the block where it was defined.

---

## 68.5 Text editor → compiler → executable

The complete development chain is:

```text
Editor
  ↓
Source code (.cpp/.cc)
  ↓
g++ / compiler
  ↓
Object code
  ↓
Linker + libraries
  ↓
Executable
```

An IDE integrates many of these activities but does not change the fundamental process.

---

# 69. High-Priority Exam Concepts

> [!important] Memorize these distinctions
>
> ### Syntax vs semantics
> - Syntax = form/structure.
> - Semantics = meaning.
>
> ### Coding vs programming
> - Coding = writing source code.
> - Programming = analysis + algorithm + data structures + implementation + testing + maintenance.
>
> ### Definition vs declaration
> - Definition creates/defines the variable.
> - Declaration states that the variable exists elsewhere.
>
> ### Variable vs constant
> - Variable can change.
> - `const` object cannot be changed after initialization.
>
> ### `=` vs `==`
> - `=` assignment.
> - `==` equality comparison.
>
> ### `x++` vs `++x`
> - postfix: old value is used, then increment;
> - prefix: increment first, then new value is used.
>
> ### `while` vs `do-while`
> - `while`: body may execute zero times;
> - `do-while`: body executes at least once.
>
> ### `while` vs `for`
> - both can implement repetition;
> - `for` groups initialization, condition, and update in one construct.
>
> ### `cout` vs `cin`
> - `cout` → output;
> - `cin` → input.
>
> ### `cin.fail()` vs `cin.clear()`
> - `fail()` checks the error state;
> - `clear()` resets the stream state.
>
> ### integer division vs real division
> - `5 / 2` → `2`;
> - `5.0 / 2.0` → `2.5`.
>
> ### logical vs bitwise operators
> - `&&`, `||`, `!` → logical conditions;
> - `&`, `|`, `^`, `~`, `<<`, `>>` → bitwise/integer operations.

---

# 70. Common Beginner Errors

## Error 1 — Forgetting the semicolon

Wrong:

```cpp
int x = 10
```

Correct:

```cpp
int x = 10;
```

---

## Error 2 — Using `=` in a comparison

Wrong:

```cpp
if (x = 5)
```

The statement performs an assignment.

For equality comparison:

```cpp
if (x == 5)
```

---

## Error 3 — Integer division when floating-point division was intended

Wrong for a mathematical `4/3`:

```cpp
double x = 4 / 3;
```

The expression `4 / 3` is evaluated as integer division first.

Prefer:

```cpp
double x = 4.0 / 3.0;
```

---

## Error 4 — Reading incompatible input

```cpp
int x;
cin >> x;
```

If the input is not interpretable as an integer, check `cin.fail()`.

---

## Error 5 — Using an uninitialized variable

Wrong:

```cpp
int x;
cout << x;
```

Better:

```cpp
int x = 0;
```

---

## Error 6 — Forgetting `break` in `switch`

An omitted `break` may intentionally or accidentally execute the next case.

Always ask:

> “Do I want fall-through here?”

---

## Error 7 — Infinite loop

Example:

```cpp
int i = 0;
while (i < 5)
{
    cout << i << endl;
}
```

`i` never changes, so the condition remains true.

Correct:

```cpp
while (i < 5)
{
    cout << i << endl;
    i++;
}
```

---

## Error 8 — Excessively complicated expressions

Avoid code such as:

```cpp
j = ++x * x++;
```

Prefer several explicit statements.

---

# 71. Mental Model for Solving a Programming Exercise

When an exercise is presented, follow this checklist.

## Step 1 — Identify the data

Ask:

- What values are provided?
- What variables are required?
- Which type does each variable need?

## Step 2 — Identify the required output

Ask:

- What must be printed?
- In what order?
- In what units?

## Step 3 — Write the mathematics / logic

Before C++:

```text
formula
or
condition
or
repetition rule
```

## Step 4 — Select operators

Examples:

- addition → `+`;
- remainder → `%`;
- comparison → `==`, `<`, `>`;
- logical composition → `&&`, `||`;
- repetition → `while`, `for`, `do-while`.

## Step 5 — Translate into C++

Start from:

```cpp
#include <iostream>
using namespace std;

int main()
{
    // variables
    // computation
    // output
    return 0;
}
```

## Step 6 — Compile early

Do not write the whole solution before testing whether the structure compiles.

## Step 7 — Test edge cases

Examples:

- zero;
- negative values where allowed;
- boundary values;
- empty/invalid input where relevant;
- minimum/maximum values.

---

# 72. Quick Reference — C++ Fundamentals

## Types

```cpp
int
short
long
long long
unsigned int
float
double
long double
char
bool
enum
```

## I/O

```cpp
#include <iostream>

cout << x;
cin >> x;
cerr << "Error";
```

## Arithmetic

```cpp
+
-
*
/
%
```

## Comparison

```cpp
==
!=
<
>
<=
>=
```

## Logical

```cpp
!
&&
||
```

## Assignment

```cpp
=
+=
-=
*=
/=
%=
```

## Increment / decrement

```cpp
++x
x++
--x
x--
```

## Conditional

```cpp
if (...)
{
}
else
{
}
```

```cpp
switch (x)
{
    case 1:
        ...
        break;
    default:
        ...
}
```

## Loops

```cpp
while (condition)
{
}
```

```cpp
do
{
}
while (condition);
```

```cpp
for (init; condition; update)
{
}
```

## Useful stream-state tools

```cpp
cin.eof()
cin.fail()
cin.clear()
cin.ignore()
```

## Useful compiler commands

```bash
g++ program.cpp
./a.out
```

```bash
g++ program.cpp -o program
./program
```

```bash
g++ -g -Wall program.cpp -o program
```

## Useful Linux commands

```bash
pwd
ls
cd
mkdir
touch
rm
cat
cp
mv
man
clear
exit
```

---

# 73. Study Summary

The logical hierarchy of the introductory course can be remembered as follows:

```text
COMPUTER
│
├── represents and stores data
│
├── executes machine instructions
│
└── runs programs
      │
      ▼
PROGRAMMING LANGUAGE
      │
      ├── syntax
      └── semantics
      │
      ▼
ALGORITHM
      │
      ├── input
      ├── computation
      ├── decisions
      └── repetition
      │
      ▼
C++ PROGRAM
      │
      ├── variables / constants
      ├── types
      ├── expressions
      ├── instructions
      ├── streams
      └── functions
      │
      ▼
SOURCE CODE
      │
      ▼
COMPILER + LINKER
      │
      ▼
EXECUTABLE
      │
      ▼
TESTING / DEBUGGING
      │
      ▼
CORRECT SOFTWARE
```

The central conceptual chain is:

> **Problem → Algorithm → Program → Compilation → Execution → Testing**

Everything introduced in the first part of Computer Programming 1 fits into this chain.

---

# 74. Obsidian Tags / Links Suggested for Revision

Possible tags:

```text
#computer-programming
#cpp
#algorithms
#programming-languages
#syntax
#semantics
#variables
#datatypes
#streams
#iostream
#control-flow
#if
#switch
#loops
#linux
#bash
#terminal
#wsl
#vscode
#laboratory
```

Useful Obsidian note structure for later lectures:

```text
[[Computer Programming 1 — University Notes]]
[[Algorithms]]
[[C++ Variables and Types]]
[[C++ Streams]]
[[C++ Control Structures]]
[[C++ Functions]]
[[C++ Arrays]]
[[C++ Pointers]]
[[C++ Dynamic Memory]]
[[C++ Data Structures]]
[[Linux Commands]]
[[WSL and VS Code]]
```

---
