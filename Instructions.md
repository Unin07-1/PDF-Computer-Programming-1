# 36. Program Instructions

The course organizes instructions into:

1. **simple instructions**;
2. **structured instructions**.

Structured instructions include:

- compound statements;
- conditional statements;
- iterative statements;
- jump statements.

---

# 37. Simple Instructions

Simple instructions are the basis for more complex constructs.

They normally end with:

```cpp
;
```

Examples:

```cpp
int x;
const int kilo = 1024;
cin >> x;
cout << 3 * x;
x = 2 * (3 - y);
```

The slides classify expressions used as instructions into several categories:

- input;
- output;
- assignment;
- arithmetic expressions;
- logical expressions;
- constants;
- conditional expressions.

---

# 38. Conditional Expression `?:`

The conditional expression has the form:

```cpp
exp1 ? exp2 : exp3;
```

Meaning:

- if `exp1` is true → evaluate/select `exp2`;
- otherwise → evaluate/select `exp3`.

Example:

```cpp
int result = (x > 10) ? 100 : 200;
```

Equivalent conceptual structure:

```text
x > 10 ?
 ├─ true  → result = 100
 └─ false → result = 200
```

It is best used when the alternatives are simple enough to remain readable.

---

# 39. Compound Statements / Blocks

A block groups several statements into one compound statement:

```cpp
{
    statement1;
    statement2;
    statement3;
}
```

Blocks are essential for:

- functions;
- `if` statements;
- loops;
- local scope.

Example:

```cpp
if (x > 0)
{
    y = x;
    z = y + 1;
}
```

---

# 40. `if` Statement

## 40.1 `if` without `else`

Syntax:

```cpp
if (exp)
    instruction;
```

Meaning:

- if the condition is true → execute the instruction;
- if false → execute nothing.

Example:

```cpp
if (x != 0)
    y = 1 / x;
```

The controlled instruction can itself be a compound statement or another conditional statement.

---

## 40.2 `if-else`

Syntax:

```cpp
if (exp)
    instruction1;
else
    instruction2;
```

Meaning:

- true → execute `instruction1`;
- false → execute `instruction2`.

Example:

```cpp
if (x < 0)
    y = -x;
else
    y = x;
```

---

## 40.3 `if-else if-else`

Useful for multiple mutually exclusive alternatives:

```cpp
if (x < 0)
    y = -x;
else if (x > 0)
    y = x;
else
    y = 0;
```

Logic:

```text
condition 1?
 ├─ yes → action 1
 └─ no → condition 2?
          ├─ yes → action 2
          └─ no → action 3
```

---

## 40.4 Nested `if`

An `if` can contain another `if`.

Example:

```cpp
if (a == 0)
{
    if (b == 0)
        cout << "Infinite solutions" << endl;
    else
        cout << "No solution" << endl;
}
else
{
    cout << "Solution: " << -b / a << endl;
}
```

The course uses this to solve the equation:

```text
ax + b = 0
```

with three cases:

- `a == 0` and `b == 0` → infinitely many solutions;
- `a == 0` and `b != 0` → no solution;
- `a != 0` → one solution, `x = -b/a`.

---

## 40.5 Dangling `else`

Nested conditions can become ambiguous to a human reader when braces are omitted.

Best practice:

> Use braces and indentation to make the association of each `else` explicit.

---

# 41. `switch`

`switch` is another conditional construct, particularly useful when selecting among discrete values.

Syntax:

```cpp
switch (exp)
{
    case const_exp1:
        instruction1;
        break;

    case const_exp2:
        instruction2;
        break;

    default:
        default_instruction;
}
```

The expression can have an integer-like type such as:

- `int`;
- `short`;
- `long`;
- `long long`;
- `char`;
- `enum`.

---

## 41.1 Execution logic

1. evaluate the switch expression;
2. select the matching `case`;
3. execute its instructions;
4. execute `break` if present;
5. otherwise continue into the following case(s);
6. if nothing matches, execute `default` if present.

---

## 41.2 `break` and fall-through

Example:

```cpp
char grade = 'D';

switch (grade)
{
    case 'A':
        cout << "Excellent!" << endl;
        break;

    case 'B':
    case 'C':
        cout << "Well done" << endl;
        break;

    case 'D':
        cout << "You passed" << endl;
        break;

    case 'F':
        cout << "Better try again" << endl;
        break;

    default:
        cout << "Invalid grade" << endl;
}
```

`case 'B'` and `case 'C'` intentionally share the same block because there is no `break` between them.

However, accidental fall-through is a common source of errors.

