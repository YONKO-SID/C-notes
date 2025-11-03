### **Lesson 6: Decision Making and Looping**


**Looping statements** (or iteration statements) are used to execute a block of code repeatedly as long as a certain condition is met.

#### **1. The `while` Loop**

The `while` loop is an entry-controlled loop. This means the condition is checked *before* the body of the loop is executed even once. If the condition is initially `false`, the loop body will never execute.

**Syntax:**
```c
while (condition) {
    // code to be executed repeatedly
    // (must contain something that eventually makes the condition false
    // to avoid an infinite loop)
}
```
* The `condition` is evaluated.
* If `true` (non-zero), the statements inside the `while` block are executed.
* After the block is executed, the `condition` is re-evaluated. This process continues until the `condition` becomes `false` (zero).
* If the condition is `false` from the start, the loop body is skipped.

#### **2. The `for` Loop**

The `for` loop is also an entry-controlled loop. It's typically used when you know in advance how many times you want to repeat a block of code, or when you need to initialize a counter, test a condition, and update the counter in one compact statement.

**Syntax:**
```c
for (initialization; condition; update) {
    // code to be executed repeatedly
}
```
* **`initialization`**: Executed once at the beginning of the loop. Often used to declare and initialize a loop counter variable (e.g., `int i = 0;`).
* **`condition`**: Evaluated before each iteration. If `true`, the loop body executes. If `false`, the loop terminates.
* **`update`**: Executed after each iteration of the loop body. Often used to increment or decrement the loop counter (e.g., `i++`, `count -= 1`).

All three parts are optional, but the semicolons are required. An infinite loop can be created with `for (;;)` (though `while(1)` is more common).

#### **3. The `do-while` Loop**

The `do-while` loop is an exit-controlled loop. This means the condition is checked *after* the body of the loop has been executed at least once. This guarantees that the loop body will run at least one time, even if the condition is initially `false`.

**Syntax:**
```c
do {
    // code to be executed repeatedly
    // (must contain something that eventually makes the condition false)
} while (condition); // Semicolon is required here!
```
* The statements inside the `do` block are executed first.
* Then, the `condition` is evaluated.
* If `true`, the loop repeats from the `do` block.
* If `false`, the loop terminates.

#### **4. Jump Statements (within Loops)**

These statements alter the flow of execution within loops.

* **`break` Statement:**
    * Immediately terminates the innermost loop (or `switch` statement) it is contained within.
    * Execution continues with the statement immediately following the loop.
    * You've seen `break` in `switch` statements to prevent fall-through.

* **`continue` Statement:**
    * Skips the rest of the current iteration of the innermost loop.
    * Execution jumps to the loop's update expression (for `for` loops) or directly to the condition check (for `while`/`do-while` loops) for the next iteration.

* **`goto` Statement (Avoid where possible):**
    * Provides an unconditional jump from one part of the program to another, specified by a label.
    * **Syntax:** `goto label;` and `label: statement;`
    * While it exists in C, `goto` is generally **discouraged** in modern programming practices as it can lead to "spaghetti code" that is difficult to read, debug, and maintain. Structured control flow (loops, `if`, functions) should almost always be preferred. You will see it occasionally in very low-level or highly optimized code, but for learning and general programming, avoid it.

---

### **Examples:**

**Example 1: `while` Loop (Countdown)**

```c
#include <stdio.h>

int main() {
    int count = 5;

    printf("--- While Loop (Countdown) ---\n");
    while (count > 0) {
        printf("%d...\n", count);
        count--; // Decrement to eventually make condition false
    }
    printf("Blast off!\n\n");

    return 0;
}
```

**Example 2: `for` Loop (Sum of N numbers)**

```c
#include <stdio.h>

int main() {
    int n, sum = 0, i;

    printf("--- For Loop (Sum of N Numbers) ---\n");
    printf("Enter a positive integer: ");
    scanf("%d", &n);

    for (i = 1; i <= n; i++) {
        sum += i; // sum = sum + i;
    }

    printf("Sum of numbers from 1 to %d is: %d\n\n", n, sum);

    return 0;
}
```

**Example 3: `do-while` Loop (Simple Menu with "Play Again")**

