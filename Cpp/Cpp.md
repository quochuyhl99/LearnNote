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
