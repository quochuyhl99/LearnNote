This is a content of learncpp.com

# Chapter 1: C++ Basics

## 1.4 — Variable assignment and initialization

### Variable assignment

### Initialization

### Default initialization

### Copy initialization

### Direct initialization

### List initialization

### Value initialization and zero initialization

### Initialize your variables

### Unused initialized variables and `[[maybe_unused]]`

## 1.5 — Introduction to iostream: cout, cin, and endl

### The input/output library

### std::cout

### std::endl

### std::cout is buffered

### std::endl vs ‘\n’

### std::cin

## Uninitialized variables and undefined behavior

### Uninitialized variables

### Undefined behavior

### Implementation-defined behavior and unspecified behavior

## 1.9 — Introduction to literals and operators

### Literals

### Operators

### Chaining operators

### Return values and side effects

## 1.10 — Introduction to expressions

### Expressions

### Expression statements

### Useless expression statements

# Chapter 2: C++ Basics: Functions and Files

## 2.4 — Introduction to function parameters and arguments

### Function parameters and arguments

### How parameters and arguments work together

-   pass by value

### Unreferenced parameters

-   A parameter without a name is called an unnamed parameter
-   When a function parameter exists but is not used in the body of the function, do not give it a name. You can optionally put a name inside a comment.

## 2.5 — Introduction to local scope

### Local variables

-   Variables defined inside the body of a function are called local variables

### Local variable lifetime

-   Function parameters are created and initialized when the function is entered, and variables within the function body are created and initialized at the point of definition statement is executed.
-   Function parameters and Local variables are destroyed at the end of the function it in

### Local scope

-   A local variable’s scope begins at the point of variable definition, and stops at the end of the set of curly braces in which it is defined (or for function parameters, at the end of the function).
-   Local variables defined in one function are also not in scope in other functions that are called.

## 2.7 — Forward declarations and definitions

-   the compiler compiles the contents of code files sequentially from top to bottom

To fix this problem:

-   Option 1: Reorder the function definitions
-   Option 2: Use a forward declaration. It is worth noting that function declarations do not need to specify the names of the parameters (as they are not considered to be part of the function declaration)
    ```cpp
    int add(int, int); // valid function declaration
    ```

### Forgetting the function body

-   Scenario 1: if don't call function, it's ok
-   Scenario 1: if call function, compiler is OK but linker is NG

### Declarations vs. definitions

-   A declaration is a statement that tells the compiler about the existence of an identifier and its associated type information
-   A definition tells the compiler how an identifier is implemented (for functions or types) or where that identifier should be instantiated (for variables)

## 2.8 — Programs with multiple code files

-   Needed Forward declarations so main.cpp knows that add() is a function defined elsewhere
-   Whenever you create a new code (.cpp) file, you will need to add it to your project so that it gets compiled.

## 2.9 — Naming collisions and an introduction to namespaces

-   If the colliding identifiers are introduced into the same file, the result will be a compiler error. If the colliding identifiers are introduced into separate files belonging to the same program, the result will be a linker error.

### What is a namespace?

-   A name declared in a namespace won’t be mistaken for an identical name declared in another scope.

### The global namespace

-   In C++, any name that is not defined inside a class, function, or a namespace is considered to be part of an implicitly defined namespace called the global namespace (sometimes also called the global scope).

### The std namespace

When you use an identifier that is defined inside a namespace (such as the std namespace), you have to tell the compiler that the identifier lives inside the namespace.

-   Explicit namespace qualifier std:: (The :: symbol is an operator called the scope resolution operator)
-   Using namespace std (and why to avoid it): When using a using-directive in this manner, any identifier we define may conflict with any identically named identifier in the std namespace. Even worse, while an identifier name may not conflict today, it may conflict with new identifiers added to the std namespace in future language revisions. This was the whole point of moving all of the identifiers in the standard library into the std namespace in the first place!

## 2.10 — Introduction to the preprocessor

-   the preprocessor does have one very important role: it is what processes `#include` directives
-   When the preprocessor has finished processing a code file, the result is called a translation unit. This translation unit is what is then compiled by the compiler.

### Preprocessor directives

-   **Preprocessor directives** (often just called directives) are instructions that start with a # symbol and end with a newline (NOT a semicolon).

### #Include

-   When you #include a file, the preprocessor replaces (copy and paste) the #include directive with the contents of the included file

