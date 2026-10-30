Collegati a  [[Arrays]]  [[Reference and Pointers]]

## 2. What is recursion?

A **recursive function** is a function that invokes itself during its execution. A function can call itself directly, or two or more functions can call one another; the latter is known as **mutual recursion**.

Recursion is useful when a problem can be naturally described in terms of a smaller version of the same problem. The lecture gives examples such as:

- **Factorial:** \(0! = 1\), and \(n! = n\cdot(n-1)!\) for positive \(n\).
- **Fibonacci sequence:** \(F_0=1\), \(F_1=1\), and \(F_n=F_{n-1}+F_{n-2}\).
- **Even and odd numbers:** checking one value can be defined in terms of checking the preceding value with the opposite parity.
- **Expressions:** a sum can be defined as a number or as a parenthesized addition of two sums.

The recursive approach is not merely “a function calling itself.” The call must reduce the problem or move it toward a stopping condition. Otherwise, the computation may repeat forever.

### 2.1 General structure

Most introductory recursive functions can be understood as having two branches:

```cpp
ReturnType function(Parameters) {
    if (base_condition) {
        return base_result;
    }

    // Reduce or transform the problem.
    return combine(current_part, function(smaller_or_simpler_input));
}
```

This is a schematic pattern, not code that can be compiled unchanged. The return type, condition, recursive argument, and way results are combined depend on the problem.

## 3. The two essential components

The lecture emphasizes two components of a recursive function.

### 3.1 Base case (end condition)

The **base case** is a condition under which the function returns a result **without making another recursive call**. It terminates the chain of calls.

For example, in a sum from 1 to \(n\), a natural base case is:

```cpp
if (n <= 0) {
    return 0;
}
```

In factorial, the lecture uses:

```cpp
if (n == 0) {
    return 1;
}
```

The base result depends on the mathematical definition of the problem. It is not always `0` or `1`.

### 3.2 Recursive case

The **recursive case** makes another call to the same function, usually using a smaller or simpler argument.

For factorial:

```cpp
return n * factorial(n - 1);
```

For the sum from 1 to \(n\):

```cpp
return n + sumToN(n - 1);
```

### 3.3 Termination condition

A well-designed recursion must satisfy both requirements:

1. A base case is reachable for valid input.
2. Every recursive step makes progress toward a base case.

If the recursive argument never approaches the base condition, the function may make calls indefinitely. In a typical C++ program, excessive recursion can exhaust the call stack and terminate the program abnormally.

> [!warning] Do not omit the base case
> A recursive function with no effective end condition can recurse indefinitely. Merely writing an `if` statement is not enough: its condition must be reachable for the inputs the function accepts.

## 4. What happens during a recursive call?

Each function call has its own execution state: its parameters, local variables, and return point. When a function calls itself, the current call cannot finish until the recursive call returns and any remaining operations are completed.

A useful mental model is the **call stack**:

1. The initial call starts.
2. It reaches a recursive call and pauses while that new call runs.
3. The new call may start another call, creating a chain of suspended calls.
4. The base case is reached.
5. Results return from the deepest call toward the original call. This is often called **unwinding** the recursive calls.

For example, evaluating `sumToN(4)` creates calls for `4`, `3`, `2`, `1`, and `0`. The deepest call returns first; each earlier call then completes its own calculation.

### 4.1 Call stack versus the result of a call

Do not confuse the sequence of function calls with the arithmetic expression they eventually produce. A call such as

```cpp
return n + sumToN(n - 1);
```

cannot calculate its final return value until `sumToN(n - 1)` has returned. The pending addition is completed as the calls unwind.

> [!note] Source connection
> The lecture illustrates the progression of `sum(4)` across several slides, showing the nested expression becoming simpler until `sum(0)` returns `0`, after which the pending additions are evaluated (slides 5–16).

## 5. Example 1 — Sum of the first n positive integers

The task is to compute:

$$
S(n)=1+2+\cdots+n
$$

For a positive integer \(n\),

$$
S(n)=n+S(n-1)
$$

and the stopping condition is

$$
S(0)=0.
$$

### 5.1 C++ implementation

```cpp
int sumToN(int n) {
    if (n > 0) {
        return n + sumToN(n - 1);
    }
    return 0;
}
```

