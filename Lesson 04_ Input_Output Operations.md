### **Lesson 4: Input/Output Operations**


---

In C, input and output operations are handled by a set of library functions available in the `<stdio.h>` header file.

#### **1. Formatted Output: `printf()` Function**

You've been using `printf()` since our first "Hello, World!" program. It's a versatile function for displaying formatted output on the standard output device (usually the screen).

**Syntax:**
`printf("format string", arg1, arg2, ...);`

* **`format string`**: A string literal that can contain:
    * **Plain characters:** Printed as they are.
    * **Escape sequences:** Special characters like `\n` (newline), `\t` (tab), `\\` (backslash), `\"` (double quote), `\'` (single quote).
    * **Format specifiers (or conversion specifiers):** Start with `%` and indicate the type of data to be printed. These act as placeholders for the values of variables or expressions provided as arguments.

* **`arg1, arg2, ...`**: The variables or expressions whose values are to be printed, matching the order and type of the format specifiers in the format string.

**Common Format Specifiers for `printf()`:**

| Specifier | Used For                          | Example Output                     |
| :-------- | :-------------------------------- | :--------------------------------- |
| `%d`      | Signed decimal integer            | `123`, `-45`                       |
| `%i`      | Signed decimal integer (same as `%d`) | `123`, `-45`                       |
| `%u`      | Unsigned decimal integer          | `123`, `65535`                     |
| `%f`      | Floating-point number (float, double) | `3.140000`, `123.456789`           |
| `%.Xf`    | Floating-point with X decimal places | `3.14` (for `%.2f`)                |
| `%lf`     | Used for `double` (though `%f` works for printing `double` too) | `123.456789`                     |
| `%c`      | Single character                  | `'A'`, `'z'`                       |
| `%s`      | String                            | `"Hello World"`                    |
| `%o`      | Octal integer                     | `012` (decimal 10 prints as `12`)  |
| `%x`      | Hexadecimal integer (lowercase)   | `0x1A` (decimal 26 prints as `1a`) |
| `%X`      | Hexadecimal integer (uppercase)   | `0x1A` (decimal 26 prints as `1A`) |
| `%%`      | Prints a literal `%` character    | `%`                                |

**Field Width and Precision (for `printf`):**

You can control the width of the output field and the precision for floating-point numbers.

* `%[width]d`: Specifies minimum field width. If the number is smaller, it's padded with spaces.
    * `printf("%5d", 123);` // Output: `  123` (2 spaces before 123)
* `%[width].[precision]f`: For floats. `width` is total characters, `precision` is decimal places.
    * `printf("%8.2f", 3.14159);` // Output: `    3.14` (4 spaces before 3.14)
* `%[width]s`: For strings.
    * `printf("%10s", "hello");` // Output: `     hello` (5 spaces before hello)

#### **2. Formatted Input: `scanf()` Function**

The `scanf()` function reads formatted input from the standard input device (usually the keyboard).

**Syntax:**
`scanf("format string", &arg1, &arg2, ...);`

* **`format string`**: Contains format specifiers (similar to `printf()`) to indicate the type of input expected.
* **`&arg1, &arg2, ...`**: The **addresses** of the variables where the input data will be stored. The `&` (address-of) operator is crucial here. It tells `scanf()` *where* in memory to put the value it reads. Without `&` for non-array variables, your program will likely crash or behave unexpectedly.

**Common Format Specifiers for `scanf()`:**

| Specifier | Used For                 |
| :-------- | :----------------------- |
| `%d`      | Decimal integer          |
| `%i`      | Integer (detects base)   |
| `%u`      | Unsigned decimal integer |
| `%f`      | Float                    |
| `%lf`     | Double (important for `scanf` to distinguish `double` from `float`) |
| `%c`      | Single character         |
| `%s`      | String (reads up to whitespace) |
| `%o`      | Octal integer            |
| `%x`      | Hexadecimal integer      |

**Important Considerations for `scanf()`:**