### Macro #define

-   The `#define` directive can be used to create a macro. In C++, a macro is a rule that defines how input text is converted into replacement output text.
-   There are two basic types of macros: object-like macros, and function-like macros (avoid use).
-   Object-like macros can be defined in one of two ways:
    ```cpp
    #define identifier
    #define identifier substitution_text
    ```

### Object-like macros with substitution text

-   When the preprocessor encounters this directive, any further occurrence of the identifier is replaced by substitution_text.
-   Recommend to use `const` instead of this

### Object-like macros without substitution text

-   Any further occurrence of the identifier is removed and replaced by nothing!
-   macros of this form are generally considered acceptable to use.

### Conditional compilation

-   The **conditional compilation** preprocessor directives allow you to specify under what conditions something will or won’t compile
-   3 most common: `#ifdef`, `#ifndef`, and `#endif`

### #if 0

-   using `#if 0` to exclude a block of code from being compiled
-   This provides a convenient way to “comment out” code that contains multi-line comments
-   Change to `#if 1` to temporarily re-enable code

### Object-like macros don’t affect other preprocessor directives

-   Macros only cause text substitution for normal code. Other preprocessor commands are ignored

```cpp
#define FOO 9 // Here's a macro substitution

#ifdef FOO // This FOO does not get replaced because it’s part of another preprocessor directive
    std::cout << FOO << '\n'; // This FOO gets replaced with 9 because it's part of the normal code
#endif
```

### The scope of #defines

-   it doesn't care about c++ syntax, so can define anywhere in file
-   Once the preprocessor has finished, all defined identifiers from that file are discarded. This means that directives are only valid from the point of definition to the end of the file in which they are defined
-   Directives defined in one code file do not have impact on other code files in the same project.

## 2.11 — Header files

### Headers, and their purpose

-   Header files allow us to put declarations in one location and then import them wherever we need them. This can save a lot of typing in multi-file programs.

### Using standard library header files

-   When you `#include` a file, the content of the included file is inserted at the point of inclusion. This provides a useful way to pull in declarations from another file.

### How including definitions in a header file results in a violation of the one-definition rule

-   avoid putting function or variable definitions in header files

### Source files should include their paired header

-   This allows the compiler to catch certain kinds of errors at compile time instead of link time.

Example:

something.h:

```c++
int something(int); // return type of forward declaration is int
```

something.cpp:

```cpp
#include "something.h" // Best practice, should include header in definition file

void something(int) // error: wrong return type
{
}
```

Because something.cpp #includes something.h, the compiler will notice that function something() has a mismatched return type and give us a compile error

### Do not #include .cpp files

### Angled brackets vs double quotes

-   Use double quotes to include header files that you’ve written or are expected to be found in the current directory.
-   Use angled brackets to include headers that come with your compiler, OS, or third-party libraries you’ve installed elsewhere on your system.

### Why doesn’t iostream have a .h extension?

### Including header files from other directories

### Headers may include other headers

-   Each file should explicitly #include all of the header files it needs to compile. Do not rely on headers included transitively from other headers.

### The #include order of header files

### Header file best practices

## 2.12 — Header guards

### The duplicate definition problem

### Header guards

```cpp
#ifndef SOME_UNIQUE_NAME_HERE
#define SOME_UNIQUE_NAME_HERE

// your declarations (and certain types of definitions) here

#endif
```

### Header guards do not prevent a header from being included once into different code files

### Can’t we just avoid definitions in header files?

### #pragma once

it do the same as header guard, but recommend to stick with traditional header guard

# Chapter 4: Fundamental Data Types

## 4.1 — Introduction to fundamental data types

### Bits, bytes, and memory addressing

### Fundamental data types

| Types                                                                      | Category             | Meaning                                          | Example |
| -------------------------------------------------------------------------- | -------------------- | ------------------------------------------------ | ------- |
| float<br>double<br>long double                                             | Floating Point       | a number with a fractional part                  | 3.14159 |
| bool                                                                       | Integral (Boolean)   | true or false                                    | true    |
| char<br>wchar_t<br>char8_t (C++20)<br>char16_t (C++11)<br>char32_t (C++11) | Integral (Character) | a single character of text                       | ‘c’     |
| short int<br>int<br>long int<br>long long int (C++11)                      | Integral (Integer)   | positive and negative whole numbers, including 0 | 64      |
| std::nullptr_t (C++11)                                                     | Null Pointer         | a null pointer                                   | nullptr |
| void                                                                       | Void                 | no type                                          | n/a     |

