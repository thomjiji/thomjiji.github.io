---
title: "Unsafe Rust (code block style test)"
date: 2023-09-09
draft: false
---

You can take five actions in unsafe Rust that you can't in safe Rust, which we call
*unsafe superpowers*.

- Dereference a raw pointer
- Call an unsafe function or method
- Access or modify a mutable static variable
- Implement an unsafe trait
- Access fields of `union`s.

`unsafe` doesn't turn off the borrow checker or disable any other of Rust's safety
checks: if you use a reference in unsafe code, it will still be checked. The `unsafe`
keyword only gives you access to these five features that are then not checked by the
compiler for memory safety.

In addition, `unsafe` does not mean the code inside the block is necessarily dangerous
or that it will definitely have memory safety problems.

### Dereferencing a Raw Pointer

Similar to references. Raw pointers can be immutable or mutable, written as `*const T`
and `*mut T`. The asterisk isn't the dereference operator; it's part of the type name.

Unlike references and smart pointer, raw pointers:

- Are allowed to ignore the borrowing rules by having both immutable and mutable
  pointers or multiple mutable pointers to the same location
- Aren’t guaranteed to point to valid memory
- Are allowed to be null
- Don’t implement any automatic cleanup

We can create raw pointers in safe code, but we need to dereference a raw pointer to
access the data in unsafe block:

```rust
    let mut num = 5;

    let r1 = &num as *const i32;
    let r2 = &mut num as *mut i32;

    unsafe {
        println!("r1 is: {}", *r1);
        println!("r2 is: {}", *r2);
    }
```

Creating a pointer does no harm; it’s only when we try to access the value that it
points at that we might end up dealing with an invalid value.

With raw pointers, we can create a mutable pointer and an immutable pointer to the same
location and **change data through the mutable pointer**, potentially creating a data
race.

### Accessing or Modifying a Mutable Static Variable

*Global variables*, in Rust, it's *static variables*.

```rust
static HELLO_WORLD: &str = "HELLO WORLD";

fn main() {
    println!("Name is {}", HELLO_WORLD);
}
```

Static variables can only store references with the `'static` lifetime, which means the
Rust compiler can figure out the lifetime and we aren't required to annotate it
explicitly. 

Access immutable static variable is *safe*. Accessing and modifying mutable static
variables is *unsafe*.

Differences between immutable *static variables* and *constants*:

- Static variable have a fixed address in memory. Using the value will always access the
  same data.
- Constants, are allowed to duplicate their data whenever they're used.
- **Static variables can be mutable**. Accessing and modifying mutable static variables
  is *unsafe*.

Any code that reads or writes from static variables must be within an `unsafe` block.

A `union` is similar to a `struct`, but only one declared field is used in a particular
instance at one time. It's primarily used to interface with unions in C code.