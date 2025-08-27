#### **1. Character Set**

A character set is the set of valid characters that a language can recognize. C uses the ASCII (American Standard Code for Information Interchange) character set. It includes:

  * **Alphabets:** Uppercase letters (A-Z) and lowercase letters (a-z).
  * **Digits:** 0, 1, 2, 3, 4, 5, 6, 7, 8, 9.
  * **Special Characters:** Symbols like `!`, `@`, `#`, `$`, `%`, `^`, `&`, `*`, `(`, `)`, `-`, `_`, `=`, `+`, `[`, `]`, `{`, `}`, `|`, `\`, `;`, `:`, `'`, `"`, `<`, `>`, `,`, `.`, `/`, `?`, `     ` (space), etc.
  * **Whitespace Characters:** Space, newline (`\n`), horizontal tab (`\t`), vertical tab (`\v`), form feed (`\f`), carriage return (`\r`).

#### **2. C Tokens**

In C, the smallest individual units are called **tokens**. The compiler breaks down a program into these tokens. There are six types of tokens in C:

  * **Keywords:** Reserved words that have special meaning to the C compiler. They cannot be used as variable names. (e.g., `int`, `float`, `char`, `if`, `else`, `while`, `for`, `return`, `void`, etc.)
  * **Identifiers:** Names given to program elements like variables, functions, arrays, etc. They are user-defined.
  * **Constants:** Fixed values that do not change during program execution.
  * **Strings:** Sequences of characters enclosed in double quotes.
  * **Operators:** Symbols that perform operations on data (e.g., `+`, `-`, `*`, `/`, `=`, `>`).
  * **Special Symbols:** Characters used for specific purposes (e.g., `{}`, `()`, `[]`, `;`, `,`).

#### **3. Keywords**

Keywords are predefined, reserved words in C. They have a specific meaning and purpose and cannot be used for any other purpose (like naming variables or functions). Here are some common C keywords:

`auto`, `break`, `case`, `char`, `const`, `continue`, `default`, `do`, `double`, `else`, `enum`, `extern`, `float`, `for`, `goto`, `if`, `int`, `long`, `register`, `return`, `short`, `signed`, `sizeof`, `static`, `struct`, `switch`, `typedef`, `union`, `unsigned`, `void`, `volatile`, `while`.

#### **4. Identifiers**

Identifiers are names used to identify variables, functions, arrays, structures, and other program elements.

**Rules for Naming Identifiers:**

  * Can consist of letters (A-Z, a-z), digits (0-9), and the underscore character (`_`).
  * Must begin with a letter or an underscore. It cannot begin with a digit.
  * Keywords cannot be used as identifiers.
  * They are case-sensitive (e.g., `age` is different from `Age`).
  * There is no limit on the length of an identifier, but only the first 31 characters are guaranteed to be significant (meaning, `very_long_variable_name_1` and `very_long_variable_name_2` might be treated as the same if they only differ beyond the 31st character by some older compilers). Modern compilers usually support longer significant lengths.

**Examples of Valid Identifiers:** `myVariable`, `_count`, `sum_of_numbers`, `data123`
**Examples of Invalid Identifiers:** `1data`, `my-variable`, `if`, `float`

#### **5. Constants**

Constants are fixed values that do not change during the execution of a program. C supports several types of constants:

  * **Integer Constants:** Whole numbers without a fractional part.

      * **Decimal:** `10`, `200`, `-50`
      * **Octal (base 8):** Start with `0` (e.g., `012` is 10 in decimal)
      * **Hexadecimal (base 16):** Start with `0x` or `0X` (e.g., `0xA` is 10 in decimal, `0xFF` is 255 in decimal)
      * Can be followed by suffixes `L` or `l` for `long` and `U` or `u` for `unsigned`. (e.g., `123L`, `456U`)

  * **Floating-point Constants (Real Constants):** Numbers with a fractional part.

      * Can be written in fractional form (e.g., `3.14`, `-0.001`) or exponential form (e.g., `0.5e-3` for $0.5 \\times 10^{-3}$).
      * By default, they are of `double` type. Can be suffixed with `F` or `f` for `float` or `L` or `l` for `long double`.

  * **Character Constants:** A single character enclosed in single quotes (e.g., `'A'`, `'5'`, `'$'`).

      * Also includes **Escape Sequences**: Special character combinations used to represent non-graphic characters or to denote special actions. They begin with a backslash `\` (e.g., `\n` for newline, `\t` for tab, `\b` for backspace, `\'` for single quote, `\"` for double quote, `\\` for backslash).

  * **String Constants:** A sequence of characters enclosed in double quotes (e.g., `"Hello"`, `"C Programming"`).

      * Automatically terminated by a null character (`\0`), which is not visible but indicates the end of the string.

