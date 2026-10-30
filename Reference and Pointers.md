## This is Part 2 of [[Data Types]]


## 1. Big picture

The lecture introduces **references** and **pointers** as mechanisms for working with objects through their **memory addresses** rather than only through their ordinary values.

The central ideas are:

- a **reference** gives another name (an alias) to an existing object;
- a **pointer** stores the address of an object;
- the **address-of operator `&`** obtains an address;
- the **dereference operator `*`** accesses the object located at an address stored in a pointer;
- pointers can themselves be addressed, which leads to **pointers to pointers**;
- pointer arithmetic is defined in relation to the size of the pointed type;
- `const` can qualify the pointed object, the pointer itself, or both.

---

# 2. Data types: where references and pointers fit

The lecture distinguishes two broad groups:

### Fundamental data types

These represent simple information.

Examples include:

- integer types;
- Boolean type;
- real types;
- character type;
- enumeration types.

### Derived data types

More complex types can be derived from fundamental types.

The lecture identifies the following constructs as mechanisms for building derived types:

- **references**;
- **pointers**;
- **arrays**;
- **structures**;
- **unions**;
- **classes**.

Structures, unions, and classes are also presented as constructs for user-derived data types.

> [!important] Position in the course
> References and pointers are therefore part of the broader **data-types and memory-management** section of Computer Programming 1, not an isolated programming-language topic.

---

# 3. Core memory terminology

To understand references and pointers, keep three ideas distinct:

| Concept | Meaning |
|---|---|
| **Value** | The content stored in an object/variable |
| **Address** | The memory location associated with an object |
| **Reference** | Another name for an existing object |
| **Pointer** | A variable whose value is an address |

For a variable such as:

```cpp
int x = 10;
```

we can conceptually distinguish:

```text
x        -> value 10
&x       -> address of x
```

A pointer can store that address:

```cpp
int *p = &x;
```

Then:

```text
p        -> address of x
*p       -> value/object stored at that address -> 10
```

This distinction is fundamental:

```text
x     = object/value
&x    = address of x
p     = address stored in pointer
*p    = object reached through p
```

---

# 4. Reference

## 4.1 Definition

A **reference** allows multiple names to refer to the same variable (or, more generally in the course terminology, to an expression that has an address).

The reference is a **synonym** of the expression to which it refers.

This produces **aliasing**:

> modifying one name modifies the same underlying object seen through the other name.

### General syntax

```cpp
type &id = exp;
```

where:

- `type` is a C++ type;
- `id` is the reference name;
- `exp` must be an expression with an address.

### Basic example

```cpp
int x = 1;
int &y = x;
y = 6;
```

After the assignment:

```text
x = 6
y = 6
```

There are two names associated with the same object.

> [!note]
> In the course terminology, `y` is described as a **reference-to `x`** and therefore as a synonym/alias of `x`.

---

# 5. Constraints on references

References have several important restrictions.

## 5.1 Initialization is mandatory

A reference cannot be defined without being initialized.

```cpp
int &y;       // WRONG
```

Correct:

```cpp
int x = 1;
int &y = x;   // OK
```

So the following is a key rule:

> **A reference must be initialized when it is defined.**

---

## 5.2 A reference cannot be rebound

Once a reference has been defined, it cannot later be made to refer to a different object.

Example:

```cpp
double x1, x2;
double &y = x1;   // OK
// double &y = x2; // WRONG: y has already been defined
```

Conceptually:

```text
reference y -> x1
```

cannot later become:

```text
reference y -> x2
```

---

## 5.3 The referred expression must have an address

This is invalid:

```cpp
float &y = 10.2;  // WRONG
```

The lecture explains that the expression used as the referent must have an address.

A normal variable has an address:

```cpp
double d = 3.1;
int &z = d;       // WRONG: incompatible type
```

The problem here is different: `d` has an address, but its type is `double`, while the reference is declared as `int&`.

Therefore, the two conditions are:

1. the expression must have an address;
2. the type must be compatible with the declared reference type.

---

# 6. The address-of operator `&`

The symbol `&` has more than one role in C++.

In:

```cpp
int &r = x;
```

it participates in the declaration of a **reference**.

In:

```cpp
&x
```

it is the **address-of operator**.

These two uses must not be confused.

## 6.1 Meaning of `&`

The address-of operator returns the **address (I-value)** of the expression to which it is applied.

Example:

```cpp
int x = 10;
cout << &x << endl;
```

This prints the address of `x`, not the value `10`.

The operator can be applied only to an expression that has an address.

Therefore:

```cpp
&(x * 5)
```

