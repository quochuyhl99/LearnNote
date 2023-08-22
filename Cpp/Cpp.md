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

### Initializing chars

```cpp
char ch2{ 'a' }; // initialize with code point for 'a' (stored as integer 97) (preferred)
char ch1{ 97 }; // initialize with integer 97 ('a') (not preferred)
```

### Printing chars

-   When using std::cout to print a char, std::cout outputs the char variable as an ASCII character

### Inputting chars

-   Note that std::cin will let you enter multiple characters. However, variable type char can only hold 1 character. Consequently, only the first input character is extracted. The rest of the user input is left in the input buffer that std::cin uses, and can be extracted with subsequent calls to std::cin

### Escape sequences

-   An escape sequence starts with a ‘\’ (backslash) character, and then a following letter or number.

| Name            | Symbol     | Meaning                                                                          |
| --------------- | ---------- | -------------------------------------------------------------------------------- |
| Alert           | \a         | Makes an alert, such as a beep                                                   |
| Backspace       | \b         | Moves the cursor back one space                                                  |
| Formfeed        | \f         | Moves the cursor to next logical page                                            |
| Newline         | \n         | Moves cursor to next line                                                        |
| Carriage return | \r         | Moves cursor to beginning of line                                                |
| Horizontal tab  | \t         | Prints a horizontal tab                                                          |
| Vertical tab    | \v         | Prints a vertical tab                                                            |
| Single quote    | \’         | Prints a single quote                                                            |
| Double quote    | \”         | Prints a double quote                                                            |
| Backslash       | \\         | Prints a backslash.                                                              |
| Question mark   | \?         | Prints a question mark.No longer relevant. You can use question marks unescaped. |
| Octal number    | \\(number) | Translates into char represented by octal                                        |
| Hex number      | \x(number) | Translates into char represented by hex number                                   |

### What about the other char types, wchar_t, char16_t, and char32_t?

## 4.12 — Introduction to type conversion and static_cast

### Implicit type conversion

### Type conversion produces a new value

-   a type conversion does not actually change the value or type of the value being converted. Instead, the value to be converted is used as input, and the conversion results in a new value of the target type.

### Implicit type conversion warnings

-   Brace initialization will ensure we don’t try to initialize a variable with an initializer that will lose value when it is implicitly type converted:

```cpp
int main()
{
    double d { 5 }; // okay: int to double is safe
    int x { 5.5 }; // error: double to int not safe

    return 0;
}
```

### An introduction to explicit type conversion via the static_cast operator

-   When we pass in a variable, that variable is evaluated to produce its value, and that value is then converted to the new type. The variable itself is not affected by casting its value to a new type

```cpp
static_cast<new_type>(expression)
```

### Converting unsigned numbers to signed numbers

-   The static_cast operator will produce undefined behavior if the value being converted doesn’t fit in range of the new type.

### std::int8_t and std::uint8_t likely behave like chars instead of integers

-   Because `std::int8_t` describes itself as an int, you might be tricked into believing that is int, but on most system, it's char
-   In cases where std::int8_t is treated as a char, input from the console can also cause problems

## 4.13 — Const variables and symbolic constants

### Const variables

-   A variable whose value can not be changed is called a constant variable.

### The const keyword

-   Place `const` before the type (because it is more conventional to do so).

### Const variables must be initialized

-   Const variables must be initialized when you define them, and then that value can not be changed via assignment

### Naming your const variables

### Const function parameters

-   Don’t use const when passing by value.

### Const return values

-   Don’t use const when returning by value.

### For symbolic constants, prefer constant variables over object-like macros

### Using constant variables throughout a multi-file program

## 4.14 — Compile-time constants, constant expressions, and constexpr

### Constant expressions

-   A constant expression is an expression that can be evaluated by the compiler at compile-time.
-   Evaluating constant expressions at compile-time makes our compilation take longer (because the compiler has to do more work), but such expressions only need to be evaluated once (rather than every time the program is run). The resulting executables are faster and use less memory.

### Compile-time constants

-   A Compile-time constant is a constant whose value is known at compile-time. Literals (e.g. ‘1’, ‘2.3’, and “Hello, world!”) are one type of compile-time constant.

### Compile-time const and Runtime const

```cpp
#include <iostream>

int getNumber()
{
    std::cout << "Enter a number: ";
    int y{};
    std::cin >> y;

    return y;
}

int main()
{
    const int x{ 3 };           // x is a compile time constant

    const int w{ 4 };           // w is a compile time constant

    const int y{ getNumber() }; // y is a runtime constant

    const int z{ x + y };       // x + y is a runtime expression
    std::cout << z << '\n';     // this is also a runtime expression

    const int t{ x + w };       // x + w is a compile time expression

    return 0;
}
```

### The `constexpr` keyword

-   A `constexpr` (which is short for “constant expression”) variable can only be a compile-time constant. If the initialization value of a constexpr variable is not a constant expression, the compiler will error.

-   Any variable that should not be modifiable after initialization and whose initializer is known at compile-time should be declared as constexpr.
-   Any variable that should not be modifiable after initialization and whose initializer is not known at compile-time should be declared as const.

-   Caveat: In the future we will discuss some types that are not currently compatible with constexpr (including std::string, std::vector, and other types that use dynamic memory allocation). For constant objects of these types, use const instead.

### Const and constexpr function parameters

-   function parameters cannot be declared as `constexpr`

### Constant folding for constant subexpressions

## 4.15 — Literals

-   Literals are values that are inserted directly into the code

### The type of a literal

| Literal value        | Examples        | Default literal type | Note                                      |
| -------------------- | --------------- | -------------------- | ----------------------------------------- |
| integer value        | 5, 0, -3        | int                  |
| boolean value        | true, false     | bool                 |
| floating point value | 1.2, 0.0, 3.4   | double (not float!)  |
| character            | ‘a’, ‘\n’       | char                 |
| C-style string       | “Hello, world!” | const char[14]       | see C-style string literals section below |

### Literal suffixes

-   If the default type of a literal is not as desired, you can change the type of a literal by adding a suffix:
    | Data type | Suffix | Meaning |
    | -------------- | -------------------------------------- | ----------------------------------------- |
    | integral | u or U | unsigned int |
    | integral | l or L | long |
    | integral | ul, uL, Ul, UL, lu, lU, Lu, LU | unsigned long |
    | integral | ll or LL | long long |
    | integral | ull, uLL, Ull, ULL, llu, llU, LLu, LLU | unsigned long long |
    | integral | z or Z | The signed version of std::size_t (C++23) |
    | integral | uz, uZ, Uz, UZ, zu, zU, Zu, ZU | std::size_t (C++23) |
    | floating point | f or F | float |
    | floating point | l or L | long double |
    | string | s | std::string |
    | string | sv | std::string_view |

-   Prefer literal suffix L (upper case) over l (lower case).

### Integral literals

### Floating point literals

-   By default, floating point literals have a type of `double`. To make them `float` literals instead, the `f` (or `F`) suffix should be used:

### Scientific notation for floating point literals

```cpp
double pi { 3.14159 }; // 3.14159 is a double literal in standard notation
double avogadro { 6.02e23 }; // 6.02 x 10^23 is a double literal in scientific notation
double electronCharge { 1.6e-19 }; // charge on an electron is 1.6 x 10^-19
```

### C-style string literals

-   All C-style string literals have an implicit null terminator. This trailing `\0` character is a special character called a null terminator, and it is used to indicate the end of the string.
-   Unlike most other literals (which are values, not objects), C-style string literals are const objects that are created at the start of the program and are guaranteed to exist for the entirety of the program. This fact will become important in a few lessons, when we discuss `std::string_view`.

### Magic numbers

-   Avoid magic numbers in your code (use constexpr variables instead).

## 4.16 — Numeral systems (decimal, binary, hexadecimal, and octal)

### Octal and hexadecimal literals

-   Octal is base 8. To use an octal literal, prefix your literal with a 0 (zero):

```cpp
#include <iostream>

int main()
{
    int x{ 012 }; // 0 before the number means this is octal
    std::cout << x << '\n';
    return 0;
}
```

This program prints: `10`

-   Hexadecimal is base 16. To use a hexadecimal literal, prefix your literal with 0x:

```cpp
#include <iostream>

int main()
{
    int x{ 0xF }; // 0x before the number means this is hexadecimal
    std::cout << x << '\n';
    return 0;
}
```

This program prints: `15`

### Binary literals

-   Can use `0x` or `0b`. See the web page for example

### Digit separators

-   Because long literals can be hard to read, C++14 also adds the ability to use a quotation mark (‘) as a digit separator.
-   Also note that the separator can not occur before the first digit of the value.

```cpp
#include <iostream>

int main()
{
    int bin { 0b1011'0010 };  // assign binary 1011 0010 to the variable
    long value { 2'132'673'462 }; // much easier to read than 2132673462
    int bin { 0b'1011'0010 };  // error: ' used before first digit of value
    return 0;
}
```

### Outputting values in decimal, octal, or hexadecimal

```cpp
#include <iostream>

int main()
{
    int x { 12 };
    std::cout << x << '\n'; // decimal (by default)
    std::cout << std::hex << x << '\n'; // hexadecimal
    std::cout << x << '\n'; // now hexadecimal
    std::cout << std::oct << x << '\n'; // octal
    std::cout << std::dec << x << '\n'; // return to decimal
    std::cout << x << '\n'; // decimal

    return 0;
}
```

### Outputting values in binary

-   See the web page for example

## 4.17 — Introduction to std::string

### Introducing std::string

-   the `std::string` type, which lives in the \<string\> header.

### `std::string` can handle strings of different lengths

### String input with `std::cin`

-   Using `std::string` with `std::cin` may yield some surprises (unexpected behavior)! See the web page for example and explanation

### Use `std::getline()` to input text

```cpp
#include <iostream>
#include <string> // For std::string and std::getline

int main()
{
    std::cout << "Enter your full name: ";
    std::string name{};
    std::getline(std::cin >> std::ws, name); // read a full line of text into name

    std::cout << "Enter your favorite color: ";
    std::string color{};
    std::getline(std::cin >> std::ws, color); // read a full line of text into color

    std::cout << "Your name is " << name << " and your favorite color is " << color << '\n';

    return 0;
}
```

### What the heck is std::ws?

-   See the web page for example and explanation
-   If using `std::getline()` to read strings, use `std::cin >> std::ws` input manipulator to ignore leading whitespace.

-   Using the extraction operator (>>) with std::cin ignores leading whitespace.
    std::getline() does not ignore leading whitespace unless you use input manipulator std::ws

### The length of a `std::string`

-   use `std::string::length()` to get length of string
-   `std::string` doesn't contain implicit null-terminator character
-   `std::string::length()` returns an unsigned integral value (most likely of type `size_t`). If you want to assign the length to an int variable, you should `static_cast` it to avoid compiler warnings about signed/unsigned conversions:
    ```cpp
    int length { static_cast<int>(name.length()) };
    ```

### `std::string` can be expensive to initialize and copy

-   Do not pass `std::string` by value, as making copies of `std::string` is expensive. Prefer `std::string_view` parameters.

### Literals for `std::string`

-   Double-quoted string literals (like “Hello, world!”) are C-style strings by default (and thus, have a strange type).

-   We can create string literals with type std::string by using a s suffix after the double-quoted string literal.

```cpp
#include <iostream>
#include <string> // for std::string

int main()
{
    using namespace std::string_literals; // easy access to the s suffix

    std::cout << "foo\n";   // no suffix is a C-style string literal
    std::cout << "goo\n"s;  // s suffix is a std::string literal

    return 0;
}
```

### Constexpr strings

-   If you try to define a constexpr std::string, your compiler will probably generate an error

## 4.18 — Introduction to std::string_view

-   `std::string_view` lives in the \<string_view\> header), provides **read-only** access to an existing string (a C-style string, a std::string, or another std::string_view) without making a copy. **Read-only** means that we can access and use the value being viewed, but we can not modify it.

    ```cpp
    #include <iostream>
    #include <string_view>

    // str provides read-only access to whatever argument is passed in
    void printSV(std::string_view str) // now a std::string_view
    {
        std::cout << str << '\n';
    }

    int main()
    {
        std::string_view s{ "Hello, world!" }; // now a std::string_view
        printSV(s);

        return 0;
    }
    ```

-   Prefer `std::string_view` over `std::string` when you need a read-only string, especially for function parameters.

### `std::string_view` can be initialized with many different types of strings

### `std::string_view` parameters will accept many different types of string arguments

### `std::string_view` will not implicitly convert to `std::string`

However, if this is desired, we have two options:

1. Explicitly create a std::string with a std::string_view initializer (which is allowed, since this will rarely be done unintentionally)
2. Convert an existing std::string_view to a std::string using static_cast

### Assignment changes what the `std::string_view` is viewing

-   Assigning a new string to a `std::string_view` causes the `std::string_view` to view the new string. It does not modify the prior string being viewed in any way.

### Literals for `std::string_view`

```cpp
#include <iostream>
#include <string>      // for std::string
#include <string_view> // for std::string_view

int main()
{
    using namespace std::string_literals;      // access the s suffix
    using namespace std::string_view_literals; // access the sv suffix

    std::cout << "foo\n";   // no suffix is a C-style string literal
    std::cout << "goo\n"s;  // s suffix is a std::string literal
    std::cout << "moo\n"sv; // sv suffix is a std::string_view literal

    return 0;
};
```

### constexpr std::string_view

Unlike std::string, std::string_view has full support for constexpr:

```cpp
#include <iostream>
#include <string_view>

int main()
{
    constexpr std::string_view s{ "Hello, world!" }; // s is a string symbolic constant
    std::cout << s << '\n'; // s will be replaced with "Hello, world!" at compile-time

    return 0;
}
```

## 4.19 — std::string_view (part 2)

### A quick guide on when to use `std::string` vs `std::string_view`

This guide is not meant to be comprehensive, but is intended to highlight the most common cases:

Use a std::string variable when:

-   You need a string that you can modify.
-   You need to store user-inputted text.
-   You need to store the return value of a function that returns a std::string.

Use a std::string_view variable when:

-   You need read-only access to part or all of a string that already exists elsewhere and will not be modified or destroyed before use of the std::string_view is complete.
-   You need a symbolic constant for a C-style string.
-   You need to continue viewing the return value of a function that returns a C-style string or a non-dangling std::string_view.

Use a std::string function parameter when:

-   The function needs to modify the string passed in as an argument without affecting the caller. This is rare.
-   You are using a language standard older than C++17.
-   You meet the criteria of the cases covered in lesson 9.5 -- Pass by lvalue reference.

Use a std::string_view function parameter when:

-   The function needs a read-only string.

Use a std::string return type when:

-   The function returns a std::string local variable.

Use a std::string_view return type when:

-   The function returns a C-style string literal.
-   The function returns a std::string_view parameter.

Things to remember about std::string:

-   Initializing and copying std::string is expensive, so avoid this as much as possible.
-   Avoid passing std::string by value, as this makes a copy.
-   If possible, avoid creating short-lived std::string objects.

Things to remember about std::string_view:

-   Because C-style string literals exist for the entire program, it is always okay to set a std::string_view to a C-style string literal.
-   When a string is destroyed, all views to that string are invalidated.
-   Modifying a std::string will invalidate any views to that string.
-   Using an invalidated view (other than using assignment to revalidate the view) will cause undefined behavior.

# Chapter 5: Operators

## 5.1 — Operator precedence and associativity

### Compound expressions

-   An expression that has multiple operators is called a compound expression.

### Operator precedence

-   All operators are assigned a level of precedence. Operators with a higher precedence level are grouped with operands first.

### Operator associativity

-   The operator’s associativity tells the compiler whether to evaluate the operators from left to right or from right to left

### Table of operator precedence and associativity

-   See the webpage for detail

### Parenthesization

### Use parenthesis to make compound expressions easier to understand

### Value computation (of operations)

### Order of evaluation (of operands)

## 5.2 — Arithmetic operators

### Unary arithmetic operators

| Operator    | Symbol | Form | Operation     |
| ----------- | ------ | ---- | ------------- |
| Unary plus  | \+     | \+x  | Value of x    |
| Unary minus | \-     | \-x  | Negation of x |

### Binary arithmetic operators

| Operator       | Symbol | Form   | Operation                       |
| -------------- | ------ | ------ | ------------------------------- |
| Addition       | \+     | x \+ y | x plus y                        |
| Subtraction    | \-     | x \- y | x minus y                       |
| Multiplication | \*     | x \* y | x multiplied by y               |
| Division       | /      | x / y  | x divided by y                  |
| Remainder      | %      | x % y  | The remainder of x divided by y |

### Integer and floating point division

-   `Floating point division` returns a floating point value, and the fraction is kept
-   `Integer division` drops any fractions and returns an integer value

### Using static_cast<> to do floating point division with integers

### Arithmetic assignment operators

| Operator                  | Symbol | Form    | Operation                       |
| ------------------------- | ------ | ------- | ------------------------------- |
| Assignment                | =      | x = y   | Assign value y to x             |
| Addition assignment       | \+=    | x \+= y | Add y to x                      |
| Subtraction assignment    | \-=    | x \-= y | Subtract y from x               |
| Multiplication assignment | \*=    | x \*= y | Multiply x by y                 |
| Division assignment       | /=     | x /= y  | Divide x by y                   |
| Remainder assignment      | %=     | x %= y  | Put the remainder of x / y in x |

## 5.3 — Remainder and Exponentiation

### The remainder operator (`operator%`)

### Remainder with negative numbers

### Where’s the exponent operator?

## 5.4 — Increment/decrement operators, and side effects

### Incrementing and decrementing variables