#### **6. Variables**

A variable is a named storage location in memory that can hold a value. The value stored in a variable can change during program execution. Think of it as a container that holds data.

**Declaring Variables:**
Before you can use a variable, you must declare it. Declaration tells the compiler:

1.  The **name** of the variable.
2.  The **type** of data it will hold (e.g., integer, floating-point, character).

**Syntax for Declaration:**
`data_type variable_name;`
or
`data_type variable_name1, variable_name2, ...;`

**Examples:**

```c
int age;           // Declares an integer variable named 'age'
float salary;      // Declares a floating-point variable named 'salary'
char initial;      // Declares a character variable named 'initial'
int count, total;  // Declares two integer variables
```

**Initializing Variables:**
You can assign an initial value to a variable at the time of its declaration.
`data_type variable_name = value;`

**Examples:**

```c
int score = 100;           // Declares and initializes 'score' to 100
float pi = 3.14159;        // Declares and initializes 'pi' to 3.14159
char grade = 'A';          // Declares and initializes 'grade' to 'A'
```

#### **7. Data Types**

Data types specify the type of data a variable can store, how much memory it occupies, and the range of values it can hold. C has several fundamental (or primary/basic) data types:

  * **`int` (Integer):** Used to store whole numbers (e.g., 10, -500).

      * Typically occupies 2 or 4 bytes of memory, depending on the system/compiler.
      * Range: Usually $-32,768$ to $32,767$ (for 2-byte) or $-2,147,483,648$ to $2,147,483,647$ (for 4-byte).
      * Can be qualified with `short`, `long`, `signed`, `unsigned`.
          * `short int`: Guaranteed to be at least 2 bytes.
          * `long int`: Guaranteed to be at least 4 bytes.
          * `unsigned int`: Stores only non-negative values, effectively doubling the positive range by using the bit normally reserved for the sign.

  * **`float` (Floating-point):** Used to store real numbers with fractional parts (e.g., 3.14, -0.001).

      - [ ] Typically occupies 4 bytes.
      * Provides about 6 decimal places of precision.

  * **`double` (Double precision floating-point):** Used for larger and more precise real numbers.

      * Typically occupies 8 bytes.
      * Provides about 15 decimal places of precision.
      * `long double`: Can offer even greater precision (usually 10 or 16 bytes).

  * **`char` (Character):** Used to store a single character.

      * Typically occupies 1 byte.
      * Internally, characters are stored as their ASCII integer values.
      * Can be qualified with `signed` or `unsigned`.

**Memory Occupancy and Range:**
The exact size and range of data types can vary slightly depending on the compiler and the architecture of the computer system (e.g., 16-bit, 32-bit, 64-bit). You can use the `sizeof` operator to find the size of any data type or variable in bytes.

**Example `sizeof` operator:**

```c
printf("Size of int: %zu bytes\n", sizeof(int));
printf("Size of float: %zu bytes\n", sizeof(float));
printf("Size of char: %zu bytes\n", sizeof(char));
```

*(Note: `%zu` is the format specifier for `size_t`, which is the type returned by `sizeof`.)*

-----

### **Examples:**

**Example 1: Declaring and Initializing Variables**

```c
#include <stdio.h>

int main() {
    // Variable declarations
    int student_id;
    float average_score;
    char grade;
    double bank_balance;

    // Variable initializations
    student_id = 12345;
    average_score = 88.5f; // 'f' suffix indicates float literal
    grade = 'B';
    bank_balance = 1234567.89;

    // Printing variable values
    printf("Student ID: %d\n", student_id);
    printf("Average Score: %.2f\n", average_score); // .2f for 2 decimal places
    printf("Grade: %c\n", grade);
    printf("Bank Balance: %.2lf\n", bank_balance); // %lf for double

    // Reassigning a variable
    student_id = 54321;
    printf("New Student ID: %d\n", student_id);

    // Using a constant
    const float PI = 3.14159; // Declares a constant PI
    printf("Value of PI: %f\n", PI);
    // PI = 3.0; // This would cause a compile-time error: assignment of read-only variable 'PI'

    return 0;
}
```