```c
#include <stdio.h>

int main() {
    char choice;

    printf("--- Do-While Loop (Play Again) ---\n");
    do {
        printf("Playing a game...\n");
        printf("Do you want to play again? (y/n): ");
        // Important: Use " %c" to consume any leftover newline from previous input
        scanf(" %c", &choice);
    } while (choice == 'y' || choice == 'Y');

    printf("Game Over. Thanks for playing!\n\n");

    return 0;
}
```

**Example 4: `break` and `continue` in `for` loop**

```c
#include <stdio.h>

int main() {
    printf("--- Break and Continue Example ---\n");

    // Break example: Loop to 10, but stop at 5
    printf("Break Example (numbers 1 to 4):\n");
    for (int i = 1; i <= 10; i++) {
        if (i == 5) {
            break; // Exit the loop when i is 5
        }
        printf("%d ", i);
    }
    printf("\n\n");

    // Continue example: Loop to 10, but skip 5
    printf("Continue Example (numbers 1 to 10, skipping 5):\n");
    for (int i = 1; i <= 10; i++) {
        if (i == 5) {
            continue; // Skip the rest of this iteration when i is 5
        }
        printf("%d ", i);
    }
    printf("\n\n");

    return 0;
}
```

---

### **Relevant DSA (Conceptual Link):**

Loops are the **engine** of almost all algorithms and data structures:

* **Iteration and Traversal:**
    * **Arrays/Lists:** Loops are used to visit every element in an array (e.g., printing all elements, finding min/max, summing values).
    * **Searching:** Linear search involves looping through a collection until an item is found.
    * **Sorting:** Most sorting algorithms (Bubble Sort, Selection Sort, Insertion Sort) rely heavily on nested loops to compare and swap elements.
* **Repetitive Computations:**
    * Calculating factorials, Fibonacci sequences, or sums of series.
    * Simulations often involve loops to advance time steps or iterations.
* **Game Loops:** As discussed, the core of any game is an infinite loop (`while(1)`) that continuously updates game state, handles input, and renders graphics.
* **Input Processing:** Reading multiple lines of input or processing data until an end-of-file marker is reached often uses loops.

Loops, combined with decision-making statements, give programs their dynamic and powerful capabilities.

---

### **Homework Project:**

**Project 6: Loop Mastery**

1.  **Count Down and Up:**
    * Prompt the user to "Enter a starting number: ".
    * Read the integer.
    * Use a `while` loop to count down from that number to 1, printing each number.
    * After the `while` loop, use a `for` loop to count up from 1 to the starting number, printing each number. Add a newline between the countdown and count-up lists.

2.  **Password Retries:**
    * Declare a `const char *password = "secret123";` (we'll cover strings and pointers more later, but this works for now, use `%s` for input).
    * Implement a `do-while` loop that gives the user **3 attempts** to enter the correct password.
    * Inside the loop, prompt the user for the password and read it using `scanf("%s", user_input_array);` (declare `char user_input_array[50];`).
    * Use `strcmp(user_input_array, password) == 0` (you'll need to `#include <string.h>`) to compare the passwords.
    * If correct, print "Login Successful!" and `break` out of the loop.
    * If incorrect, print "Incorrect password. Attempts left: X" (where X decreases).
    * After the loop, if the user ran out of attempts, print "Account locked. Too many failed attempts."

3.  **Even Numbers Only:**
    * Use a `for` loop to iterate from 1 to 20.
    * Inside the loop, use an `if` statement with the `continue` keyword to **skip** printing odd numbers.
    * Print only the even numbers on a single line, separated by spaces.

**Self-Check/Debugging:**

* Are your loop conditions correctly set to terminate the loop? (Avoid infinite loops!)
* Is your loop counter or control variable being updated inside `while` and `do-while` loops?
* Did you use the correct type of loop for each problem (e.g., `do-while` for "at least once" scenarios)?
* Did you correctly use `break` and `continue` to alter loop flow?
* Remember to include `<string.h>` for `strcmp`.

This lesson, combined with the previous one, unlocks the ability to write truly dynamic and interactive programs. The Mario problem from CS50 Week 1 relies heavily on these looping concepts.