| Operator                           | Symbol | Form | Operation                                      |
| ---------------------------------- | ------ | ---- | ---------------------------------------------- |
| Prefix increment (pre-increment)   | ++     | ++x  | Increment x, then return x                     |
| Prefix decrement (pre-decrement)   | ––     | ––x  | Decrement x, then return x                     |
| Postfix increment (post-increment) | ++     | x++  | Copy x, then increment x, then return the copy |
| Postfix decrement (post-decrement) | ––     | x––  | Copy x, then decrement x, then return the copy |

### Side effects

-   A function or expression is said to have a side effect if it has some observable effect beyond producing a return value.

### Side effects can cause order of evaluation issues

### The sequencing of side effects

-   C++ does not define the order of evaluation for function arguments or the operands of operators.
-   Don’t use a variable that has a side effect applied to it more than once in a given statement. If you do, the result may be undefined.

## 5.5 — Comma and conditional operators

### The comma operator

| Operator | Symbol | Form | Operation                             |
| -------- | ------ | ---- | ------------------------------------- |
| Comma    | ,      | x, y | Evaluate x then y, returns value of y |

-   The comma **operator (,)** allows you to evaluate multiple expressions wherever a single expression is allowed. The comma operator evaluates the left operand, then the right operand, and then returns the result of the right operand.

-   Avoid using the comma operator, except within for loops.

### Comma as a separator

```cpp
void foo(int x, int y) // Comma used to separate parameters in function definition
{
    add(x, y); // Comma used to separate arguments in function call
    constexpr int z{ 3 }, w{ 5 }; // Comma used to separate multiple variables being defined on the same line (don't do this)
}
```

### The conditional operator

| Operator    | Symbol | Form      | Operation                                                    |
| ----------- | ------ | --------- | ------------------------------------------------------------ |
| Conditional | ?:     | c ? x : y | If c is nonzero (true) then evaluate x, otherwise evaluate y |

### Parenthesization of the conditional operator

-   Always parenthesize the conditional part of the conditional operator, and consider parenthesizing the whole thing as well.

### The conditional operator evaluates as an expression

### The type of the expressions must match or be convertible

### So when should you use the conditional operator?

-   It’s most useful when we need a conditional initializer (or assignment) for a variable, or to pass a conditional value to a function.

-   It should not be used for complex if/else statements, as it quickly becomes both unreadable and error prone.

## 5.6 — Relational operators and floating point comparisons

| Operator               | Symbol | Form      | Operation                                                |
| ---------------------- | ------ | --------- | -------------------------------------------------------- |
| Greater than           | &gt;   | x &gt; y  | true if x is greater than y, false otherwise             |
| Less than              | &lt;   | x &lt; y  | true if x is less than y, false otherwise                |
| Greater than or equals | &gt;=  | x &gt;= y | true if x is greater than or equal to y, false otherwise |
| Less than or equals    | &lt;=  | x &lt;= y | true if x is less than or equal to y, false otherwise    |
| Equality               | ==     | x == y    | true if x equals y, false otherwise                      |
| Inequality             | !=     | x != y    | true if x does not equal y, false otherwise              |

### Comparison of calculated floating point values can be problematic

-   Comparing floating point values using any of the relational operators can be dangerous. This is because floating point values are not precise, and small rounding errors in the floating point operands may cause them to be slightly smaller or slightly larger than expected. And this can throw off the relational operators.
-   We discussed rounding errors in lesson 4.8 -- Floating point numbers.

### Floating point less-than and greater-than

-   If the consequence of getting a wrong answer when the operands are similar is acceptable, then using these operators can be acceptable. This is an application-specific decision.

### Floating point equality and inequality

-   Avoid using operator== and operator!= to compare floating point values if there is any chance those values have been calculated.

-   It is okay to compare a low-precision (few significant digits) floating point literal to the same literal value of the same type.

### Comparing floating point numbers (advanced / optional reading)

## 5.7 — Logical operators

| Operator    | Symbol     | Example Usage  | Operation                                       |
| ----------- | ---------- | -------------- | ----------------------------------------------- |
| Logical NOT | \!         | \!x            | true if x is false, or false if x is true       |
| Logical AND | &amp;&amp; | x &amp;&amp; y | true if both x and y are true, false otherwise  |
| Logical OR  | \|\|       | x \|\| y       | true if either x or y are true, false otherwise |

### Short circuit evaluation

-   In order for logical AND to return true, both operands must evaluate to true. If the left operand evaluates to false, logical AND knows it must return false regardless of whether the right operand evaluates to true or false. In this case, the logical AND operator will go ahead and return false immediately without even evaluating the right operand! This is known as short circuit evaluation, and it is done primarily for optimization purposes.

-   Similarly, if the left operand for logical OR is true, then the entire OR condition has to evaluate to true, and the right operand won’t be evaluated.

-   Avoid using expressions with side effects in conjunction with these operators.:

    ```cpp
    if (x == 1 && ++y == 2)
        // do something
    ```

### De Morgan’s laws

-   when you distribute the logical NOT, you also need to flip logical AND to logical OR, and vice-versa!
    `!(x && y)` is equivalent to `!x || !y`
    `!(x || y)` is equivalent to `!x && !y`

### Where’s the logical exclusive or (XOR) operator?

-   Logical XOR is a logical operator provided in some languages that is used to test whether an odd number of conditions is true.

| Left operand | Right operand | Result |
| ------------ | ------------- | ------ |
| false        | false         | false  |
| false        | true          | true   |
| true         | false         | true   |
| true         | true          | false  |

-   C++ doesn’t provide a logical XOR operator (operator^ is a bitwise XOR, not a logical XOR).

### Alternative operator representations

# Chapter 6 - Scope, Duration, and Linkage

## 6.1 — Compound statements (blocks)

### Blocks inside other blocks

### Using blocks to execute multiple statements conditionally

### Block nesting levels

## 6.2 — User-defined namespaces and the scope resolution operator

foo.cpp:

```cpp
// This doSomething() belongs to namespace Foo
int doSomething(int x, int y)
{
    return x + y;
}
```

goo.cpp:

```cpp
// This doSomething() belongs to namespace Goo
int doSomething(int x, int y)
{
    return x - y;
}
```

main.cpp:

```cpp
int doSomething(int x, int y); // forward declaration for doSomething

int main()
{
    std::cout << doSomething(4, 3) << '\n'; // which doSomething will we get?
    return 0;
}
```

Result error: Multi definition

```
goo.cpp:3: multiple definition of `doSomething(int, int)'; foo.cpp:3: first defined here
```

### Defining your own namespaces

-   define our own namespaces via the `namespace` keyword

foo.cpp:

```cpp
namespace Foo // define a namespace named Foo
{
    // This doSomething() belongs to namespace Foo
    int doSomething(int x, int y)
    {
        return x + y;
    }
}
```

goo.cpp:

```cpp
namespace Goo // define a namespace named Goo
{
    // This doSomething() belongs to namespace Goo
    int doSomething(int x, int y)
    {
        return x - y;
    }
}
```

main.cpp:

```cpp
int doSomething(int x, int y); // forward declaration for doSomething

int main()
{
    std::cout << doSomething(4, 3) << '\n'; // which doSomething will we get?
    return 0;
}
```

Result error: Linker can't link

```
ConsoleApplication1.obj : error LNK2019: unresolved external symbol "int __cdecl doSomething(int,int)" (?doSomething@@YAHHH@Z) referenced in function _main
```

### Accessing a namespace with the scope resolution operator (::)

```cpp
#include <iostream>

namespace Foo // define a namespace named Foo
{
    // This doSomething() belongs to namespace Foo
    int doSomething(int x, int y)
    {
        return x + y;
    }
}

namespace Goo // define a namespace named Goo
{
    // This doSomething() belongs to namespace Goo
    int doSomething(int x, int y)
    {
        return x - y;
    }
}

int main()
{
    std::cout << Foo::doSomething(4, 3) << '\n'; // use the doSomething() that exists in namespace Foo
    std::cout << Goo::doSomething(4, 3) << '\n'; // use the doSomething() that exists in namespace Goo
    return 0;
}
```

### Using the scope resolution operator with no name prefix

-   it's global namespace

```cpp
#include <iostream>

void print() // this print() lives in the global namespace
{
	std::cout << " there\n";
}

namespace Foo
{
	void print() // this print() lives in the Foo namespace
	{
		std::cout << "Hello";
	}
}

int main()
{
	Foo::print(); // call print() in Foo namespace
	::print();    // call print() in global namespace (same as just calling print() in this case)

	return 0;
}
```

### Identifier resolution from within a namespace

-   If an identifier inside a namespace is used and no scope resolution is provided, the compiler will first try to find a matching declaration in that same namespace. If no matching identifier is found, the compiler will then check each containing namespace in sequence to see if a match is found, with the global namespace being checked last.

### Multiple namespace blocks are allowed

-   It’s legal to declare namespace blocks in multiple locations (either across multiple files, or multiple places within the same file). All declarations within the namespace are considered part of the namespace.

-   Do not add custom functionality to the std namespace.

-   When you separate your code into multiple files, you’ll have to use a namespace in the header and source file.

    add.h:

    ```cpp
    #ifndef ADD_H
    #define ADD_H

    namespace BasicMath
    {
        // function add() is part of namespace BasicMath
        int add(int x, int y);
    }

    #endif
    ```

    add.cpp:

    ```cpp
    #include "add.h"

    namespace BasicMath
    {
        // define the function add()
        int add(int x, int y)
        {
            return x + y;
        }
    }
    ```

    If the namespace is omitted in the source file, the linker won’t find a definition of BasicMath::add, because the source file only defines add (global namespace). If the namespace is omitted in the header file, “main.cpp” won’t be able to use BasicMath::add, because it only sees a declaration for add (global namespace).

### Nested namespaces

```cpp
#include <iostream>

namespace Foo
{
    namespace Goo // Goo is a namespace inside the Foo namespace
    {
        int add(int x, int y)
        {
            return x + y;
        }
    }
}

int main()
{
    std::cout << Foo::Goo::add(1, 2) << '\n';
    return 0;
}
```

Since C++17, nested namespaces can also be declared this way:

```cpp
#include <iostream>

namespace Foo::Goo // Goo is a namespace inside the Foo namespace (C++17 style)
{
  int add(int x, int y)
  {
    return x + y;
  }
}

int main()
{
    std::cout << Foo::Goo::add(1, 2) << '\n';
    return 0;
}
```

### Namespace aliases

-   create namespace aliases, which allow us to temporarily shorten a long sequence of namespaces into something shorter:

```cpp
#include <iostream>

namespace Foo::Goo
{
    int add(int x, int y)
    {
        return x + y;
    }
}

int main()
{
    namespace Active = Foo::Goo; // active now refers to Foo::Goo

    std::cout << Active::add(1, 2) << '\n'; // This is really Foo::Goo::add()

    return 0;
} // The Active alias ends here
```

## 6.3 — Local variables

### Local variables have block scope

-   Local variables have block scope, which means they are in scope from their point of definition to the end of the block they are defined within.

-   Although function parameters are not defined inside the function body, for typical functions they can be considered to be part of the scope of the function body block.

### All variable names within a scope must be unique

### Local variables have automatic storage duration (lifetime)

### Local variables in nested blocks

-   nested blocks are considered part of the scope of the outer block in which they are defined. Consequently, variables defined in the outer block can be seen inside a nested block:

```cpp
#include <iostream>

int main()
{ // outer block

    int x { 5 }; // x enters scope and is created here

    { // nested block
        int y { 7 }; // y enters scope and is created here

        // x and y are both in scope here
        std::cout << x << " + " << y << " = " << x + y << '\n';
    } // y goes out of scope and is destroyed here

    // y can not be used here because it is out of scope in this block

    return 0;
} // x goes out of scope and is destroyed here
```

### Local variables have no linkage

-   Identifiers have another property named `linkage`. An identifier’s **linkage** determines whether other declarations of that name refer to the same object or not.
-   Local variables have no linkage, which means that each declaration refers to a unique object

```cpp
int main()
{
    int x { 2 }; // local variable, no linkage

    {
        int x { 3 }; // this identifier x refers to a different object than the previous x
    }

    return 0;
}
```

### Variables should be defined in the most limited scope

-   Define variables in the most limited existing scope. Avoid creating new blocks whose only purpose is to limit the scope of variables.

## 6.4 — Introduction to global variables

In C++, variables can also be declared outside of a function. Such variables are called global variables.

### Declaring global variables

By convention, global variables are declared at the top of a file, below the includes, in the global namespace.

```cpp
#include <iostream>

// Variables declared outside of a function are global variables
int g_x {}; // global variable g_x

void doSomething()
{
    // global variables can be seen and used everywhere in the file
    g_x = 3;
    std::cout << g_x << '\n';
}

int main()
{
    doSomething();
    std::cout << g_x << '\n';

    // global variables can be seen and used everywhere in the file
    g_x = 5;
    std::cout << g_x << '\n';

    return 0;
}
// g_x goes out of scope here
```

### The scope of global variables

-   global variables are visible from the point of declaration until the end of the file in which they are declared.
-   Global variables can also be defined inside a user-defined namespace. Variables declared inside a namespace but outside a function are global variables.

```cpp
#include <iostream>

namespace Foo // Foo is defined in the global scope
{
    int g_x {}; // g_x is now inside the Foo namespace, but is still a global variable
}

void doSomething()
{
    // global variables can be seen and used everywhere in the file
    Foo::g_x = 3;
    std::cout << Foo::g_x << '\n';
}

int main()
{
    doSomething();
    std::cout << Foo::g_x << '\n';

    // global variables can be seen and used everywhere in the file
    Foo::g_x = 5;
    std::cout << Foo::g_x << '\n';

    return 0;
}
```

### Global variables have static duration

Global variables are created when the program starts, and destroyed when it ends. This is called static duration. Variables with static duration are sometimes called static variables.

### Naming global variables

Consider using a “g” or “g\_” prefix when naming non-const global variables, to help differentiate them from local variables and function parameters.

### Global variable initialization

Unlike local variables, which are uninitialized by default, variables with static duration are zero-initialized by default.

Non-constant global variables can be optionally initialized:

```cpp
int g_x;         // no explicit initializer (zero-initialized by default)
int g_y {};      // value initialized (resulting in zero-initialization)
int g_z { 1 };   // list initialized with specific value
bool g_w;        // false
std::string g_s; // empty string
```

### Constant global variables

Just like local variables, global variables can be constant. As with all constants, constant global variables must be initialized.

### Quick Summary

```cpp
// Non-constant global variables
int g_x;                 // defines non-initialized global variable (zero initialized by default)
int g_x {};              // defines explicitly zero-initialized global variable
int g_x { 1 };           // defines explicitly initialized global variable

// Const global variables
const int g_y;           // error: const variables must be initialized
const int g_y { 2 };     // defines initialized global const

// Constexpr global variables
constexpr int g_y;       // error: constexpr variables must be initialized
constexpr int g_y { 3 }; // defines initialized global constexpr
```

## 6.5 — Variable shadowing (name hiding)

Each block defines its own scope region. So what happens when we have a variable inside a nested block that has the same name as a variable in an outer block? When this happens, the nested variable “hides” the outer variable in areas where they are both in scope. This is called name hiding or shadowing.

### Shadowing of local variables

```cpp
#include <iostream>

int main()
{ // outer block
    int apples { 5 }; // here's the outer block apples

    { // nested block
        // apples refers to outer block apples here
        std::cout << apples << '\n'; // print value of outer block apples

        int apples{ 0 }; // define apples in the scope of the nested block

        // apples now refers to the nested block apples
        // the outer block apples is temporarily hidden

        apples = 10; // this assigns value 10 to nested block apples, not outer block apples

        std::cout << apples << '\n'; // print value of nested block apples
    } // nested block apples destroyed


    std::cout << apples << '\n'; // prints value of outer block apples

    return 0;
} // outer block apples destroyed
```

If you run this program, it prints:

```
5
10
5
```

-   Note that if the nested block `apples` had not been defined, the name `apples` in the nested block would still refer to the outer block `apples`

### Shadowing of global variables

local variables with the same name as a global variable will shadow the global variable wherever the local variable is in scope:

```cpp
#include <iostream>
int value { 5 }; // global variable

void foo()
{
    std::cout << "global variable value: " << value << '\n'; // value is not shadowed here, so this refers to the global value
}

int main()
{
    int value { 7 }; // hides the global variable value until the end of this block

    ++value; // increments local value, not global value

    std::cout << "local variable value: " << value << '\n';

    foo();

    return 0;
} // local value is destroyed
```

However, because global variables are part of the global namespace, we can use the scope operator ( :: ) with no prefix to tell the compiler we mean the global variable instead of the local variable.

### Avoid variable shadowing

## 6.6 — Internal linkage

-   An identifier’s linkage determines whether other declarations of that name refer to the same object or not
-   local variables have `no linkage`
-   Global variable and functions identifiers can have either `internal linkage` or `external linkage`
-   An identifier with `internal linkage` can be seen and used within a single translation unit, but it is not accessible from other translation units (that is, it is not exposed to the linker). This means that if two source files have identically named identifiers with `internal linkage`, those identifiers will be treated as independent (and do not result in an ODR violation for having duplicate definitions).

### Global variables with internal linkage

-   Global variables with internal linkage are sometimes called **internal variables**

```cpp
#include <iostream>

static int g_x{}; // non-constant globals have external linkage by default, but can be given internal linkage via the static keyword

const int g_y{ 1 }; // const globals have internal linkage by default
constexpr int g_z{ 2 }; // constexpr globals have internal linkage by default

