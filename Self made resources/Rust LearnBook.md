# Difference b/w Str and String::from()
`String::from()` is a way to **create a `String` from another value**, in our case a string literal.

For example:

```
let s = String::from("Akshat");
```

Here:

```
"Akshat"              → &str
String::from("Akshat") → String
```

### Why do we need this?

`"Akshat"` is a **string literal**. Its type is `&str`, which is essentially a reference to string data.

`String` is an **owned, growable string** that can store its data on the heap.

That's why we used:

```
let mut s = String::from("Akshat");
```

Because we want to later do:

```
s.push_str(" Chauhan");
```

and modify the string.

Think of it as:

```
"Akshat"
   ↓
String::from(...)
   ↓
owned String
   ↓
can grow / mutate
```

Don't worry about the `::` syntax yet. We'll cover it properly when we learn **associated functions and methods**.

---
There isn't a problem with `&str`. It's useful. The difference is **ownership and mutability**.

### `&str`

```
let s = "Akshat";
```

`s` is a **borrowed string slice**.

Think:

```
s ──→ "Akshat"
      ↑
   some existing string data
```

You don't own that string data, so you can't grow or modify it.

```
s.push_str(" Chauhan"); // ❌
```

### `String`

```
let s = String::from("Akshat");
```

Now `s` **owns** the string data.

```
s ──owns──→ "Akshat"
```

So:

```
let mut s = String::from("Akshat");
s.push_str(" Chauhan"); // ✅
```

### The practical distinction

||`&str`|`String`|
|---|---|---|
|Owns data|❌|✅|
|Can grow|❌|✅|
|Can mutate|❌|✅ with `mut`|
|Usually heap-allocated|Not necessarily|Yes|
|Common use|Read-only text|Owned/dynamic text|

One important correction to your mental model: **`&str` doesn't mean "a worse String."** It means a borrowed view into string data. It's often exactly what you want when you only need to read text.

We'll later learn why Rust code frequently prefers `&str` for function parameters.

---
Because Rust deliberately makes the **safe, cheap, non-owning option easy**.

But one correction first: Rust does **not** make `&str` the default string type. A string literal like:

```
"Akshat"
```

has type `&'static str`.

Why?

A literal is known when the program is compiled. There's no reason to dynamically allocate heap memory and create an owned `String` just to read fixed text.

Conceptually:

```
"Akshat"
   ↓
already exists as program data
   ↓
&str points to it
```

Whereas:

```
String::from("Akshat")
```

means:

```
literal
  ↓
allocate owned storage
  ↓
copy the characters
  ↓
String owns that storage
```

So Rust's design avoids unnecessary allocation.

This becomes important in systems programming because **allocation isn't free**. If you only need to read some text, creating an owned heap allocation would often be pointless.

Think of it as:

> **Borrow when you only need access. Own when you need responsibility over the data.**

That's a recurring theme throughout Rust.