* **Address-of Operator (`&`):** Always use `&` before the variable name for `int`, `float`, `char`, `double`, etc., when using `scanf()`, because `scanf()` needs the memory address to store the value. **Strings (character arrays) are an exception;** when reading into a character array using `%s`, you typically *don't* use `&` because the array name itself decays into a pointer (its starting address). We'll cover this more with arrays.
* **Whitespace:** `scanf()` skips leading whitespace (spaces, tabs, newlines) for all specifiers except `%c`.
* **Multiple Inputs:** When reading multiple values with `scanf()`, you can separate the inputs with spaces, tabs, or newlines.
    * `scanf("%d %f", &myInt, &myFloat);`
* **Return Value:** `scanf()` returns the number of items successfully read. You can check this return value for error handling. A return value of `EOF` (End Of File) indicates an error or end of input.

#### **3. Character Input/Output Functions**

For single character input/output, C provides `getchar()` and `putchar()`.

* **`int getchar()`:** Reads a single character from the standard input. It returns the character read (as an `int`) or `EOF` on end-of-file or error.
    * `char ch = getchar();`

* **`int putchar(int char_to_write)`:** Writes a single character to the standard output. It returns the character written or `EOF` on error.
    * `putchar('A');`

#### **4. String Input/Output Functions**

For string input/output, C provides `gets()` (generally unsafe) and `fgets()` (safer).

* **`char *gets(char *str)`:** Reads a line from `stdin` into the buffer pointed to by `str` until a newline character or `EOF` is encountered. The newline character is replaced with a null character (`\0`).
    * **WARNING:** `gets()` is **highly dangerous** because it doesn't check for buffer overflows. If the input string is longer than the buffer you provide, it will write beyond the allocated memory, leading to security vulnerabilities and crashes. **Avoid using `gets()` in new code.**

* **`char *fgets(char *str, int size, FILE *stream)`:** Reads at most `size-1` characters from the `stream` (e.g., `stdin`) and stores them into the buffer pointed to by `str`. Reading stops after a newline character, `EOF`, or `size-1` characters have been read. The newline character *is* included in the string, if read. A null character `\0` is appended.
    * **`str`**: Pointer to the buffer where the string will be stored.
    * **`size`**: Maximum number of characters to read (including the null terminator).
    * **`stream`**: The input stream (use `stdin` for keyboard input).
    * `char name[100]; fgets(name, sizeof(name), stdin);` **This is the preferred way to read strings safely.**

---

### **Examples:**

**Example 1: Using `scanf()` for various data types**

```c
#include <stdio.h> // Required for printf and scanf

int main() {
    int age;
    float height;
    char initial;
    char name[50]; // Character array to store a string

    printf("Enter your age: ");
    scanf("%d", &age); // Read an integer
    printf("You entered age: %d\n\n", age);

    printf("Enter your height (in meters, e.g., 1.75): ");
    scanf("%f", &height); // Read a float
    printf("You entered height: %.2f meters\n\n", height);

    // Clear the input buffer before reading a char/string after a number
    // This is important because scanf("%f", ...) leaves the newline character
    // in the buffer, which would immediately be consumed by the next %c or fgets.
    while (getchar() != '\n'); // Consume remaining characters including newline

    printf("Enter your first initial: ");
    scanf("%c", &initial); // Read a single character
    printf("You entered initial: %c\n\n", initial);

    // Clear buffer again if necessary, though after %c it's usually clear
    while (getchar() != '\n');

    - [ ] printf("Enter your full name (use underscores for spaces, e.g., John_Doe, for scanf demo): ");
    scanf("%s", name); // Read a string (stops at first whitespace)
    printf("Hello, %s!\n\n", name);

    // A better way to read strings with spaces: fgets
    printf("Enter your full name (with spaces, using fgets): ");
    // Clear the buffer from previous scanf("%s") if there's still input.
    // If not, fgets might read previous newline.
    // while (getchar() != '\n'); // Uncomment if you face issues after scanf("%s")
    fgets(name, sizeof(name), stdin); // Read a line including spaces
    // fgets includes the newline, so you might want to remove it
    name[strcspn(name, "\n")] = 0; // Remove trailing newline if present (requires <string.h>)
    printf("Nice to meet you, %s!\n\n", name);


    return 0;
}
```

