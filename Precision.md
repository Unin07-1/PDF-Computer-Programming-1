## Part 3 of [[Data Types]]
# 2. Fundamental types

## 2.1 Definition

The lecture identifies the following as **fundamental types**:

1. **Boolean**
2. **Integer**
3. **Real**
4. **Enum**
5. **Char**

For the current lecture, the most detailed treatment concerns **integer** and **real** types, especially the latter because precision becomes a central issue. The source explicitly lists these five categories. 

## 2.2 Fundamental types vs. derived types

In the broader data-type classification used in the course, fundamental types are the basic language types, while more complex types are constructed from types and language mechanisms. This lecture focuses on the fundamental side of that classification.

A useful conceptual distinction is:

```text
Data types
├── Fundamental types
│   ├── Boolean
│   ├── Integer
│   ├── Real
│   ├── Enum
│   └── Char
└── Derived / user-derived types
    └── discussed in other course material
```

---

# 3. Integer types

The lecture lists four integer types with the following sizes/ranges in the examples shown in the slides:

| Type | Size shown in lecture | Range shown in lecture | Standard minimum |
|---|---:|---:|---:|
| `short` | 16 bits | `[-32768, 32767]` | at least 16 bits |
| `int` | 32 bits | `[-2147483648, 2147483647]` | at least 16 bits |
| `long` | 64 bits | `[-9223372036854775808, 9223372036854775807]` | at least 32 bits |
| `long long` | 64 bits | `[-9223372036854775808, 9223372036854775807]` | at least 64 bits |

### 3.1 Important idea: size and range

For a signed integer type, the number of available bit patterns determines how many different values can be represented. More bits generally allow a larger range.

However, the lecture distinguishes between the concrete sizes shown in the examples and the **minimum guarantees of the C++ standard**. Therefore, the exact size of a type should not automatically be assumed to be identical on every implementation.

### 3.2 Practical memory of the lecture

Remember the progression presented in the source:

```text
short → int → long → long long
```

The important concept for later sections is that numeric types have different representational capacities, and conversions between them may therefore be harmless in some directions and lossy in others.

---

# 4. Real (floating-point) types

The lecture considers three real numeric types:

| Type | Size shown | Decimal precision shown |
|---|---:|---:|
| `float` | 32 bits | about 7 decimal digits |
| `double` | 64 bits | about 15 decimal digits |
| `long double` | 96 bits | at least 15 decimal digits |

## 4.1 Precision is not the same thing as number of printed digits

One of the most important ideas in this lecture is that **precision of a type** and **precision used by output formatting** are not the same concept.

For example:

```cpp
float x = 3.14159265358979f;
```

The variable is stored as a `float`, so the representational precision is limited by the `float` type. Increasing the number of digits printed by `cout` does **not** create new information that was not stored in the value.

This distinction becomes especially important in the precision examples later in the lecture.

---

# 5. Controlling output precision with `cout`

## 5.1 Default precision

The lecture states that `cout` prints **6 or 7 digits depending on the implementation**, considering the integer and decimal parts together when using the default formatting.

The current precision can be inspected with:

```cpp
cout.precision();
```

The return value can be stored in a variable of type `streamsize`:

```cpp
streamsize psize = cout.precision();
```

The lecture describes `streamsize` as a special signed integral type used for this purpose.

## 5.2 Setting the precision

The precision can be changed with:

```cpp
cout.precision(n);
```

where `n` is the number of digits used by the output according to the current formatting mode.

### Default notation

Without `fixed` or `scientific`, the precision is considered with respect to the standard/default formatting behaviour described in the slides.

### Fixed notation

```cpp
cout << fixed;
```

When `fixed` is active, `precision(n)` determines the number of digits **after the decimal point**.

Example:

```cpp
cout.precision(5);
cout << fixed << 3.14159265358979 << endl;
```

Output:

```text
3.14159
```

### Scientific notation

```cpp
cout << scientific;
```

This displays the number in scientific notation, with precision applied to the decimal representation according to the formatting rules shown in the lecture.

Example style:

```text
3.14159e+00
1.33142e+02
```

### Restoring the default formatting