**Explanation of Example 1:**

  * We declare variables of different data types (`int`, `float`, `char`, `double`).
  * We initialize them with values. Notice the `f` suffix for `float` literals and the `.2f` / `.2lf` format specifiers for printing floating-point numbers with two decimal places.
  * We demonstrate that variable values can be changed (reassigned).
  * We introduce `const` keyword to declare a symbolic constant. Once declared, its value cannot be changed.

**Example 2: Using `sizeof` operator and understanding ranges**

```c
#include <stdio.h>
#include <limits.h> // For INT_MAX, INT_MIN etc.
#include <float.h>  // For FLT_MAX, DBL_MAX etc.

int main() {
    printf("Sizes of data types:\n");
    printf("Size of char: %zu byte(s)\n", sizeof(char));
    printf("Size of int: %zu byte(s)\n", sizeof(int));
    printf("Size of short int: %zu byte(s)\n", sizeof(short int));
    printf("Size of long int: %zu byte(s)\n", sizeof(long int));
    printf("Size of long long int: %zu byte(s)\n", sizeof(long long int)); // C99 standard
    printf("Size of float: %zu byte(s)\n", sizeof(float));
    printf("Size of double: %zu byte(s)\n", sizeof(double));
    printf("Size of long double: %zu byte(s)\n", sizeof(long double));

    printf("\nRanges for integer types (example):\n");
    printf("Min int: %d\n", INT_MIN);
    printf("Max int: %d\n", INT_MAX);
    printf("Max unsigned int: %u\n", UINT_MAX); // %u for unsigned int

    printf("\nRanges for floating-point types (example):\n");
    printf("Max float: %e\n", FLT_MAX);  // %e for scientific notation
    printf("Max double: %e\n", DBL_MAX);

    return 0;
}
```

**Explanation of Example 2:**

  * We use `sizeof` to dynamically determine the memory footprint of each data type on your specific system.
  * We include `<limits.h>` and `<float.h>` to access predefined constants that represent the maximum and minimum values for various integer and floating-point types, respectively. This gives you an idea of the range of values each type can hold.

-----

### **Relevant DSA (Conceptual Link):**

  * **Data Representation:** Understanding data types is fundamental to how data is stored and processed by any algorithm. Whether you're working with numbers in a sorting algorithm or characters in a string search, the underlying data type dictates how that data is represented in memory.
  * **Memory Efficiency:** Choosing the correct data type can impact memory efficiency. For example, using `short int` instead of `int` if your numbers are always small can save memory, which is important in memory-constrained environments or for very large datasets in DSA.
  * **Variable Scope (Upcoming):** While not explicitly covered in this lesson, the concept of variable declaration lays the groundwork for understanding variable scope (where a variable is accessible in your program), which is crucial for organizing functions and managing data in complex algorithms.

-----

### **Homework Project:**

**Project 2: Data Type Exploration and Simple Calculations**

1.  **Personal Information Program:**

      * Declare variables to store the following personal information:
          * Your `age` (integer)
          * Your `height` in meters (floating-point, e.g., 1.75)
          * Your `first_initial` (character)
          * Your `net_worth` (double, potentially a large number)
          * Your `favorite_letter` (character constant)
      * Initialize these variables with your actual or hypothetical data.
      * Print all these values to the console using appropriate `printf` format specifiers. Ensure the `height` and `net_worth` are displayed with 2 decimal places.

2.  **Basic Unit Conversion:**

      * Declare an integer variable `distance_km` and initialize it with a value (e.g., 10 kilometers).
      * Declare a floating-point variable `distance_miles`.
      * Calculate `distance_miles` using the conversion factor: $1 \\text{ km} = 0.621371 \\text{ miles}$.
      * Print both `distance_km` and `distance_miles` to the console, making sure `distance_miles` is displayed with 4 decimal places.

3.  **`sizeof` Operator Experiment:**

      * Modify Example 2 (the `sizeof` program) to also print the size of `unsigned short int` and `unsigned long int`.
      * Observe if there are any differences in size compared to their `signed` counterparts (usually there aren't for `sizeof`, but it reinforces the concept).

**Self-Check/Debugging:**

  * Are all your variables declared before use?
  * Are you using the correct data types for the kind of data you're storing?
  * Are the `printf` format specifiers (`%d`, `%f`, `%c`, `%lf`, `%.Xf`, `%.Xlf`) used correctly for each data type?
  * Did you include `<stdio.h>` for `printf` and potentially `<limits.h>`/`<float.h>` if you experimented with ranges?

