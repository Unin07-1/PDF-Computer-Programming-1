# 17. Fundamental Data Types

The course distinguishes:

### Fundamental types

Used to represent simple information:

- integer;
- Boolean;
- enumeration;
- character;
- real/floating-point.

### Derived types

Used to build more complex structures from fundamental types, for example:

- arrays;
- pointers;
- Reference;
- other constructed data types.

Types provide two major advantages:

1. **semantic correctness** — operations are interpreted according to the object's type;
2. **memory efficiency** — the compiler knows what storage and representation are required.

---

# 18. Integer Types

The main integer types introduced are:

```cpp
short
int
long
long long
```

They can also appear in unsigned form:

```cpp
unsigned short
unsigned int
unsigned long
unsigned long long
```

---

## 18.1 Signed integers

A signed integer can represent negative and positive values.

For a representation using `N` bits, the course describes the typical range as:

```text
[-2^(N-1), 2^(N-1)-1]
```

The slides discuss two's-complement representation.

Typical implementation examples given in the course include:

| Type | Typical size in the slides | Typical signed range |
|---|---:|---:|
| `short` | 16 bits | -32768 … 32767 |
| `int` | 32 bits | -2147483648 … 2147483647 |
| `long` | 64 bits in the example environment | large signed range |
| `long long` | 64 bits | large signed range |

> [!warning] Portability
> Exact sizes depend on the implementation/platform. Never assume that every C++ implementation has exactly the same size for every integer type.

---

## 18.2 Signed overflow and representation limits

The slides use an example such as:

```text
32767 + 1
```

for a signed 16-bit `short`, illustrating that finite representations cannot represent every mathematical integer.

The important conceptual point is:

> A computer type represents a finite set of values, not an unlimited mathematical number system.

---

## 18.3 Unsigned integers

Unsigned integer types represent non-negative values.

With `N` bits, the conceptual range is:

```text
[0, 2^N - 1]
```

Examples:

```cpp
unsigned int x = 1232;
unsigned short y = 567;
```

Unsigned arithmetic requires particular care around range limits.

---

# 19. Integer Arithmetic Operators

For integers:

| Operator | Meaning |
|---|---|
| `+` | addition |
| `-` | subtraction |
| `*` | multiplication |
| `/` | integer division |
| `%` | remainder |
| unary `-` | sign inversion |

### Integer division

If both operands are integers:

```cpp
5 / 2
```

produces:

```text
2
```

not `2.5`.

### Remainder

```cpp
5 % 2   // 1
4 % 2   // 0
```

The remainder operator is especially useful for:

- detecting divisibility;
- extracting digits;
- converting units;
- separating hours/minutes/seconds.

---

# 20. Bitwise Operators

For integer values, the course introduces bit-to-bit operators:

| Operator | Meaning |
|---|---|
| `x >> n` | right shift |
| `x << n` | left shift |
| `x & y` | bitwise AND |
| `x \| y` | bitwise OR |
| `x ^ y` | bitwise XOR |
| `~x` | bitwise complement |

The important conceptual distinction is:

- logical operators work with logical truth values;
- bitwise operators act on the binary representation of integer values.

For example:

```text
AND
0 0 → 0
0 1 → 0
1 0 → 0
1 1 → 1
```

```text
OR
0 0 → 0
0 1 → 1
1 0 → 1
1 1 → 1
```

```text
XOR
0 0 → 0
0 1 → 1
1 0 → 1
1 1 → 0
```

---

# 21. Assignment

The assignment operator is:

```cpp
=
```

Syntax:

```cpp
exp1 = exp2;
```

The basic process is:

1. evaluate `exp2`;
2. convert it when necessary to a compatible type;
3. store the result in `exp1`.

Example:

```cpp
int x;
x = 5 + 2;
```

After the assignment, `x` contains `7`.

---

## 21.1 Compound assignment operators

Instead of:

```cpp
x = x + y;
```

we can use:

```cpp
x += y;
```

Similarly:

```text
x = x - y   →   x -= y
x = x * y   →   x *= y
x = x / y   →   x /= y
x = x % y   →   x %= y
```

These forms are shorter and are often used for accumulator-style computations.

---

# 22. Increment and Decrement

The operators are:

```cpp
x++
x--
++x
--x
```

## 22.1 Post-increment

```cpp
x++
```

means:

1. use the current value of `x`;
2. then increase `x` by one.

Example:

```cpp
int x = 5;
int b = x++;
```

Result:

```text
b = 5
x = 6
```

## 22.2 Pre-increment

```cpp
++x
```

means:

1. increase `x` by one;
2. use the new value.

Example:

```cpp
int x = 5;
int b = ++x;
```

Result:

```text
x = 6
b = 6
```

The same distinction applies to decrement.

> [!warning] Side effects
> Avoid overly complex expressions combining increments/decrements with other operations. The course explicitly warns about evaluation-order problems and side effects.

---

# 23. Evaluation Order

One of the subtler C++ concepts introduced by the course is **evaluation order**.

A programmer should not assume that every subexpression is evaluated in the order it appears textually.

For example, expressions involving side effects such as:

```cpp
j = ++x * x++;
```

are unsafe and can lead to undefined behavior.

### Practical rule

Prefer clear, separate statements:

```cpp
++x;
y = x * x;
```

instead of combining multiple changes in one complicated expression.

This improves both correctness and readability.

---

# 24. Relational Operators

Relational operators compare values:

| Operator | Meaning |
|---|---|
| `==` | equal |
| `!=` | not equal |
| `<` | less than |
| `>` | greater than |
| `<=` | less than or equal |
| `>=` | greater than or equal |

Example:

```cpp
x == y
x != y
x < y
x >= y
```

A relational expression produces a Boolean result.

---

# 25. Boolean Type

C++ provides the fundamental type:

```cpp
bool
```

with two main logical values:

```cpp
true
false
```

The course explains the relationship between Boolean values and integer truth interpretation:

- `0` corresponds to false;
- a non-zero integer corresponds to true.

---

## 25.1 Boolean operators

| Operator | Meaning |
|---|---|
| `!` | NOT |
| `&&` | AND |
| `\|\|` | OR |

Examples:

```cpp
!x
x && y
x || y
```

### De Morgan's laws

The slides explicitly illustrate:

```text
!(x || y)  ≡  (!x && !y)
!(x && y)  ≡  (!x || !y)
```

These identities are extremely useful when rewriting conditions.

---

## 25.2 Truth table

| x | y | `!x` | `x && y` | `x || y` |
|---|---|---|---|---|
| false | false | true | false | false |
| false | true | true | false | true |
| true | false | false | false | true |
| true | true | false | true | true |

---

## 25.3 Lazy evaluation / short-circuit evaluation

For `&&`:

```cpp
exp1 && exp2
```

if `exp1` is false, `exp2` does not need to be evaluated.

For `||`:

```cpp
exp1 || exp2
```

if `exp1` is true, `exp2` does not need to be evaluated.

This is called **short-circuit / lazy evaluation**.

It is useful because it can avoid unnecessary work and can safely guard operations.

Example:

```cpp
if (x != 0 && y / x > 2)
{
    ...
}
```

If `x != 0` is false, the division is not evaluated.

> [!warning] Side effects in conditions
> The course recommends avoiding expressions with side effects such as `++` inside Boolean expressions, because short-circuit evaluation can change which parts execute.

---

# 26. Real / Floating-Point Types

The main real types introduced are:

```cpp
float
double
long double
```

The slides describe increasing precision roughly as:

| Type | Typical size in the slides | Approx. decimal precision in the slides |
|---|---:|---:|
| `float` | 32 bits | about 7 digits |
| `double` | 64 bits | about 15 digits |
| `long double` | implementation-dependent | at least about 15 digits in the slide example |

Important: exact representation and precision depend on the implementation.

Examples:

```cpp
float a = 3.5f;
double b = 3.5;
long double c = 3.5L;
```

For floating-point operands:

```cpp
7.0 / 2.0
```

produces approximately:

```text
3.5
```

---

# 27. Enumeration Type (`enum`)

An enumerated type represents a finite set of named integer constants defined by the programmer.

Syntax:

```cpp
enum Color { RED, GREEN, BLUE };
```

By default, enumerators receive successive integer values starting from zero:

```text
RED   = 0
GREEN = 1
BLUE  = 2
```

