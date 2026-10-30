Part 2 of [[Precision]]
## What is an array?

An **array** is a finite sequence of elements of the **same type**, stored in **consecutive memory locations**.

If `A` is an array of type `T` and dimension `dim`, the compiler reserves space for `dim` elements, each occupying `sizeof(T)` bytes. The lecture describes this as **static allocation**: the array's size is fixed when the program is compiled.

```cpp
int values[4] = {42, 13, 99, 7};
```

Conceptually, the array looks like this:

| Index | Element expression | Value | Relative address |
|---:|---|---:|---:|
| 0 | `values[0]` | 42 | `base + 0 * sizeof(int)` |
| 1 | `values[1]` | 13 | `base + 1 * sizeof(int)` |
| 2 | `values[2]` | 99 | `base + 2 * sizeof(int)` |
| 3 | `values[3]` | 7 | `base + 3 * sizeof(int)` |

The actual memory addresses depend on the program and execution environment. The key property is that the elements are contiguous.

### 2.1 Indexing is zero-based

For an array with `dim` elements, the valid indices are:

\[
0, 1, 2, \ldots, dim - 1
\]

For `int values[4]`, `values[0]` is the first element and `values[3]` is the last. Index `4` is already outside the array.

> [!warning] Common indexing error
> A loop that processes every element normally starts at `0` and continues while `i < size`. Do not start at `1` or use `i <= size`.

## 3. Declaration and initialization

### 3.1 Basic syntax

```cpp
type identifier[dimension];
type identifier[dimension] = {value1, value2, ...};
type identifier[] = {value1, value2, ...};
```

Examples:

```cpp
double measurements[25];          // 25 doubles; elements are not initialized here
char vowels[] = {'a', 'e', 'i', 'o'}; // dimension inferred as 4
int marks[5] = {18, 24, 30, 27, 21};
```

The compiler must be able to determine the dimension of a built-in array at compile time. The dimension can be supplied explicitly or inferred from a non-empty initialization list.

```cpp
double a[];     // ill-formed: no dimension and no initializer
```

### 3.2 Partial and zero initialization

If an initialization list supplies fewer values than the array has elements, the remaining elements are initialized to zero values of the element type.

```cpp
int a[5] = {10, 20};
// a becomes {10, 20, 0, 0, 0}
```

An empty braced initializer also zero-initializes all elements:

```cpp
int b[4] = {};
// b becomes {0, 0, 0, 0}
```

**Technical note:** this differs from an uninitialized local array declaration such as `int c[4];`. For a local built-in array of fundamental types, the elements are not automatically given zero values; reading them before assigning values leads to erroneous/undefined program behaviour. Always initialize the elements or fill them before reading them.

### 3.3 Prefer symbolic dimensions

The lecture recommends a named constant instead of repeating a numeric dimension.

```cpp
const int NUMBER_OF_GRADES = 10;
double grades[NUMBER_OF_GRADES];

for (int i = 0; i < NUMBER_OF_GRADES; ++i) {
    // process grades[i]
}
```

This improves readability and reduces the chance of inconsistent limits.

## 4. Accessing array elements

For built-in arrays, the element selection operation is subscripting:

```cpp
identifier[expression]
```

The index expression must evaluate to an integer. The selected element can generally be read, assigned, supplied to an input operation, or passed to a function according to its parameter type.

```cpp
int a[4] = {1, 2, 3, 4};
a[2] = 99;       // modify the third element
int x = a[0];    // read the first element
// cin >> a[3];  // read a value into the fourth element
```

The subscript denotes an element, not the entire array.

### 4.1 A typical traversal

```cpp
#include <iostream>
using namespace std;

int main() {
    const int SIZE = 4;
    double a[SIZE];

    cout << "Enter " << SIZE << " real numbers:\n";
    for (int i = 0; i < SIZE; ++i) {
        cout << i << ": ";
        cin >> a[i];
    }

    cout << "Values in reverse order:\n";
    for (int i = SIZE - 1; i >= 0; --i) {
        cout << "a[" << i << "] = " << a[i] << '\n';
    }
}
```

