### **Lesson 3: Operators and Expressions**

In C, an **operator** is a symbol that tells the compiler to perform specific mathematical, relational, or logical operations. An **expression** is a combination of operators, constants, and variables that evaluates to a single value.

#### **1. Types of Operators**

C offers a rich set of operators, which can be broadly categorized as follows:

* **Arithmetic Operators:** Perform mathematical calculations.
* **Relational Operators:** Used for comparison.
* **Logical Operators:** Combine or modify boolean expressions.
* **Assignment Operators:** Assign values to variables.
* **Increment and Decrement Operators:** Used for quickly increasing or decreasing a variable's value.
* **Conditional (Ternary) Operator:** A shorthand for a simple `if-else` statement.
* **Bitwise Operators:** Perform operations on individual bits.
* **Special Operators:** Includes `sizeof`, comma operator, pointer operators, etc.

Let's explore each category:

##### **1.1. Arithmetic Operators**

These operators perform basic mathematical operations.

| Operator | Meaning        | Example        | Result (if `a=10, b=3`) |
| :------- | :------------- | :------------- | :----------------------- |
| `+`      | Addition       | `a + b`        | `13`                     |
| `-`      | Subtraction    | `a - b`        | `7`                      |
| `*`      | Multiplication | `a * b`        | `30`                     |
| `/`      | Division       | `a / b`        | `3` (integer division)   |
| `%`      | Modulo (Remainder) | `a % b`    | `1` (remainder of 10/3)  |

**Important Note on Division (`/`):**
* If both operands are integers, the division results in an **integer quotient** (the fractional part is truncated).
    * `10 / 3` evaluates to `3`
    * `5 / 2` evaluates to `2`
* If at least one operand is a floating-point type, the division results in a **floating-point value**.
    * `10.0 / 3` evaluates to `3.333333`
    * `10 / 3.0` evaluates to `3.333333`

**Modulo Operator (`%`):**
* The modulo operator works only with **integer operands**.
* It gives the remainder of an integer division.

##### **1.2. Relational Operators**

These operators are used to compare two values. They evaluate to either `true` (represented as `1` in C) or `false` (represented as `0` in C).

| Operator | Meaning                | Example        | Result (if `a=10, b=3`) |
| :------- | :--------------------- | :------------- | :----------------------- |
| `<`      | Less than              | `a < b`        | `0` (false)              |
| `>`      | Greater than           | `a > b`        | `1` (true)               |
| `<=`     | Less than or equal to  | `a <= b`       | `0` (false)              |
| `>=`     | Greater than or equal to | `a >= b`       | `1` (true)               |
| `==`     | Equal to               | `a == b`       | `0` (false)              |
| `!=`     | Not equal to           | `a != b`       | `1` (true)               |

**Common Mistake:** Confusing `=` (assignment) with `==` (equality comparison).

##### **1.3. Logical Operators**

These operators combine or modify relational or logical expressions. They also return `true` (1) or `false` (0).

| Operator | Meaning      | Example (if `x=5, y=10`) | Result (`x>0 && y<100`) |
| :------- | :----------- | :---------------------- | :----------------------- |
| `&&`     | Logical AND  | `(x > 0) && (y < 100)`  | `1` (true) - Both true   |
| `||`     | Logical OR   | `(x > 10) || (y < 5)`   | `0` (false) - Both false |
| `!`      | Logical NOT  | `!(x > 10)`             | `1` (true) - `x > 10` is false, so `!` makes it true |

* **Logical AND (`&&`):** Returns `true` only if *both* operands are `true`.
* **Logical OR (`||`):** Returns `true` if *at least one* operand is `true`.
* **Logical NOT (`!`):** Inverts the logical state of its operand (true becomes false, false becomes true).

##### **1.4. Assignment Operators**

The basic assignment operator is `=`. It assigns the value of the right-hand side expression to the variable on the left-hand side.

**Compound Assignment Operators:** These are shorthand operators that combine an arithmetic operator with an assignment operator.

| Operator | Example      | Equivalent to      |
| :------- | :----------- | :----------------- |
| `+=`     | `a += b`     | `a = a + b`        |
| `-=`     | `a -= b`     | `a = a - b`        |
| `*=`     | `a *= b`     | `a = a * b`        |
| `/=`     | `a /= b`     | `a = a / b`        |
| `%=`     | `a %= b`     | `a = a % b`        |

##### **1.5. Increment and Decrement Operators**

These are unary operators that increase or decrease the value of a variable by `1`.

* **Increment Operator (`++`):** Increases the value of the operand by `1`.
    * **Prefix Increment:** `++variable` (Increments the variable *first*, then uses its new value in the expression)
    * **Postfix Increment:** `variable++` (Uses the current value of the variable in the expression *first*, then increments it)