A variable can then be declared:

```cpp
Color c = GREEN;
```

Explicit values can be assigned:

```cpp
enum Color { RED = 10, GREEN = 11, BLUE = 12 };
```

If later values are not specified explicitly, they continue from the previous value:

```cpp
enum Color2 { ORANGE = 13, GREEN, GRAY };
```

so:

```text
ORANGE = 13
GREEN  = 14
GRAY   = 15
```

The course examples also show that enum values are printed as integers.

---

# 28. Character Type (`char`)

The `char` type represents characters such as:

```cpp
'a'
'A'
'6'
'+'
' '
```

The slides note that `char` usually requires one byte.

---

## 28.1 Character encoding

Characters are represented internally using numeric codes according to an encoding.

The course mentions:

- ASCII;
- EBCDIC;
- BCD.

ASCII is used as the practical example.

However, the important programming principle is:

> Use character literals such as `'A'` rather than hard-coding numeric character codes whenever possible.

For example:

```cpp
char c = 'A';
```

is preferable to:

```cpp
char c = 65;
```

when the intention is to represent the character `A`.

---

## 28.2 Ordering rules

The slides identify these ordering/consecutiveness rules:

```text
'a' < 'b' < ... < 'z'
'A' < 'B' < ... < 'Z'
'0' < '1' < ... < '9'
```

This property is useful for character-range tests and conversions.

---

## 28.3 Character arithmetic

Because characters have associated numeric representations, arithmetic operations can be applied to them.

This makes operations such as uppercase/lowercase conversion possible in introductory exercises.

Conceptually:

```text
'A' → 'B' → 'C' → ...
```

according to the encoding's ordered character values.

---

# 29. `sizeof`

The `sizeof` operator returns the size, in bytes, of an object or type.

Examples:

```cpp
sizeof(x)
sizeof(double)
sizeof(int)
sizeof('a')
```

With variables, the parentheses are optional:

```cpp
sizeof x
sizeof(x)
```

With a type, parentheses are normally required:

```cpp
sizeof(int)      // valid
sizeof(double)   // valid
```

The key idea is that `sizeof` is about the storage representation used by the implementation.

---

# 30. Type Conversion

Different types may occur in the same expression.

Example:

```cpp
int price = 275000;
double weight = 0.3;
int cost = price * weight;
```

A conversion is necessary because the operands do not have exactly the same type.

Two broad categories are distinguished:

- **implicit conversion**;
- **explicit conversion / cast**.

---

## 30.1 Implicit conversion

The compiler performs the conversion automatically.

Examples from the course:

```cpp
float x = 3;
```

conceptually converts `3` to a floating-point value.

Another example:

```cpp
int y = 2 * 3.6;
```

the real result is assigned to an integer, so the fractional part is lost during the conversion to `int`.

---

## 30.2 Explicit conversion / cast

The programmer explicitly requests conversion.

Examples shown in the slides:

```cpp
int x = (int)3.14;
```

and:

```cpp
double x = double(3);
```

Modern C++ also provides more explicit cast forms, but the course examples at this stage use the above forms.

---

## 30.3 Promotion

Promotion converts a type into a related type with at least as much representational capacity.

The slides show sequences such as:

```text
short → int → long → long long
float → double → long double
```

Conceptually, a promotion is intended to preserve the represented value.

---

## 30.4 Real → integer

When a floating-point value is converted to an integer, the fractional component is truncated.

Examples:

```cpp
(int)4.7    // 4
(int)-4.7   // -4
```

This is **not rounding**.

---

## 30.5 Integer → real

Converting an integer to a floating-point representation can lose precision if the integer is too large to be represented exactly by that floating-point type.

---

# 31. Streams and Standard Input/Output

The course introduces streams as the mechanism through which a program communicates with the outside world.

## 31.1 What is a stream?

A **stream** is a logical sequence of characters used to transfer information.

Streams can be associated with:

- keyboard input;
- screen output;
- error output;
- files.

The standard stream facilities are provided by:

```cpp
#include <iostream>
```

The three predefined streams emphasized in the course are:

| Stream | Role |
|---|---|
| `cin` | standard input |
| `cout` | standard output |
| `cerr` | standard error |

---