The source uses:

```cpp
cout << defaultfloat;
```

and explains that this restores the default notation and the default way of computing precision.

---

# 6. `setprecision`

An alternative syntax is provided by the `<iomanip>` library:

```cpp
#include <iomanip>
```

and then:

```cpp
setprecision(X)
```

Example:

```cpp
#include <iostream>
#include <iomanip>

int main() {
    cout << setprecision(5) << 3.14159265358979 << endl;
}
```

The source presents `setprecision(X)` as an alternative to calling `cout.precision(X)` directly.

## 6.1 Typical formatting combinations

The lecture demonstrates three common forms:

```cpp
cout << setprecision(5) << x;
cout << setprecision(5) << fixed << x;
cout << setprecision(5) << scientific << x;
```

Conceptually:

| Form | Main effect |
|---|---|
| `setprecision(5)` | precision of the default notation |
| `setprecision(5) << fixed` | 5 digits after the decimal point |
| `setprecision(5) << scientific` | scientific notation with the selected precision |

---

# 7. Precision example: `float` vs `double`

The lecture uses values such as:

```cpp
float fl1 = 3.14159265358979f;
float fl2 = 133.14159265358979f;

double db1 = 3.14159265358979;
double db2 = 133.14159265358979;
```

The output changes when the stream precision changes.

### Example sequence from the lecture

With the default precision:

```text
3.14159
133.142
```

With precision 4:

```text
3.142
133.1
```

With precision 7:

```text
3.141593
133.1416
```

With precision 10:

```text
3.141592741
133.1415863
```

The important observation is that the `float` values do not suddenly become more accurate merely because more output digits are requested.

For the `double` values, the lecture shows more digits matching the intended decimal value:

```text
3.141592654
133.1415927
```

## 7.1 Key principle

> **Output formatting controls how many digits are shown; it does not increase the amount of information stored in the variable.**

A `float` may display apparently detailed digits when the output precision is increased, but some of those digits reflect the actual binary floating-point representation rather than the original decimal literal exactly.

---

# 8. Representation in memory vs. representation in standard output

The lecture explicitly warns about the difference between:

- the representation of a value in **memory**;
- the representation of the same value in **standard output**.

This distinction is essential.

For example:

```cpp
float fl1 = 3.14159265358979f;
```

and:

```cpp
cout.precision(10);
cout << fl1;
```

may produce more printed digits than the type can accurately preserve as a real mathematical decimal value.

The output is a **formatted representation**, not a direct dump of the mathematical number with unlimited precision.

---

# 9. Precision and arithmetic: a concrete example

The lecture considers:

```cpp
float fl1 = 3.14159265358979f;
float sum = fl1 + 0.001;
```

Mathematically:

```text
3.14159265358979 + 0.001 = 3.14259265358979
```

But the value actually represented by a `float` is constrained by the precision of that type.

When printing with different `cout.precision(...)` values, different rounded representations appear, for example:

```text
precision 4 → 3.142
precision 5 → 3.1416
```

and for the sum:

```text
precision 4 → 3.143
precision 5 → 3.1426
```

### Lesson

Changing the output precision changes **what is displayed**, while the stored floating-point value remains subject to the representational limits of the chosen type.

---

# 10. Why floating-point equality is problematic

## 10.1 The basic problem

The lecture demonstrates that mathematically equivalent calculations can produce floating-point values that are not represented identically.

Consider:

```cpp
float actual1 = 10 - 9;
float actual2 = 10.0f - 0.2f * 45;
```

The first two outputs are shown as:

```text
actual1 = 1
actual2 = 1
```

However, the lecture then performs repeated subtraction:

```cpp
float actual3 = 10.0f;
float decrement = 0.2f;
int count = 45;

for (int index = 0; index < count; ++index)
    actual3 -= decrement;
```

The resulting value is shown as approximately:

```text
1.00001
```

Therefore:

```cpp
if (actual3 == 1.0f)
```

is false in the example.

The output is:

```text
NOT-EQUAL executed
```

## 10.2 Why this happens

The lecture's examples show the effect of finite floating-point representation and repeated arithmetic operations. A decimal quantity such as `0.2` generally does not have an exact representation in binary floating-point.

