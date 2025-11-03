- [ ] 
### **Lesson 7: User-Defined Functions**


---


A **function** is a self-contained block of statements that performs a specific task. Think of it as a mini-program within your main program. Using functions offers several advantages:

* **Modularity:** Breaking down a large problem into smaller, independent functions makes the program easier to understand, design, and manage.
* **Reusability:** Once a function is written, it can be called multiple times from different parts of the program without rewriting the code.
* **Debugging:** It's easier to find and fix errors in smaller, isolated functions than in one large block of code.
* **Abstraction:** Functions allow you to focus on *what* a block of code does rather than *how* it does it.

#### **Core Concepts of Functions:**

1.  **Function Definition:**
    This is the actual implementation of the function. It tells the compiler what the function does.

    **Syntax:**
    ```c
    return_type function_name(parameter_list) {
        // body of the function
        // (statements to perform the task)
        return value; // Optional, if return_type is not void
    }
    ```
    * **`return_type`**: The data type of the value that the function sends back to the calling code. If a function does not return any value, its `return_type` is `void`.
    * **`function_name`**: A unique identifier for the function (e.g., `addNumbers`, `calculateArea`, `printMessage`).
    * **`parameter_list`**: A comma-separated list of variables along with their data types, representing the input values (arguments) the function expects. If a function takes no parameters, the list is empty (`()`) or can be `(void)`.
    * **`return` statement**: Used to send a value back to the caller. The type of the returned value must match `return_type`. If `return_type` is `void`, no `return` statement with a value is needed.

2.  **Function Declaration (Prototype):**
    A function prototype tells the compiler about a function *before* its actual definition, usually placed at the top of the file (before `main()`) or in a header file. This is necessary if a function is called before its definition appears in the code.

    **Syntax:**
    ```c
    return_type function_name(parameter_types); // Parameter names are optional in prototype
    ```
    * The `parameter_types` are crucial; parameter names are optional but often included for readability.
    * Always ends with a semicolon `;`.

3.  **Function Call:**
    To use a function, you "call" it from another part of the program (e.g., from `main()` or another function).

    **Syntax:**
    ```c
    function_name(arguments); // For functions returning void or if you don't need the return value
    result = function_name(arguments); // If the function returns a value
    ```
    * **`arguments`**: The actual values (expressions, variables, literals) passed to the function when it's called. These arguments must match the number, order, and type of the parameters defined in the function's parameter list.

#### **Types of Function Arguments (Parameters):**

Functions can handle arguments in a few ways, but the most common for basic types are:

* **Call by Value:**
    * When arguments are passed by value, a copy of the argument's actual value is passed to the function's parameter.
    * Any changes made to the parameter inside the function **do not affect** the original variable in the calling function.
    * This is the default way arguments are passed for fundamental data types (like `int`, `float`, `char`).

    ```c
    void increment(int x) { // x is a copy
        x = x + 1;
        printf("Inside function: x = %d\n", x);
    }

    int main() {
        int num = 10;
        printf("Before function call: num = %d\n", num); // num is 10
        increment(num); // Pass copy of num (10)
        printf("After function call: num = %d\n", num);  // num is still 10
        return 0;
    }
    ```

* **Call by Reference (using Pointers - will cover in detail later):**
    * When arguments are passed by reference, the *memory address* (pointer) of the original variable is passed to the function.
    * This allows the function to directly access and modify the original variable in the calling function.
    * We'll delve into this properly when we cover pointers, but it's essential for functions that need to change the values of variables passed into them.

#### **`main()` Function:**

The `main()` function is a special function in C. It's the **entry point** of every C program. Execution always begins from `main()`.

* It typically has a `return_type` of `int` and often takes `void` parameters or command-line arguments (`int argc, char *argv[]`).
* `return 0;` at the end of `main()` indicates successful program execution.

---

### **Examples:**

**Example 1: Function without arguments and without return value (`void`)**

