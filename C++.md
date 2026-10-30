# 9. Why C++?

The course chooses **C++** as its principal programming language.

## 9.1 Characteristics of C++

C++ is presented as:

- a **general-purpose** programming language;
- a **multi-paradigm** language;
- a language supporting procedural, functional, and object-oriented approaches;
- an ISO/IEC-standardized language;
- an evolution of the C language.

The slides mention the following standards:

- C++98
- C++03
- C++11
- C++14
- C++17
- C++20

The language originated at Bell Labs in the early 1980s as an evolution of C.

---

## 9.2 Why C++ is useful for this course

The material identifies several reasons.

### Understanding computers

C++ is useful for learning about:

- how data is represented;
- how memory is managed;
- how compilers work;
- compile time and load time;
- pointers;
- dynamic libraries;
- optimization.

### Learning other languages

Once fundamental programming concepts are understood in C++, transferring them to other languages becomes easier.

### Performance

C++ enables the development of lightweight, high-performance software.

### System-level interaction

C++ can interact closely with:

- operating-system facilities;
- network APIs;
- hardware-oriented environments;
- embedded systems.

---

# 10. C++ Source Code and Program Development

## 10.1 Source code

A C++ program is written in one or more text files called **source code**.

Typical source extensions include:

- `.cc`
- `.cpp`
- `.h`
- `.hpp`

Source code is:

- human-readable;
- editable with a text editor;
- translated by a compiler.

---

## 10.2 Compilation and linking

A simplified development pipeline is:

```text
Source code
   │
   │ compiler
   ▼
Object file(s)
   │
   │ linker + libraries
   ▼
Executable file
```

For example:

```bash
g++ -c test.cc
```

creates an object file such as `test.o`.

Then:

```bash
g++ test.o
```

links the object file with the required system libraries and produces an executable, traditionally named `a.out` by default.

A specific output name can be assigned with `-o`:

```bash
g++ test.o -o test
```

Or compilation and linking can be performed together:

```bash
g++ test.cc -o test
```

Execution in Linux is then:

```bash
./test
```

or, with the default output:

```bash
./a.out
```

---

## 10.3 Important compiler options

The laboratory material introduces several `g++` options:

| Option | Purpose |
|---|---|
| `-L<dir>` | Search directory for binary libraries |
| `-lfoo` | Link library `foo` |
| `-I<dir>` | Search directory for header/include files |
| `-g` | Generate debugging information |
| `-g0` | Disable generation of source-level debug information |
| `-Wall` | Enable a broad set of warnings |
| `-o <name>` | Specify executable name |
| `-v` | Verbose compiler output |

A common development command is therefore:

```bash
g++ -g -Wall source.cpp -o program
```

---

# 11. Structure of a C++ Program

A minimal C++ program is:

```cpp
int main()
{
    return 0;
}
```

The course uses a common template:

```cpp
#include <iostream>
using namespace std;

int main()
{
    // instructions
    return 0;
}
```

### Components

#### `#include <iostream>`

Requests the declarations needed for standard input/output facilities such as `cin`, `cout`, and `cerr`.

#### `using namespace std;`

Makes names from the `std` namespace available without writing `std::` each time.

#### `int main()`

The main function is the program's starting function in the course examples.

#### `{ ... }`

Defines the body/block of the function.

#### `return 0;`

Terminates `main` and returns the value `0` to the operating environment.

---

# 12. Basic C++ Lexical Concepts

## 12.1 Characters used in C++ source

Source code is built from:

- letters;
- digits;
- special characters;
- whitespace.

Whitespace includes, among other things:

- spaces;
- tabs;
- new-line characters.

Whitespace often separates tokens without changing their semantic meaning.

---

## 12.2 Comments

Comments are ignored for program execution.

### Single-line comment

```cpp
// this is a comment
```

### Multi-line comment

```cpp
/*
   this comment can span
   multiple lines
*/
```

Comments are used to document code and explain intent.

---

## 12.3 Identifiers

An **identifier** is a programmer-defined name, for example:

```cpp
int age;
double average;
```

The course emphasizes:

- identifiers should be meaningful;
- C++ is **case-sensitive**;
- `_` is a valid identifier character;
- reserved keywords cannot be used as ordinary identifiers.

Examples:

```text
Fact ≠ fact
```

Naming styles mentioned include:

```text
nameSurname
NameSurname
name_surname
```

Bad example:

```cpp
int 1var;   // invalid
```

Good example:

```cpp
int var1;   // valid
```

> [!tip] Style rule
> Meaningful identifiers reduce cognitive load and improve readability and maintainability.

---

## 12.4 Keywords

Keywords are reserved words whose meaning is predefined by C++.

Examples:

```text
int
char
const
if
else
for
while
switch
case
enum
return
void
class
struct
static
sizeof
```

They cannot be redefined as ordinary variable names.

---

# 13. Literals

A **literal** is a constant value written directly in the source code.

The course distinguishes:

- character literals;
- string literals;
- integer literals;
- real / floating-point literals.

---

## 13.1 Character literals

A character literal is written between single quotes:

```cpp
'A'
'a'
'7'
'+'
```

It represents one character.

---

## 13.2 Escape sequences

Escape sequences represent special characters inside character or string literals.

Important examples:

| Escape | Meaning |
|---|---|
| `\n` | new line |
| `\t` | horizontal tab |
| `\v` | vertical tab |
| `\b` | backspace |
| `\r` | carriage return |
| `\f` | form feed |
| `\a` | acoustic signal |
| `\\` | backslash |
| `\'` | single quote |
| `\"` | double quote |

Example:

```cpp
cout << "Hello\nWorld";
```

produces two lines.

---

## 13.3 String literals

A string literal is a sequence of characters between double quotes:

```cpp
"Hello!"
"Hello World!\n"
```

String literals can contain:

- spaces;
- punctuation;
- non-alphanumeric characters;
- escape sequences.

---

## 13.4 Integer literals and number bases

The slides explicitly distinguish multiple representations.

### Decimal

Default representation:

```cpp
12
-9
22
```

### Octal

Starts with `0`:

```cpp
012
```

`012` represents decimal `10`.

### Hexadecimal

Starts with `0x` or `0X`:

```cpp
0x12
```

`0x12` represents decimal `18`.

### Binary

The slides also mention binary notation with `0b`, for example:

```cpp
0b10      // decimal 2
0b01100   // decimal 12
```

> [!warning] Common trap
> In C++ an integer literal beginning with `0` can be interpreted as octal. Thus `010` is not decimal ten; it is octal ten, equal to decimal eight.

---

## 13.5 Floating-point literals

A real number can be represented using:

- a decimal point;
- scientific notation with `e` or `E`.

Examples:

```cpp
0.0000234
-12.45
-12.451e3
-12.451e-3
```

The notation:

```text
1.23e4
```

means:

```text
1.23 × 10^4
```

---

# 14. Operators and Expressions

Operators specify operations on values.

Examples include:

```text
+  -  *  /  %
== != < > <= >=
&& || !
= += -= *= /= %=
++ --
& | ^ ~ << >>
? :
```

---

## 14.1 Prefix, postfix, infix

### Prefix

Operator before its operand:

```cpp
++x
!x
-x
```

### Postfix

Operator after its operand:

```cpp
x++
x--
```

### Infix

Operator between operands:

```cpp
x + y
x < y
x && y
```

---

## 14.2 Arity

The **arity** of an operator is the number of operands it operates on.

Examples:

- unary: `-x`
- binary: `x + y`

---

## 14.3 Precedence

Precedence determines which operators are applied first.

Example:

```cpp
1 + 2 * 3
```

is interpreted as:

```cpp
1 + (2 * 3)
```

not:

```cpp
(1 + 2) * 3
```

Use parentheses whenever they make the intended evaluation order clearer.

---

## 14.4 Associativity

When operators have the same precedence, **associativity** determines how the expression is grouped.

For example, assignment is right-associative:

```cpp
a = b = c = 5;
```

is grouped as:

```cpp
a = (b = (c = 5));
```

---

## 14.5 Separators

Important separators include:

```text
( )  ,  ;  :  { }
```

They can:

- terminate statements;
- separate list elements;
- group expressions;
- delimit blocks.

Brackets must be balanced and correctly paired.

---

# 15. Variables and Constants

## 15.1 Why variables exist

A variable provides a named memory location in which a value can be stored.

The course characterizes a variable by four main concepts:

- **name / identifier**;
- **type**;
- **memory location / address (l-value)**;
- **value (r-value)**.

A useful mental model is:

```text
identifier ─────► memory location
                    │
                    ▼
                  value
```

---

## 15.2 Definition

A variable definition specifies its type and identifier:

```cpp
int x;
```

The compiler can then arrange the required storage.

---

## 15.3 Definition with initialization

```cpp
int x = 3;
float pi = 3.1415;
char c = 'C';
```

The initial value is assigned when the variable is created.

A variable can also be initialized with an expression:

```cpp
int x = 3 * 2;
int y = 3 * x;
```

---

## 15.4 Declaration

The course introduces external declarations with `extern`:

```cpp
extern int x;
```

This tells the compiler that the variable is defined elsewhere.

### Difference

| Definition | Declaration |
|---|---|
| Provides the variable definition/storage arrangement | Refers to a variable defined elsewhere |
| Example: `int x;` | Example: `extern int x;` |

---

## 15.5 Initialization matters

An uninitialized local variable does **not** automatically contain a meaningful value.

Therefore:

```cpp
int x;
cout << x;
```

can produce an unpredictable/indeterminate result.

> [!important] Best practice from the course
> **Always initialize defined variables whenever possible.**

---

## 15.6 Constants

A constant is declared with `const`:

```cpp
const int kilo = 1024;
const double pi = 3.14159;
```

After initialization, the constant cannot be modified:

```cpp
const int x = 10;
x = 20;   // error
```

A constant should normally be initialized with a value that is known appropriately at compile time when the course material requires it.

---

# 16. Scope, Blocks, and Visibility

A **compound statement** is a sequence of statements enclosed in `{` and `}`.

```cpp
{
    int a = 4;
    a *= 6;
    char b = 'c';
}
```

The enclosed sequence is a **block**.

Variables defined in a block have visibility limited by their scope.

Example:

```cpp
int n = 44;

{
    cout << n << endl;  // outer n is visible
    int n = 3;          // inner n shadows outer n
    cout << n << endl;  // prints 3
}

cout << n << endl;      // outer n is visible again
```

### Shadowing principle

If an identifier exists in both an outer and an inner scope, the **innermost declaration takes precedence** while that inner scope is active.

> [!tip] Readability
> Avoid unnecessary shadowing even when the language allows it. It makes code harder to reason about.