**Note on `scanf()` and `getchar()` after numeric input:**
When you use `scanf()` to read numbers (`%d`, `%f`, etc.), it reads the number but leaves the newline character (`\n`) that you press (after typing the number and hitting Enter) in the input buffer. If you then try to read a character (`%c`) or a string (`%s` or `fgets`) immediately after, that pending newline might be consumed unexpectedly. The `while (getchar() != '\n');` loop is a common way to "clear" the input buffer by consuming all remaining characters up to and including the newline.

**Example 2: Using `getchar()` and `putchar()`**

```c
#include <stdio.h>

int main() {
    char ch;

    printf("Enter a character: ");
    ch = getchar(); // Read a character

    printf("You entered: ");
    putchar(ch);    // Print the character
    putchar('\n');  // Print a newline

    return 0;
}
```

---

### **Relevant DSA (Conceptual Link):**

Input/Output operations are the bridge between your algorithm and the real world:

* **Data Acquisition:** All algorithms that process data (like sorting a list of numbers, searching for an item, encrypting text) first need to get that data. `scanf()` and `fgets()` are how you'll typically feed initial data into your C programs.
* **Result Presentation:** Once an algorithm finishes its computation, `printf()` is used to display the results in a user-friendly format. This is how you'll demonstrate the output of your search algorithm, the sorted array, or the encrypted message.
* **Interactive Algorithms:** For interactive programs (like simple games or command-line tools), input functions allow the user to control the program's flow or provide runtime data, while output functions provide feedback.

These functions are fundamental for making your programs useful and testable.

---

### **Homework Project:**

**Project 4: Interactive Bio Data and Simple Calculations**

1.  **Dynamic Bio Data Program:**
    * Modify your previous "Personal Information Program" from Homework 2.
    * Instead of hardcoding your `age`, `height`, `first_initial`, and `net_worth`, prompt the user to **enter each of these values** using `printf()` and read them using `scanf()`.
    * For the `first_initial`, ensure you handle the newline character left in the buffer by a previous `scanf()` (use the `while (getchar() != '\n');` trick before `scanf("%c", ...)`).
    * For `net_worth`, use `%lf` with `scanf()`.
    * After reading all inputs, print a summary of the entered data in a formatted way, just like your previous homework.

2.  **Simple Unit Converter (Interactive):**
    * Prompt the user to enter a distance in **kilometers**.
    * Read the `float` value using `scanf()`.
    * Convert the kilometers to miles ($1 \text{ km} = 0.621371 \text{ miles}$).
    * Print the result, displaying the miles with **4 decimal places**.

3.  **Character Repetition:**
    * Prompt the user to "Enter a character to repeat: ".
    * Read a single character using `getchar()`.
    * Prompt the user to "Enter how many times to repeat (integer): ".
    * Read an integer using `scanf()`.
    * Using a simple `for` loop (you'll formally learn loops soon, but you can guess how `for (int i = 0; i < count; i++)` works, or just print it multiple times manually for now if loops are too new), print the character that many times on a single line using `putchar()`. Don't forget a newline at the end.

**Self-Check/Debugging:**

* Did you include `<stdio.h>`?
* Did you use the `&` (address-of) operator for all non-array variables in `scanf()`?
* Are you using the correct format specifiers (`%d`, `%f`, `%c`, `%lf`, `%s`) for both `printf()` and `scanf()`?
* Did you handle the leftover newline character in the input buffer when switching from reading numbers/strings to reading single characters with `scanf("%c")`?
* Does your program correctly read *all* the inputs you provide?

.