As a result, operations that are mathematically expected to produce the same real value can end up producing slightly different representable values.

---

# 11. Direct `==` and `!=` on floating-point values

The lecture also demonstrates compilation with:

```bash
g++ -Wfloat-equal float_comparison.cc
```

which produces a warning for:

```cpp
if (actual3 == 1.0f)
```

The compiler warning states that comparing floating-point values with `==` or `!=` can be unsafe.

This does **not** mean that `==` is syntactically invalid. It means that direct equality is often inappropriate when the intention is to ask whether two calculated floating-point values are numerically close.

---

# 12. Epsilon-based floating-point comparison

## 12.1 Basic idea

Instead of testing:

```cpp
actual3 == 1.0f
```

the lecture proposes computing the absolute difference and checking whether it is small enough:

```cpp
float epsilon = 0.0001f;
bool isEqual = fabs(actual3 - 1.0f) <= epsilon;
```

Then:

```cpp
if (isEqual)
    cout << "EQUAL executed" << endl;
else
    cout << "NOT-EQUAL executed" << endl;
```

For the example, the source reports:

```text
diff = 5.00679e-06
epsilon = 0.0001
isEq = 1
```

so the comparison succeeds.

## 12.2 Mathematical interpretation

The test:

```cpp
fabs(a - b) <= epsilon
```

means:

$$
|a-b| \le \varepsilon
$$

where:

- `a` and `b` are the numbers being compared;
- `fabs` computes the absolute value for floating-point values;
- `epsilon` defines the tolerated difference.

The comparison therefore asks:

> Are the two floating-point values sufficiently close according to the chosen tolerance?

---

# 13. Choosing `epsilon`

The lecture also shows that the value used as epsilon matters.

### Example 1: manually chosen tolerance

```cpp
float epsilon = 0.0001f;
```

For the example:

```text
actual difference ≈ 5.00679 × 10^-6
```

and:

```text
epsilon = 1 × 10^-4
```

so the test succeeds.

### Example 2: machine epsilon from `numeric_limits`

The lecture then uses:

```cpp
float epsilon = numeric_limits<float>::epsilon();
```

For the shown example:

```text
diff ≈ 5.00679e-06
eps  ≈ 1.19209e-07
```

Since the difference is larger than this epsilon, the comparison becomes false.

## 13.1 Important distinction

The lecture therefore implicitly highlights an important concept:

> **Machine epsilon is not automatically the correct tolerance for every numerical comparison.**

The threshold required by an algorithm depends on the problem and on the scale of the values involved.

---

# 14. `numeric_limits`

The lecture introduces the `<limits>` header:

```cpp
#include <limits>
```

and shows examples such as:

```cpp
numeric_limits<float>::epsilon()
numeric_limits<double>::max()
numeric_limits<int>::max()
```

The source also points to related constants and functions, including:

```text
epsilon
min
max
```

## 14.1 Purpose

`numeric_limits<T>` provides information about the properties and limits of a numeric type `T`.

Examples from the lecture:

```cpp
numeric_limits<float>::epsilon()
numeric_limits<double>::max()
numeric_limits<int>::max()
```

These are useful when writing code that should reason about the capabilities of the type rather than hard-code implementation-specific numeric constants.

## 14.2 `<cfloat>` alternative

The lecture also mentions:

```cpp
#include <cfloat>
```

with constants such as:

```cpp
FLT_EPSILON
FLT_MAX
FLT_MIN

DBL_EPSILON
DBL_MAX
DBL_MIN
```

The source cites `numeric_limits` and `<cfloat>` as alternative ways to access information about numeric limits.

---

# 15. Mixed operations and type conversion

## 15.1 General situation

The lecture notes that mixed-type expressions are common:

- different operand types can occur in the same expression;
- a value can be assigned to a variable of a different type.

Whenever this happens, a **type conversion** is required.

The conversion can be:

- **implicit**, performed automatically by the compiler;
- **explicit**, requested by the programmer.

## 15.2 Example

The source gives:

```cpp
int price = 275000;
double weight = 0.3;
int cost = price * weight;
```