The reverse loop starts at `SIZE - 1` because that is the last valid index. With a positive fixed `SIZE`, the signed `int` index reaches `-1` and the condition `i >= 0` becomes false.

## 5. Operations that are not array-wide operations

The lecture emphasizes that built-in arrays do not support ordinary whole-array arithmetic, assignment, or stream input/output.

```cpp
int a[4] = {1, 2, 3, 4};
int b[4] = {1, 2, 3, 4};

// a++;       // invalid
// a = b;     // invalid: built-in arrays are not assignable
// cin >> a;  // invalid: no whole-array extraction operation
```

To copy, compare, print, or process all elements, write a loop or a dedicated function (or use a standard-library container with suitable operations).

### 5.1 Comparing or printing an array name

A built-in array name commonly converts to a pointer to its first element in expressions. Consequently, expressions such as `cout << a` print a pointer-like address rather than all the array elements. Comparisons such as `a == b` do **not** compare every element; after array-to-pointer conversion, they compare the addresses the pointers designate, not the contents of the two sequences.

The exact output of printing an address is implementation-dependent. Relational pointer comparisons are meaningful only in the cases defined by the language; they should not be used as a way to lexicographically compare unrelated arrays.

```cpp
for (int i = 0; i < 4; ++i) {
    cout << a[i] << ' ';
}
```

## 6. Array bounds and memory safety

For `T a[dim]`, only indices in the range `0` through `dim - 1` are valid.

```cpp
int x[5] = {};
x[0] = 10;  // valid
x[4] = 20;  // valid
// x[-1] = 1; // invalid access
// x[5] = 1;  // invalid access
```

Built-in C++ array indexing does not automatically enforce bounds. An out-of-bounds write may overwrite unrelated memory; an out-of-bounds read may produce an arbitrary result. The program may crash, appear to work, or fail elsewhere later. These effects are not reliably predictable.

The lecture mentions that compiler/runtime instrumentation such as `-fsanitize=bounds` can help find some invalid accesses. Sanitizers are diagnostic tools, not a replacement for correct index logic.

### 6.1 Bounds-checking checklist

- Confirm the array's dimension.
- Use `i < size`, not `i <= size`.
- Check that a computed index is non-negative and smaller than the dimension.
- When the array is passed to a function, pass the relevant number of elements explicitly.
- Test edge cases: an empty logical range, the first element, and the last element.

## 7. Arrays and pointers

The lecture connects array indexing with pointer arithmetic.

### 7.1 Array-to-pointer conversion

In most expressions, the name of an array converts to a pointer to its first element. For an array `A`, this means `A` behaves as a pointer to `A[0]` in many contexts. It does **not** mean that an array variable is itself an ordinary reassignable pointer variable: the array occupies the full storage for its elements, and its name cannot be assigned a new address.

```cpp
int A[3] = {10, 20, 30};
int *p = A;        // points to A[0]
// A = p;          // invalid
```

### 7.2 Pointer arithmetic is scaled by the pointed-to type

If `p` has type `T*`, then `p + i` points `i` elements after `p`, not `i` bytes after it. The address change is scaled by `sizeof(T)`.

- `p + 1`: one element forward.
- `p + i`: `i` elements forward.
- `p++`: advance to the next element.
- `p += i`: advance by `i` elements.
- For pointers to elements of the same array, `p2 - p1` gives the number of elements between them.

```cpp
short A[3] = {1, 2, 3};
for (int i = 0; i < 3; ++i) {
    cout << "A[" << i << "] = " << A[i]
         << ", address = " << static_cast<const void*>(&A[i]) << '\n';
}

for (int i = 0; i < 3; ++i) {
    cout << "address = " << static_cast<const void*>(A + i)
         << ", value = " << *(A + i) << '\n';
}
```

Addresses shown by a program vary from run to run and machine to machine. The important relationship is how the addresses are spaced, not their particular hexadecimal values.

### 7.3 Equivalent indexing expressions

For a pointer `p` pointing to an element of a valid array range:

| Indexing notation | Pointer notation | Meaning |
|---|---|---|
| `p[i]` | `*(p + i)` | Value of element `i` positions after `p` |
| `&p[i]` | `p + i` | Address of element `i` positions after `p` |
| `A[i]` | `*(A + i)` | Array element at index `i` |
| `&A[i]` | `A + i` | Address of array element `i` |