int main()
{
    std::cout << g_x << ' ' << g_y << ' ' << g_z << '\n';
    return 0;
}
```

-   The use of the `static` keyword above is an example of a **storage class specifier**
-   The most commonly used storage class specifiers are `static`, `extern`, and `mutable`

### Functions with internal linkage

-   Functions default to external linkage
-   Add `static` keyword to make it internal linkage

### The one-definition rule and internal linkage

In lesson 2.7 -- Forward declarations and definitions, we noted that the one-definition rule says that an object or function can’t have more than one definition, either within a file or a program.

However, it’s worth noting that internal objects (and functions) that are defined in different files are considered to be independent entities (even if their names and types are identical), so there is no violation of the one-definition rule. Each internal object only has one definition.

### `static` vs unnamed namespaces

In modern C++, use of the static keyword for giving identifiers internal linkage is falling out of favor. Unnamed namespaces can give internal linkage to a wider range of identifiers (e.g. type identifiers), and they are better suited for giving many identifiers internal linkage.

We cover unnamed namespaces in lesson 6.15 -- Unnamed and inline namespaces.

### Why bother giving identifiers internal linkage?

Give identifiers internal linkage when you have an explicit reason to disallow access from other files.

Consider giving all identifiers you don’t want accessible to other files internal linkage (use an unnamed namespace for this).

### Quick Summary

```cpp
// Internal global variables definitions:
static int g_x;          // defines non-initialized internal global variable (zero initialized by default)
static int g_x{ 1 };     // defines initialized internal global variable

const int g_y { 2 };     // defines initialized internal global const variable
constexpr int g_y { 3 }; // defines initialized internal global constexpr variable

// Internal function definitions:
static int foo() {};     // defines internal function
```

## 6.7 — External linkage and variable forward declarations

An identifier with external linkage can be seen and used both from the file in which it is defined, and from other code files (via a forward declaration). In this sense, identifiers with external linkage are truly “global” in that they can be used anywhere in your program!

### Functions have external linkage by default

In lesson 2.8 -- Programs with multiple code files, you learned that you can call a function defined in one file from another file. This is because functions have external linkage by default.

In order to call a function defined in another file, you must place a forward declaration for the function in any other files wishing to use the function. The forward declaration tells the compiler about the existence of the function, and the linker connects the function calls to the actual function definition.

### Global variables with external linkage

Global variables with external linkage are sometimes called external variables. To make a global variable external (and thus accessible by other files), we can use the `extern` keyword to do so:

```cpp
int g_x { 2 }; // non-constant globals are external by default

extern const int g_y { 3 }; // const globals can be defined as extern, making them external
extern constexpr int g_z { 3 }; // constexpr globals can be defined as extern, making them external (but this is useless, see the note in the next section)

int main()
{
    return 0;
}
```

Non-const global variables are external by default (if used, the `extern` keyword will be ignored).

### Variable forward declarations via the extern keyword

-   For variables, creating a forward declaration is also done via the `extern` keyword (with no initialization value).
-   If you want to define an uninitialized non-const global variable, do not use the `extern` keyword, otherwise C++ will think you’re trying to make a forward declaration for the variable. the compiler is able to tell whether you’re defining a new function or making a forward declaration based on whether you supply a function body or not.
-   Although `constexpr` variables can be given external linkage via the `extern` keyword, they can not be forward declared, so there is no value in giving them external linkage.
    This is because the compiler needs to know the value of the `constexpr` variable (at compile time). If that value is defined in some other file, the compiler has no visibility on what value was defined in that other file.

### Quick summary

```cpp
// External global variable definitions:
int g_x;                       // defines non-initialized external global variable (zero initialized by default)
extern const int g_x{ 1 };     // defines initialized const external global variable
extern constexpr int g_x{ 2 }; // defines initialized constexpr external global variable

// Forward declarations
extern int g_y;                // forward declaration for non-constant global variable
extern const int g_y;          // forward declaration for const global variable
extern constexpr int g_y;      // not allowed: constexpr variables can't be forward declared
```

### Quiz time

**Question #1:** What’s the difference between a variable’s scope, duration, and linkage? What kind of scope, duration, and linkage do global variables have?

<details>
<summary><b>Answer #1</b></summary>

-   Scope determines where a variable is accessible. Duration determines when a variable is created and destroyed. Linkage determines whether the variable can be exported to another file or not.

-   Global variables have global scope (a.k.a. file scope), which means they can be accessed from the point of declaration to the end of the file in which they are declared.

-   Global variables have static duration, which means they are created when the program is started, and destroyed when it ends.

-   Global variables can have either internal or external linkage, via the static and extern keywords respectively.
</details>

## 6.8 — Why (non-const) global variables are evil

## 6.9 — Sharing global constants across multiple files (using inline variables)

### Global constants as internal variables

Prior to C++17, the following is the easiest and most common solution:

1. Create a header file to hold these constants
2. Inside this header file, define a namespace (discussed in lesson 6.2 -- User-defined namespaces and the scope resolution operator)
3. Add all your constants inside the namespace (make sure they’re constexpr)
4. #include the header file wherever you need it

### Global constants as external variables

1. define the constants in a .cpp file (to ensure the definitions only exist in one place)
2. create a header file, define a namespace, put forward declarations of the constants
3. #include the header file wherever you need it

**Note:** We use const instead of constexpr in this method because constexpr variables can’t be forward declared, even if they have external linkage. This is because the compiler needs to know the value of the variable at compile time, and a forward declaration does not provide this information.

### Global constants as inline variables (C++17)

-   In C++, the term `inline` has evolved to mean “multiple definitions are allowed”. Thus, an **inline variable** is one that is allowed to be defined in multiple files without violating the one definition rule. Inline global variables have external linkage by default.

-   Let’s say you have a normal constant that you’re #including into 10 code files. Without inline, you get 10 definitions. With inline, the compiler picks 1 definition to be the canonical definition, so you only get 1 definition. This means you save 9 constants worth of memory.

Inline variables have two primary restrictions that must be obeyed:

-   All definitions of the inline variable must be identical (otherwise, undefined behavior will result).
-   The inline variable definition (not a forward declaration) must be present in any file that uses the variable.

If you need global constants and your compiler is C++17 capable, prefer defining inline constexpr global variables in a header file.
Use std::string_view for constexpr strings. We cover this in lesson 4.18 -- Introduction to std::string_view.

## 6.10 — Static local variables

In lesson 2.5 -- Introduction to local scope, you learned that local variables have **automatic duration** by default, which means they are created at the point of definition, and destroyed when the block is exited.

Using the `static` keyword on a local variable changes its duration from **automatic duration** to **static duration**. This means the variable is now created at the start of the program, and destroyed at the end of the program (just like a global variable). As a result, the static variable will retain its value even after it goes out of scope!

```cpp
#include <iostream>

void incrementAndPrint()
{
    static int s_value{ 1 }; // static duration via static keyword.  This initializer is only executed once.
    ++s_value;
    std::cout << s_value << '\n';
} // s_value is not destroyed here, but becomes inaccessible because it goes out of scope

int main()
{
    incrementAndPrint();
    incrementAndPrint();
    incrementAndPrint();

    return 0;
}
```

Result:

```
2
3
4
```

**Static variables** offer some of the benefit of global variables (they don’t get destroyed until the end of the program) while limiting their visibility to block scope. This makes them easier to understand and safer to use even if you change their values regularly.

### Static local constants

Static local variables can be made const (or constexpr). One good use for a const static local variable is when you have a function that needs to use a const value, but creating or initializing the object is expensive (e.g. you need to read the value from a database). If you used a normal local variable, the variable would be created and initialized every time the function was executed. With a const/constexpr static local variable, you can create and initialize the expensive object once, and then reuse it whenever the function is called.

### Don’t use static local variables to alter flow

Avoid static local variables unless the variable never needs to be reset.

### Quiz time

**Question #1:** What effect does using keyword `static` have on a global variable? What effect does it have on a local variable?

<details>
<summary><b>Answer #1</b></summary>

-   When applied to a global variable, the static keyword defines the global variable as having internal linkage, meaning the variable cannot be exported to other files.

-   When applied to a local variable, the static keyword defines the local variable as having static duration, meaning the variable will only be created once, and will not be destroyed until the end of the program.
</details>

## 6.11 — Scope, duration, and linkage summary

The concepts of scope, duration, and linkage cause a lot of confusion, so we’re going to take an extra lesson to summarize everything. Some of these things we haven’t covered yet, and they’re here just for completeness / reference later.

### Scope summary

An identifier’s scope determines where the identifier can be accessed within the source code.

-   Variables with **block (local) scope** can only be accessed from the point of declaration until the end of the block in which they are declared (including nested blocks). This includes:

    -   Local variables
    -   Function parameters
    -   Program-defined type definitions (such as enums and classes) declared inside a block

-   Variables and functions with global scope can be accessed from the point of declaration until the end of the file. This includes:
    -   Global variables
    -   Functions
    -   Program-defined type definitions (such as enums and classes) declared inside a namespace or in the global scope

### Duration summary

A **variable’s duration** determines when it is created and destroyed.

-   Variables with **automatic duration** are created at the point of definition, and destroyed when the block they are part of is exited. This includes:

    -   Local variables
    -   Function parameters

-   Variables with **static duration** are created when the program begins and destroyed when the program ends. This includes:

    -   Global variables
    -   Static local variables

-   Variables with **dynamic duration** are created and destroyed by programmer request. This includes:
    -   Dynamically allocated variables

### Linkage summary

An **identifier’s linkage** determines whether multiple declarations of an identifier refer to the same entity (object, function, reference, etc…) or not.

-   An identifier with **no linkage** means the identifier only refers to itself. This includes:

    -   Local variables
    -   Program-defined type definitions (such as enums and classes) declared inside a block

-   An identifier with **internal linkage** can be accessed anywhere within the file it is declared. This includes:

    -   Static global variables (initialized or uninitialized)
    -   Static functions
    -   Const global variables
    -   Functions declared inside an unnamed namespace
    -   Program-defined type definitions (such as enums and classes) declared inside an unnamed namespace

-   An identifier with external linkage can be accessed anywhere within the file it is declared, or other files (via a forward declaration). This includes:
    -   Functions
    -   Non-const global variables (initialized or uninitialized)
    -   Extern const global variables
    -   Inline const global variables

Identifiers with external linkage will generally cause a duplicate definition linker error if the definitions are compiled into more than one .cpp file (due to violating the one-definition rule). There are some exceptions to this rule (for types, templates, and inline functions and variables) -- we’ll cover these further in future lessons when we talk about those topics.

Also note that functions have external linkage by default. They can be made internal by using the static keyword.

### Variable scope, duration, and linkage summary

Because variables have scope, duration, and linkage, let’s summarize in a chart:

| Type                                    | Example                         | Scope | Duration  | Linkage  | Notes                        |
| --------------------------------------- | ------------------------------- | ----- | --------- | -------- | ---------------------------- |
| Local variable                          | int x;                          | Block | Automatic | None     |
| Static local variable                   | static int s_x;                 | Block | Static    | None     |
| Dynamic variable                        | int\* x { new int{} };          | Block | Dynamic   | None     |
| Function parameter                      | void foo(int x)                 | Block | Automatic | None     |
| External non-constant global variable   | int g_x;                        | File  | Static    | External | Initialized or uninitialized |
| Internal non-constant global variable   | static int g_x;                 | File  | Static    | Internal | Initialized or uninitialized |
| Internal constant global variable       | constexpr int g_x { 1 };        | File  | Static    | Internal | Must be initialized          |
| External constant global variable       | extern const int g_x { 1 };     | File  | Static    | External | Must be initialized          |
| Inline constant global variable (C++17) | inline constexpr int g_x { 1 }; | File  | Static    | External | Must be initialized          |

### Forward declaration summary

You can use a forward declaration to access a function or variable in another file. The scope of the declared variable is as per usual (global scope for globals, block scope for locals).
| Type | Example | Notes |
|-------------------------------------------|---------------------------|---------------------------------------------------|
| Function forward declaration | void foo(int x); | Prototype only, no function body |
| Non-constant variable forward declaration | extern int g_x; | Must be uninitialized |
| Const variable forward declaration | extern const int g_x; | Must be uninitialized |
| Constexpr variable forward declaration | extern constexpr int g_x; | Not allowed, constexpr cannot be forward declared |

### What the heck is a storage class specifier?

When used as part of an identifier declaration, the `static` and `extern` keywords are called **storage class specifiers**. In this context, they set the storage duration and linkage of the identifier.

C++ supports 4 active storage class specifiers:

| Specifier    | Meaning                                                                    | Note                |
| ------------ | -------------------------------------------------------------------------- | ------------------- |
| extern       | static (or thread_local) storage duration and external linkage             |
| static       | static (or thread_local) storage duration and internal linkage             |
| thread_local | thread storage duration                                                    |
| mutable      | object allowed to be modified even if containing class is const            |
| auto         | automatic storage duration                                                 | Deprecated in C++11 |
| register     | automatic storage duration and hint to the compiler to place in a register | Deprecated in C++17 |

## 6.12 — Using declarations and using directives

A **qualified name** is a name that includes an associated scope. Most often, names are qualified with a namespace using the scope resolution operator ( :: ). For example:

```cpp
std::cout // identifier cout is qualified by namespace std
::foo // identifier foo is qualified by the global namespace

class C; // some class

C::s_member; // s_member is qualified by class C
obj.x; // x is qualified by class object obj
ptr->y; // y is qualified by pointer to class object ptr
```

An unqualified name is a name that does not include a scoping qualifier. For example, `cout` and `x` are unqualified names, as they do not include an associated scope.

### Using-declarations

A **using declaration** allows us to use an unqualified name (with no scope) as an alias for a qualified name.

```cpp
#include <iostream>

int main()
{
   using std::cout; // this using declaration tells the compiler that cout should resolve to std::cout
   cout << "Hello world!\n"; // so no std:: prefix is needed here!

   return 0;
} // the using declaration expires here
```

### Using-directives

Slightly simplified, a **using directive** imports all of the identifiers from a namespace into the scope of the using-directive.

```cpp
#include <iostream>

int main()
{
   using namespace std; // this using directive tells the compiler to import all names from namespace std into the current namespace without qualification
   cout << "Hello world!\n"; // so no std:: prefix is needed here
   return 0;
}
```

### Problems with using-directives (a.k.a. why you should avoid “using namespace std;”)

### The scope of using-declarations and using-directives

If a using-declaration or using-directive is used within a block, the names are applicable to just that block (it follows normal block scoping rules). This is a good thing, as it reduces the chances for naming collisions to occur to just within that block.

If a using-declaration or using-directive is used in the global namespace, the names are applicable to the entire rest of the file (they have file scope).

### Cancelling or replacing a using-statement

Once a using-statement has been declared, there’s no way to cancel or replace it with a different using-statement within the scope in which it was declared.

The best you can do is intentionally limit the scope of the using-statement from the outset using the block scoping rules.

Of course, all of this headache can be avoided by explicitly using the scope resolution operator ( :: ) in the first place.

### Best practices for using-statements

Prefer explicit namespaces over using-statements. Avoid using-directives whenever possible. Using-declarations are okay to use inside blocks.

## 6.13 — Inline functions

### Inline expansion

### The performance of inline code

### When inline expansion occurs

### The inline keyword, historically

Do not use the `inline` keyword to request inline expansion for your functions.

### The inline keyword, modernly

in modern C++, the `inline` concept has evolved to have a new meaning: multiple definitions are allowed in the program. This is true for functions as well as variables. Thus, if we mark a function as inline, then that function is allowed to have multiple definitions (in different files), as long as those definitions are identical.

Avoid the use of the inline keyword for functions unless you have a specific, compelling reason to do so.

## 6.14 — Constexpr and consteval functions

### Constexpr functions can be evaluated at compile-time

```cpp
#include <iostream>

constexpr int greater(int x, int y) // now a constexpr function
{
    return (x > y ? x : y);
}

int main()
{
    constexpr int x{ 5 };
    constexpr int y{ 6 };

    // We'll explain why we use variable g here later in the lesson
    constexpr int g { greater(x, y) }; // will be evaluated at compile-time

    std::cout << g << " is greater!\n";

    return 0;
}
```

the function call `greater(x, y)` will be evaluated at compile-time instead of runtime!

To be eligible for compile-time evaluation, a function must have a constexpr return type and not call any non-constexpr functions. Additionally, a call to the function must have constexpr arguments (e.g. constexpr variables or literals).

### Constexpr functions can also be evaluated at runtime

```cpp
#include <iostream>

constexpr int greater(int x, int y)
{
    return (x > y ? x : y);
}

int main()
{
    int x{ 5 }; // not constexpr
    int y{ 6 }; // not constexpr

    std::cout << greater(x, y) << " is greater!\n"; // will be evaluated at runtime

    return 0;
}
```

### So when is a constexpr function evaluated at compile-time?

According to the C++ standard, a constexpr function that is eligible for compile-time evaluation must be evaluated at compile-time if the return value is used where a constant expression is required. Otherwise, the compiler is free to evaluate the function at either compile-time or runtime.

```cpp
#include <iostream>

constexpr int greater(int x, int y)
{
    return (x > y ? x : y);
}

int main()
{
    constexpr int g { greater(5, 6) };            // case 1: evaluated at compile-time
    std::cout << g << " is greater!\n";

    int x{ 5 }; // not constexpr
    std::cout << greater(x, 6) << " is greater!\n"; // case 2: evaluated at runtime

    std::cout << greater(5, 6) << " is greater!\n"; // case 3: may be evaluated at either runtime or compile-time

    return 0;
}
```

In case 1, we’re calling `greater()` with constexpr arguments, so it is eligible to be evaluated at compile-time. The initializer of constexpr variable `g` must be a constant expression, so the return value is used in a context that requires a constant expression. Thus, `greater()` must be evaluated at compile-time.

In case 2, we’re calling `greater()` with one parameter that is non-constexpr. Thus `greater()` cannot be evaluated at compile-time, and must evaluate at runtime.

Case 3 is the interesting case. The `greater()` function is again being called with constexpr arguments, so it is eligible for compile-time evaluation. However, the return value is not being used in a context that requires a constant expression (operator<< always executes at runtime), so the compiler is free to choose whether this call to `greater()` will be evaluated at compile-time or runtime!

### Determining if a constexpr function call is evaluating at compile-time or runtime

In C++20:

```cpp
#include <type_traits> // for std::is_constant_evaluated
constexpr int someFunction()
{
    if (std::is_constant_evaluated()) // if compile-time evaluation
        // do something
    else // runtime evaluation
        // do something else
}
```

### Forcing a constexpr function to be evaluated at compile-time

No way. However, we can force a constexpr function that is eligible to be evaluated at compile-time to actually evaluate at compile-time by ensuring the return value is used where a constant expression is required.

### Consteval (C++20)

C++20 introduces the keyword `consteval`, which is used to indicate that a function must evaluate at compile-time, otherwise a compile error will result. Such functions are called **immediate functions**.

```cpp
#include <iostream>