is invalid according to the lecture because `x * 5` is an expression without an address suitable for this operation.

---

# 7. Reference and address: essential distinction

Suppose:

```cpp
int n = 10;
int &r = n;
```

Then conceptually:

```text
n  -> 10
r  -> 10
```

and:

```text
&n -> address of n
&r -> same address
```

The important point is that `r` is an alias of `n`.

Changing `r` changes `n`:

```cpp
r = 20;
```

results in:

```text
n = 20
r = 20
```

---

# 8. Important special case: address of a `char`

The lecture includes a specific warning about printing the address of a `char` variable.

Because of the way `cout` handles characters, directly writing:

```cpp
char c = 'a';
cout << &c;
```

can be interpreted as character data rather than as an address representation.

The lecture therefore uses:

```cpp
cout << (void *)&c;
```

to obtain the address representation.

### Remember

For a normal numeric object:

```cpp
cout << &x;
```

For a `char` address, in the lecture examples:

```cpp
cout << (void *)&c;
```

> [!warning]
> Do not interpret `&c` as automatically meaning “print a nice memory address” when `c` is a `char`. The course explicitly demonstrates the `(void*)` workaround.

---

# 9. Pointers

## 9.1 Definition

A **pointer** is a variable that contains the address of an object.

The pointer's **r-value is an address**.

### Syntax

```cpp
type *id = init;
```

Example:

```cpp
int *px;
```

This means that `px` is a pointer to an object of type `int`.

---

## 9.2 Type of the pointed object

A pointer has an associated pointed type.

Examples:

```cpp
int *p;
char *c;
double *d;
```

The pointer must contain an address compatible with its declared pointed type.

For example:

```cpp
int x;
int *p = &x;      // OK
```

The pointer variable itself occupies memory, but the definition of the pointer does **not** reserve the memory occupied by the object it points to.

---

## 9.3 Pointer initialization

Unlike references, pointer initialization is **not mandatory** when the pointer is declared:

```cpp
int *p;
```

This is permitted by the course material.

However, using an uninitialized pointer to access an object is unsafe because the pointer does not yet contain a valid known address.

A meaningful pointer setup is:

```cpp
int x = 10;
int *p = &x;
```

---

# 10. Dereference operator `*`

The symbol `*` also has multiple roles in C++.

In:

```cpp
int *p;
```

it is used in the declaration of a pointer.

In:

```cpp
*p
```

it is the **dereference operator**.

## 10.1 Meaning of dereference

Dereferencing a pointer accesses the object whose address is stored in the pointer.

Suppose:

```cpp
int x = 1;
int *p = &x;
```

Then:

```text
p  -> address of x
*p -> x
```

So:

```cpp
*p = 2;
```

modifies `x`:

```text
x = 2
```

The lecture describes `*p` as a temporary synonym for the object pointed to by `p`.

---

# 11. The fundamental `&` / `*` relationship

This is one of the most important patterns in the lecture.

Starting from:

```cpp
int x = 10;
```

we can obtain its address:

```cpp
&x
```

and store it in a pointer:

```cpp
int *p = &x;
```

Then dereferencing the pointer:

```cpp
*p
```

gives access to `x`.

Conceptually:

```text
x
│
├── value: 10
└── address: A

p
└── value: A

*p
└── object at address A -> x -> 10
```

Therefore:

```text
&x = address of x
p  = address of x
*p = x
```

---

# 12. Assignments between pointers

Suppose:

```cpp
int a = 5;
int b = 7;

int *p;
int *q;

p = &a;
q = &b;
```

Initially:

```text
p -> a
q -> b
```

Then:

```cpp
*q = *p;
```

means:

```text
b = a
```

So `b` becomes `5`.

But:

```cpp
q = p;
```

has a completely different meaning.

It assigns the **pointer value**:

```text
q -> same address stored in p
```

Therefore both pointers point to the same object:

```text
p -> a
q -> a
```

### Critical distinction

```cpp
*q = *p;
```

copies the **pointed value**.

```cpp
q = p;
```

copies the **address stored in the pointer**.

This difference is essential.

---

# 13. Reference to an object reached through a pointer

The lecture uses the example:

```cpp
int n = 5;
int *p = &n;
int &r = *p;
```

Interpretation:

1. `n` is an integer object;
2. `p` stores the address of `n`;
3. `*p` accesses `n`;
4. `r` becomes a reference to `*p`, therefore a reference to `n`.

Conceptually:

```text
p  -> n
*p -> n
r  -> n
```

Now consider:

```cpp
int m = 2;
p = &m;
```

