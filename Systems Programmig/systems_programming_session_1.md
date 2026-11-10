# Systems Programming Mastery - Session 1

## 1. Core Mental Model

A program is a set of instructions that a computer executes.

At a simplified level:

```text
Source code
    ↓
Compiler
    ↓
Machine instructions
    ↓
CPU executes instructions
    ↓
Memory stores instructions and data
```

The CPU repeatedly performs something like:

```text
Fetch instruction
    ↓
Decode instruction
    ↓
Execute instruction
    ↓
Repeat
```

The CPU and memory work together. The CPU executes instructions, while memory holds the instructions and data needed during execution.

---

## 2. Values and Addresses

A **value** is the actual information being stored.

An **address** identifies where something is located in memory.

Example:

```text
Address     Value
0x1000       5
0x1004       3
0x1008       8
```

Important:

- `0x` indicates hexadecimal notation.
- An address is itself represented as a numeric value.
- A variable name is not the same thing as its memory address.

If:

```text
x = 42
```

and `x` happens to be stored at `0x1000`:

```text
x  = 42
&x = 0x1000
```

`&x` means "the address of `x`."

---

## 3. Pointers

A **pointer** is a variable whose value is an address, usually the address of another object.

Example:

```text
x = 42
&x = 0x1000
p = 0x1000
```

Then:

```text
p  → contains an address
*p → accesses the value at that address
```

If:

```text
*p = 99
```

then the memory at the address stored in `p` is changed to `99`.

If `p` points to `x`, this means `x` becomes `99`.

Conceptually:

```text
p
│
└────→ 0x1000
         │
         └────→ x = 42
```

After:

```text
*p = 99
```

the value at `0x1000` becomes `99`.

### Important distinction

A pointer is fundamentally an address. It does not necessarily have to point to a normal variable. It can point to an array element, struct, dynamically allocated memory, or other memory.

---

## 4. CPU, ALU, and Memory

For:

```text
x = 10
y = 20
z = x + y
```

conceptually:

```text
1. CPU reads the value of x → 10
2. CPU reads the value of y → 20
3. ALU performs 10 + 20 → 30
4. CPU stores 30 where z belongs
```

The ALU is responsible for arithmetic and logical operations.

Variables such as `x`, `y`, and `z` are names in source code. During execution, their values are represented in memory and/or CPU registers.

---

## 5. Stack Memory

The stack is closely associated with **function calls**.

A function call creates a stack frame that can contain things such as local variables, saved state, and return information.

Example:

```text
main()
 ├── x
 ├── y
 └── calls foo()

foo()
 ├── a
 └── b
```

When `foo()` returns, its stack frame is removed and its local variables can no longer be accessed through their local names.

Example:

```text
fn foo() {
    let x = 10;
}
```

When `foo()` returns, `x`'s lifetime ends.

The underlying memory is not necessarily physically erased immediately. The space becomes available for reuse.

---

## 6. Heap Memory

The heap is used for **dynamically allocated data** whose size and/or lifetime is not conveniently tied to a normal stack frame.

For example, data created inside a function can live beyond that function's stack frame if it is stored in an appropriate heap allocation.

A simplified model:

```text
Stack                 Heap

local variables       dynamically allocated data
function frames       data with independent lifetimes
```

Do not use the oversimplified rule:

> Stack = primitive variables, Heap = objects.

That is not a reliable systems-level model.

---

## 7. Object Lifetime and Dangling Pointers

Memory has both a **location** and a **lifetime**.

Suppose a function has a local variable:

```text
foo()
    x = 10
    p → x

foo() returns
```

After `foo()` returns, `x`'s lifetime has ended.

If `p` still contains the old address, it is a **dangling pointer**.

The pointer may still contain the old numeric address, but the object that used to exist there is no longer valid.

Using such a pointer is invalid.

---

## 8. Memory Leaks

Heap memory needs to eventually be reclaimed.