These are two notations for access to the same element, provided the pointer arithmetic stays within the valid array range (or the one-past position where the language permits forming such a pointer without dereferencing it).

### 7.4 One-past-the-end pointer

C++ permits forming a pointer to the position immediately after the last element of an array. It is useful as an end marker in loops and iterator-style processing. It is **not** valid to dereference it.

```cpp
int a[3] = {4, 5, 6};
int *begin = a;
int *end = a + 3;  // one past the final element; do not read *end

for (int *p = begin; p != end; ++p) {
    cout << *p << ' ';
}
```

## 8. Determining the size of an array

For a built-in array whose actual array type is still visible, a common size calculation is:

```cpp
int a[5] = {1, 2, 3, 4, 5};
int size = sizeof(a) / sizeof(a[0]);
```

`sizeof(a)` is the total number of bytes used by the array and `sizeof(a[0])` is the number of bytes in one element. Their quotient is the element count.

The lecture also demonstrates a pointer-arithmetic expression involving `&A + 1`. Treat such expressions as demonstrations of array layout rather than as the preferred everyday method. The clearest approach for a built-in array in its defining scope is `sizeof(a) / sizeof(a[0])`.

> [!warning] `sizeof` inside a function
> Once an array parameter has adjusted to a pointer parameter, `sizeof(parameter)` reports the size of the pointer, not the size of the original array. It cannot recover the caller's array length.

## 9. Passing arrays to Functions

### 9.1 What is passed?

When a built-in array is passed to a function using an ordinary array parameter, the array is not copied element by element. The function receives a pointer to the first element. The lecture refers to this as **pointer decay**.

```cpp
float sum(const float values[], int n) {
    float total = 0.0f;
    for (int i = 0; i < n; ++i) {
        total += values[i];
    }
    return total;
}
```

The size is usually supplied as a second argument because the pointer alone does not carry the number of elements.

```cpp
float readings[4] = {1.5f, 2.0f, 3.5f, 4.0f};
float total = sum(readings, 4);
```

### 9.2 Array parameter and pointer parameter

For a simple one-dimensional built-in array parameter, the following declarations are adjusted to pointer parameter forms by C++:

```cpp
void f(int arr[]);
void f(int arr[10]);
void f(int *arr);
```

Writing a dimension in the parameter brackets does not make the function automatically know or enforce that number of elements. The function still needs a reliable length supplied separately or through another interface.

A read-only parameter can be declared as:

```cpp
void print(const int arr[], int n);
```

`const` prevents the function from modifying the elements through this parameter. It does not make a copy and does not make the caller's array immutable through every other alias.

### 9.3 Example: reading and printing a sequence

```cpp
#include <iostream>
using namespace std;

void print(const int values[], int size) {
    for (int i = 0; i < size; ++i) {
        cout << values[i] << (i + 1 == size ? '\n' : ' ');
    }
}

int main() {
    const int SIZE = 5;
    int values[SIZE] = {};

    for (int i = 0; i < SIZE; ++i) {
        cin >> values[i];
    }

    print(values, SIZE);
}
```

### 9.4 Why `sizeof` changes meaning in a function

In the caller, `sizeof(a)` for `int a[3]` measures all three integers. In a function declared with `int a[]`, the parameter is treated as `int*`, so `sizeof(a)` measures a pointer.

```cpp
void inspect(int a[]) {
    cout << sizeof(a) << '\n'; // size of a pointer, not the full array
}

int main() {
    int a[3] = {1, 2, 3};
    cout << sizeof(a) << '\n'; // size of all three int elements
    inspect(a);
}
```

Do not rely on a specific pointer size unless the platform and data model are specified.

## 10. Multidimensional arrays

A multidimensional built-in array is an **array whose elements are arrays**. A two-dimensional array is often used as a matrix.

### 10.1 Declaration and initialization

```cpp
int mat[2][3];
int mat2[2][3] = {{1, 2, 3}, {4, 5, 6}};
int mat3[][3] = {{1, 2, 3}, {4, 5, 6}}; // first dimension inferred
```