* **Decrement Operator (`--`):** Decreases the value of the operand by `1`.
    * **Prefix Decrement:** `--variable` (Decrements the variable *first*, then uses its new value in the expression)
    * **Postfix Decrement:** `variable--` (Uses the current value of the variable in the expression *first*, then decrements it)

**Example Difference (Crucial for understanding):**
```c
int x = 5;
int y = ++x; // x becomes 6, then y becomes 6

int a = 5;
int b = a++; // b becomes 5, then a becomes 6
```

##### **1.6. Conditional (Ternary) Operator (`? :`)**

This is a unique operator as it takes three operands. It's a shorthand for a simple `if-else` statement.

**Syntax:** `expression1 ? expression2 : expression3;`

* If `expression1` evaluates to `true` (non-zero), then `expression2` is evaluated, and its value becomes the result of the expression.
* If `expression1` evaluates to `false` (zero), then `expression3` is evaluated, and its value becomes the result.

**Example:**
`int max = (a > b) ? a : b;` // If `a` is greater than `b`, `max` gets `a`; otherwise, `max` gets `b`.

##### **1.7. Bitwise Operators**

These operators perform operations at the bit level directly on integer operands. They are fundamental in low-level programming, embedded systems, and optimizing certain operations.

| Operator | Meaning          | Example        |
| :------- | :--------------- | :------------- |
| `&`      | Bitwise AND      | `a & b`        |
| `|`      | Bitwise OR       | `a | b`        |
| `^`      | Bitwise XOR      | `a ^ b`        |
| `~`      | Bitwise NOT (One's Complement) | `~a` |
| `<<`     | Left Shift       | `a << 2`       |
- [ ] | `>>`     | Right Shift      | `a >> 2`       |

*(We will delve deeper into Bitwise Operators when their practical application becomes more relevant, but it's good to know they exist now.)*

##### **1.8. Special Operators**

* **`sizeof` operator:** Returns the size of a variable or data type in bytes. (We used this in the previous lesson!)
    * `sizeof(variable)` or `sizeof(data_type)`
* **Comma operator (`,`):** Evaluates expressions from left to right and returns the value of the rightmost expression. Often used in `for` loops.
    * `int x = (a=10, b=20, a+b);` // `a` becomes 10, `b` becomes 20, `x` becomes 30.
* **Pointer operators (`*` and `&`):** Used for pointer manipulation (dereference and address-of). (Will be covered in detail in the Pointers lesson).
    * `&variable` (address-of operator): Gives the memory address of a variable.
    * `*pointer` (dereference operator): Accesses the value at the address stored in a pointer.

#### **2. Operator Precedence and Associativity**

When multiple operators appear in an expression, C follows specific rules to determine the order of evaluation:

* **Precedence:** Determines which operator is evaluated first. Operators with higher precedence are evaluated before operators with lower precedence. (e.g., `*` and `/` have higher precedence than `+` and `-`).
    * Example: `5 + 3 * 2` is `5 + (3 * 2) = 11`, not `(5 + 3) * 2 = 16`.
* **Associativity:** Determines the order of evaluation when operators have the same precedence.
    * Most operators (like arithmetic) are **left-to-right associative**.
        * Example: `10 / 2 * 5` is `(10 / 2) * 5 = 5 * 5 = 25`, not `10 / (2 * 5) = 10 / 10 = 1`.
    * Some operators (like assignment, unary, conditional) are **right-to-left associative**.
        * Example: `a = b = c;` is `a = (b = c);`

It's usually a good practice to use **parentheses `()`** to explicitly control the order of evaluation, even if you know the precedence rules. This makes your code clearer and less prone to errors.

---

### **Examples:**

**Example 1: Arithmetic Operations**

```c
#include <stdio.h>

int main() {
    int num1 = 20, num2 = 7;
    float fnum1 = 20.0, fnum2 = 7.0;

    printf("--- Arithmetic Operators ---\n");
    printf("num1 + num2 = %d\n", num1 + num2); // 27
    printf("num1 - num2 = %d\n", num1 - num2); // 13
    printf("num1 * num2 = %d\n", num1 * num2); // 140
    printf("num1 / num2 (integer div) = %d\n", num1 / num2); // 2 (20/7 = 2 remainder 6)
    printf("num1 %% num2 = %d\n", num1 % num2); // 6

    printf("fnum1 / fnum2 (float div) = %.2f\n", fnum1 / fnum2); // 2.86
    printf("num1 / fnum2 (mixed type) = %.2f\n", num1 / fnum2);   // 2.86

    return 0;
}
```

**Example 2: Relational and Logical Operators**

```c
#include <stdio.h>

int main() {
    int a = 10, b = 20, c = 10;
    int result;

    printf("--- Relational Operators ---\n");
    result = (a == c);
    printf("a == c: %d\n", result); // 1 (true)
    result = (a > b);
    printf("a > b: %d\n", result);  // 0 (false)
    result = (a != b);
    printf("a != b: %d\n", result); // 1 (true)

    printf("--- Logical Operators ---\n");
    // (10 > 5) is true (1)
    // (20 < 30) is true (1)
    result = (a > 5) && (b < 30);
    printf("(a > 5) && (b < 30): %d\n", result); // 1 (true)

    // (a == 5) is false (0)
    // (b == 20) is true (1)
    result = (a == 5) || (b == 20);
    printf("(a == 5) || (b == 20): %d\n", result); // 1 (true)

    // !(a > b) is !(false) which is true
    result = !(a > b);
    printf("!(a > b): %d\n", result); // 1 (true)

    return 0;
}
```

**Example 3: Increment/Decrement and Conditional Operators**

```c
#include <stdio.h>

int main() {
    int x = 5, y = 5;
    int pre_inc_result, post_inc_result;

    printf("--- Increment/Decrement Operators ---\n");
    pre_inc_result = ++x; // x becomes 6, then pre_inc_result gets 6
    printf("After ++x: x = %d, pre_inc_result = %d\n", x, pre_inc_result); // x=6, pre_inc_result=6

    post_inc_result = y++; // post_inc_result gets 5, then y becomes 6
    printf("After y++: y = %d, post_inc_result = %d\n", y, post_inc_result); // y=6, post_inc_result=5

    int a = 10, b = 5;
    int max_val;

    printf("\n--- Conditional (Ternary) Operator ---\n");
    max_val = (a > b) ? a : b;
    printf("Max of %d and %d is %d\n", a, b, max_val); // Max is 10

    int age = 18;
    char *status = (age >= 18) ? "Adult" : "Minor"; // Using pointer to string literal
    printf("Status for age %d: %s\n", age, status); // %s for string

    return 0;
}
```

---

### **Relevant DSA (Conceptual Link):**

Operators are the bedrock of all algorithms:

* **Arithmetic Operators:** Used in virtually every algorithm involving numerical computation (e.g., calculating array indices, performing sums, averages, in complex mathematical algorithms like finding prime numbers or sorting algorithms).
* **Relational and Logical Operators:** Essential for all decision-making and control flow in algorithms.
    * `if` statements (e.g., checking if an element is found in search, comparing values in sorting).
    * `while` and `for` loop conditions (e.g., continuing a loop until a condition is met, iterating a specific number of times).
* **Increment/Decrement Operators:** Widely used in loops for iterating through collections (like arrays) or managing counters.
* **Bitwise Operators:** While specific, they are crucial for algorithms that manipulate data at the lowest level, such as hashing algorithms, compression, encryption (like the simple cryptography mentioned in CS50 Lecture 2), or efficient data packing.

Without operators, you cannot express the logic or computations that algorithms perform.

---

### **Homework Project:**

**Project 3: Operator Playground**

1.  **Simple Calculator:**
    * Declare two integer variables, `num1` and `num2`. Assign them any integer values.
    * Declare a `float` variable `result_float` and an `int` variable `result_int`.
    * Perform and print the results of the following operations:
        * Addition (`num1 + num2`)
        * Subtraction (`num1 - num2`)
        * Multiplication (`num1 * num2`)
        * Integer Division (`num1 / num2`) - store in `result_int`
        * Floating-point Division (e.g., `(float)num1 / num2`) - store in `result_float`. Make sure to cast at least one operand to `float` to get a floating-point result. Print with 2 decimal places.
        * Modulo (`num1 % num2`)

2.  **Age Eligibility Checker:**
    * Declare an integer variable `person_age`.
    * Using the **conditional (ternary) operator**, determine if the `person_age` is `18` or greater.
    * Print a message: "The person is eligible to vote." if `true`, otherwise "The person is not eligible to vote yet."

3.  **Increment/Decrement Puzzle:**
    * Declare two integer variables: `val1 = 10` and `val2 = 20`.
    * Declare two more integer variables: `output1`, `output2`.
    * Write the following statements and print the values of `val1`, `val2`, `output1`, and `output2` after each statement. Try to predict the output before running!
        * `output1 = ++val1;`
        * `output2 = val2--;`
        * `output1 = val1++;`
        * `output2 = --val2;`

**Self-Check/Debugging:**

* Are you using the correct operators for the desired operation?
* Are you careful with integer vs. floating-point division?
* Are you using parentheses to clarify operator precedence when necessary?
* Did you correctly apply the prefix vs. postfix increment/decrement logic?