If a program repeatedly allocates heap memory but never releases it:

```text
allocation → allocation → allocation → allocation → ...
```

memory consumption can continuously grow.

This is a **memory leak**.

Eventually, the program may consume excessive memory and crash or be killed by the operating system.

---

## 9. Shared Memory Problems

If multiple parts of a program can access the same memory, problems can occur.

### Concurrent read and modification

If one part modifies data while another reads it, the reader may observe unexpected or inconsistent state.

When timing determines the result, this can become a **race condition**.

### Use-after-free

If one part frees memory while another still tries to use it, the second part may access memory that is no longer valid.

This is called **use-after-free**.

These are major classes of problems that Rust's memory-safety model is designed to prevent.

---

# Rust Fundamentals

## 10. `fn main()`

A basic Rust executable:

```rust
fn main() {
    println!("Hello, systems programming!");
}
```

`main` is the conventional entry point of a Rust executable.

`fn` is the keyword used to define a function.

`main` itself is not a keyword.

---

## 11. `println!`

`println!` is a **macro**, not a keyword.

The `!` indicates that it is being invoked as a macro.

Example:

```rust
println!("hello");
```

Formatting values:

```rust
println!("{}", x);
```

The `{}` is a formatting placeholder.

---

## 12. Variables and Immutability

Rust variables are immutable by default.

```rust
let x = 10;
x = 20; // ❌
```

To allow mutation:

```rust
let mut x = 10;
x = 20; // ✅
```

Important:

```text
let     → bind a variable
let mut → bind a mutable variable
```

Rust makes mutation explicit.

---

## 13. Type Inference

Rust can infer many types automatically.

```rust
let x = 10;
let y = 10.5;
```

In this context:

```text
x → integer, typically i32
y → f64
```

Rust does not silently convert between integer and floating-point types.

Therefore:

```rust
let x = 10;
let y = 10.5;
let z = x + y; // ❌
```

requires an explicit conversion.

---

## 14. Different Integer Types

Rust distinguishes between types such as `i32` and `i64`.

This does not compile:

```rust
let x: i32 = 10;
let y: i64 = x;
```

because the types are different.

An explicit conversion is required:

```rust
let y: i64 = x as i64;
```

This is a **type issue**, not an ownership issue.

Important debugging lesson:

> Do not classify every Rust compiler error as an ownership problem.

Potential categories include:

- type mismatch
- ownership/move
- borrowing
- mutability
- lifetime
- trait requirements
- syntax

---

## 15. `Copy` vs `Move`

Consider:

```rust
let x: i32 = 10;
let y = x;
println!("{}", x);
```

This compiles because `i32` implements `Copy`.

Conceptually:

```text
x → 10
y → 10
```

The value is copied.

Compare this with:

```rust
let a = String::from("hello");
let b = a;
```

Here ownership moves from `a` to `b`.

```text
Before:

a ──owns──→ "hello"

After:

b ──owns──→ "hello"
a → invalid
```

`a` can no longer be used as the owner of that `String`.

---

## 16. Why `String` Moves

A `String` owns heap-allocated data.

Conceptually:

```text
Stack                 Heap

String metadata ───→ "hello"
```

The metadata includes information needed to manage the heap allocation, including a pointer to the buffer, its length, and its capacity.

If Rust automatically made a shallow copy of the ownership information, two values could potentially appear to own the same allocation.

That could lead to problems such as **double-free**.

Therefore Rust uses move semantics for types such as `String`.

---

## 17. Ownership

The central ownership idea:

> A value has one owner at a time.

Example:

```rust
let a = String::from("hello");
let b = a;
```

Ownership moves:

```text
a ──owns──→ "hello"

     move

b ──owns──→ "hello"
a → invalid
```

Ownership helps determine who is responsible for the lifetime and eventual cleanup of data.

---

## 18. Borrowing

Rust allows access to data without transferring ownership.

Example:

```rust
let a = String::from("hello");
let b = &a;

println!("{}", a);
println!("{}", b);
```