The last two examples each represent two rows and three columns. Missing values in a braced initialization list are zero-initialized:

```cpp
int mat[2][3] = {{1, 2}, {4}};
// logical contents: {{1, 2, 0}, {4, 0, 0}}
```

For a multidimensional array, the compiler must know the inner dimensions so it can calculate how many bytes to skip when moving between rows. In a declaration or function parameter, all dimensions except the first generally need to be specified for a built-in multidimensional array.

### 10.2 Nested loops

```cpp
const int ROWS = 2;
const int COLS = 3;
int mat[ROWS][COLS] = {{1, 2, 3}, {4, 5, 6}};

for (int i = 0; i < ROWS; ++i) {
    for (int j = 0; j < COLS; ++j) {
        cout << mat[i][j] << ' ';
    }
    cout << '\n';
}
```

The outer loop selects a row; the inner loop visits the columns of that row.

### 10.3 Physical layout

C++ built-in multidimensional arrays are stored contiguously, with the elements of the last dimension varying fastest. For `int mat[2][3]`, the physical order is:

```text
mat[0][0], mat[0][1], mat[0][2],
mat[1][0], mat[1][1], mat[1][2]
```

This is commonly called **row-major order**. It explains why the compiler needs the column count to calculate the address of `mat[i][j]`.

Conceptually:

\[
\operatorname{address}(mat[i][j]) = \operatorname{base} + (i \cdot COLS + j)\cdot sizeof(T)
\]

This formula illustrates the memory layout for a conventional built-in two-dimensional array; it is not a manual substitute for correct indexing.

### 10.4 Passing a multidimensional array to a function

The function parameter must preserve the inner dimension information:

```cpp
const int COLS = 3;

void printMatrix(const int mat[][COLS], int rows) {
    for (int i = 0; i < rows; ++i) {
        for (int j = 0; j < COLS; ++j) {
            cout << mat[i][j] << ' ';
        }
        cout << '\n';
    }
}
```

The row count can be supplied separately. The column count is part of the parameter type because it determines row stride.

**Technical note:** for a true built-in 2D array, the pointer-style form of the parameter is a pointer to an array of `COLS` elements, for example `const int (*mat)[COLS]`. It is not the same type as `int**`; a 2D array is not automatically an array of independently allocated row pointers.

### 10.5 Understanding address steps

For a matrix such as `int mat[3][2]`:

- `mat` converts to a pointer to its first row in most expressions.
- `mat + 1` advances by one whole row, i.e. by `2 * sizeof(int)` bytes.
- `&mat + 1` advances by one whole matrix, i.e. by `3 * 2 * sizeof(int)` bytes.
- `mat[1]` refers to the second row, which itself converts to a pointer to its first integer in many expressions.

The pointer type controls the step size. Do not confuse “pointer to an element” with “pointer to a row”.

## 11. Returning array data from functions

A function cannot return a built-in array by value in the ordinary way. It can return a pointer, but the pointed-to storage must still exist after the function returns.

### 11.1 Returning the address of an array supplied by the caller

The source demonstrates a function that modifies an externally allocated array and returns its address:

```cpp
int* multiplyInPlace(int values[], int size, int factor) {
    for (int i = 0; i < size; ++i) {
        values[i] *= factor;
    }
    return values;
}
```