consteval int greater(int x, int y) // function is now consteval
{
    return (x > y ? x : y);
}

int main()
{
    constexpr int g { greater(5, 6) };              // ok: will evaluate at compile-time
    std::cout << g << '\n';

    std::cout << greater(5, 6) << " is greater!\n"; // ok: will evaluate at compile-time

    int x{ 5 }; // not constexpr
    std::cout << greater(x, 6) << " is greater!\n"; // error: consteval functions must evaluate at compile-time

    return 0;
}
```

Use `consteval` if you have a function that must run at compile-time for some reason (e.g. performance).

### Using consteval to make constexpr execute at compile-time

### Constexpr functions are implicitly inline

Because constexpr functions may be evaluated at compile-time, the compiler must be able to see the full definition of the constexpr function at all points where the function is called. A forward declaration will not suffice, even if the actual function definition appears later in the same compilation unit.

This means that a constexpr function called in multiple files needs to have its definition included into each such file -- which would normally be a violation of the one-definition rule. To avoid such problems, constexpr functions are implicitly inline, which makes them exempt from the one-definition rule.

As a result, constexpr functions are often defined in header files, so they can be #included into any .cpp file that requires the full definition.

-   The compiler must be able to see the full definition of a constexpr function, not just a forward declaration.
-   Constexpr functions used in a single source file (.cpp) can be defined in the source file above where they are used.
-   Constexpr functions used in multiple source files should be defined in a header file so they can be included into each source file.

### Constexpr/consteval function parameters are not constexpr, but can be used as arguments to other constexpr functions

## 6.15 — Unnamed and inline namespaces

### Unnamed (anonymous) namespaces

```cpp
#include <iostream>

namespace // unnamed namespace
{
    void doSomething() // can only be accessed in this file
    {
        std::cout << "v1\n";
    }
}

int main()
{
    doSomething(); // we can call doSomething() without a namespace prefix

    return 0;
}
```

All content declared in an unnamed namespace is treated as if it is part of the parent namespace. So even though function `doSomething()` is defined in the unnamed namespace, the function itself is accessible from the parent namespace (which in this case is the global namespace), which is why we can call `doSomething()` from m`ain()` without any qualifiers.

This might make unnamed namespaces seem useless. But the other effect of unnamed namespaces is that all identifiers inside an unnamed namespace are treated as if they have internal linkage, which means that the content of an unnamed namespace can’t be seen outside of the file in which the unnamed namespace is defined.

For functions, this is effectively the same as defining all functions in the unnamed namespace as static functions. The following program is effectively identical to the one above:

```cpp
#include <iostream>

static void doSomething() // can only be accessed in this file
{
    std::cout << "v1\n";
}

int main()
{
    doSomething(); // we can call doSomething() without a namespace prefix

    return 0;
}
```

### Inline namespaces

An **inline namespace** is a namespace that is typically used to version content. Much like an unnamed namespace, anything declared inside an inline namespace is considered part of the parent namespace. However, unlike unnamed namespaces, inline namespaces don’t affect linkage.

```cpp
#include <iostream>

inline namespace V1 // declare an inline namespace named V1
{
    void doSomething()
    {
        std::cout << "V1\n";
    }
}

namespace V2 // declare a normal namespace named V2
{
    void doSomething()
    {
        std::cout << "V2\n";
    }
}

int main()
{
    V1::doSomething(); // calls the V1 version of doSomething()
    V2::doSomething(); // calls the V2 version of doSomething()

    doSomething(); // calls the inline version of doSomething() (which is V1)

    return 0;
}
```

### Mixing inline and unnamed namespaces

# Chapter 7: Control Flow and Error Handling

## 7.1 — Control flow introduction

| Category               | Meaning                                                                                                             | Implementated in C++ by          |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------- | -------------------------------- |
| Conditional statements | Conditional statements cause a sequence of code to execute only if some condition is met.                           | If, switch                       |
| Jumps                  | Jumps tell the CPU to start executing the statements at some other location.                                        | Goto, break, continue            |
| Function calls         | Function calls are jumps to some other location and back.                                                           | Function calls, return           |
| Loops                  | Loops tell the program to repeatedly execute some sequence of code zero or more times, until some condition is met. | While, do-while, for, ranged-for |
| Halts                  | Halts tell the program to quit running.                                                                             | std::exit(), std::abort()        |
| Exceptions             | Exceptions are a special kind of flow control structure designed for error handling.                                | Try, throw, catch                |

## 7.2 — If statements and blocks

## 7.3 — Common if statement problems

## 7.4 — Constexpr if statements

### Constexpr if statements (C++17)

C++17 introduces the constexpr if statement, which requires the conditional to be a constant expression. The conditional of a constexpr-if-statement will be evaluated at compile-time.

If the constexpr conditional evaluates to `true`, the entire if-else will be replaced by the true-statement. If the constexpr conditional evaluates to `false`, the entire if-else will be replaced by the false-statement (if it exists) or nothing (if there is no else).

```cpp
#include <iostream>

int main()
{
	constexpr double gravity{ 9.8 };

	if constexpr (gravity == 9.8) // now using constexpr if
		std::cout << "Gravity is normal.\n";
	else
		std::cout << "We are not on Earth.\n";

	return 0;
}
```

When the above code is compiled, it will compile this:

```cpp
int main()
{
	std::cout << "Gravity is normal.\n";

	return 0;
}
```

## 7.5 — Switch statement basics

### Starting a switch

The one restriction is that the condition must evaluate to an integral type (see lesson 4.1 -- Introduction to fundamental data types) or an enumerated type (covered in future lesson 10.2 -- Unscoped enumerations), or be convertible to one.

Expressions that evaluate to floating point types, strings, and most other non-integral types may not be used here.

## 7.6 — Switch fallthrough and scoping

### Fallthrough

Execution will then continue sequentially until one of the following termination conditions happens:

-   The end of the switch block is reached.
-   Another control flow statement (typically a break or return) causes the switch block or function to exit.
-   Something else interrupts the normal flow of the program (e.g. the OS shuts the program down, the universe implodes, etc…)

Example:

```cpp
#include <iostream>

int main()
{
    switch (2)
    {
    case 1: // Does not match
        std::cout << 1 << '\n'; // Skipped
    case 2: // Match!
        std::cout << 2 << '\n'; // Execution begins here
    case 3:
        std::cout << 3 << '\n'; // This is also executed
    case 4:
        std::cout << 4 << '\n'; // This is also executed
    default:
        std::cout << 5 << '\n'; // This is also executed
    }

    return 0;
}
```

### The \[\[fallthrough]] attribute

**Attributes** are a modern C++ feature that allows the programmer to provide the compiler with some additional data about the code. To specify an attribute, the attribute name is placed between double brackets. Attributes are not statements -- rather, they can be used almost anywhere where they are contextually relevant.

The \[\[fallthrough]] attribute modifies a null statement to indicate that fallthrough is intentional (and no warnings should be triggered):

```cpp
#include <iostream>

int main()
{
    switch (2)
    {
    case 1:
        std::cout << 1 << '\n';
        break;
    case 2:
        std::cout << 2 << '\n'; // Execution begins here
        [[fallthrough]]; // intentional fallthrough -- note the semicolon to indicate the null statement
    case 3:
        std::cout << 3 << '\n'; // This is also executed
        break;
    }

    return 0;
}
```

### Sequential case labels

```cpp
bool isVowel(char c)
{
    return (c=='a' || c=='e' || c=='i' || c=='o' || c=='u' ||
        c=='A' || c=='E' || c=='I' || c=='O' || c=='U');
}
```

Equivalent to

```cpp
bool isVowel(char c)
{
    switch (c)
    {
        case 'a': // if c is 'a'
        case 'e': // or if c is 'e'
        case 'i': // or if c is 'i'
        case 'o': // or if c is 'o'
        case 'u': // or if c is 'u'
        case 'A': // or if c is 'A'
        case 'E': // or if c is 'E'
        case 'I': // or if c is 'I'
        case 'O': // or if c is 'O'
        case 'U': // or if c is 'U'
            return true;
        default:
            return false;
    }
}
```

### Switch case scoping

```cpp
switch (1)
{
    case 1: // does not create an implicit block
        foo(); // this is part of the switch scope, not an implicit block to case 1
        break; // this is part of the switch scope, not an implicit block to case 1
    default:
        std::cout << "default case\n";
        break;
}
```

### Variable declaration and initialization inside case statements

```cpp
switch (1)
{
    int a; // okay: definition is allowed before the case labels
    int b{ 5 }; // illegal: initialization is not allowed before the case labels

    case 1:
        int y; // okay but bad practice: definition is allowed within a case
        y = 4; // okay: assignment is allowed
        break;

    case 2:
        int z{ 4 }; // illegal: initialization is not allowed if subsequent cases exist
        y = 5; // okay: y was declared above, so we can use it here too
        break;

    case 3:
        break;
}
```

If defining variables used in a case statement, do so in a block inside the case.

```cpp
switch (1)
{
    case 1:
    { // note addition of explicit block here
        int x{ 4 }; // okay, variables can be initialized inside a block inside a case
        std::cout << x;
        break;
    }
    default:
        std::cout << "default case\n";
        break;
}
```

## 7.7 — Goto statements

An unconditional jump causes execution to jump to another spot in the code. The term “unconditional” means the jump always happens (unlike an `if statement` or `switch statement`, where the jump only happens conditionally based on the result of an expression).

```cpp
#include <iostream>
#include <cmath> // for sqrt() function

int main()
{
    double x{};
tryAgain: // this is a statement label
    std::cout << "Enter a non-negative number: ";
    std::cin >> x;

    if (x < 0.0)
        goto tryAgain; // this is the goto statement

    std::cout << "The square root of " << x << " is " << std::sqrt(x) << '\n';
    return 0;
}
```

### Statement labels have function scope

In the chapter on object scope (chapter 6), we covered two kinds of scope: local (block) scope, and file (global) scope. Statement labels utilize a third kind of scope: **function scope**, which means the label is visible throughout the function even before its point of declaration. The `goto statement` and its corresponding `statement label` must appear in the same function.

While the above example shows a `goto statement` that jumps backwards (to a preceding point in the function), `goto statements` can also jump forward:

```cpp
#include <iostream>

void printCats(bool skip)
{
    if (skip)
        goto end; // jump forward; statement label 'end' is visible here due to it having function scope

    std::cout << "cats\n";
end:
    ; // statement labels must be associated with a statement
}

int main()
{
    printCats(true);  // jumps over the print statement and doesn't print anything
    printCats(false); // prints "cats"

    return 0;
}
```

note that `statement labels` must be associated with a statement (hence their name: they label a statement). Because the end of the function had no statement, we had to use a `null statement` so we had a statement to label.

There are two primary limitations to jumping: You can jump only within the bounds of a single function (you can’t jump out of one function and into another), and if you jump forward, you can’t jump forward over the initialization of any variable that is still in scope at the location being jumped to. For example:

```cpp
int main()
{
    goto skip;   // error: this jump is illegal because...
    int x { 5 }; // this initialized variable is still in scope at statement label 'skip'
skip:
    x += 3;      // what would this even evaluate to if x wasn't initialized?
    return 0;
}
```

### Avoid using goto

## 7.8 — Introduction to loops and while statements

### Introduction to loops

### While statements

```
while (condition)
    statement;
```

### While-statements that evaluate to false initially

### Infinite loops

### Intentional infinite loops

Favor `while(true)` for intentional infinite loops.

```cpp
while (true)
{
  // this loop will execute forever
}
```

### Loop variables and naming

### Integral loop variables should be signed

Integral loop variables should generally be a signed integral type.

### Doing something every N iterations

### Nested loops

## 7.9 — Do while statements

```
do
    statement; // can be a single statement or a compound statement
while (condition);
```

## 7.10 — For statements

```
for (init-statement; condition; end-expression)
   statement;
```

The easiest way to initially understand how a `for statement` works is to convert it into an equivalent `while statement`:

```
{ // note the block here
    init-statement; // used to define variables used in the loop
    while (condition)
    {
        statement;
        end-expression; // used to modify the loop variable prior to reassessment of the condition
    }
} // variables defined inside the loop go out of scope here
```

### Omitted expressions

```cpp
#include <iostream>

int main()
{
    int count{ 0 };
    for ( ; count < 10; ) // no init-statement or end-expression
    {
        std::cout << count << ' ';
        ++count;
    }

    std::cout << '\n';

    return 0;
}
```

infinite loop:

```cpp
for (;;)
    statement;
```

### For loops with multiple counters

Defining multiple variables (in the init-statement) and using the comma operator (in the end-expression) is acceptable inside a `for statement`.

```cpp
#include <iostream>

int main()
{
    for (int x{ 0 }, y{ 9 }; x < 10; ++x, --y)
        std::cout << x << ' ' << y << '\n';

    return 0;
}
```

## 7.11 — Break and continue

## 7.12 — Halts (exiting your program early)

### The std::exit() function

The `std::exit()` function causes your program to terminate normally

The `std::exit()` function does not clean up local variables in the current function or up the call stack. Because of this, it’s generally better to avoid calling s`td::exit()`.

### std::atexit

`std::atexit()` function allows you to specify a function that will automatically be called on program termination

We discuss passing functions as arguments in lesson 12.1 -- Function Pointers.

### std::abort and std::terminate

The `std::abort()` function causes your program to terminate abnormally

We will see cases later in this chapter (7.18 -- Assert and static_assert) where `std::abort` is called implicitly.

The `std::terminate()` function is typically used in conjunction with `exceptions` (we’ll cover exceptions in a later chapter). Although `std::terminate` can be called explicitly, it is more often called implicitly when an exception isn’t handled (and in a few other exception-related cases). By default, `std::terminate()` calls `std::abort()`.

### When should you use a halt?

The short answer is “almost never”. Destroying local objects is an important part of C++ (particularly when we get into classes), and none of the above-mentioned functions clean up local variables. Exceptions are a better and safer mechanism for handling error cases.

## 7.13 — Introduction to testing your code

## 7.14 — Code coverage

## 7.15 — Common semantic errors in C++

## 7.16 — Detecting and handling errors

There are 4 general strategies that can be used:

-   Handle the error within the function
-   Pass the error back to the caller to deal with
-   Halt the program
-   Throw an exception

### Fatal errors

If the error is so bad that the program can not continue to operate properly, this is called a **non-recoverable** error (also called a **fatal error**). In such cases, the best thing to do is terminate the program.

### Exceptions

### When to use `std::cout` vs `std::cerr` vs logging

## 7.17 — std::cin and handling invalid input

### std::cin, buffers, and extraction

In order to discuss how std::cin and operator>> can fail, it first helps to know a little bit about how they work.

When we use operator>> to get user input and put it into a variable, this is called an “extraction”. The >> operator is accordingly called the extraction operator when used in this context.

When the user enters input in response to an extraction operation, that data is placed in a buffer inside of std::cin. A buffer (also called a data buffer) is simply a piece of memory set aside for storing data temporarily while it’s moved from one place to another. In this case, the buffer is used to hold user input while it’s waiting to be extracted to variables.

When the extraction operator is used, the following procedure happens:

-   If there is data already in the input buffer, that data is used for extraction.
-   If the input buffer contains no data, the user is asked to input data for extraction (this is the case most of the time). When the user hits enter, a ‘\n’ character will be placed in the input buffer.
-   operator>> extracts as much data from the input buffer as it can into the variable (ignoring any leading whitespace characters, such as spaces, tabs, or ‘\n’).
-   Any data that can not be extracted is left in the input buffer for the next extraction.

Extraction succeeds if at least one character is extracted from the input buffer. Any unextracted input is left in the input buffer for future extractions.

Extraction fails if the input data does not match the type of the variable being extracted to.

### A sample program

```cpp
#include <iostream>

double getDouble()
{
    std::cout << "Enter a decimal number: ";
    double x{};
    std::cin >> x;
    return x;
}

char getOperator()
{
    std::cout << "Enter one of the following: +, -, *, or /: ";
    char op{};
    std::cin >> op;
    return op;
}

void printResult(double x, char operation, double y)
{
    switch (operation)
    {
    case '+':
        std::cout << x << " + " << y << " is " << x + y << '\n';
        break;
    case '-':
        std::cout << x << " - " << y << " is " << x - y << '\n';
        break;
    case '*':
        std::cout << x << " * " << y << " is " << x * y << '\n';
        break;
    case '/':
        std::cout << x << " / " << y << " is " << x / y << '\n';
        break;
    }
}