### The \_t suffix

## 4.2 — Void

## 4.3 — Object sizes and the sizeof operator

### Object sizes

### Fundamental data type sizes

| Category       | Type           | Minimum Size | Typical Size       | Note                  |
| -------------- | -------------- | ------------ | ------------------ | --------------------- |
| Boolean        | bool           | 1 byte       | 1 byte             |                       |
| character      | char           | 1 byte       | 1 byte             | always exactly 1 byte |
|                | wchar_t        | 1 byte       | 2 or 4 bytes       |                       |
|                | char8_t        | 1 byte       | 1 byte             |                       |
|                | char16_t       | 2 bytes      | 2 bytes            |                       |
|                | char32_t       | 4 bytes      | 4 bytes            |                       |
| integer        | short          | 2 bytes      | 2 bytes            |                       |
|                | int            | 2 bytes      | 4 bytes            |                       |
|                | long           | 4 bytes      | 4 or 8 bytes       |                       |
|                | long long      | 8 bytes      | 8 bytes            |                       |
| floating point | float          | 4 bytes      | 4 bytes            |                       |
|                | double         | 8 bytes      | 8 bytes            |                       |
|                | long double    | 8 bytes      | 8, 12, or 16 bytes |                       |
| pointer        | std::nullptr_t | 4 bytes      | 4 or 8 bytes       |                       |

### The sizeof operator

The `sizeof` operator is a unary operator that takes either a type or a variable, and returns its size in bytes.

## 4.4 — Signed integers

## 4.5 — Unsigned integers, and why to avoid them

## 4.6 — Fixed-width integers and size_t

### Fixed-width integers

They can be accessed by including the `<cstdint>` header
| Name | Type | Range |
| ------------- | --------------- | ------------------------------------------------------- |
| std::int8_t | 1 byte signed | -128 to 127 |
| std::uint8_t | 1 byte unsigned | 0 to 255 |
| std::int16_t | 2 byte signed | -32,768 to 32,767 |
| std::uint16_t | 2 byte unsigned | 0 to 65,535 |
| std::int32_t | 4 byte signed | -2,147,483,648 to 2,147,483,647 |
| std::uint32_t | 4 byte unsigned | 0 to 4,294,967,295 |
| std::int64_t | 8 byte signed | -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807 |
| std::uint64_t | 8 byte unsigned | 0 to 18,446,744,073,709,551,615 |

### Fast and least integers

### What is std::size_t?

-   `sizeof` (and many functions that return a size or length value) return a value of type `std::size_t`. `std::size_t` is defined as an unsigned integral type, and it is typically used to represent the size or length of objects.

## 4.7 — Introduction to scientific notation

## 4.8 — Floating point numbers

Read on the webpage for more info

### Floating point precision

-   When outputting floating point numbers, std::cout has a default precision of 6 -- that is, it assumes all floating point variables are only significant to 6 digits (the minimum precision of a float), and hence it will truncate anything after that.
-   Float values have between 6 and 9 digits of precision, with most float values having at least 7 significant digits. Double values have between 15 and 18 digits of precision, with most double values having at least 16 significant digits. Long double has a minimum precision of 15, 18, or 33 significant digits depending on how many bytes it occupies.

### NaN and Inf

## 4.9 — Boolean values

### Printing Boolean values

-   When we print Boolean values, std::cout prints 0 for false, and 1 for true
-   If you want std::cout to print “true” or “false” instead of 0 or 1, you can use `std::boolalpha`. Here’s an example:

```cpp
#include <iostream>

int main()
{
    std::cout << true << '\n';
    std::cout << false << '\n';

    std::cout << std::boolalpha; // print bools as true or false

    std::cout << true << '\n';
    std::cout << false << '\n';
}
```

### Integer to Boolean conversion

-   You can’t initialize a Boolean with an integer using uniform initialization
-   However, in any context where an integer can be converted to a Boolean, the integer 0 is converted to false, and any other integer is converted to true.

### Inputting Boolean values

-   To allow std::cin to accept “false” and “true” as inputs, the `std::boolalpha` option has to be enabled

## 4.10 — Introduction to if statements

## 4.11 — Chars