This is valid.

Here:

```text
a ──owns──→ "hello"
│
└── b borrows access
```

`b` does not become the owner.

A borrow does not permanently take ownership from the original owner.

---

## 19. Immutable vs Mutable Borrowing

Rust's core borrowing rule:

> You can have either multiple immutable borrowers, or one mutable borrower, but not both at the same time.

Safe:

```rust
let a = &x;
let b = &x;
```

Multiple readers are allowed.

Unsafe:

```rust
let a = &mut x;
let b = &mut x;
```

Multiple simultaneous mutable references are not allowed.

Also unsafe:

```rust
let a = &x;
let b = &mut x;
```

because a mutable borrow cannot coexist with an active immutable borrow.

The reason is to prevent situations where one part of the program reads data while another part modifies it.

---

## 20. Borrow Lifetimes and Usage

Rust's borrow checking is usage-aware.

For example, this can compile:

```rust
let b = &a;
println!("{}", b);

let c = &mut a;
```

because the immutable borrow is no longer being used when the mutable borrow begins.

The simplified rule "a borrow lasts until the end of the scope" is therefore not always accurate.

---

## 21. `String` vs `&str`

A string literal:

```rust
"Akshat"
```

has type:

```text
&str
```

A `String` can be created with:

```rust
String::from("Akshat")
```

Conceptually:

```text
"Akshat"
   ↓
String::from(...)
   ↓
owned String
```

### `&str`

`&str` is a borrowed string slice.

It is a non-owning view into string data.

Example:

```rust
let s = "Akshat";
```

`s` does not own the underlying string data.

### `String`

`String` is an owned, growable string.

Example:

```rust
let mut s = String::from("Akshat");
s.push_str(" Chauhan");
```

This works because `s` owns the string data and is mutable.

### Practical distinction

```text
&str
- borrowed
- non-owning
- useful for reading string data
- cannot grow the underlying string

String
- owned
- growable
- can be mutated when declared mut
- manages owned string data
```

`&str` is not a "worse String." It is a different abstraction that is often preferable when ownership is unnecessary.

---

# Key Mental Models From Session 1

## Memory

```text
Value
  ↓
stored somewhere
  ↓
has an address
```

## Pointer

```text
pointer → address → data
```

## Lifetime

```text
object created
    ↓
object exists
    ↓
object's lifetime ends
    ↓
memory becomes available for reuse
```

## Ownership

```text
one owner
    ↓
responsible for data
    ↓
eventual cleanup
```

## Borrowing

```text
owner
  ↓
temporarily lends access
  ↓
borrower
```

## Rust's core safety idea

```text
Ownership
    +
Borrowing
    +
Lifetime rules
    +
Type system
    ↓
Memory safety without a traditional garbage collector
```

---

# Mistakes To Watch

These were the main conceptual mistakes encountered today:

1. `0x1000` is hexadecimal, not octal.
2. A variable name is not the same thing as the value or address associated with it.
3. A pointer stores an address. `*p` dereferences the pointer.
4. Stack memory is not simply "for primitive variables."
5. Heap memory is not simply "for objects."
6. A pointer to a local variable can become dangling after the variable's lifetime ends.
7. Not every Rust compiler error is an ownership problem.
8. `&str` and `String` are different abstractions.
9. Borrowing does not transfer ownership.
10. An immutable borrow and a mutable borrow cannot be active simultaneously.

---

# Session 1 Mastery Check

Before moving forward, you should be able to explain these without memorizing definitions:

- What is a value?
- What is an address?
- What is a pointer?
- What does dereferencing mean?
- What happens to a local variable when a function returns?
- What is a dangling pointer?
- What is a memory leak?
- Why does heap data need lifetime management?
- What is ownership?
- What is borrowing?
- Difference between `&str` and `String`
- Difference between `Copy` and `Move`
- Why Rust prevents simultaneous mutable and immutable access