The lecture expresses the same logic with a local variable `res`: initialize it to `0`, assign `x + sum(x - 1)` when `x > 0`, and return `res`.

### 5.2 Trace of `sumToN(4)`

The **descent** of the recursive calls is:

```text
sumToN(4) = 4 + sumToN(3)
sumToN(3) = 3 + sumToN(2)
sumToN(2) = 2 + sumToN(1)
sumToN(1) = 1 + sumToN(0)
sumToN(0) = 0              <- base case
```

Substituting the values gives:

$$
\begin{aligned}
S(4) &= 4+S(3)\\
     &= 4+(3+S(2))\\
     &= 4+(3+(2+S(1)))\\
     &= 4+(3+(2+(1+S(0))))\\
     &= 4+3+2+1+0\\
     &= 10.
\end{aligned}
$$

The output for the lecture's example is `Sum = 10`.

### 5.3 Key observations

- Each call handles the current value `n` and delegates the remaining work to `sumToN(n - 1)`.
- The input moves closer to the base case because `n` decreases by one.
- The base call returns immediately; it does not make another recursive call.
- The pending additions are completed as the recursive calls return.

### 5.4 Iterative counterpart

The same task can be solved with a loop:

```cpp
int sumToNIterative(int n) {
    int result = 0;
    for (int i = 1; i <= n; ++i) {
        result += i;
    }
    return result;
}
```

This version is included as an explanatory comparison. The recursive and iterative approaches solve the same task, but they organize the repeated work differently.

## 6. Example 2 — Factorial

The factorial of a non-negative integer \(n\) is the product of all positive integers up to \(n\):

\[
 n! = 1\cdot2\cdot3\cdots n
\]

with the special definition:

\[
0! = 1.
\]

This definition yields the recursive rule:

\[
n! = n\cdot(n-1)! \quad \text{for } n>0.
\]

The base case `0! = 1` is necessary because it stops the recursion and makes the recursive rule produce the expected product.

### 6.1 Iterative implementation from the lecture

```cpp
long long factorialIterative(int n) {
    long long result = 1;

    for (int i = 1; i <= n; ++i) {
        result *= i;
    }

    return result;
}
```

In the lecture's example, the accumulator is named `fact_var`. It starts at `1`, and each iteration multiplies it by the current value of `i`.

### 6.2 Recursive implementation

```cpp
long long factorialRecursive(int n) {
    if (n == 0) {
        return 1;
    }

    return n * factorialRecursive(n - 1);
}
```

**Precondition:** this introductory implementation assumes `n >= 0`. Handling negative input is not covered by the slide example; robust programs should validate the input before calling it.

### 6.3 Trace of `factorialRecursive(2)`

```text
factorialRecursive(2)
= 2 * factorialRecursive(1)
= 2 * (1 * factorialRecursive(0))
= 2 * (1 * 1)
= 2
```

For a larger example:

```text
factorialRecursive(4)
= 4 * factorialRecursive(3)
= 4 * (3 * factorialRecursive(2))
= 4 * (3 * (2 * factorialRecursive(1)))
= 4 * (3 * (2 * (1 * factorialRecursive(0))))
= 4 * 3 * 2 * 1 * 1
= 24
```

### 6.4 Recursive versus iterative factorial

| Aspect | Recursive factorial | Iterative factorial |
|---|---|---|
| Main mechanism | Calls `factorialRecursive(n - 1)` | Repeats a loop over `i` |
| End condition | `n == 0` | The loop finishes after `i > n` |
| Intermediate state | Stored across active calls | Stored in the accumulator and loop variable |
| Conceptual style | Mirrors the mathematical definition | Makes repetition explicit with a loop |
| Call-stack use | One active call per level of recursion | No recursive call stack is needed |

> [!important] Integer overflow
> `long long` has a finite range. Factorials grow rapidly, so sufficiently large inputs overflow the type's representable range. The lecture compares implementations but does not specify overflow handling; this is an additional practical limitation to remember.

## 7. Example 3 — Fibonacci

The lecture uses this sequence definition:

\[
F_0=1,\qquad F_1=1,\qquad F_n=F_{n-1}+F_{n-2}\quad(n\ge2).
\]

Under this convention, the initial terms are:

