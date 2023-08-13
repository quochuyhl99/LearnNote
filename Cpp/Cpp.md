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
-   If using s`td::getline()` to read strings, use `std::cin >> std::ws` input manipulator to ignore leading whitespace.

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