> [!warning] `switch` case uniqueness
> The compile-time values of the `case` expressions must be distinct. Two cases that evaluate to the same value are invalid.

---

# 42. `while` Loop

Syntax:

```cpp
while (exp)
{
    instruction;
}
```

where `exp` is a Boolean expression.

Execution:

1. evaluate `exp`;
2. if false → terminate the loop;
3. if true → execute the body;
4. repeat.

Example:

```cpp
int index = 0;

while (index < 5)
{
    // instructions
    index = index + 1;
}
```

### Important properties

- the body may execute **zero times**;
- the loop may become infinite if the condition never becomes false;
- the control variable normally moves toward the termination condition.

---

## 42.1 Accumulator example

Sum the integers from `1` to `n`:

```cpp
int n, index, sum;

cin >> n;
index = 1;
sum = 0;

while (index <= n)
{
    sum += index;
    index++;
}

cout << sum << endl;
```

For `n = 3`:

```text
sum = 0
sum = 0 + 1 = 1
sum = 1 + 2 = 3
sum = 3 + 3 = 6
```

---

# 43. `do-while` Loop

Syntax:

```cpp
do
{
    instruction;
}
while (exp);
```

Execution:

1. execute the body;
2. evaluate the condition;
3. if true → repeat;
4. if false → stop.

### Main difference from `while`

The `do-while` body executes **at least once**.

Example:

```cpp
int index = 0;

do
{
    index++;
}
while (index < 5);
```

This property makes `do-while` useful for situations such as:

- menus;
- repeated user interaction;
- cases where one execution must occur before the condition is checked.

### Common danger

If the mathematical problem assumes that the loop may execute zero times, a `do-while` loop can introduce an error unless the initial case is handled explicitly.

---

# 44. `for` Loop

Syntax:

```cpp
for (init; exp; update)
{
    instruction;
}
```

The three components are:

- `init` → initializes the control variable;
- `exp` → loop continuation condition;
- `update` → changes the control variable.

Execution order:

```text
init
 ↓
condition ── false ──→ end
 ↓ true
body
 ↓
update
 ↓
condition
```

Example:

```cpp
for (int index = 0; index < 5; index++)
{
    cout << index << endl;
}
```

The course notes that the control variable can be declared inside the loop:

```cpp
for (int index = 0; ...)
```

and its scope is then limited to that loop structure.

---

## 44.1 `for` and `while` equivalence

A `for` loop:

```cpp
for (init; condition; update)
{
    body;
}
```

can conceptually be rewritten as:

```cpp
init;
while (condition)
{
    body;
    update;
}
```

This is important because it shows that the loop constructs express the same fundamental idea—repeated execution controlled by a condition.

---

# 45. Jump Instructions

The course introduces:

- `break`;
- `continue`;
- `return`.

The lecture strongly emphasizes avoiding unnecessary jump instructions because explicit loop exit conditions generally produce clearer structured programs.

---

## 45.1 `break`

`break` immediately terminates the surrounding loop or switch.

Example:

```cpp
while (condition)
{
    if (error)
        break;
}
```

The course recommends preferring a well-designed loop condition where possible.

---

## 45.2 `continue`

`continue` ends the current loop iteration and moves toward the next iteration.

In a `for` loop, the update expression is then processed as part of the normal loop mechanism.

Again, the course recommends using clear conditional structures instead of unnecessarily introducing `continue`.

---

## 45.3 `return`

Inside `main`:

```cpp
return 0;
```

terminates the function and returns a value to the calling environment.

---
# 46. Code Quality and Coding Standards

The course gives explicit style recommendations.

## 46.1 Analyze before coding

Do not begin by randomly writing statements.

First establish at least a mental schema of the solution.

---

## 46.2 Use simple, readable code

Prefer:

- simple control flow;
- meaningful variable names;
- adequate spacing;
- clear block structure.

---

## 46.3 Indentation

Indentation is strongly recommended because it:

- improves readability;
- exposes nesting;
- helps understand control flow;
- makes errors easier to detect.

Example:

```cpp
if (x < 0)
{
    y = -x;

    if (y == 5)
    {
        y = 1;
    }
}
else
{
    y = x;
}
```

---

## 46.4 Meaningful names

Prefer:

```cpp
int numberOfStudents;
double averageTemperature;
```

over:

```cpp
int a;
double q;
```

unless a very short mathematical variable name is natural for the problem.

---

## 46.5 Coding standards

The course presents coding standards as a way to produce code with a relatively uniform and clear format.

Benefits include:

- lower perceived complexity;
- higher readability;
- better maintainability;
- easier error identification;
- greater potential for reuse.

---