| Index | Value |
|---:|---:|
| \(F_0\) | 1 |
| \(F_1\) | 1 |
| \(F_2\) | 2 |
| \(F_3\) | 3 |
| \(F_4\) | 5 |
| \(F_5\) | 8 |

Some books use a different starting convention, with \(F_0=0\) and \(F_1=1\). For this lecture and these notes, use the convention shown in the source slides.

### 7.1 Direct recursive implementation

```cpp
long long fibonacci(int n) {
    if (n == 0 || n == 1) {
        return 1;
    }

    return fibonacci(n - 1) + fibonacci(n - 2);
}
```

This example assumes `n >= 0`. The base cases cover the first two sequence elements; every other call invokes the function twice.

### 7.2 Why this recursion can be inefficient

For example:

```text
fibonacci(4)
├── fibonacci(3)
│   ├── fibonacci(2)
│   │   ├── fibonacci(1)
│   │   └── fibonacci(0)
│   └── fibonacci(1)
└── fibonacci(2)
    ├── fibonacci(1)
    └── fibonacci(0)
```

`fibonacci(2)` is computed more than once, and the same pattern repeats at larger inputs. The lecture calls attention to the risk of **combinatorial explosion** caused by identical calls in different branches.

This demonstrates an important distinction:

- A recursive definition can be elegant and closely match the mathematics.
- That does not guarantee that a direct recursive implementation is efficient.

> [!info] Further context — beyond the source slides
> Memoization or dynamic programming can store already-computed Fibonacci values and avoid repeating the same subproblems. These are useful extensions, but the slide deck's main point here is to recognize repeated recursive calls and the resulting efficiency risk.

## 8. Mutual recursion — Even and odd

In **mutual recursion**, one function calls another, and the second function may eventually call the first. The lecture presents the relationships:

- `even(n)` can be defined in terms of `odd(n - 1)`.
- `odd(n)` can be defined in terms of `even(n - 1)`.

A C++ illustration for non-negative integers is:

```cpp
bool isOdd(int n);  // declaration, needed before isEven calls it

bool isEven(int n) {
    if (n == 0) {
        return true;
    }
    return isOdd(n - 1);
}

bool isOdd(int n) {
    if (n == 0) {
        return false;
    }
    return isEven(n - 1);
}
```

**Precondition:** `n >= 0`. These base cases and the decrementing argument ensure the calls eventually reach zero. Negative arguments are outside the scope of this example.

### Trace: `isEven(3)`

```text
isEven(3)
-> isOdd(2)
-> isEven(1)
-> isOdd(0)
-> false
```

The functions alternate until reaching the base case. This is mutual recursion rather than a single function directly calling itself.

## 9. Recursive definitions of expressions

The lecture briefly illustrates recursive definitions for expressions:

- A sum can be a **number**.
- A sum can be a parenthesized expression formed as `(sum + sum)`.

This is an example of a **recursive structure**: a more complex object is defined using smaller objects of the same general kind. In programming, recursive definitions are often useful for processing structures that contain smaller substructures, although the slide deck does not develop a full expression-processing algorithm here.

## 10. Helper functions and wrapper functions

The lecture notes that a recursive solution may require two functions:

1. A **recursive helper function**, which accepts an additional recursion parameter (similar to a loop counter).
2. A **wrapper function**, which calls the helper with an appropriate initial value so the caller does not need to manage the extra parameter.

This pattern is particularly common when recursively processing arrays or ranges.

### 10.1 Example based on the lecture's factorial helper

The lecture gives a helper that multiplies the integers from `m` through `n`, then a wrapper that starts `m` at `1`:

```cpp
long long factorialHelper(int m, int n) {
    if (n == 0) {
        return 1;
    }
    if (m == n) {
        return m;
    }
    return m * factorialHelper(m + 1, n);
}

long long factorial(int n) {
    return factorialHelper(1, n);
}
```

The intended input is a non-negative `n`. The wrapper hides the initial value `m = 1` and presents a simpler interface to the rest of the program.

### 10.2 Why use a wrapper?

- It keeps the public function interface simple.
- It initializes the helper's extra state or recursion parameter.
- It separates the interface used by the caller from the internal recursive implementation.