# 32. `cout` — Standard Output

`cout` is used by the program to send output to the standard output device, typically the monitor.

Basic syntax:

```cpp
cout << expression;
```

Conceptually, writing involves:

1. computing the expression's value;
2. converting the value into a character sequence;
3. transferring that sequence to the output stream.

Example:

```cpp
cout << 3 * 4;
```

prints:

```text
12
```

---

## 32.1 `endl` and `\n`

The course uses both:

```cpp
endl
```

and:

```cpp
"\n"
```

`endl` starts a new line and also performs the stream flushing behavior associated with the manipulator.

For basic output, both are commonly encountered:

```cpp
cout << "Hello" << endl;
```

or:

```cpp
cout << "Hello\n";
```

---

## 32.2 Multiple output operations

C++ allows chained insertion:

```cpp
cout << x << y << endl;
```

This is equivalent conceptually to:

```cpp
cout << x;
cout << y;
cout << endl;
```

Example:

```cpp
int x = 2024;
cout << "Hello World " << x << endl;
```

---

# 33. `cin` — Standard Input

`cin` reads data from the standard input stream, typically the keyboard.

Basic syntax:

```cpp
cin >> variable;
```

The input process can be understood as:

1. obtain characters from the stream;
2. convert them to the requested type;
3. assign the resulting value to the variable.

That conversion step is critical because invalid input may put the stream into an error state.

---

## 33.1 Reading a `char`

Suppose:

```cpp
char x;
cin >> x;
```

Leading whitespace is skipped. The next non-space character is extracted and stored in `x`.

---

## 33.2 Reading an integer

Suppose:

```cpp
int x;
cin >> x;
```

The stream tries to interpret the input as a representation of an integer.

Examples:

```text
728        → valid integer input
-14        → valid integer input
abc        → invalid for int
```

When the character sequence cannot be converted appropriately, the stream enters an error state.

---

## 33.3 Multiple input operations

Input can be chained:

```cpp
cin >> x >> y >> z;
```

This is equivalent to:

```cpp
cin >> x;
cin >> y;
cin >> z;
```

Example:

```cpp
char x;
float y;
int z;

cin >> x >> y >> z;
```

Input can be given on one line or across several lines because whitespace separates extracted values.

---

# 34. Input Errors and Stream State

This is one of the most important practical parts of the stream lecture.

## 34.1 `cin.eof()`

`cin.eof()` checks whether the input stream has reached the end-of-stream condition.

The course notes that it should be considered after a reading operation.

---

## 34.2 `cin.fail()`

`cin.fail()` reports whether the stream has entered an error state, including cases where the input cannot be interpreted as required.

Example:

```cpp
int x;
cin >> x;

if (cin.fail())
{
    cout << "Input error" << endl;
}
```

Equivalent logic:

```cpp
if (!cin.fail())
{
    // input succeeded
}
```

---

## 34.3 `cin.clear()`

After an input failure, the stream remains in an error state.

`cin.clear()` restores the stream to a normal state.

```cpp
cin.clear();
```

---

## 34.4 `cin.ignore()`

`cin.ignore()` removes characters from the input stream.

Basic form:

```cpp
cin.ignore();
```

The material also introduces:

```cpp
cin.ignore(n);
```

for ignoring a specified number of characters.

### Typical recovery pattern

```cpp
if (cin.fail())
{
    cin.clear();
    cin.ignore();
}
```

The logic is:

```text
bad input
   ↓
stream enters error state
   ↓
clear the error state
   ↓
remove problematic input
   ↓
try reading again
```

---

## 34.5 Why unvalidated input is dangerous

Consider:

```cpp
int x1;
int x2;

cin >> x1;
cin >> x2;
```

If the user enters something incompatible with `int`, the second extraction may also be affected because the stream is already in a failed state.

Therefore robust input code must consider the stream state explicitly.

---

# 35. Boolean Input

For:

```cpp
bool b;
cin >> b;
```

input such as `0` or `1` is interpreted as Boolean values.

The course notes special behavior for other input values: conversion may fail or produce a Boolean result according to the stream conversion rules.

### Practical rule

At introductory level, prefer explicit Boolean inputs such as:

```text
0
1
```

and validate the stream when input correctness matters.