The storage belongs to the caller, so it remains alive after the function returns (assuming the caller's array itself is still in scope). The returned pointer aliases the same array; it does not represent a new copy.

### 11.2 Never return a pointer to an ordinary local array

```cpp
int* wrong(int size) {
    int local[10] = {};
    // fill local...
    return local; // WRONG: local storage lifetime ends with the function
}
```

When the function returns, an ordinary local array's lifetime ends. A pointer to it becomes dangling; using that pointer is undefined behaviour. A crash is one possible outcome, but not the only one.

### 11.3 Static arrays: lifetime versus design

A local `static` array survives after the function returns, so returning its address does not create the same dangling pointer problem. However, a static array is allocated once and reused across calls. Every caller receives access to the same storage, so a later call can overwrite data that an earlier caller thought it still had.

This design introduces shared mutable state and makes the function harder to reuse safely.

### 11.4 Safer modern alternative

When the goal is to create and return a new variable-length sequence, prefer a standard-library container such as `std::vector`:

```cpp
#include <vector>

std::vector<int> multiplyCopy(const int values[], int size, int factor) {
    std::vector<int> result;
    result.reserve(size);
    for (int i = 0; i < size; ++i) {
        result.push_back(values[i] * factor);
    }
    return result;
}
```

This is additional C++ context beyond the lecture's core examples. It avoids returning a pointer to a local built-in array and gives the returned sequence ownership of its memory.

## 12. Common misconceptions

| Misconception | Correct understanding |
|---|---|
| An array index starts at 1. | The first index is `0`. |
| `a[size]` is the last element. | `a[size - 1]` is the last element. |
| `a = b` copies one built-in array into another. | Whole-array assignment is not supported for built-in arrays. |
| `cout << a` prints every element. | It generally prints a pointer/address-like value after array-to-pointer conversion. |
| `sizeof(array_parameter)` is the full array size. | An ordinary array parameter is adjusted to a pointer parameter. |
| A pointer increment advances one byte. | `p + 1` advances by one pointed-to element. |
| `int**` is the type of every 2D array. | Built-in 2D arrays and pointer-to-pointer structures are different types and layouts. |
| A local array's address can be returned safely. | Its lifetime ends when the function returns. |
| Out-of-bounds access is always detected by C++. | Built-in array indexing is not automatically bounds-checked. |

## 13. Complexity intuition

For an array with `n` elements:

- Access by a known index (`a[i]`) is constant-time, conventionally written **O(1)**.
- A full traversal takes **O(n)** time.
- Reversing by printing elements from the end takes **O(n)** time and does not require rearranging the array.
- A naive element-by-element copy takes **O(n)** time.

These are algorithmic observations that follow from the array model; the lecture's main emphasis is the memory layout and mechanics of access.

## 14. Practice questions

1. If `int a[8]` is declared, what is the first valid index and the last valid index?
2. What is the difference between `a`, `&a[0]`, and `&a`?
3. Why is `a = b` invalid for built-in arrays?
4. If `p` is an `int*`, how far does `p + 3` advance in bytes on a system where `sizeof(int) == 4`?
5. Why should a function taking `int a[]` also receive a length argument?
6. Why can `sizeof(a)` measure the entire array in `main` but only the pointer in a function?
7. Why must a 2D array parameter specify its inner dimension?
8. What does `const int a[]` prevent a function from doing?
9. What is wrong with returning `local` from a function that declares `int local[10]`?
10. Why can a static local array lead to surprising results across calls?

### Answer key

1. First index `0`, last index `7`.
2. `a` usually converts to a pointer to the first element; `&a[0]` is the address of the first element; `&a` is the address of the entire array and has a pointer-to-array type.
3. Built-in arrays are not assignable objects in this way.
4. Twelve bytes in that illustrative environment; more generally, three `int` elements.
5. The converted pointer carries no array length.
6. The array type is preserved in `main`, but the function parameter is adjusted to a pointer.
7. The compiler needs the row size to calculate the address of an element in a later row.
8. It prevents modification of elements through that parameter.
9. The local array's lifetime ends when the function returns, leaving a dangling pointer.
10. All calls reuse the same storage; a later call can change data seen through an earlier pointer.

## 15. Quick revision sheet

- **Array:** finite, fixed-size sequence of same-type elements in contiguous storage.
- **Declaration:** `type a[N];`
- **Last valid index:** `N - 1`.
- **Safe loop:** `for (int i = 0; i < N; ++i)`.
- **Indexing equivalence:** `a[i] == *(a + i)` for valid access.
- **Address equivalence:** `&a[i] == a + i`.
- **Pointer arithmetic:** step size is scaled by `sizeof(*p)`.
- **Function parameter:** an ordinary array parameter is adjusted to a pointer parameter; pass the element count separately.
- **2D array:** contiguous rows; inner dimensions determine row stride.
- **Bounds:** built-in array accesses are not automatically checked.
- **Lifetime:** never return a pointer to an ordinary local array.
