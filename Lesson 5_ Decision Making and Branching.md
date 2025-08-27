 ### **Lesson 5: Decision Making and Branching**

In C programming, **decision-making statements** are used to control the flow of program execution based on certain conditions. A condition is an expression that evaluates to either `true` (non-zero) or `false` (zero).

#### **1. The `if` Statement**

The `if` statement is the simplest decision-making statement. It executes a block of code only if a specified condition is `true`.

**Syntax:**
```c
if (condition) {
    // code to be executed if condition is true
}
```
* The `condition` is an expression (often using relational or logical operators) that results in a non-zero (true) or zero (false) value.
* If the condition is `true`, the statements inside the curly braces `{}` (the `if` block) are executed.
* If the condition is `false`, the `if` block is skipped, and execution continues with the code immediately following the `if` statement.
* If there's only a single statement to execute in the `if` block, the curly braces are optional, but it's good practice to always use them for clarity and to avoid errors when adding more statements later.

#### **2. The `if-else` Statement**

The `if-else` statement allows your program to execute one block of code if a condition is `true` and a different block of code if the condition is `false`.

**Syntax:**
```c
if (condition) {
    // code to be executed if condition is true
} else {
    // code to be executed if condition is false
}
```
* If the `condition` is `true`, the `if` block is executed.
* If the `condition` is `false`, the `else` block is executed.
* Only one of the two blocks will ever be executed.

#### **3. The `else-if` Ladder (or `if-else if` statement)**

When you have multiple conditions to check in a sequence, and each condition leads to a different action, an `else-if` ladder is used. The conditions are evaluated from top to bottom. As soon as a `true` condition is found, its corresponding block of code is executed, and the rest of the ladder is skipped. If none of the conditions are `true`, the final `else` block (if present) is executed.

**Syntax:**
```c
if (condition1) {
    // code to be executed if condition1 is true
} else if (condition2) {
    // code to be executed if condition2 is true
} else if (condition3) {
    // code to be executed if condition3 is true
} else {
    // code to be executed if none of the above conditions are true
}
```
* You can have as many `else if` blocks as needed.
* The final `else` block is optional.

#### **4. The `switch` Statement**

The `switch` statement is a multi-way decision statement that provides an easier way to dispatch execution to different parts of code based on the value of a single integer expression. It's often used as an alternative to a long `else-if` ladder when comparing a single variable against multiple constant values.

**Syntax:**
```c
switch (expression) {
    case constant1:
        // code to be executed if expression == constant1
        break; // Exits the switch
    case constant2:
        // code to be executed if expression == constant2
        break;
    // ... more cases
    default:
        // code to be executed if expression doesn't match any case
        break; // Optional for the last block
}
```
* **`expression`**: An integer expression (e.g., an `int` variable, a `char` variable, or an expression that evaluates to an integer).
* **`case constantN`**: `constantN` must be an integer constant or a constant expression (e.g., `'A'`, `5`, `100`).
* **`break` statement**: This is crucial! It causes execution to jump out of the `switch` statement after a `case` block is executed. **Without `break`, execution will "fall through"** to the next `case` block, even if its constant doesn't match the expression, which is a common source of bugs (but can also be intentionally used for specific logic).
* **`default` case**: This is optional. If present, its code is executed if the `expression` does not match any of the `case` constants. It acts like the `else` in an `if-else if` ladder.

---

### **Examples:**

**Example 1: `if` and `if-else` Statement**

```c
#include <stdio.h>

int main() {
    int number;

    printf("Enter an integer: ");
    scanf("%d", &number);

    // Using if statement
    if (number > 0) {
        printf("You entered a positive number.\n");
    }

    // Using if-else statement
    if (number % 2 == 0) { // Check if divisible by 2 (even)
        printf("The number is even.\n");
    } else {
        printf("The number is odd.\n");
    }

    return 0;
}
```

**Example 2: `else-if` Ladder**