The multiplication combines an `int` and a `double`, so the operands are converted as part of the mixed expression. The final result is then assigned to an `int` variable.

---

# 16. Implicit type conversion

## 16.1 Definition

An **implicit conversion** is a conversion performed automatically by the compiler.

The lecture highlights two major cases.

### Case A — numeric expression

During the evaluation of a numeric expression, operands are converted toward the type associated with the larger size / greater representational capability shown in the lecture.

### Case B — assignment

When assigning an expression to a variable, the resulting expression is converted to the type of the destination variable.

---

# 17. Examples of implicit conversion

## 17.1 Integer to float

```cpp
float x1 = 3;
```

The integer value is converted to a floating-point value before being stored in `x1`.

Conceptually:

```text
3  →  3.0
```

The value is represented using the destination type.

## 17.2 Double to float

```cpp
float x2 = 3.0;
```

The literal `3.0` is a `double`, and the assignment converts it to `float`.

## 17.3 Float to float

```cpp
float x3 = 3.0f;
```

Here the literal is explicitly a `float`, so the assignment uses the same type.

The suffix:

```text
f
```

is important because it marks the floating-point literal as a `float` rather than the default `double` form shown by the lecture examples.

---

# 18. Integer assignment and truncation

The lecture provides:

```cpp
int x4 = 2 * 3.6;
```

The expression is numerically:

```text
7.2
```

but the destination type is `int`, so the value becomes:

```text
x4 = 7
```

The fractional part is discarded in the conversion from real to integer.

## 18.1 Integer division

The source also gives:

```cpp
int x5 = 4 / 3;
```

Because both operands are integers, the operation is integer division:

```text
4 / 3 = 1
```

Therefore:

```cpp
x5 == 1
```

not `1.33...`.

## 18.2 A particularly important example

The lecture gives:

```cpp
float x6 = 4 / 3;
```

The result is still:

```text
x6 = 1
```

not `1.33`.

### Why?

The division is evaluated **before** the assignment, and the operands are both integers:

```text
4 / 3
```

therefore integer division produces:

```text
1
```

Only afterwards is that integer result converted to `float`:

```text
1 → 1.0f
```

This is a very important distinction:

> The destination type does not retroactively change the type of the already-evaluated operands.

---

# 19. Getting floating-point division

To obtain a floating-point result, at least one operand must be floating-point.

The lecture gives:

```cpp
float x7 = 4.0 / 3.0;
```

which produces approximately:

```text
1.333
```

The crucial difference is:

```cpp
4 / 3       // integer division
4.0 / 3.0   // floating-point division
```

This distinction is fundamental in C++ expression evaluation.

---

# 20. Explicit type conversion (casting)

## 20.1 Definition

An **explicit type conversion** is requested directly by the programmer.

The lecture calls this **casting**.

Two notations are presented.

---

# 21. C-style / prefix cast notation

The first syntax is:

```cpp
(int) 3.14
```

Example:

```cpp
int x1 = (int) 3.14;
```

The expression is explicitly converted to `int`.

For a real-to-integer conversion:

```text
3.14 → 3
```

according to the truncation behaviour shown in the lecture.

---

# 22. Functional cast notation

The second syntax is:

```cpp
double(3)
```

Example:

```cpp
double x2 = double(3);
```

The syntax resembles a function call, but here it explicitly constructs/converts a value to the requested type.

---

# 23. Casting to control expression evaluation

The lecture uses explicit casts to transform integer division into floating-point division.

### Without cast

```cpp
float x6 = 4 / 3;
```

Result:

```text
1
```

### With casts

```cpp
float x6 = (double)4 / (double)3;
```

Result:

```text
1.33...
```

or:

```cpp
float x6 = 4.0 / double(3);
```

Again:

```text
≈ 1.33
```

### Core idea

The casts are applied to the operands **before** the division, so the division itself is performed in floating-point arithmetic.

Compare:

```text
4 / 3
↓
integer / integer
↓
integer division
↓
1
```

with:

```text
(double)4 / (double)3
↓
real / real
↓
floating-point division
↓
1.333...
```