The pointer `p` changes what it points to.

But `r` does **not** follow the pointer.

It remains a reference to the original object `n`.

Before:

```text
p -> n
r -> n
```

After:

```text
p -> m
r -> n
```

> [!important] Key conceptual rule
> A reference initialized from `*p` refers to the object selected at that moment. Reassigning the pointer later does not “move” the reference.

This is one of the main conceptual differences between references and pointers.

---

# 14. References vs pointers

| Property | Reference | Pointer |
|---|---|---|
| Basic idea | Another name / alias for an object | Variable containing an address |
| Example | `int &r = x;` | `int *p = &x;` |
| Initialization at definition | Mandatory | Not mandatory |
| Rebinding | Not possible | Possible by assignment |
| Access object | Use reference name directly | Use dereference `*p` |
| Address | `&r` gives address of referred object | `p` contains an address |
| Main role in this lecture | Aliasing | Explicit address manipulation |

A useful mental model:

```text
REFERENCE
r ─────────► same object as x

POINTER
p ──stores──► address of x
              │
              ▼
             x
```

---

# 15. Pointer arithmetic

The lecture introduces arithmetic operations on pointers and addresses.

Suppose `p` has type:

```cpp
T *p;
```

and `index` is an integer.

Then:

```cpp
p + index
```

has type `T*`.

The essential rule is:

> the integer displacement is implicitly multiplied by `sizeof(T)`.

So pointer arithmetic is based on **objects of the pointed type**, not on raw bytes.

---

## 15.1 Example with `int`

If:

```cpp
int *p;
```

and the size of `int` is 4 bytes in the lecture example, then conceptually:

```text
p + 1  -> address + 4 bytes
p + 2  -> address + 8 bytes
p + 3  -> address + 12 bytes
```

Thus:

```cpp
p + 4
```

means four `int` positions ahead, not four bytes ahead.

---

## 15.2 Increment and decrement

The same idea applies to:

```cpp
p++;
++p;
p--;
--p;
p += index;
```

For example, with an `int*`, incrementing by one advances by one `int` position, i.e. by `sizeof(int)` bytes.

---

# 16. Pointer difference

If `p1` and `p2` have the same pointer type `T*`, then:

```cpp
p2 - p1
```

produces an integer representing the number of `T`-sized positions between the two pointer positions.

In the lecture's formulation:

```text
p2 - p1
```

is implicitly divided by:

```text
sizeof(T)
```

Conceptually:

```text
p1 ------------------------------> p2
       4 objects of type T

p2 - p1 = 4
```

The lecture also notes that comparison operators can be applied to compatible pointers, e.g.:

```cpp
p1 < p2
p1 >= p2
```

---

# 17. Pointer arithmetic is type-dependent

Consider the difference between:

```cpp
int *p;
```

and:

```cpp
char *p;
```

If the pointed object occupies 4 bytes:

```cpp
p + 1
```

moves to the next `int` position.

If the pointed object occupies 1 byte:

```cpp
p + 1
```

moves one byte.

So pointer arithmetic always depends on the pointed type `T`.

### Important formula

```text
p + n
      = address(p) + n * sizeof(T)
```

Conceptually, the pointer is moving across **objects**, not simply across arbitrary bytes.

---

# 18. `p + 1` is not the same as `(long)p + 1`

The lecture explicitly demonstrates a subtle but important distinction.

Consider:

```cpp
(long)p1 + 1
```

The pointer is first converted to a numeric integer representation, and then `1` is added as a raw numeric value.

By contrast:

```cpp
(long)(p1 + 1)
```

performs pointer arithmetic first and only then converts the resulting address.

Therefore these are not equivalent.

### Example idea

For an `int*` where `sizeof(int) == 4`:

```cpp
(long)p1 + 1
```

means roughly:

```text
raw numeric address + 1 byte
```

while:

```cpp
(long)(p1 + 1)
```

means:

```text
next int position
= address + sizeof(int)
= address + 4 bytes
```

> [!warning]
> Parentheses matter. Pointer arithmetic and numeric arithmetic are not interchangeable.

---

# 19. Priority: `*` vs arithmetic and increment

The lecture gives several examples where the position of parentheses changes the meaning.

## 19.1 `*pv + 1`

```cpp
*pv + 1
```

is equivalent to:

```cpp
(*pv) + 1
```

It is **not** equivalent to:

```cpp
*(pv + 1)
```

because the first computes the pointed value and adds one, while the second moves the pointer and then dereferences it.

---

## 19.2 `*pv++`

The lecture states:

```cpp
*pv++
```

is equivalent to:

```cpp
*(pv++)
```

and not to:

```cpp
(*pv)++
```

These are different operations:

- `*(pv++)` → use the current pointer target, then increment the pointer;
- `(*pv)++` → increment the pointed object's value.

> [!tip]
> When pointer expressions become complicated, use explicit parentheses rather than relying on precedence from memory.

---

# 20. Pointer to `void`

The lecture introduces:

```cpp
void *p;
```

as a pointer to an unspecified type.

A `void*` can be assigned an address obtained from a pointer of another type.

Example structure from the lecture:

```cpp
int a;
int *pa = &a;

char c;
char *pc = &c;

void *tp;
tp = pa;
```

At this point `tp` contains the same address as `pa`.

However, a `void*` does not provide the concrete pointed type needed for normal dereferencing. Therefore the lecture explicitly converts it back to the appropriate pointer type before dereferencing:

```cpp
*(int*)tp = 3;
```

and for a character:

```cpp
tp = pc;
*(char*)tp = 'C';
```

### Conceptual pattern

```text
specific pointer
      │
      ▼
   void *
      │
      ▼
convert back to the correct type
      │
      ▼
dereference
```

---

# 21. Pointers to constant objects

The lecture distinguishes several forms involving `const`.

## 21.1 Pointer to constant

Syntax:

```cpp
const type *id = init;
```

Example:

```cpp
const int *p = &x;
```

Interpretation:

> the pointed object cannot be modified **through this pointer** by dereferencing it.

For example:

```cpp
const int *pc2 = &c2;
```

allows reading the pointed value:

```cpp
*pc2
```

but not:

```cpp
*pc2 = 2;   // WRONG
```

The pointer itself, however, is not constant.

Therefore it can point somewhere else:

```cpp
pc2 = pc1;  // OK
```

---

## 21.2 Constant pointer

Syntax:

```cpp
type *const id = exp;
```

Example:

```cpp
int *const pa = &a;
```

Interpretation:

> the pointer itself cannot be changed to point to another object.

Therefore:

```cpp
*pa = 3;   // OK: pointed object can be modified
```

but:

```cpp
pa = &b;   // WRONG: pa itself is constant
```

Initialization is mandatory here because the constant pointer must be given its value when defined.

---

# 22. Constant pointer to constant

Syntax:

```cpp
const type *const id = exp;
```

Example:

```cpp
const int *const a = &c;
```

Now there are **two restrictions**:

1. the pointer cannot be reassigned;
2. the pointed object cannot be modified through the pointer.

Therefore both are invalid:

```cpp
a = &b;     // WRONG
*a = 2;     // WRONG
```

### The three forms to memorize

| Syntax | Pointer can change target? | Object can be modified through pointer? |
|---|---:|---:|
| `const int *p` | Yes | No |
| `int *const p` | No | Yes |
| `const int *const p` | No | No |

This table is an excellent exam memorization item.

---

# 23. Pointers to pointers

A pointer variable is itself a variable and therefore has an address.

This makes it possible to create a pointer to a pointer.

### Types

```cpp
int *p;
int **pp;
```

Interpretation:

```text
p  -> pointer to int
pp -> pointer to pointer to int
```

Similarly:

```cpp
char **c;
```

means a pointer to a pointer to `char`.

---

## 23.1 Basic example

Consider:

```cpp
int a = 2;
int *pa = &a;
int **ppa = &pa;
```

The relationships are:

```text
ppa -> pa -> a
```

More precisely:

```text
ppa
 │
 └── stores address of pa

pa
 │
 └── stores address of a

 a
 └── stores value 2
```

---

## 23.2 Dereferencing step by step

Given:

```cpp
int a = 2;
int *pa = &a;
int **ppa = &pa;
```

then:

```cpp
*pa
```

returns the value of `a`:

```text
*pa = 2
```

While:

```cpp
*ppa
```

returns the value stored in `pa`, which is the address of `a`.

Finally:

```cpp
**ppa
```

means:

```cpp
*(*ppa)
```

and therefore reaches the final object `a`:

```text
**ppa = 2
```

### Mental model

```text
ppa
 │
 ▼
 pa
 │
 ▼
 a = 2
```

Each `*` removes one level of indirection:

```text
ppa        -> pointer-to-pointer
*ppa       -> pointer
**ppa      -> final int value
```

---

# 24. Addresses of pointers

The lecture emphasizes that the pointer variable itself has its own address.

For:

```cpp
int a;
int *pa = &a;
int **ppa = &pa;
```

we can distinguish:

```text
&a   -> address of a
pa   -> address of a
&pa  -> address of the pointer variable pa
ppa  -> address of pa
&ppa -> address of the pointer-to-pointer variable ppa
```

This is why a pointer can point to another pointer:

```cpp
ppa = &pa;
```

---

# 25. Pointer arithmetic and memory layout

The lecture's examples show addresses such as:

```text
address of v1
address of v2
address of v3
```

and use them to illustrate the relationship between consecutive objects and `sizeof(T)`.

For `int` examples, the lecture uses a typical size of 4 bytes. Thus consecutive `int` objects are illustrated as being separated by 4 bytes.

For `char` examples, a character is illustrated as occupying 1 byte, so consecutive characters differ by 1 byte in the examples.

> [!note]
> The lecture uses concrete addresses mainly to illustrate the mechanism. The important theoretical point is not memorizing an address, but understanding that pointer arithmetic is scaled according to `sizeof(T)`.

---

# 26. The role of `sizeof`

The lecture relies on `sizeof(T)` when explaining pointer arithmetic.

For a type `T`:

```cpp
sizeof(T)
```

returns the size in bytes used to represent that type in the implementation.

This is crucial for understanding why:

```cpp
p + 1
```

does not simply mean “add one byte”.

Instead, it means move by one position of the pointed type:

```text
p + 1
= address + sizeof(T)
```

and:

```text
p + n
= address + n * sizeof(T)
```

conceptually.

---

# 27. Core examples to know

## Example A — reference alias

```cpp
int x = 1;
int &r = x;
r = 6;
```

Result:

```text
x = 6
r = 6
```

### Why?

`r` is an alias of `x`.

---

## Example B — address-of

```cpp
int x = 10;
cout << &x << endl;
```

Result: the address of `x` is printed.

---

## Example C — pointer and dereference

```cpp
int x = 1;
int *p = &x;
*p = x + 1;
```

Result:

```text
x = 2
```

because `*p` accesses `x`.

---

## Example D — changing the pointer target

```cpp
int n = 5;
int *p = &n;
int &r = *p;

int m = 2;
p = &m;
```

After the last assignment:

```text
p -> m
r -> n
```

The reference does not follow the pointer.

---

## Example E — copying a value vs copying an address

```cpp
int a = 5;
int b = 7;

int *p = &a;
int *q = &b;
```

Then:

```cpp
*q = *p;
```

means:

```cpp
b = a;
```

whereas:

```cpp
q = p;
```

means:

```text
q points where p points
```

---

## Example F — pointer to pointer

```cpp
int a = 2;
int *pa = &a;
int **ppa = &pa;
```

Then:

```cpp
*pa       // 2
*ppa      // address of a
**ppa     // 2
```

---

# 28. Exam-oriented distinctions

## 28.1 Reference vs address

```cpp
int &r = x;
```

`r` is a reference/alias.

```cpp
&x
```

is the address of `x`.

These are different uses of the same symbol `&`.

---

## 28.2 Pointer value vs pointed value

Suppose:

```cpp
int x = 10;
int *p = &x;
```

Then:

```cpp
p   // address
*p  // value/object at that address
```

---

## 28.3 `p = q` vs `*p = *q`

```cpp
p = q;
```

copies an address.

```cpp
*p = *q;
```

copies the pointed value.

This distinction is extremely important.

---

## 28.4 Reference vs pointer rebinding

Reference:

```cpp
int &r = x;
```

cannot later be redirected to another variable.

Pointer:

```cpp
int *p = &x;
p = &y;
```

can change its target.

---

## 28.5 `*p + 1` vs `*(p + 1)`

```cpp
*p + 1
```

means:

```cpp
(*p) + 1
```

while:

```cpp
*(p + 1)
```

means:

1. move the pointer one position;
2. dereference the new pointer position.

---

## 28.6 `*(p++)` vs `(*p)++`

```cpp
*(p++)
```

updates the pointer after using the current pointer value.

```cpp
(*p)++
```

updates the pointed object's value.

These operations must not be confused.

---

# 29. Concept map

```text
DATA TYPES
│
├── Fundamental types
│
└── Derived types
     │
     ├── References
     │    └── alias / synonym
     │
     ├── Pointers
     │    ├── address stored as value
     │    ├── dereference *
     │    ├── address-of &
     │    ├── pointer assignment
     │    ├── pointer arithmetic
     │    ├── void*
     │    ├── const pointers
     │    └── pointers to pointers
     │
     ├── Arrays
     ├── Structures
     ├── Unions
     └── Classes
```

---