```c
#include <stdio.h>

int main() {
    int score;

    printf("Enter your score (0-100): ");
    scanf("%d", &score);

    if (score >= 90 && score <= 100) {
        printf("Grade: A\n");
    } else if (score >= 80 && score < 90) {
        printf("Grade: B\n");
    } else if (score >= 70 && score < 80) {
        printf("Grade: C\n");
    } else if (score >= 60 && score < 70) {
        printf("Grade: D\n");
    } else if (score >= 0 && score < 60) {
        printf("Grade: F\n");
    } else {
        printf("Invalid score entered.\n");
    }

    return 0;
}
```

**Example 3: `switch` Statement**

```c
#include <stdio.h>

int main() {
    char grade_char;

    printf("Enter a letter grade (A, B, C, D, F): ");
    scanf(" %c", &grade_char); // Note the space before %c to consume leftover newline

    switch (grade_char) {
        case 'A':
        case 'a': // Allows both uppercase and lowercase
            printf("Excellent!\n");
            break;
        case 'B':
        case 'b':
            printf("Very Good!\n");
            break;
        case 'C':
        case 'c':
            printf("Good.\n");
            break;
        case 'D':
        case 'd':
            printf("Pass.\n");
            break;
        case 'F':
        case 'f':
            printf("Fail.\n");
            break;
        default:
            printf("Invalid grade entered.\n");
            break;
    }

    return 0;
}
```
**Important note on `scanf(" %c", &grade_char);`:** The space before `%c` in `scanf()` is a trick to consume any leftover whitespace characters (like the newline `\n` from a previous `Enter` press) in the input buffer. This prevents the `scanf("%c")` from immediately reading the newline as the character input.

---

### **Relevant DSA (Conceptual Link):**

Decision-making and branching are absolutely fundamental to **every algorithm**:

* **Conditional Logic:** Algorithms constantly make decisions based on data.
    * **Searching:** "If the current element matches the target, stop."
    * **Sorting:** "If element A is greater than element B, swap them."
    * **Graph Traversal:** "If this path has not been visited, explore it."
    * **Game AI:** "If enemy is close, attack; else, patrol."
* **Error Handling and Validation:** Programs need to check for valid input or unexpected conditions. `if` statements are used to validate user input, check for file opening errors, or prevent division by zero.
* **Control Flow:** They dictate the path of execution. Without them, a program would simply execute line by line without any dynamic behavior.

Essentially, any time an algorithm needs to respond differently to different inputs or states, it uses decision-making statements.

---

### **Homework Project:**

**Project 5: Decision Maker**

1.  **Positive, Negative, or Zero Checker:**
    * Prompt the user to enter an integer.
    * Use an `if-else if-else` ladder to determine if the number is:
        * Positive
        * Negative
        * Zero
    * Print an appropriate message for each case.

2.  **Simple User Authentication:**
    * Declare a `const int` variable for a `PIN = 1234`.
    * Prompt the user to "Enter your 4-digit PIN: ".
    * Read the user's input into an `int` variable using `scanf()`.
    * Use an `if-else` statement to check if the entered PIN matches the `const PIN`.
    * If it matches, print "Access Granted!".
    * If it does not match, print "Access Denied. Incorrect PIN.".

3.  **Basic Menu System using `switch`:**
    * Display a menu to the user:
        ```
        Choose an option:
        1. Say Hello
        2. Say Goodbye
        3. Display Current Date
        ```
    * Prompt the user to "Enter your choice (1-3): ".
    * Read the user's choice into an integer variable.
    * Use a `switch` statement to execute different actions based on the choice:
        * Case 1: Print "Hello there!"
        * Case 2: Print "Goodbye for now!"
        * Case 3: Print "Today's date is: [Current_Date_Placeholder]" (You can just type "June 14, 2025" or similar here, as we haven't covered date functions yet).
        * Default: Print "Invalid choice. Please enter 1, 2, or 3."
    * **Ensure you use `break` statements correctly!**

**Self-Check/Debugging:**

* Are your conditions logically sound (e.g., `&&` vs `||`)?
* Are you handling edge cases (like zero in the number checker)?
* Are you using `break` statements correctly in your `switch` statement to prevent fall-through?
* Did you remember the `&` for `scanf()` and the correct format specifiers?