---

# 24. Promotion

The lecture calls **promotion** a conversion from one numeric type to a similar type with greater representational capacity.

The progressions explicitly shown are:

```text
short → int → long → long long
```

and:

```text
float → double → long double
```

The source states that promotion **guarantees preservation of the same value** within the intended hierarchy.

## 24.1 Conceptual interpretation

Promotion normally moves a value toward a type with a wider representational range or greater precision.

Example:

```cpp
short s = 10;
int i = s;
```

The same numerical value is represented as an `int`.

Similarly:

```cpp
float f = 3.14f;
double d = f;
```

The value is promoted from `float` to `double`.

---

# 25. Conversion from real to integer

The lecture explicitly states:

> Conversion from real to integer: the value is truncated.

Examples:

```cpp
int x = (int) 4.7;   // x == 4
int y = (int) -4.7;  // y == -4
```

## 25.1 Truncation is not rounding

The operation is not:

```text
4.7 → 5
```

and it is not:

```text
-4.7 → -5
```

Instead, the fractional part is removed:

```text
4.7   →  4
-4.7  → -4
```

A useful mental model is that the conversion discards the fractional component rather than selecting the mathematically nearest integer.

---

# 26. Conversion from integer to real

The source highlights the opposite direction as potentially problematic because the integer value may not be represented exactly by a low-precision real type.

The lecture gives the example:

```cpp
float y = float(2147483600);
```

and notes that the resulting value can become:

```text
2147483648.0
```

instead of the original integer value.

## 26.1 Why can this happen?

A 32-bit `float` does not have enough precision to represent every possible 32-bit integer exactly.

Therefore:

```text
int → float
```

can lose precision for sufficiently large integers.

The lecture contrasts this with:

```text
int → double
```

and notes that, for the values considered there, `double` has enough bits to preserve the integer exactly.

---

# 27. Numeric conversion summary

| Conversion | Main effect in the lecture |
|---|---|
| `short → int` | Promotion; same numerical value preserved |
| `int → long` | Promotion; same numerical value preserved |
| `long → long long` | Promotion; same numerical value preserved within the presented hierarchy |
| `float → double` | Promotion to a type with greater precision |
| `double → long double` | Promotion to the wider real type shown |
| `real → int` | Fractional part is truncated |
| `int → float` | Can lose precision for sufficiently large values |
| `int → double` | The lecture notes that `double` has sufficient bits for the demonstrated case |

---

# 28. Precision, range and representation

These three ideas should not be confused.

## 28.1 Range

**Range** refers to the set of magnitudes/values that can be represented.

Example from the lecture:

```text
int: approximately -2.1×10^9 to +2.1×10^9
```

for the 32-bit representation shown.

## 28.2 Precision

**Precision** concerns how finely values can be represented, especially for floating-point types.

The lecture gives approximately:

```text
float       → 7 decimal digits
 double      → 15 decimal digits
```

## 28.3 Output precision

**Output precision** is a property of the stream formatting configuration:

```cpp
cout.precision(n);
```

It controls what is printed, not what the variable can intrinsically store.

---

# 29. Floating-point values: practical consequences

From the examples in the lecture, several conclusions follow.

### 29.1 More output digits do not necessarily mean more accuracy

```cpp
cout.precision(10);
cout << someFloat;
```

can print many digits, but the extra digits may simply reveal the approximated binary representation of the `float`.

### 29.2 Repeated operations can accumulate representation effects

The repeated subtraction example produces approximately:

```text
1.00001
```

rather than exactly `1.0f`.

### 29.3 Equality should be chosen according to the numerical problem

For computed floating-point values, a tolerance-based test such as:

```cpp
fabs(a - b) <= epsilon
```

is often more suitable than:

```cpp
a == b
```

when the intention is approximate numerical equality.

---

# 30. Headers and utilities used in the lecture