int main()
{
    double x{ getDouble() };
    char operation{ getOperator() };
    double y{ getDouble() };

    printResult(x, operation, y);

    return 0;
}
```

### Types of invalid text input

We can generally separate input text errors into four types:

-   Input extraction succeeds but the input is meaningless to the program (e.g. entering ‘k’ as your mathematical operator).
-   Input extraction succeeds but the user enters additional input (e.g. entering ‘\*q hello’ as your mathematical operator).
-   Input extraction fails (e.g. trying to enter ‘q’ into a numeric input).
-   Input extraction succeeds but the user overflows a numeric value.

### Error case 1: Extraction succeeds but input is meaningless

The solution here is simple: do input validation. This usually consists of 3 steps:

1. Check whether the user’s input was what you were expecting.
2. If so, return the value to the caller.
3. If not, tell the user something went wrong and have them try again.

### Error case 2: Extraction succeeds but with extraneous input

Any extraneous characters entered were simply ignored:

```cpp
std::cin.ignore(100, '\n');  // clear up to 100 characters out of the buffer, or until a '\n' character is removed
```

This call would remove up to 100 characters, but if the user entered more than 100 characters we’ll get messy output again.

```cpp
std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
```

`std::numeric_limits<std::streamsize>::max()` returns the largest value that can be stored in a variable of type `std::streamsize`

Because this line is quite long for what it does, it’s handy to wrap it in a function which can be called in place of `std::cin.ignore()`.

```cpp
#include <limits> // for std::numeric_limits

void ignoreLine()
{
    std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
}
```

Since the last character the user entered must be a ‘\n’, we can tell std::cin to ignore buffered characters until it finds a newline character (which is removed as well).

### Error case 3: Extraction fails

Now consider the following execution

```
Enter a decimal number: a
Enter one of the following: +, -, *, or /: Oops, that input is invalid.  Please try again.
Enter one of the following: +, -, *, or /: Oops, that input is invalid.  Please try again.
Enter one of the following: +, -, *, or /: Oops, that input is invalid.  Please try again.
```

When the user enters ‘a’, that character is placed in the buffer. Then operator>> tries to extract ‘a’ to variable x, which is of type double. Since ‘a’ can’t be converted to a double, operator>> can’t do the extraction. Two things happen at this point: ‘a’ is left in the buffer, and std::cin goes into “failure mode”.

Once in “failure mode”, future requests for input extraction will silently fail. Thus in our calculator program, the output prompts still print, but any requests for further extraction are ignored. This means that instead waiting for us to enter an operation, the input prompt is skipped, and we get stuck in an infinite loop because there is no way to reach one of the valid cases.

Fortunately, we can detect whether an extraction has failed:

```cpp
if (std::cin.fail()) // if the previous extraction failed
{
    // let's handle the failure
    std::cin.clear(); // put us back in 'normal' operation mode
    ignoreLine();     // and remove the bad input
}
```

A failed extraction due to invalid input will cause the variable to be zero-initialized. Zero initialization means the variable is set to 0, 0.0, “”, or whatever value 0 converts to for that type.

On Unix systems, entering an end-of-file (EOF) character (via ctrl-D) closes the input stream. This is something that std::cin.clear() can’t fix, so std::cin never leaves failure mode, which causes all subsequent input operations to fail. When this happens inside an infinite loop, your program will then loop endlessly until killed.

To handle this case more elegantly, you can explicitly test for EOF:

```cpp
if (!std::cin) // if the previous extraction failed
{
    if (std::cin.eof()) // if the stream was closed
    {
        exit(0); // shut down the program now
    }

    // let's handle the failure
    std::cin.clear(); // put us back in 'normal' operation mode
    ignoreLine();     // and remove the bad input
}
```

### Error case 4: Extraction succeeds but the user overflows a numeric value

Consider the following simple example:

```cpp
#include <cstdint>
#include <iostream>

int main()
{
    std::int16_t x{}; // x is 16 bits, holds from -32768 to 32767
    std::cout << "Enter a number between -32768 and 32767: ";
    std::cin >> x;

    std::int16_t y{}; // y is 16 bits, holds from -32768 to 32767
    std::cout << "Enter another number between -32768 and 32767: ";
    std::cin >> y;

    std::cout << "The sum is: " << x + y << '\n';
    return 0;
}
```

What happens if the user enters a number that is too large (e.g. 40000)?

```
Enter a number between -32768 and 32767: 40000
Enter another number between -32768 and 32767: The sum is: 32767
```

In the above case, std::cin goes immediately into “failure mode”, but also assigns the closest in-range value to the variable. Consequently, x is left with the assigned value of 32767. Additional inputs are skipped, leaving y with the initialized value of 0. We can handle this kind of error in the same way as a failed extraction.

### Putting it all together

```cpp
#include <iostream>
#include <limits>

void ignoreLine()
{
    std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
}

double getDouble()
{
    while (true) // Loop until user enters a valid input
    {
        std::cout << "Enter a decimal number: ";
        double x{};
        std::cin >> x;

        // Check for failed extraction
        if (!std::cin) // if the previous extraction failed
        {
            if (std::cin.eof()) // if the stream was closed
            {
                exit(0); // shut down the program now
            }

            // let's handle the failure
            std::cin.clear(); // put us back in 'normal' operation mode
            ignoreLine();     // and remove the bad input

            std::cout << "Oops, that input is invalid.  Please try again.\n";
        }
        else
        {
            ignoreLine(); // remove any extraneous input
            return x;
        }
    }
}

char getOperator()
{
    while (true) // Loop until user enters a valid input
    {
        std::cout << "Enter one of the following: +, -, *, or /: ";
        char operation{};
        std::cin >> operation;

        if (!std::cin) // if the previous extraction failed
        {
            if (std::cin.eof()) // if the stream was closed
            {
                exit(0); // shut down the program now
            }

            // let's handle the failure
            std::cin.clear(); // put us back in 'normal' operation mode
        }

        ignoreLine(); // remove any extraneous input

        // Check whether the user entered meaningful input
        switch (operation)
        {
        case '+':
        case '-':
        case '*':
        case '/':
            return operation; // return it to the caller
        default: // otherwise tell the user what went wrong
            std::cout << "Oops, that input is invalid.  Please try again.\n";
        }
    } // and try again
}

void printResult(double x, char operation, double y)
{
    switch (operation)
    {
    case '+':
        std::cout << x << " + " << y << " is " << x + y << '\n';
        break;
    case '-':
        std::cout << x << " - " << y << " is " << x - y << '\n';
        break;
    case '*':
        std::cout << x << " * " << y << " is " << x * y << '\n';
        break;
    case '/':
        std::cout << x << " / " << y << " is " << x / y << '\n';
        break;
    default: // Being robust means handling unexpected parameters as well, even though getOperator() guarantees operation is valid in this particular program
        std::cout << "Something went wrong: printResult() got an invalid operator.\n";
    }
}

int main()
{
    double x{ getDouble() };
    char operation{ getOperator() };
    double y{ getDouble() };

    printResult(x, operation, y);

    return 0;
}
```

### Conclusion

As you write your programs, consider how users will misuse your program, especially around text input. For each point of text input, consider:

-   Could extraction fail?
-   Could the user enter more input than expected?
-   Could the user enter meaningless input?
-   Could the user overflow an input?

You can use if statements and boolean logic to test whether input is expected and meaningful.

The following code will clear any extraneous input:

```cpp
std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n');
```

The following code will test for and fix failed extractions or overflow:

```cpp
if (!std::cin) // has a previous extraction failed or overflowed?
{
    if (std::cin.eof()) // if the stream was closed
    {
        exit(0); // shut down the program now
    }

    // yep, so let's handle the failure
    std::cin.clear(); // put us back in 'normal' operation mode
    std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); // and remove the bad input
}
```

Finally, use loops to ask the user to re-enter input if the original input was invalid.

## 7.18 — Assert and static_assert

### Assertions

An **assertion** is an expression that will be true unless there is a bug in the program. If the expression evaluates to `true`, the assertion statement does nothing. If the conditional expression evaluates to `false`, an error message is displayed and the program is terminated (via `std::abort`). This error message typically contains the expression that failed as text, along with the name of the code file and the line number of the assertion. This makes it very easy to tell not only what the problem was, but where in the code the problem occurred. This can help with debugging efforts immensely.

Although asserts are most often used to validate function parameters, they can be used anywhere you would like to validate that something is true.

Although we told you previously to avoid preprocessor macros, asserts are one of the few preprocessor macros that are considered acceptable to use. We encourage you to use assert statements liberally throughout your code.

### Making your assert statements more descriptive

Assertions and error handling are similar enough that their purposes can be confused, so let’s clarify:

The goal of an assertion is to catch programming errors by documenting something that should never happen. If that thing does happen, then the programmer made an error somewhere, and that error can be identified and fixed. Assertions do not allow recovery from errors (after all, if something should never happen, there’s no need to recover from it), and the program will not produce a friendly error message.

On the other hand, error handling is designed to gracefully handle cases that could happen (however rarely) in release configurations. These may or may not be recoverable, but one should always assume a user of the program may encounter them.

### NDEBUG

The assert macro comes with a small performance cost that is incurred each time the assert condition is checked. Furthermore, asserts should (ideally) never be encountered in production code (because your code should already be thoroughly tested). Consequently, many developers prefer that asserts are only active in debug builds. C++ comes with a way to turn off asserts in production code. If the macro NDEBUG is defined, the assert macro gets disabled.

Some IDEs set NDEBUG by default as part of the project settings for release configurations. For example, in Visual Studio, the following preprocessor definitions are set at the project level: WIN32;NDEBUG;\_CONSOLE. If you’re using Visual Studio and want your asserts to trigger in release builds, you’ll need to remove NDEBUG from this setting.

If you’re using an IDE or build system that doesn’t automatically define NDEBUG in release configuration, you will need to add it in the project or compilation settings manually.

### static_assert

C++ also has another type of assert called `static_assert`. A `static_assert` is an assertion that is checked at compile-time rather than at runtime, with a failing static_assert causing a compile error. Unlike assert, which is declared in the <cassert> header, static_assert is a keyword, so no header needs to be included to use it.

```cpp
static_assert(condition, diagnostic_message)
```

A few useful notes about static_assert:

-   Because `static_assert` is evaluated by the compiler, the condition must be a constant expression.
-   `static_assert` can be placed anywhere in the code file (even in the global namespace).
-   `static_assert` is not compiled out in release builds.

Prior to C++17, the diagnostic message must be supplied as the second parameter. Since C++17, providing a diagnostic message is optional.

## 7.19 — Introduction to random number generation

### Pseudo-random number generators (PRNGs)

### Seeding a PRNG

### Randomization in C++

The randomization capabilities in C++ are accessible via the <random> header of the standard library. Within the random library, there are 6 PRNG families available for use (as of C++20):

| Type name               | Family                                 | Period  | State size\* | Performance | Quality | Should I use this?          |
| ----------------------- | -------------------------------------- | ------- | ------------ | ----------- | ------- | --------------------------- |
| minstd_randminstd_rand0 | Linear congruential generator          | 2^31    | 4 bytes      | Bad         | Awful   | No                          |
| mt19937mt19937_64       | Mersenne twister                       | 2^19937 | 2500 bytes   | Decent      | Decent  | Probably (see next section) |
| ranlux24ranlux48        | Subtract and carry                     | 10^171  | 96 bytes     | Awful       | Good    | No                          |
| knuth_b                 | Shuffled linear congruential generator | 2^31    | 1028 bytes   | Awful       | Bad     | No                          |
| default_random_engine   | Any of above (implementation defined)  | Varies  | Varies       | ?           | ?       | No2                         |
| rand()                  | Linear congruential generator          | 2^31    | 4 bytes      | Bad         | Awful   | Nono                        |

## 7.20 — Generating random numbers using Mersenne Twister

The random library has support for two Mersenne Twister types:

-   mt19937 is a Mersenne Twister that generates 32-bit unsigned integers
-   mt19937_64 is a Mersenne Twister that generates 64-bit unsigned integers

```cpp
#include <iostream>
#include <random> // for std::mt19937

int main()
{
	std::mt19937 mt{}; // Instantiate a 32-bit Mersenne Twister

	// Print a bunch of random numbers
	for (int count{ 1 }; count <= 40; ++count)
	{
		std::cout << mt() << '\t'; // generate a random number

		// If we've printed 5 numbers, start a new row
		if (count % 5 == 0)
			std::cout << '\n';
	}

	return 0;
}
```

### Rolling a dice using Mersenne Twister

A **random number distribution** converts the output of a PRNG into some other distribution of numbers.

There’s one random number distribution that’s extremely useful: a **uniform distribution** is a random number distribution that produces outputs between two numbers X and Y (inclusive) with equal probability.

```cpp
#include <iostream>
#include <random> // for std::mt19937 and std::uniform_int_distribution

int main()
{
	std::mt19937 mt{};

	// Create a reusable random number generator that generates uniform numbers between 1 and 6
	std::uniform_int_distribution die6{ 1, 6 }; // for C++14, use std::uniform_int_distribution<> die6{ 1, 6 };

	// Print a bunch of random numbers
	for (int count{ 1 }; count <= 40; ++count)
	{
		std::cout << die6(mt) << '\t'; // generate a roll of the die here

		// If we've printed 10 numbers, start a new row
		if (count % 10 == 0)
			std::cout << '\n';
	}

	return 0;
}
```

### The above program isn’t as random as it seems

In the prior lesson (7.19 -- Introduction to random number generation), we covered that each number in a PRNG sequence is in a deterministic way. And that the state of the PRNG is initialized from the seed value. Thus, given any starting seed number, PRNGs will always generate the same sequence of numbers from that seed as a result.

Because we are value initializing our Mersenne Twister, it is being initialized with the same seed every time the program is run. And because the seed is the same, the random numbers being generated are also the same.

we just need to pick something that changes each time the program is run. Then we can use our PRNG to generate a unique sequence of pseudo-random numbers from that seed.

There are two methods that are commonly used to do this:

-   Use the system’s random device
-   Use the system clock

### Seeding with the system clock

```cpp
#include <iostream>
#include <random> // for std::mt19937
#include <chrono> // for std::chrono

int main()
{
	// Seed our Mersenne Twister using steady_clock
	std::mt19937 mt{ static_cast<std::mt19937::result_type>(
		std::chrono::steady_clock::now().time_since_epoch().count()
		) };

	// Create a reusable random number generator that generates uniform numbers between 1 and 6
	std::uniform_int_distribution die6{ 1, 6 }; // for C++14, use std::uniform_int_distribution<> die6{ 1, 6 };

	// Print a bunch of random numbers
	for (int count{ 1 }; count <= 40; ++count)
	{
		std::cout << die6(mt) << '\t'; // generate a roll of the die here

		// If we've printed 10 numbers, start a new row
		if (count % 10 == 0)
			std::cout << '\n';
	}

	return 0;
}
```

### Seeding with the random device

```cpp
#include <iostream>
#include <random> // for std::mt19937 and std::random_device

int main()
{
	std::mt19937 mt{ std::random_device{}() };

	// Create a reusable random number generator that generates uniform numbers between 1 and 6
	std::uniform_int_distribution die6{ 1, 6 }; // for C++14, use std::uniform_int_distribution<> die6{ 1, 6 };

	// Print a bunch of random numbers
	for (int count{ 1 }; count <= 40; ++count)
	{
		std::cout << die6(mt) << '\t'; // generate a roll of the die here

		// If we've printed 10 numbers, start a new row
		if (count % 10 == 0)
			std::cout << '\n';
	}

	return 0;
}
```

**Best practice**
Use `std::random_device` to seed your PRNGs (unless it’s not implemented properly for your target compiler/architecture).

### Random numbers across multiple functions or files (Random.h)

Read at website

# Chapter 8: Type Conversion and Function Overloading

## 8.1 — Implicit type conversion (coercion)

### Introduction to type conversion

The process of producing a new value of some type from a value of a different type is called a **conversion**.

Conversions do not change the value or type being converted. Instead, a new value with the desired type is created as a result of the conversion.

### Implicit type conversion

**Implicit type conversion** (also called **automatic type conversion** or **coercion**) is performed automatically by the compiler when one data type is required, but a different data type is supplied.

```cpp
double d{ 3 }; // int value 3 implicitly converted to type double
d = 6; // int value 6 implicitly converted to type double

float doSomething()
{
    return 3.0; // double value 3.0 implicitly converted to type float
}

double division{ 4.0 / 3 }; // int value 3 implicitly converted to type double

if (5) // int value 5 implicitly converted to type bool
{
}

void doSomething(long l)
{
}

doSomething(3); // int value 3 implicitly converted to type long
```

### What happens when a type conversion is invoked

When a type conversion is invoked (whether implicitly or explicitly), the compiler will determine whether it can convert the value from the current type to the desired type. If a valid conversion can be found, then the compiler will produce a new value of the desired type. Note that type conversions don’t change the value or type of the value or object being converted.

If the compiler can’t find an acceptable conversion, then the compilation will fail with a compile error. Type conversions can fail for any number of reasons. For example, the compiler might not know how to convert a value between the original type and the desired type. In other cases, statements may disallow certain types of conversions. For example:

```cpp
int x { 3.5 }; // brace-initialization disallows conversions that result in data loss
```

Even though the compiler knows how to convert a double value to an int value, such conversions are disallowed when using brace-initialization.

There are also cases where the compiler may not be able to figure out which of several possible type conversions is unambiguously the best one to use. We’ll see examples of this in lesson 8.12 -- Function overload resolution and ambiguous matches.

So how does the compiler actually determine whether it can convert a value from one type to another?

### The standard conversions

The standard conversions can be broadly divided into 4 categories, each covering different types of conversions:

-   Numeric promotions (covered in lesson 8.2 -- Floating-point and integral promotion)
-   Numeric conversions (covered in lesson 8.3 -- Numeric conversions)
-   Arithmetic conversions (covered in lesson 8.5 -- Arithmetic conversions)
-   Other conversions (which includes various pointer and reference conversions)

## 8.2 — Floating-point and integral promotion

### Numeric promotion

A **numeric promotion** is the type conversion of certain narrower numeric types (such as a char) to certain wider numeric types (typically int or double) that can be processed efficiently and is less likely to have a result that overflows.

All numeric promotions are **value-preserving**, which means that the converted value will always be equal to the source value (it will just have a different type). Since all values of the source type can be precisely represented in the destination type, value-preserving conversions are said to be “safe conversions”.

Because promotions are safe, the compiler will freely use numeric promotion as needed, and will not issue a warning when doing so

### Numeric promotion reduces redundancy

Numeric promotion solves another problem as well. Consider the case where you wanted to write a function to print a value of type `int`:

```cpp
#include <iostream>