```c
#include <stdio.h>

// Function prototype
void sayHello();

int main() {
    sayHello(); // Function call
    sayHello(); // Call it again!
    return 0;
}

// Function definition
void sayHello() {
    printf("Hello from a function!\n");
}
```

**Example 2: Function with arguments and without return value (`void`)**

```c
#include <stdio.h>

// Function prototype: print a message n times
void printNTimes(int n, char character);

int main() {
    printNTimes(3, '*'); // Call 1
    printf("---\n");
    printNTimes(5, '#'); // Call 2
    return 0;
}

// Function definition
void printNTimes(int n, char character) {
    for (int i = 0; i < n; i++) {
        printf("%c", character);
    }
    printf("\n"); // Newline after printing all characters
}
```

**Example 3: Function with arguments and with return value**

```c
#include <stdio.h>

// Function prototype: adds two integers and returns the sum
int add(int num1, int num2);

int main() {
    int a = 10, b = 20;
    int sum_result;

    sum_result = add(a, b); // Call add function, store returned value
    printf("Sum of %d and %d is: %d\n", a, b, sum_result);

    printf("Sum of 5 and 7 is: %d\n", add(5, 7)); // Call directly in printf

    return 0;
}

// Function definition
int add(int num1, int num2) {
    int result = num1 + num2;
    return result; // Return the sum
}
```

---

### **Relevant DSA (Conceptual Link):**

Functions are the cornerstone of modular and efficient algorithms:

* **Modular Design:** Any complex algorithm (like a sorting algorithm, a search algorithm, or a machine learning model) is typically broken down into smaller, specialized functions. For example, a sorting algorithm might have separate functions for `swapElements()`, `partition()`, or `merge()`.
* **Abstraction:** When you write a function, you're creating an abstraction. You can then use `sort(array)` without needing to know the exact steps of how it sorts. This is crucial for managing complexity in large programs.
* **Code Reusability:** If an operation is needed in multiple parts of an algorithm (e.g., calculating the distance between two points in a graphics program, or converting a string to lowercase), it's implemented once as a function and then called wherever needed.
* **Recursion:** A powerful problem-solving technique where a function calls itself. This is often used in algorithms that can be broken down into smaller, self-similar subproblems (e.g., tree traversals, some sorting algorithms like Merge Sort). We'll touch on recursion more later.

Without functions, programs quickly become monolithic, unreadable, and nearly impossible to debug or modify. They are essential for structured programming.

---

### **Homework Project:**

**Project 7: Function Junction**

1.  **Greeting Function:**
    * Write a function named `greetUser()` that takes no arguments and returns `void`.
    * Inside the function, prompt the user for their name (use `fgets` for safety) and then print "Hello, [Name]! Welcome to the function world!".
    * Call this `greetUser()` function from your `main()` function.

2.  **Calculator Function:**
    * Write a function named `calculateProduct()` that takes two integer arguments (`int num1`, `int num2`) and returns their product as an `int`.
    * In your `main()` function:
        * Prompt the user to enter two integers.
        * Read these integers using `scanf()`.
        * Call `calculateProduct()` with these two integers.
        * Print the returned product to the console.

3.  **Temperature Converter Function:**
    * Write a function named `celsiusToFahrenheit()` that takes one `float` argument (Celsius temperature) and returns the equivalent Fahrenheit temperature as a `float`.
        * *Formula:* `Fahrenheit = (Celsius * 9 / 5) + 32`
    * In your `main()` function:
        * Prompt the user to enter a temperature in Celsius.
        * Read this `float` value.
        * Call `celsiusToFahrenheit()` with the input.
        * Print both the original Celsius temperature and the calculated Fahrenheit temperature, formatted to two decimal places.

**Self-Check/Debugging:**

* Did you include `<stdio.h>` and `<string.h>` (for `fgets` and `strcspn`)?
* Did you provide function **prototypes** before `main()`?
* Are your function definitions correct (return type, name, parameters)?
* Are you calling your functions correctly, passing the right types of arguments?
* Are you using and printing the return values correctly?
* Did you handle the newline character when reading the name with `fgets`?