For example, a wrapper for an array operation might call a helper with an initial index of `0`; the lecture notes that this is a frequent situation when using arrays. The exact array-recursion examples are referenced in the slides but their implementations are not reproduced in this PDF.

## 11. Recursion versus iteration

The lecture summarizes the trade-off as follows:

- Recursion is often more natural, simple, or elegant for problems that can be defined recursively.
- Critical recursive implementations can be inefficient, especially when different branches repeat identical calls.
- Stack usage depends on recursion depth, and each active call adds overhead.
- When possible, iteration is typically more efficient for straightforward repeated operations.
- Some recursive problems, especially non-tail recursion or algorithms with multiple recursive branches, are not as easy to rewrite as simple loops.

### 11.1 Comparison table

| Criterion | Recursion | Iteration |
|---|---|---|
| Repetition mechanism | Function calls | Loop (`for`, `while`, etc.) |
| State | Parameters and local state of active calls | Loop variables and accumulators |
| Stopping condition | Base case | Loop condition |
| Memory | Uses call-stack space for active recursive calls | Usually avoids extra call-stack frames for repetition |
| Clarity | Can mirror mathematical or structural definitions | Often clearer for simple counting and accumulation |
| Repeated subproblems | Can recompute results if branches overlap | Can avoid repeated work when explicitly designed to do so |
| Conversion | Tail recursion is often straightforward to transform | Some forms of recursion require an explicit stack or a more involved redesign |

### 11.2 Stack depth and overhead

If recursive calls keep making further recursive calls, earlier calls remain active while waiting for later calls to return. The deeper the chain, the more stack space is required. The lecture therefore warns about significant overhead and memory use when recursion depth becomes large.

The lecture also notes that, when passing large objects, passing by reference or pointer is important to avoid unnecessary copying. This point applies to parameter-passing decisions in recursive code as well as in ordinary functions.

## 12. Tail recursion

A recursive call is **tail-recursive** when it is the final operation performed by the current function call: once the recursive call returns, the caller has no further computation to perform other than returning that result (or completing the void function).

### 12.1 A non-tail-recursive example

```cpp
int sumToN(int n) {
    if (n <= 0) {
        return 0;
    }
    return n + sumToN(n - 1);
}
```

This is recursive, but not tail-recursive under the standard definition. After `sumToN(n - 1)` returns, the current call still has to add `n`.

### 12.2 A tail-recursive pattern

A tail-recursive version often carries an **accumulator** or helper parameter, so the remaining computation is passed into the next call:

```cpp
int sumHelper(int n, int accumulator) {
    if (n <= 0) {
        return accumulator;
    }
    return sumHelper(n - 1, accumulator + n);
}

int sumToN(int n) {
    return sumHelper(n, 0);
}
```

Here, the recursive call to `sumHelper` is the final operation in the recursive branch. This example is an explanatory addition demonstrating the pattern.

### 12.3 Tail recursion is not the same as recursion generally

Do not assume every recursive function is tail-recursive. Check what happens **after** the recursive call:

- `return n + f(n - 1);` — not tail-recursive, because addition remains.
- `return f(n - 1);` — can be tail-recursive, provided there is no further work after the call.
- `f(n - 1);` as the final action in a `void` function — can be tail-recursive if the function performs no remaining work after the call.

The lecture shows tail-recursive functions being rewritten as loops and notes that the compiler command `g++ -O2` may convert tail-recursive code into iterative form. This is a compiler optimization, not a guarantee that every recursive function will be transformed in every build.

## 13. Transforming tail recursion into iteration

The slides provide a general template for converting tail recursion into a loop.

### 13.1 General `void` pattern

Recursive form:

```cpp
void F(State x) {
    if (baseCondition(x)) {
        baseInstructions(x);
    } else {
        instructions(x);
        x = update(x);
        F(x);  // tail call
    }
}
```

Equivalent iterative structure:

```cpp
void F(State x) {
    while (!baseCondition(x)) {
        instructions(x);
        x = update(x);
    }
    baseInstructions(x);
}
```

This pattern assumes that the recursive call is a tail call and that the order of instructions and state updates is preserved by the conversion.

### 13.2 General function returning a value

A corresponding value-returning structure is:

```cpp
ReturnType F(State x) {
    while (!baseCondition(x)) {
        instructions(x);
        x = update(x);
    }
    return baseInstructions(x);
}
```