void printInt(int x)
{
    std::cout << x << '\n';
}
```

While this is straightforward, what happens if we want to also be able to print a value of type short, or type char? If type conversions did not exist, we’d have to write a different print function for short and another one for char. And don’t forget another version for unsigned char, signed char, unsigned short, wchar_t, char8_t, char16_t, and char32_t! You can see how this quickly becomes unmanageable.

### Numeric promotion categories

The numeric promotion rules are divided into two subcategories: `integral promotions` and `floating point promotions`. Only the conversions listed in these categories are considered to be numeric promotions.

### Floating point promotions

Using the **floating point promotion** rules, a value of type `float` can be converted to a value of type `double`.

This means we can write a function that takes a `double` and then call it with either a `double` or a `float` value:

```cpp
#include <iostream>

void printDouble(double d)
{
    std::cout << d << '\n';
}

int main()
{
    printDouble(5.0); // no conversion necessary
    printDouble(4.0f); // numeric promotion of float to double

    return 0;
}
```

### Integral promotions

Using the integral promotion rules, the following conversions can be made:

-   signed char or signed short can be converted to int.
-   unsigned char, char8_t, and unsigned short can be converted to int if int can hold the entire range of the type, or unsigned int otherwise.
-   If char is signed by default, it follows the signed char conversion rules above. If it is unsigned by default, it follows the unsigned char conversion rules above.
-   bool can be converted to int, with false becoming 0 and true becoming 1.

Assuming an 8 bit byte and an `int` size of 4 bytes or larger (which is typical these days), the above basically means that `bool`, `char`, `signed char`, `unsigned char`, `signed short`, and `unsigned short` all get promoted to `int`.

In most cases, this lets us write a function taking an int parameter, and then use it with a wide variety of other integral types. For example:

```cpp
#include <iostream>

void printInt(int x)
{
    std::cout << x << '\n';
}

int main()
{
    printInt(2);

    short s{ 3 }; // there is no short literal suffix, so we'll use a variable for this one
    printInt(s); // numeric promotion of short to int

    printInt('a'); // numeric promotion of char to int
    printInt(true); // numeric promotion of bool to int

    return 0;
}
```

### Not all widening conversions are numeric promotions

Some widening type conversions (such as `char` to `short`, or `int` to `long`) are not considered to be numeric promotions in C++ (they are **numeric conversions**, which we’ll cover shortly in lesson 8.3 -- Numeric conversions). This is because such conversions do not assist in the goal of converting smaller types to larger types that can be processed more efficiently.

The distinction is mostly academic. However, in certain cases, the compiler will favor numeric promotions over numeric conversions. We’ll see examples where this makes a difference when we cover function overload resolution (in upcoming lesson 8.12 -- Function overload resolution and ambiguous matches).

## 8.3 — Numeric conversions

There are five basic types of numeric conversions.

1. Converting an integral type to any other integral type (excluding integral promotions):

    ```cpp
    short s = 3; // convert int to short
    long l = 3; // convert int to long
    char ch = s; // convert short to char
    unsigned int u = 3; // convert int to unsigned int
    ```

2. Converting a floating point type to any other floating point type (excluding floating point promotions):

    ```cpp
    float f = 3.0; // convert double to float
    long double ld = 3.0; // convert double to long double
    ```

3. Converting a floating point type to any integral type:

    ```cpp
    int i = 3.5; // convert double to int
    ```

4. Converting an integral type to any floating point type:

    ```cpp
    double d = 3; // convert int to double
    ```

5. Converting an integral type or a floating point type to a bool:
    ```cpp
    bool b1 = 3; // convert int to bool
    bool b2 = 3.0; // convert double to bool
    ```

### Safe and potentially unsafe conversions

Numeric conversions fall into one of three safety categories:

1. Value-preserving conversions are safe numeric conversions where the destination type can precisely represent all the values in the source type.

```cpp
int main()
{
    int n { 5 };
    long l = n; // okay, produces long value 5

    short s { 5 };
    double d = s; // okay, produces double value 5.0

    return 0;
}
```

A value converted using a value-preserving conversion can always be converted back to the source type, resulting in a value that is equivalent to the original value:

```cpp
#include <iostream>

int main()
{
    int n = static_cast<int>(static_cast<long>(3)); // convert int 3 to long and back
    std::cout << n << '\n';                         // prints 3

    char c = static_cast<char>(static_cast<double>('c')); // convert 'c' to double and back
    std::cout << c << '\n';                               // prints 'c'

    return 0;
}
```

2. Reinterpretive conversions are potentially unsafe numeric conversions where the result may be outside the range of the source type. Signed/unsigned conversions fall into this category.

```cpp
int main()
{
    int n1 { 5 };
    unsigned int u1 { n1 }; // okay: will be converted to unsigned int 5 (value preserved)

    int n2 { -5 };
    unsigned int u2 { n2 }; // bad: will result in large integer outside range of signed int

    return 0;
}
```

3. Lossy conversions are potentially unsafe numeric conversions where some data may be lost during the conversion.

For example, `double` to `int` is a conversion that may result in data loss:

```cpp
int i = 3.0; // okay: will be converted to int value 3 (value preserved)
int j = 3.5; // data lost: will be converted to int value 3 (fractional value 0.5 lost)
```

Conversion from `double` to `float` can also result in data loss:

```cpp
float f = 1.2;        // okay: will be converted to float value 1.2 (value preserved)
float g = 1.23456789; // data lost: will be converted to float 1.23457 (precision lost)
```

Converting a value that has lost data back to the source type will result in a value that is different than the original value:

```cpp
#include <iostream>

int main()
{
    double d { static_cast<double>(static_cast<int>(3.5)) }; // convert double 3.5 to int and back
    std::cout << d << '\n'; // prints 3

    double d2 { static_cast<double>(static_cast<float>(1.23456789)) }; // convert double 1.23456789 to float and back
    std::cout << d2 << '\n'; // prints 1.23457

    return 0;
}
```

### More on numeric conversions

-   In all cases, converting a value into a type whose range doesn’t support that value will lead to results that are probably unexpected.

-   Converting from a larger integral or floating point type to a smaller type from the same family will generally work so long as the value fits in the range of the smaller type.

-   In the case of floating point values, some rounding may occur due to a loss of precision in the smaller type.

-   Converting from an integer to a floating point number generally works as long as the value fits within the range of the floating point type.

-   Converting from a floating point to an integer works as long as the value fits within the range of the integer, but any fractional values are lost.

## 8.4 — Narrowing conversions, list initialization, and constexpr initializers

The following conversions are defined to be narrowing:

-   From a floating point type to an integral type.
-   From a floating point type to a narrower or lesser ranked floating point type, unless the value being converted is constexpr and is in range of the destination type (even if the destination type doesn’t have the precision to store all the significant digits of the number).
-   From an integral to a floating point type, unless the value being converted is constexpr and whose value can be stored exactly in the destination type.
-   From an integral type to another integral type that cannot represent all values of the original type, unless the value being converted is constexpr and whose value can be stored exactly in the destination type. This covers both wider to narrower integral conversions, as well as integral sign conversions (signed to unsigned, or vice-versa).

### Make intentional narrowing conversions explicit

If you need to perform a narrowing conversion, use `static_cast` to convert it into an explicit conversion.

```cpp
void someFcn(int i)
{
}

int main()
{
    double d{ 5.0 };

    someFcn(d); // bad: implicit narrowing conversion will generate compiler warning

    // good: we're explicitly telling the compiler this narrowing conversion is intentional
    someFcn(static_cast<int>(d)); // no warning generated

    return 0;
}
```

### Brace initialization disallows narrowing conversions

Narrowing conversions are disallowed when using brace initialization (which is one of the primary reasons this initialization form is preferred), and attempting to do so will produce a compile error.

If you actually want to do a narrowing conversion inside a brace initialization, use `static_cast` to convert the narrowing conversion into an explicit conversion:

```cpp
int main()
{
    double d { 3.5 };

    // static_cast<int> converts double to int, initializes i with int result
    int i { static_cast<int>(d) };

    return 0;
}
```

### Some constexpr conversions aren’t considered narrowing

### List initialization with constexpr initializers

This allows us to avoid:

-   Having to use literal suffixes in most cases
-   Having to clutter our initializations with a static_cast

For example:

```cpp
int main()
{
    // We can avoid literals with suffixes
    unsigned int u { 5 }; // okay (we don't need to use `5u`)
    float f { 1.5 };      // okay (we don't need to use `1.5f`)

    // We can avoid static_casts
    constexpr int n{ 5 };
    double d { n };       // okay (we don't need a static_cast here)
    short s { 5 };        // okay (there is no suffix for short, we don't need a static_cast here)

    return 0;
}
```

## 8.5 — Arithmetic conversions

Consider the following expression:

```cpp
int x { 2 + 3 };
```

When binary operator+ is invoked, it is given two operands, both of type `int`. Because both operands are of the same type, that type will be used to perform the calculation and to return the result. Thus, `2 + 3` will evaluate to `int` value `5`

But what happens when the operands of a binary operator are of different types?

```cpp
??? y { 2 + 3.5 };
```

In this case, operator+ is being given one operand of type `int` and another of type `double`. Should the result of the operator be returned as an `int`, a `double`, or possibly something else altogether? When defining a variable, we can choose what type it has. In other cases, for example when using `std::cout <<`, the type the calculation evaluates to changes the behavior of what is output.

### The operators that require operands of the same type

The following operators require their operands to be of the same type:

-   The binary arithmetic operators: +, -, \*, /, %
-   The binary relational operators: <, >, <=, >=, ==, !=
-   The binary bitwise arithmetic operators: &, ^, |
-   The conditional operator ?: (excluding the condition, which is expected to be of type `bool`)

### The usual arithmetic conversion rules

The compiler has a prioritized list of types that looks something like this:

-   long double (highest)
-   double
-   float
-   unsigned long long
-   long long
-   unsigned long
-   long
-   unsigned int
-   int (lowest)

There are only two rules:

-   If the type of at least one of the operands is on the priority list, the operand with lower priority is converted to the type of the operand with higher priority.
-   Otherwise (the type of neither operand is on the list), both operands are numerically promoted (see 8.2 -- Floating-point and integral promotion).

### Some examples

First, let’s add an int and a double:

```cpp
#include <iostream>
#include <typeinfo> // for typeid()

int main()
{
    int i{ 2 };
    double d{ 3.5 };
    std::cout << typeid(i + d).name() << ' ' << i + d << '\n'; // show us the type of i + d

    return 0;
}
```

In this case, the `double` operand has the highest priority, so the lower priority operand (of type `int`) is type converted to `double` value `2.0`. Then `double` values `2.0` and `3.5` are added to produce `double` result `5.5`.

Result:

```
double 5.5
```

Now let’s add two values of type `short`:

```cpp
#include <iostream>
#include <typeinfo> // for typeid()

int main()
{
    short a{ 4 };
    short b{ 5 };
    std::cout << typeid(a + b).name() << ' ' << a + b << '\n'; // show us the type of a + b

    return 0;
}
```

Because neither operand appears on the priority list, both operands undergo integral promotion to type `int`. The result of adding two ints is an `int`, as you would expect:

```
int 9
```

### Signed and unsigned issues

```cpp
#include <iostream>
#include <typeinfo> // for typeid()

int main()
{
    std::cout << typeid(5u-10).name() << ' ' << 5u - 10 << '\n'; // 5u means treat 5 as an unsigned integer

    return 0;
}
```

Because the `unsigned int` operand has higher priority, the `int` operand is converted to an `unsigned int`. And since the value `-5` is out of range of an `unsigned int`, we get a result we don’t expect.

```
unsigned int 4294967291
```

Other example:

```cpp
#include <iostream>

int main()
{
    std::cout << std::boolalpha << (-3 < 5u) << '\n';

    return 0;
}
```

While it’s clear to us that `5` is greater than `-3`, when this expression evaluates, `-3` is converted to a large `unsigned int` that is larger than `5`. Thus, the above prints `false` rather than the expected result of `true`.

This is one of the primary reasons to avoid unsigned integers -- when you mix them with signed integers in arithmetic expressions, you’re at risk for unexpected results. And the compiler probably won’t even issue a warning.

## 8.6 — Explicit type conversion (casting) and static_cast

### Type casting

C++ supports 5 different types of casts: `C-style casts`, `static casts`, `const casts`, `dynamic casts`, and `reinterpret casts`. The latter four are sometimes referred to as **named casts**.

Avoid `const casts` and `reinterpret casts` unless you have a very good reason to use them.

### C-style casts

```cpp
#include <iostream>

int main()
{
    int x { 10 };
    int y { 4 };

    double d { (double)x / y }; // convert x to a double so we get floating point division
    double d { double(x) / y }; // this is still ok
    std::cout << d << '\n'; // prints 2.5

    return 0;
}
```

Although a `C-style cast` appears to be a single cast, it can actually perform a variety of different conversions depending on context. This can include a `static cast`, a `const cast` or a `reinterpret cast` (the latter two of which we mentioned above you should avoid).
=> Avoid using C-style casts.

### static_cast

```cpp
static_cast<Type>(x)
```

The main advantage of `static_cast` is that it provides **compile-time** type checking, making it harder to make an inadvertent error.

```cpp
// a C-style string literal can't be converted to an int, so the following is an invalid conversion
int x { static_cast<int>("Hello") }; // invalid: will produce compilation error

const int x{ 5 };
// can't remove const like C-style cast
int& ref{ static_cast<int&>(x) }; // invalid: will produce compilation error
```

### Using static_cast to make narrowing conversions explicit

Compilers will often issue warnings when a potentially unsafe (narrowing) implicit type conversion is performed. For example, consider the following program, compiler will typically complain that converting a double to an int may result in loss of data:

```cpp
int i { 100 };
i = i / 2.5;
```

To tell the compiler that we explicitly mean to do this:

```cpp
int i { 100 };
i = static_cast<int>(i / 2.5);
```

## 8.7 — Typedefs and type aliases

### Type aliases

In C++, **using** is a keyword that creates an alias for an existing data type. To create such a type alias, we use the `using` keyword, followed by a name for the type alias, followed by an equals sign and an existing data type. For example:

```cpp
#include <iostream>

int main()
{
    using Distance = double; // define Distance as an alias for type double

    Distance milesToDestination{ 3.4 }; // defines a variable of type double

    std::cout << milesToDestination << '\n'; // prints a double value

    return 0;
}
```

### Naming type aliases

Name your type aliases starting with a capital letter and do not use a suffix (unless you have a specific reason to do otherwise).

### Type aliases are not distinct types

An alias does not actually define a new, distinct type (one that is considered separate from other types) -- it just introduces a new identifier for an existing type. A type alias is completely interchangeable with the aliased type.

### The scope of a type alias

a type alias defined inside a block has block scope and is usable only within that block, whereas a type alias defined in the global namespace has global scope and is usable to the end of the file.

If you need to use one or more type aliases across multiple files, they can be defined in a header file and #included into any code files that needs to use the definition:

```cpp
#ifndef MYTYPES_H
#define MYTYPES_H

    using Miles = long;
    using Speed = long;

#endif
```

Type aliases #included this way will be imported into the global namespace and thus have global scope.

### Typedefs

A **typedef** (which is short for “type definition”) is an older way of creating an alias for a type. To create a typedef alias, we use the `typedef` keyword:

```cpp
// The following aliases are identical
typedef long Miles;
using Miles = long;
```

Prefer type aliases over typedefs.

### When should we use type aliases?

-   Using type aliases for platform independent coding
-   Using type aliases to make complex types easier to read
-   Using type aliases to document the meaning of a value
-   Using type aliases for easier code maintenance

Use type aliases judiciously, when they provide a clear benefit to code readability or code maintena

## 8.8 — Type deduction for objects using the auto keyword

### Type deduction for initialized variables

**Type deduction** (also sometimes called **type inference**) is a feature that allows the compiler to deduce the type of an object from the object’s initializer. To use type deduction, the `auto` keyword is used in place of the variable’s type:

```cpp
int main()
{
    auto d{ 5.0 }; // 5.0 is a double literal, so d will be type double
    auto i{ 1 + 2 }; // 1 + 2 evaluates to an int, so i will be type int
    auto x { i }; // i is an int, so x will be type int too

    return 0;
}
```

Type deduction will not work for objects that do not have initializers or empty initializers. Thus, the following is not valid:

```cpp
int main()
{
    auto x; // The compiler is unable to deduce the type of x
    auto y{ }; // The compiler is unable to deduce the type of y

    return 0;
}
```

### Type deduction drops const / constexpr qualifiers

In most cases, type deduction will drop the const or constexpr qualifier from deduced types. If you want a deduced type to be `const` or `constexpr`, you must supply the `const` or `constexpr` yourself. To do so, simply use the `const` or `constexpr` keyword in conjunction with the `auto` keyword:

```cpp
int main()
{
    const int x { 5 };  // x has type const int (compile-time const)
    auto y { x };       // y will be type int (const is dropped)

    constexpr auto z { x }; // z will be type constexpr int (constexpr is reapplied)

    return 0;
}
```

### Type deduction for string literals

For historical reasons, string literals in C++ have a strange type. Therefore, the following probably won’t work as expected:

```cpp
auto s { "Hello, world" }; // s will be type const char*, not std::string
```

If you want the type deduced from a string literal to be std::string or std::string_view, you’ll need to use the `s` or `sv` literal suffixes (covered in lesson 4.15 -- Literals):

```cpp
#include <string>
#include <string_view>

