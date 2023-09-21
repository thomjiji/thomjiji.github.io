---
title: "C++ Preprocessor"
date: 2023-09-21
draft: false
---

## preprocessor

- .cpp file => preprocessor:
    - Make change to the text of the code file, but not actually to the original code file, rather temporarily in-memory
      or using temporary files.
    - Uninteresting things: strip out comments, ensures each code file ends in a newline.
    - One important role: **process`#include` directives**.
- => What comes out of the preprocessor is called **translation unit**.
- The translation unit is then compiled by the compiler.

```
+--------+      +---------------------+        +-------------------+       +-----------------+
| .cpp   |  =>  | Preprocessor        |   =>   | Translation  Unit |  =>   | Compiled output |
| file   |      |                     |        |                   |       | (object code)   |
+--------+      +---------------------+        +-------------------+       +-----------------+
   |          /----> |#include files  | <\                     |                            |
   |         /       |Macro expansion |   \                    |                            |
   |        /        |Remove comments |    \                   |                            |
   |       /         |Add line-breaks |     \                  |                            |
   |      /          |etc.            |      \                 |                            |
   +-- Original file +--Processes the file   +--Output from      +--Binary code resulted 
                         with directives         preprocessor        from compilation
                                                (processed code)                                                           
```

> The entire process of preprocessing, compiling and linking is called **translation**.

## preprocessor directives

Preprocessor directives (often just called directives) are instructions that start with a `#` symbol and end with a
newline (NOT a semicolon).

## `#include`

When you #include a file, the preprocessor replaces the #include directive with the contents of the included file.
The included contents are then preprocessed recursively.

## Macro defines

In C++, a macro is a rule that defines how input text is converted into replacement output text.

- Object-like macros
- Function-like macros

Object-like macros:

```c++
#define identifier
#define identifier substitution_text
```

### Object-like macros (with substitution text)

When the preprocessor encounters this directive, any further occurrence of the identifier is replaced by
substitution_text.

### Object-like macros (without substitution text)

```c++
#define USE_YEN
```

Any further occurrence of the identifier is removed and replaced by **nothing**.

It seems pretty useless, but the common use of it is **below**.

## Conditional compilation

The conditional compilation preprocessor directives allow you to specify under what conditions something will or won’t
compile.

- `#ifdef`
- `ifndef`
- `endif`

```c++
#include <iostream>

#define PRINT_JOE

int main()
{
#ifdef PRINT_JOE
    std::cout << "Joe\n"; // will be compiled since PRINT_JOE is defined
#endif

#ifdef PRINT_BOB
    std::cout << "Bob\n"; // will be excluded since PRINT_BOB is not defined
#endif

    return 0;
}
```

## The scope of `#define`s

Directives are only valid from the point of definition to the end of the file in which they are defined.