The exact type of the return expression and the updates depends on the function. The essential transformation is to replace the recursive state transition with a loop that repeatedly updates the state until the base condition is satisfied.

### 13.3 Example from the lecture: adding by repeated increment

The lecture presents a tail-recursive function with parameters `n` and `m`. While `m` is not zero, it increments `n` and decrements `m`; when `m` reaches zero, it returns `n`.

Tail-recursive version:

```cpp
int addRecursive(int n, int m) {
    if (m == 0) {
        return n;
    }
    ++n;
    --m;
    return addRecursive(n, m);
}
```

Iterative version:

```cpp
int addIterative(int n, int m) {
    while (m != 0) {
        ++n;
        --m;
    }
    return n;
}
```

Both examples assume `m >= 0`; a negative `m` would not reach the stated base case with this decrementing strategy.

> [!note] Condition syntax
> The slides show the loop condition schematically as `while (! m==0)`. In ordinary C++, write `while (m != 0)` to express the intended condition clearly and avoid operator-precedence ambiguity.

### 13.4 When conversion is not straightforward

The lecture explains that it may not be easy to rewrite recursion as a simple loop when:

- the recursive call is not the final operation;
- the function makes multiple recursive calls in different branches;
- pending operations or partial results must be remembered while a recursive call is running.

In such cases, converting the algorithm may require an explicit stack to store the state that the program would otherwise keep in its recursive calls.

## 14. Common errors and risks

### 14.1 Missing or incorrect base case

**Problem:** the recursive call chain never terminates.

**Prevention:** identify the smallest valid input and decide the exact result that should be returned for it.

### 14.2 Failure to make progress

**Problem:** a recursive call repeats the same input or moves away from the base condition.

**Prevention:** verify the argument update. For example, `n - 1` moves toward zero when `n` is positive; calling `f(n)` again without changing any relevant state may not.

### 14.3 Incorrect base result

**Problem:** the recursion terminates but produces an incorrect result.

**Prevention:** derive the base value from the mathematical definition. For instance, the lecture defines `0! = 1`, while the sum example defines `sum(0) = 0`.

### 14.4 Wrong return expression

**Problem:** a recursive call is made, but its returned result is not combined or propagated correctly.

**Prevention:** write down the recurrence mathematically before implementing it in code.

### 14.5 Excessive stack depth

**Problem:** a long chain of active calls uses too much call-stack memory.

**Prevention:** consider an iterative implementation when the algorithm permits it, especially for large inputs.

### 14.6 Repeated computations

**Problem:** recursive branches compute the same subproblem many times, as in the direct Fibonacci implementation.

**Prevention:** analyze the recursion tree before assuming the algorithm is efficient. A different approach may avoid recomputation.

### 14.7 Incorrect wrapper initialization

**Problem:** the helper starts with an invalid index, counter, or initial value.

**Prevention:** specify exactly what the helper parameters mean and ensure the wrapper provides the correct starting state.

## 15. Programming patterns and best practices

1. **Write the recurrence first.** Decide how the current result depends on a smaller result.
2. **Specify the base case explicitly.** Confirm its returned value is mathematically correct.
3. **Prove progress informally.** Explain why each valid recursive call gets closer to a base case.
4. **Trace a small input by hand.** For example, use `sumToN(3)` or `factorialRecursive(2)`.
5. **Identify pending work.** If an operation remains after the recursive call, the function is not tail-recursive.
6. **Estimate call depth.** A function that decreases an integer by one has a call chain proportional to that input for the straightforward versions shown here.
7. **Look for repeated subproblems.** A recursive tree with overlapping branches can waste time.
8. **Keep interfaces simple.** Use a wrapper to set recursion parameters when that makes the function easier to call.
9. **Validate preconditions.** If the algorithm assumes non-negative inputs, check that condition at the program boundary.
10. **Use references or pointers thoughtfully.** The lecture notes that passing large objects by reference or pointer can avoid copying costs.

## 16. Review sheet

### Essential definitions