int main()
{
    using namespace std::literals; // easiest way to access the s and sv suffixes

    auto s1 { "goo"s };  // "goo"s is a std::string literal, so s1 will be deduced as a std::string
    auto s2 { "moo"sv }; // "moo"sv is a std::string_view literal, so s2 will be deduced as a std::string_view

    return 0;
}
```

### Type deduction benefits and downsides

Read on website

## 8.9 — Type deduction for functions

```cpp
auto add(int x, int y)
{
    return x + y;
}
```

Because the return statement is returning an `int` value, the compiler will deduce that the return type of this function is `int`.

When using an `auto` return type, all return statements within the function must return values of the same type, otherwise an error will result. For example:

```cpp
auto someFcn(bool b)
{
    if (b)
        return 5; // return type int
    else
        return 6.7; // return type double
}
```

In the above function, the two return statements return values of different types, so the compiler will give an error.

A major downside of functions that use an auto return type is that such functions must be fully defined before they can be used (a forward declaration is not sufficient). For example:

```cpp
#include <iostream>

auto foo();

int main()
{
    std::cout << foo() << '\n'; // the compiler has only seen a forward declaration at this point

    return 0;
}

auto foo()
{
    return 5;
}
```

=> Favor explicit return types over function return type deduction for normal functions.

### Trailing return type syntax

Consider the following function:

```cpp
int add(int x, int y)
{
  return (x + y);
}
```

Using the **trailing return syntax**, this could be equivalently written as:

```cpp
auto add(int x, int y) -> int
{
  return (x + y);
}
```

The trailing return syntax is also required for some advanced features of C++, such as lambdas (which we cover in lesson 12.7 -- Introduction to lambdas (anonymous functions)).

### Type deduction can’t be used for function parameter types

```cpp
#include <iostream>

void addAndPrint(auto x, auto y)
{
    std::cout << x + y << '\n';
}

int main()
{
    addAndPrint(2, 3); // case 1: call addAndPrint with int parameters
    addAndPrint(4.5, 6.7); // case 2: call addAndPrint with double parameters

    return 0;
}
```

prior to C++20, the above program won’t compile (you’ll get an error about function parameters not being able to have an auto type).

In C++20, the `auto` keyword was extended so that the above program will compile and function correctly -- however, `auto` is not invoking type deduction in this case. Rather, it is triggering a different feature called `function templates` that was designed to actually handle such cases.

We introduce function templates in lesson 8.14 -- Function templates, and discuss use of auto in the context of function templates in lesson 8.16 -- Function templates with multiple template types.

## 8.10 — Introduction to function overloading

Functions can be overloaded so long as each overloaded function can be differentiated by the compiler. If an overloaded function can not be differentiated, a compile error will result.

```cpp
int add(int x, int y) // integer version
{
    return x + y;
}

double add(double x, double y) // floating point version
{
    return x + y;
}

int main()
{
    return 0;
}
```

Operators can also be overloaded in a similar manner. We’ll discuss operator overloading in 14.1 -- Introduction to operator overloading.

### Introduction to overload resolution

when a function call is made to a function that has been overloaded, the compiler will try to match the function call to the appropriate overload based on the arguments used in the function call. This is called **overload resolution**.

```cpp
#include <iostream>

int add(int x, int y)
{
    return x + y;
}

double add(double x, double y)
{
    return x + y;
}

int main()
{
    std::cout << add(1, 2); // calls add(int, int)
    std::cout << '\n';
    std::cout << add(1.2, 3.4); // calls add(double, double)

    return 0;
}
```

### Making it compile

In order for a program using overloaded functions to compile, two things have to be true:

-   Each overloaded function has to be differentiated from the others. We discuss how functions can be differentiated in lesson 8.11 -- Function overload differentiation.
-   Each call to an overloaded function has to resolve to an overloaded function. We discuss how the compiler matches function calls to overloaded functions in lesson 8.12 -- Function overload resolution and ambiguous matches.

If an overloaded function is not differentiated, or if a function call to an overloaded function can not be resolved to an overloaded function, then a compile error will result.

## 8.11 — Function overload differentiation

### How overloaded functions are differentiated

| Function property    | Used for differentiation | Notes                                                                                        |
| -------------------- | ------------------------ | -------------------------------------------------------------------------------------------- |
| Number of parameters | Yes                      |
| Type of parameters   | Yes                      | Excludes typedefs, type aliases, and const qualifier on value parameters. Includes ellipses. |
| Return type          | No                       |

For member functions, additional function-level qualifiers are also considered:

| Function-level qualifier | Used for overloading |
| ------------------------ | -------------------- |
| const or volatile        | Yes                  |
| Ref-qualifiers           | Yes                  |

As an example, a const member function can be differentiated from an otherwise identical non-const member function (even if they share the same set of parameters).

### Type signature

A function’s **type signature** (generally called a **signature**) is defined as the parts of the function header that are used for differentiation of the function. In C++, this includes the function name, number of parameter, parameter type, and function-level qualifiers. It notably does not include the return type.

## 8.12 — Function overload resolution and ambiguous matches

what happens in cases where the argument types in the function call don’t exactly match the parameter types in any of the overloaded functions? For example:

```cpp
#include <iostream>

void print(int x)
{
     std::cout << x << '\n';
}

void print(double d)
{
     std::cout << d << '\n';
}

int main()
{
     print('a'); // char does not match int or double
     print(5L); // long does not match int or double

     return 0;
}
```

Just because there is no exact match here doesn’t mean a match can’t be found -- after all, a `char` or `long` can be implicitly type converted to an `int` or a `double`. But which is the best conversion to make in each case?

### Resolving overloaded function calls

When a function call is made to an overloaded function, the compiler steps through a sequence of rules to determine which (if any) of the overloaded functions is the best match.

At each step, the compiler applies a bunch of different type conversions to the argument(s) in the function call. For each conversion applied, the compiler checks if any of the overloaded functions are now a match. After all the different type conversions have been applied and checked for matches, the step is done. The result will be one of three possible outcomes:

-   No matching functions were found. The compiler moves to the next step in the sequence.
-   A single matching function was found. This function is considered to be the best match. The matching process is now complete, and subsequent steps are not executed.
-   More than one matching function was found. The compiler will issue an ambiguous match compile error. We’ll discuss this case further in a bit.

If the compiler reaches the end of the entire sequence without finding a match, it will generate a compile error that no matching overloaded function could be found for the function call.

### The argument matching sequence

6 steps read on website

### Ambiguous matches

read on website

### Resolving ambiguous matches

1. define a new overloaded function that takes parameters of exactly the type you are trying to call the function
2. explicitly cast the ambiguous argument(s) to match the type of the function you want to call

```cpp
int x{ 0 };
print(static_cast<unsigned int>(x)); // will call print(unsigned int)
```

3. If your argument is a literal, you can use the literal suffix to ensure your literal is interpreted as the correct type:

```cpp
print(0u); // will call print(unsigned int) since 'u' suffix is unsigned int, so this is now an exact match
```

### Matching for functions with multiple arguments

read on website

## 8.13 — Default arguments

A **default argument** is a default value provided for a function parameter. For example:

```cpp
#include <iostream>

void print(int x, int y=4) // 4 is the default argument
{
    std::cout << "x: " << x << '\n';
    std::cout << "y: " << y << '\n';
}

int main()
{
    print(1, 2); // y will use user-supplied argument 2
    print(3); // y will use default argument 4, as if we had called print(3, 4)

    return 0;
}
```

Note that you must use the equals sign to specify a default argument. Using parenthesis or brace initialization won’t work:

```cpp
void foo(int x = 5);   // ok
void goo(int x ( 5 )); // compile error
void boo(int x { 5 }); // compile error
```

### Multiple default arguments

A function can have multiple parameters with default arguments:

```cpp
#include <iostream>

void print(int x=10, int y=20, int z=30)
{
    std::cout << "Values: " << x << " " << y << " " << z << '\n';
}

int main()
{
    print(1, 2, 3); // all explicit arguments
    print(1, 2); // rightmost argument defaulted
    print(1); // two rightmost arguments defaulted
    print(); // all arguments defaulted

    return 0;
}
```

C++ does not (as of C++20) support a function call syntax such as `print(,,3)`
The following is not allowed:

```cpp
void print(int x=10, int y); // not allowed
```

### Default arguments can not be redeclared

Once declared, a default argument can not be redeclared (in the same file). That means for a function with a forward declaration and a function definition, the default argument can be declared in either the forward declaration or the function definition, but not both.

```cpp
#include <iostream>

void print(int x, int y=4); // forward declaration

void print(int x, int y=4) // error: redefinition of default argument
{
    std::cout << "x: " << x << '\n';
    std::cout << "y: " << y << '\n';
}
```

Best practice is to declare the default argument in the forward declaration and not in the function definition, as the forward declaration is more likely to be seen by other files (particularly if it’s in a header file).

### Default arguments and function overloading

```cpp
void print(int x);
void print(int x, int y = 10);
void print(int x, double y = 20.5);
```

Parameters with default values will differentiate a function overload (meaning the above will compile).
However, such functions can lead to potentially ambiguous function calls. For example:

```cpp
print(1, 2); // will resolve to print(int, int)
print(1, 2.5); // will resolve to print(int, double)
print(1); // ambiguous function call
```

## 8.14 — Function templates

C++ supports 3 different kinds of template parameters:

-   Type template parameters (where the template parameter represents a type).
-   Non-type template parameters (where the template parameter represents a constexpr value).
-   Template template parameters (where the template parameter represents a template).

Type template parameters are by far the most common, so we’ll be focused on those. We’ll cover non-type template parameters in the chapter on arrays.

```cpp
template <typename T> // this is the template parameter declaration
T max(T x, T y) // this is the function template definition for max<T>
{
    return (x < y) ? y : x;
}
```

## 8.15 — Function template instantiation

### Using a function template

```cpp
max<actual_type>(arg1, arg2); // actual_type is some actual type, like int or double
```

```cpp
#include <iostream>

template <typename T>
T max(T x, T y) // function template for max(T, T)
{
    return (x < y) ? y : x;
}

int main()
{
    std::cout << max<int>(1, 2) << '\n';    // instantiates and calls function max<int>(int, int)
    std::cout << max<int>(4, 3) << '\n';    // calls already instantiated function max<int>(int, int)
    std::cout << max<double>(1, 2) << '\n'; // instantiates and calls function max<double>(double, double)

    return 0;
}
```

When the compiler encounters the function call `max<int>(1, 2)`, it will determine that a function definition for `max<int>(int, int)` does not already exist. Consequently, the compiler will use our `max<T>` function template to create one.

Here’s the same example as above, showing what the compiler actually compiles after all instantiations are done:

```cpp
#include <iostream>

// a declaration for our function template (we don't need the definition any more)
template <typename T>
T max(T x, T y);

template<>
int max<int>(int x, int y) // the generated function max<int>(int, int)
{
    return (x < y) ? y : x;
}

template<>
double max<double>(double x, double y) // the generated function max<double>(double, double)
{
    return (x < y) ? y : x;
}

int main()
{
    std::cout << max<int>(1, 2) << '\n';    // instantiates and calls function max<int>(int, int)
    std::cout << max<int>(4, 3) << '\n';    // calls already instantiated function max<int>(int, int)
    std::cout << max<double>(1, 2) << '\n'; // instantiates and calls function max<double>(double, double)

    return 0;
}
```

You can compile this yourself and see that it works

One additional thing to note here: when we instantiate `max<double>`, the instantiated function has parameters of type `double`. Because we’ve provided `int` arguments, those arguments will be implicitly converted to `double`.

### Template argument deduction

In cases where the type of the arguments match the actual type we want, we do not need to specify the actual type -- instead, we can use **template argument deduction** to have the compiler deduce the actual type that should be used from the argument types in the function call.

```cpp
std::cout << max<int>(1, 2) << '\n'; // specifying we want to call max<int>
std::cout << max<>(1, 2) << '\n';
std::cout << max(1, 2) << '\n';
```

# Chapter 9: Compound Types: References and Pointers

## 9.1 — Introduction to compound data types

**Compound data types** (also sometimes called **composite data types**) are data types that can be constructed from fundamental data types (or other compound data types). Each compound data type has its own unique properties as well.

C++ supports the following compound types:

-   Functions
-   Arrays
-   Pointer types:
    -   Pointer to object
    -   Pointer to function
-   Pointer to member types:
    -   Pointer to data member
    -   Pointer to member function
-   Reference types:
    -   L-value references
    -   R-value references
-   Enumerated types:
    -   Unscoped enumerations
    -   Scoped enumerations
-   Class types:
    -   Structs
    -   Classes
    -   Unions

## 9.2 — Value categories (lvalues and rvalues)

### The properties of an expression

To help determine how expressions should evaluate and where they can be used, all expressions in C++ have two properties: a type and a value category.

### The type of an expression

The type of an expression is equivalent to the type of the value, object, or function that results from the evaluated expression. For example:

```cpp
int main()
{
    auto v1 { 12 / 4 }; // int / int => int
    auto v2 { 12.0 / 4 }; // double / int => double

    return 0;
}
```

### The value category of an expression

The **value category** of an expression (or subexpression) indicates whether an expression resolves to a value, a function, or an object of some kind.

Prior to C++11, there were only two possible value categories: `lvalue` and `rvalue`.

### Lvalue and rvalue expressions

An **lvalue** (pronounced “ell-value”, short for “left value” or “locator value”, and sometimes written as `l-value`) is an expression that evaluates to an identifiable object or function (or bit-field).

An **rvalue** (pronounced “arr-value”, short for “right value”, and sometimes written as `r-value`) is an expression that is not an l-value. Commonly seen rvalues include literals (except C-style string literals, which are lvalues) and the return value of functions and operators. Rvalues aren’t identifiable (meaning they have to be used immediately), and only exist within the scope of the expression in which they are used.

```cpp
int return5()
{
    return 5;
}

int main()
{
    int x{ 5 }; // 5 is an rvalue expression
    const double d{ 1.2 }; // 1.2 is an rvalue expression

    int y { x }; // x is a modifiable lvalue expression
    const double e { d }; // d is a non-modifiable lvalue expression
    int z { return5() }; // return5() is an rvalue expression (since the result is returned by value)

    int w { x + 1 }; // x + 1 is an rvalue expression
    int q { static_cast<int>(d) }; // the result of static casting d to an int is an rvalue expression

    return 0;
}
```

Explain why `x = 5` is valid but `5 = x` is not

```cpp
int main()
{
    int x{};

    // Assignment requires the left operand to be a modifiable lvalue expression and the right operand to be an rvalue expression
    x = 5; // valid: x is a modifiable lvalue expression and 5 is an rvalue expression
    5 = x; // error: 5 is an rvalue expression and x is a modifiable lvalue expression

    return 0;
}
```

### L-value to r-value conversion

Consider this case:

```cpp
int main()
{
    int x{ 1 };
    int y{ 2 };

    x = y; // y is a modifiable lvalue, not an rvalue, but this is legal

    return 0;
}
```

The answer is because lvalues will implicitly convert to rvalues, so an lvalue can be used wherever an rvalue is required.

Now consider this snippet:

```cpp
int main()
{
    int x { 2 };

    x = x + 1;

    return 0;
}
```

In this statement, the variable `x` is being used in two different contexts. On the left side of the assignment operator, `x` is an lvalue expression that evaluates to variable `x`. On the right side of the assignment operator, `x + 1` is an rvalue expression that evaluates to the value 3.

### Conclusion

A rule of thumb to identify lvalue and rvalue expressions:

-   Lvalue expressions are those that evaluate to variables or other identifiable objects that persist beyond the end of the expression.
-   Rvalue expressions are those that evaluate to literals or values returned by functions/operators that are discarded at the end of the expression.

## 9.3 — Lvalue references

In C++, a **reference** is an alias for an existing object. Once a reference has been defined, any operation on the reference is applied to the object being referenced.

A reference is essentially identical to the object being referenced.

This means we can use a reference to read or modify the object being referenced

### Lvalue reference types

An **lvalue reference** (commonly just called a reference since prior to C++11 there was only one type of reference) acts as an alias for an existing lvalue (such as a variable).

To declare an lvalue reference type, we use an ampersand (&) in the type declaration:

```cpp
int      // a normal int type
int&     // an lvalue reference to an int object
double&  // an lvalue reference to a double object
```

### Lvalue reference variables

An **lvalue reference variable** is a variable that acts as a reference to an lvalue (usually another variable).

To create an lvalue reference variable, we simply define a variable with an lvalue reference type:

```cpp
#include <iostream>

int main()
{
    int x { 5 };    // x is a normal integer variable
    int& ref { x }; // ref is an lvalue reference variable that can now be used as an alias for variable x

    std::cout << x << '\n';  // print the value of x (5)
    std::cout << ref << '\n'; // print the value of x via ref (5)

    return 0;
}
```

When defining a reference, place the ampersand next to the type (not the reference variable’s name).

### Modifying values through an lvalue reference

```cpp
#include <iostream>

int main()
{
    int x { 5 }; // normal integer variable
    int& ref { x }; // ref is now an alias for variable x

    std::cout << x << ref << '\n'; // print 55

    x = 6; // x now has value 6

    std::cout << x << ref << '\n'; // prints 66

    ref = 7; // the object being referenced (x) now has value 7

    std::cout << x << ref << '\n'; // prints 77

    return 0;
}
```

In the above example, `ref` is an alias for `x`, so we are able to change the value of `x` through either `x` or `ref`.

### Initialization of lvalue references

```cpp
int main()
{
    int x { 5 };
    int& ref { x }; // valid: lvalue reference bound to a modifiable lvalue

    const int y { 5 };
    int& invalidRef { y };  // invalid: can't bind to a non-modifiable lvalue
    int& invalidRef2 { 0 }; // invalid: can't bind to an rvalue

    return 0;
}
```

When a reference is initialized with an object (or function), we say it is **bound** to that object (or function). The process by which such a reference is bound is called **reference binding**. The object (or function) being referenced is sometimes called the **referent**.

Lvalue references must be bound to a modifiable lvalue.

Lvalue references can’t be bound to non-modifiable lvalues or rvalues

In most cases, the type of the reference must match the type of the referent (there are some exceptions to this rule that we’ll discuss when we get into inheritance):

```cpp
int main()
{
    int x { 5 };
    int& ref { x }; // okay: reference to int is bound to int variable

    double y { 6.0 };
    int& invalidRef { y }; // invalid; reference to int cannot bind to double variable
    double& invalidRef2 { x }; // invalid: reference to double cannot bind to int variable

    return 0;
}
```

### References can’t be reseated (changed to refer to another object)

```cpp
#include <iostream>