| Header | Main elements used |
|---|---|
| `<iostream>` | `cout`, `endl` |
| `<iomanip>` | `setprecision`, formatting manipulators such as `fixed`, `scientific`, `defaultfloat` |
| `<cmath>` | `fabs` and mathematical functions |
| `<limits>` | `numeric_limits<T>::epsilon()`, `min()`, `max()` |
| `<cfloat>` | constants such as `FLT_EPSILON`, `FLT_MAX`, `FLT_MIN`, `DBL_EPSILON`, `DBL_MAX`, `DBL_MIN` |

---

# 31. Complete precision example

A compact example combining the main mechanisms is:

```cpp
#include <iostream>
#include <iomanip>
#include <cmath>
#include <limits>

using namespace std;

int main() {
    float actual = 10.0f;
    float decrement = 0.2f;

    for (int index = 0; index < 45; ++index)
        actual -= decrement;

    cout << "default: " << actual << endl;

    cout.precision(10);
    cout << "precision 10: " << actual << endl;

    float epsilon = 0.0001f;
    bool isEqual = fabs(actual - 1.0f) <= epsilon;

    cout << "difference: " << fabs(actual - 1.0f) << endl;
    cout << "epsilon: " << epsilon << endl;
    cout << "equal: " << isEqual << endl;

    cout << "machine epsilon: "
         << numeric_limits<float>::epsilon() << endl;

    return 0;
}
```

This example demonstrates four separate concepts:

1. floating-point arithmetic;
2. output precision;
3. approximate comparison with epsilon;
4. machine epsilon obtained from `numeric_limits`.

---

# 32. Complete type-conversion example

```cpp
#include <iostream>

using namespace std;

int main() {
    int a = 4;
    int b = 3;

    float x1 = a / b;                 // 1
    float x2 = 4.0 / 3.0;             // about 1.333
    float x3 = (double)a / (double)b; // about 1.333
    float x4 = 4.0 / double(3);       // about 1.333

    int x5 = (int)4.7;                // 4
    int x6 = (int)-4.7;               // -4

    return 0;
}
```

## What happens step by step?

### `x1`

```cpp
float x1 = a / b;
```

Both operands are integers → integer division → `1` → assignment to `float` gives `1.0`.

### `x2`

```cpp
float x2 = 4.0 / 3.0;
```

Both operands are real → floating-point division → approximately `1.333...`.

### `x3`

```cpp
float x3 = (double)a / (double)b;
```

Both operands are explicitly converted to `double` before division → floating-point division.

### `x4`

```cpp
float x4 = 4.0 / double(3);
```

Again, at least one operand is floating-point, so the division occurs in floating-point arithmetic.

### `x5` and `x6`

The explicit cast to `int` removes the fractional parts.

---

# 33. Exam-oriented distinctions

These are the distinctions most worth memorising from the lecture.

## 33.1 `precision()` vs. `setprecision()`

```cpp
cout.precision(n);
```

changes the stream precision directly.

```cpp
cout << setprecision(n);
```

is the stream manipulator alternative provided by `<iomanip>`.

## 33.2 `fixed` vs. default formatting

```cpp
cout << fixed;
```

makes the precision refer to digits after the decimal point.

```cpp
cout << defaultfloat;
```

restores the default notation and precision behaviour.

## 33.3 `==` vs. approximate comparison

```cpp
x == y
```

can be unsafe for computed floating-point values.

A tolerance-based comparison is:

```cpp
fabs(x - y) <= epsilon
```

## 33.4 Machine epsilon vs. chosen tolerance

```cpp
numeric_limits<float>::epsilon()
```

is a property of the type's floating-point representation.

A manually selected:

```cpp
epsilon = 0.0001f;
```

is an application-level tolerance.

They are conceptually different.

## 33.5 Integer division vs. floating-point division

```cpp
4 / 3
```

→ `1`

```cpp
4.0 / 3.0
```

→ approximately `1.333...`

## 33.6 Assignment conversion vs. expression evaluation

The destination type does not change how an expression was evaluated.

Therefore:

```cpp
float x = 4 / 3;
```

does **not** become `1.333...` merely because `x` is a `float`.

The division already occurred as integer division.

## 33.7 Truncation vs. rounding

```cpp
(int)4.7
```

→ `4`

```cpp
(int)-4.7
```

→ `-4`

The fractional part is truncated.

---

# 34. Common mistakes