| Term | Definition |
|---|---|
| Recursion | A function calls itself to solve a problem in terms of a smaller or simpler instance. |
| Mutual recursion | Two or more functions call one another. |
| Base case | The condition and result that stop further recursive calls. |
| Recursive case | The branch that calls the function again. |
| Call stack | The runtime structure that keeps track of active function calls and their execution state. |
| Call unwinding | The stage in which calls return and pending operations are completed. |
| Wrapper function | A function that invokes a helper with the initial state needed by the recursive algorithm. |
| Helper function | A function that performs the internal recursive work, often with extra parameters. |
| Tail recursion | Recursion in which the recursive call is the final operation of the current call. |
| Iteration | Repetition implemented using loops rather than recursive calls. |

### Formulas used in the lecture

**Sum of positive integers:**

\[
$S(0)=0,\qquad S(n)=n+S(n-1)\quad(n>0)$
\]

**Factorial:**

\[
$0!=1,\qquad n!=n(n-1)!\quad(n>0)$
\]

**Fibonacci convention used in these slides:**

\[
$F_0=1,\quad F_1=1,\quad F_n=F_{n-1}+F_{n-2}\quad(n\ge2)$
\]

### Exam-ready distinctions

- **Recursive case vs base case:** one makes another call; the other stops the calls.
- **Direct vs mutual recursion:** one function calls itself; multiple functions call each other.
- **Recursion vs iteration:** recursive calls versus loop-based repetition.
- **Recursive vs tail-recursive:** a tail-recursive function has no work left after its recursive call returns.
- **Elegant vs efficient:** a natural recursive definition can still cause excessive work, as in naive Fibonacci.
- **Local array vs returned pointer:** this is a related lifetime topic from the Arrays lecture, not developed in this recursion lecture; ensure that any returned address still refers to an object that exists after the function returns.

## 17. Self-check questions

Try to answer these without looking at the preceding sections.

1. What two components does every introductory recursive solution need?
2. Why is `return n + sumToN(n - 1);` not tail-recursive?
3. Trace `sumToN(3)` and give the result.
4. Trace `factorialRecursive(3)` from the first call to the final return.
5. Why does factorial use `1` as the base result, while the sum example uses `0`?
6. What is mutual recursion? Give the even/odd example.
7. Which Fibonacci values result from the lecture's convention for \(F_0\) through \(F_4\)?
8. Why does naive recursive Fibonacci repeat work?
9. What is the purpose of a wrapper function?
10. What makes a recursive call a tail call?
11. Name two reasons iteration may be preferable to recursion.
12. Why can converting non-tail recursion to a loop require an explicit stack?

### Brief answer key

1. A recursive case and one or more end/base conditions.
2. The addition must be performed after the recursive call returns.
3. `3 + 2 + 1 + 0 = 6`.
4. `3 * fact(2) = 3 * (2 * fact(1)) = 3 * (2 * (1 * fact(0))) = 6`.
5. `0` is the neutral element for addition; `1` is the neutral element for multiplication, consistent with the definitions shown.
6. Functions call one another, such as `isEven(n)` calling `isOdd(n-1)` and vice versa.
7. `1, 1, 2, 3, 5`.
8. Different branches make calls for the same smaller argument.
9. It supplies initial helper parameters while exposing a simpler interface.
10. It is the final operation, so the caller has no pending work after it returns.
11. Typically lower call overhead and reduced stack use for simple repetition.
12. Because partial results or pending operations may need to be stored explicitly instead of being retained in active function calls.

## 18. Source and related notes

### Primary source

- **Lecture file:** `L13_function_part3.pdf`
- **Course:** Computer Programming 1
- **Lecture title:** Functions (part 3)
- **Lecture date shown in the slides:** 16 October 2026
- **Main source sections:** recursive functions; recursive sum; iterative and recursive factorial; mutual recursion; helper/wrapper functions; recursion versus iteration; tail recursion and its conversion to loops.

### Suggested Obsidian placement

```text
Computer Programming 1/
└── Functions/
    ├── Functions Part 1 — Introduction and Definition.md
    ├── Functions Part 2 — Parameter Passing.md
    └── Functions Part 3 — Recursive Functions.md   ← this note
```

### Related note links
- [[Arrays]]
- [[Reference and Pointers]]

The first two links are suggested companion notes for the earlier parts of the lecture series. If their filenames differ in your vault, change the wiki links to match the actual note titles.