int main()
{
    int x { 5 };
    int y { 6 };

    int& ref { x }; // ref is now an alias for x

    ref = y; // assigns 6 (the value of y) to x (the object being referenced by ref)
    // The above line does NOT change ref into a reference to variable y!

    std::cout << x << '\n'; // user is expecting this to print 5

    return 0;
}
```

### Lvalue reference scope and duration

Reference variables follow the same scoping and duration rules that normal variables do:

```cpp
#include <iostream>

int main()
{
    int x { 5 }; // normal integer
    int& ref { x }; // reference to variable value

     return 0;
} // x and ref die here
```

### References and referents have independent lifetimes

With one exception (that we’ll cover next lesson), the lifetime of a reference and the lifetime of its referent are independent. In other words, both of the following are true:

-   A reference can be destroyed before the object it is referencing.
-   The object being referenced can be destroyed before the reference.
-   When a reference is destroyed before the referent, the referent is not impacted. The following program demonstrates this:

```cpp
#include <iostream>

int main()
{
    int x { 5 };

    {
        int& ref { x };   // ref is a reference to x
        std::cout << ref << '\n'; // prints value of ref (5)
    } // ref is destroyed here -- x is unaware of this

    std::cout << x << '\n'; // prints value of x (5)

    return 0;
} // x destroyed here
```

### Dangling references

When an object being referenced is destroyed before a reference to it, the reference is left referencing an object that no longer exists. Such a reference is called a dangling reference. Accessing a dangling reference leads to undefined behavior.

Dangling references are fairly easy to avoid, but we’ll show a case where this can happen in practice in lesson 9.12 -- Return by reference and return by address.

### References aren’t objects

Because references aren’t objects, they can’t be used anywhere an object is required (e.g. you can’t have a reference to a reference, since an lvalue reference must reference an identifiable object). In cases where you need a reference that is an object or a reference that can be reseated, `std::reference_wrapper` (which we cover in lesson 16.3 -- Aggregation) provides a solution.

C++ doesn’t support references to references:

```cpp
int var{};
int& ref1{ var };  // an lvalue reference bound to var
int& ref2{ ref1 }; // an lvalue reference bound to var
```

## 9.4 — Lvalue references to const

```cpp
#include <iostream>

int main()
{
    const int x { 5 };    // x is a non-modifiable lvalue
    const int& ref { x }; // okay: ref is a an lvalue reference to a const value

    std::cout << ref << '\n'; // okay: we can access the const object
    ref = 6;                  // error: we can not modify an object through a const reference

    return 0;
}
```

### Initializing an lvalue reference to const with a modifiable lvalue

```cpp
#include <iostream>

int main()
{
    int x { 5 };          // x is a modifiable lvalue
    const int& ref { x }; // okay: we can bind a const reference to a modifiable lvalue

    std::cout << ref << '\n'; // okay: we can access the object through our const reference
    ref = 7;                  // error: we can not modify an object through a const reference

    x = 6;                // okay: x is a modifiable lvalue, we can still modify it through the original identifier

    return 0;
}
```

Favor `lvalue references to const` over `lvalue references to non-const` unless you need to modify the object being referenced.

### Initializing an lvalue reference to const with an rvalue

```cpp
#include <iostream>

int main()
{
    const int& ref { 5 }; // okay: 5 is an rvalue

    std::cout << ref << '\n'; // prints 5

    return 0;
}
```

When this happens, a temporary object is created and initialized with the rvalue, and the reference to const is bound to that temporary object.

A **temporary object** (also sometimes called an **anonymous object**) is an object that is created for temporary use (and then destroyed) within a single expression.

### Const references bound to temporary objects extend the lifetime of the temporary object

Temporary objects are normally destroyed at the end of the expression in which they are created.

However, consider what would happen in the above example if the temporary object created to hold rvalue `5` was destroyed at the end of the expression that initializes ref. Reference `ref` would be left dangling (referencing an object that had been destroyed), and we’d get undefined behavior when we tried to access `ref`.

To avoid dangling references in such cases, C++ has a special rule: When a const lvalue reference is bound to a temporary object, the lifetime of the temporary object is extended to match the lifetime of the reference.

```cpp
#include <iostream>

int main()
{
    const int& ref { 5 }; // The temporary object holding value 5 has its lifetime extended to match ref

    std::cout << ref << '\n'; // Therefore, we can safely use it here

    return 0;
} // Both ref and the temporary object die here
```

#### Key insight

Lvalue references can only bind to modifiable lvalues.

Lvalue references to const can bind to modifiable lvalues, non-modifiable lvalues, and rvalues. This makes them a much more flexible type of reference.

## 9.5 — Pass by lvalue reference

in lesson 2.4 -- Introduction to function parameters and arguments we discussed pass by value, where an argument passed to a function is copied into the function’s parameter

because fundamental types are cheap to copy, this isn’t a problem.

### Some objects are expensive to copy

Most of the types provided by the standard library (such as `std::string`) are `class types`. Class types are usually expensive to copy. Whenever possible, we want to avoid making unnecessary copies of objects that are expensive to copy, especially when we will destroy those copies almost immediately.

```cpp
#include <iostream>
#include <string>

void printValue(std::string y)
{
    std::cout << y << '\n';
} // y is destroyed here

int main()
{
    std::string x { "Hello, world!" }; // x is a std::string

    printValue(x); // x is passed by value (copied) into parameter y (expensive)

    return 0;
}
```

### Pass by reference

One way to avoid making an expensive copy of an argument when calling a function is to use `pass by reference` instead of `pass by value`. When using **pass by reference**, we declare a function parameter as a reference type (or const reference type) rather than as a normal type. When the function is called, each reference parameter is bound to the appropriate argument. Because the reference acts as an alias for the argument, no copy of the argument is made.

```cpp
#include <iostream>
#include <string>

void printValue(std::string& y) // type changed to std::string&
{
    std::cout << y << '\n';
} // y is destroyed here

int main()
{
    std::string x { "Hello, world!" };

    printValue(x); // x is now passed by reference into reference parameter y (inexpensive)

    return 0;
}
```

Pass by reference allows us to pass arguments to a function without making copies of those arguments each time the function is called.

### Pass by reference allows us to change the value of an argument

Passing values by reference to non-const allows us to write functions that modify the value of arguments passed in.

Pass by Value:

```cpp
#include <iostream>

void addOne(int y) // y is a copy of x
{
    ++y; // this modifies the copy of x, not the actual object x
}

int main()
{
    int x { 5 };

    std::cout << "value = " << x << '\n';

    addOne(x);

    std::cout << "value = " << x << '\n'; // x has not been modified

    return 0;
}
```

This program outputs:

```
value = 5
value = 5
```

Pass by Reference:

```cpp
#include <iostream>

void addOne(int& y) // y is bound to the actual object x
{
    ++y; // this modifies the actual object x
}

int main()
{
    int x { 5 };

    std::cout << "value = " << x << '\n';

    addOne(x);

    std::cout << "value = " << x << '\n'; // x has been modified

    return 0;
}
```

This program outputs:

```
value = 5
value = 6
```

### Pass by reference can only accept modifiable lvalue arguments

Because a reference to a non-const value can only bind to a modifiable lvalue (essentially a non-const variable), this means that pass by reference only works with arguments that are modifiable lvalues.

```cpp
#include <iostream>

void printValue(int& y) // y only accepts modifiable lvalues
{
    std::cout << y << '\n';
}

int main()
{
    int x { 5 };
    printValue(x); // ok: x is a modifiable lvalue

    const int z { 5 };
    printValue(z); // error: z is a non-modifiable lvalue

    printValue(5); // error: 5 is an rvalue

    return 0;
}
```

## 9.6 — Pass by const lvalue reference

Favor passing by const reference over passing by non-const reference unless you have a specific reason to do otherwise (e.g. the function needs to change the value of an argument).

```cpp
#include <iostream>

void printValue(const int& y) // y is now a const reference
{
    std::cout << y << '\n';
}

int main()
{
    int x { 5 };
    printValue(x); // ok: x is a modifiable lvalue

    const int z { 5 };
    printValue(z); // ok: z is a non-modifiable lvalue

    printValue(5); // ok: 5 is a literal rvalue

    ++y; // not allowed: ref is const

    return 0;
}
```

### Mixing pass by value and pass by reference

```cpp
#include <string>

void foo(int a, int& b, const std::string& c)
{
}

int main()
{
    int x { 5 };
    const std::string s { "Hello, world!" };

    foo(5, x, s);

    return 0;
}
```

### When to pass by (const) reference

As a rule of thumb, pass fundamental types by value, and class (or struct) types by const reference.

-   Other common types to pass by value: enumerated types and `std::string_view`.
-   Other common types to pass by (const) reference: `std::string`, `std::array`, and `std::vector`.

### The cost of pass by value vs pass by reference (Advanced)

Read on website

### For function parameters, prefer std::string_view over const std::string& in most cases

Prefer passing strings using std::string_view (by value) instead of const std::string&, unless your function calls other functions that require C-style strings or std::string parameters.

### Why std::string_view parameters are more efficient than const std::string& (Advanced)

Read on website

## 9.7 — Introduction to pointers

### The address-of operator (&)

The **address-of operator** (&) returns the memory address of its operand. This is pretty straightforward:

```cpp
#include <iostream>

int main()
{
    int x{ 5 };
    std::cout << x << '\n';  // print the value of variable x
    std::cout << &x << '\n'; // print the memory address of variable x

    return 0;
}
```

In the above example, we use the address-of operator (&) to retrieve the address assigned to variable x and print that address to the console. Memory addresses are typically printed as hexadecimal values (we covered hex in lesson 4.16 -- Numeral systems (decimal, binary, hexadecimal, and octal)), often without the 0x prefix.

For objects that use more than one byte of memory, address-of will **return the memory address of the first byte used by the object**.

**Tip**

The & symbol tends to cause confusion because it has different meanings depending on context:

-   When following a type name, & denotes an lvalue reference: `int& ref`.
-   When used in a unary context in an expression, & is the address-of operator: `std::cout << &x`.
-   When used in a binary context in an expression, & is the Bitwise AND operator: `std::cout << x & y`.

### The dereference operator (\*)

The most useful thing we can do with an address is access the value stored at that address. The **dereference operator** (\*) (also occasionally called the indirection operator) returns the value at a given memory address as an lvalue:

```cpp
#include <iostream>

int main()
{
    int x{ 5 };
    std::cout << x << '\n';  // print the value of variable x
    std::cout << &x << '\n'; // print the memory address of variable x

    std::cout << *(&x) << '\n'; // print the value at the memory address of variable x (parentheses not required, but make it easier to read)

    return 0;
}
```

**Key insight**
Given a memory address, we can use the dereference operator (\*) to get the value at that address (as an lvalue).

The address-of operator (&) and dereference operator (\*) work as opposites: address-of gets the address of an object, and dereference gets the object at an address.

### Pointers

A **pointer** is an object that holds a memory address (typically of another variable) as its value. This allows us to store the address of some other object to use later.

Much like reference types are declared using an ampersand (&) character, pointer types are declared using an asterisk (\*):

```cpp
int;  // a normal int
int&; // an lvalue reference to an int value

int*; // a pointer to an int value (holds the address of an integer value)
```

To create a pointer variable, we simply define a variable with a pointer type:

```cpp
int main()
{
    int x { 5 };    // normal variable
    int& ref { x }; // a reference to an integer (bound to x)

    int* ptr;       // a pointer to an integer

    return 0;
}
```

should not declare multiple variables on a single line, if you do, the asterisk has to be included with each variable.

```cpp
int* ptr1, ptr2;   // incorrect: ptr1 is a pointer to an int, but ptr2 is just a plain int!
int* ptr3, * ptr4; // correct: ptr3 and ptr4 are both pointers to an int
```

### Pointer initialization

Like normal variables, pointers are not initialized by default. A pointer that has not been initialized is sometimes called a **wild pointer**. Wild pointers contain a garbage address, and dereferencing a wild pointer will result in undefined behavior. Because of this, **you should always initialize your pointers to a known value**.

```cpp
int main()
{
    int x{ 5 };

    int* ptr;        // an uninitialized pointer (holds a garbage address)
    int* ptr2{};     // a null pointer (we'll discuss these in the next lesson)
    int* ptr3{ &x }; // a pointer initialized with the address of variable x

    return 0;
}
```

Much like the type of a reference has to match the type of object being referred to, the type of the pointer has to match the type of the object being pointed to:

```cpp
int main()
{
    int i{ 5 };
    double d{ 7.0 };

    int* iPtr{ &i };     // ok: a pointer to an int can point to an int object
    int* iPtr2 { &d };   // not okay: a pointer to an int can't point to a double object
    double* dPtr{ &d };  // ok: a pointer to a double can point to a double object
    double* dPtr2{ &i }; // not okay: a pointer to a double can't point to an int object

    return 0;
}
```

With one exception that we’ll discuss next lesson, initializing a pointer with a literal value is disallowed:

```cpp
int* ptr{ 5 }; // not okay
int* ptr{ 0x0012FF7C }; // not okay, 0x0012FF7C is treated as an integer literal
```

### Pointers and assignment

We can use assignment with pointers in two different ways:

1. To change what the pointer is pointing at (by assigning the pointer a new address)

```cpp
#include <iostream>

int main()
{
    int x{ 5 };
    int* ptr{ &x }; // ptr initialized to point at x

    std::cout << *ptr << '\n'; // print the value at the address being pointed to (x's address)

    int y{ 6 };
    ptr = &y; // // change ptr to point at y

    std::cout << *ptr << '\n'; // print the value at the address being pointed to (y's address)

    return 0;
}
```

2. To change the value being pointed at (by assigning the dereferenced pointer a new value)

```cpp
#include <iostream>

int main()
{
    int x{ 5 };
    int* ptr{ &x }; // initialize ptr with address of variable x

    std::cout << x << '\n';    // print x's value
    std::cout << *ptr << '\n'; // print the value at the address that ptr is holding (x's address)

    *ptr = 6; // The object at the address held by ptr (x) assigned value 6 (note that ptr is dereferenced here)

    std::cout << x << '\n';
    std::cout << *ptr << '\n'; // print the value at the address that ptr is holding (x's address)

    return 0;
}
```

### Pointers behave much like lvalue references

```cpp
#include <iostream>

int main()
{
    int x{ 5 };
    int& ref { x };  // get a reference to x
    int* ptr { &x }; // get a pointer to x

    std::cout << x;
    std::cout << ref;  // use the reference to print x's value (5)
    std::cout << *ptr << '\n'; // use the pointer to print x's value (5)

    ref = 6; // use the reference to change the value of x
    std::cout << x;
    std::cout << ref;  // use the reference to print x's value (6)
    std::cout << *ptr << '\n'; // use the pointer to print x's value (6)

    *ptr = 7; // use the pointer to change the value of x
    std::cout << x;
    std::cout << ref;  // use the reference to print x's value (7)
    std::cout << *ptr << '\n'; // use the pointer to print x's value (7)

    return 0;
}
```

There are some other differences between pointers and references worth mentioning:

-   References must be initialized, pointers are not required to be initialized (but should be).
-   References are not objects, pointers are.
-   References can not be reseated (changed to reference something else), pointers can change what they are pointing at.
-   References must always be bound to an object, pointers can point to nothing (we’ll see an example of this in the next lesson).
-   References are “safe” (outside of dangling references), pointers are inherently dangerous (we’ll also discuss this in the next lesson).

### The address-of operator returns a pointer

It’s worth noting that the address-of operator (&) doesn’t return the address of its operand as a literal. Instead, it returns a pointer containing the address of the operand, whose type is derived from the argument (e.g. taking the address of an `int` will return the address in an `int` pointer).

```cpp
#include <iostream>
#include <typeinfo>

int main()
{
	int x{ 4 };
	std::cout << typeid(&x).name() << '\n'; // print the type of &x

	return 0;
}
```

On Visual Studio, this printed:

```
int *
```

### The size of pointers

The size of a pointer is dependent upon the architecture the executable is compiled for -- a 32-bit executable uses 32-bit memory addresses -- consequently, a pointer on a 32-bit machine is 32 bits (4 bytes). With a 64-bit executable, a pointer would be 64 bits (8 bytes). Note that this is true regardless of the size of the object being pointed to:

```cpp
#include <iostream>

int main() // assume a 32-bit application
{
    char* chPtr{};        // chars are 1 byte
    int* iPtr{};          // ints are usually 4 bytes
    long double* ldPtr{}; // long doubles are usually 8 or 12 bytes

    std::cout << sizeof(chPtr) << '\n'; // prints 4
    std::cout << sizeof(iPtr) << '\n';  // prints 4
    std::cout << sizeof(ldPtr) << '\n'; // prints 4

    return 0;
}
```

The size of the pointer is always the same. This is because a pointer is just a memory address, and the number of bits needed to access a memory address is constant.

### Dangling pointers

Much like a dangling reference, a **dangling pointer** is a pointer that is holding the address of an object that is no longer valid (e.g. because it has been destroyed).
