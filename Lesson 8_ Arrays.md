### **Lesson 8: Arrays**
-----

An **array** is a collection of elements of the **same data type**, stored at **contiguous memory locations**. This means that array elements are stored one after another in memory without any gaps, allowing for efficient access.

#### **1. Declaring Arrays**

To declare an array, you need to specify the data type of its elements and its size (the number of elements it can hold).

**Syntax:**
`data_type array_name[size];`

  * **`data_type`**: The type of elements the array will store (e.g., `int`, `float`, `char`, `double`).
  * **`array_name`**: The name you give to your array.
  * **`size`**: A constant integer expression that specifies the number of elements the array can hold. It must be a positive integer.

**Examples:**

```c
int numbers[5];        // An array named 'numbers' that can hold 5 integers
float temperatures[10]; // An array for 10 float temperatures
char name[20];         // An array for 20 characters (can store a string up to 19 chars + null terminator)
```

#### **2. Initializing Arrays**

Arrays can be initialized at the time of declaration.

  * **Partial Initialization:** If you provide fewer initializers than the size, the remaining elements are automatically initialized to zero for numeric types and null characters (`\0`) for character types.
    ```c
    int numbers[5] = {10, 20, 30}; // numbers[0]=10, numbers[1]=20, numbers[2]=30, numbers[3]=0, numbers[4]=0
    ```
  * **Initialization without Size:** If you initialize an array without specifying its size, the compiler automatically determines the size based on the number of initializers provided.
    ```c
    int scores[] = {85, 92, 78, 65}; // Compiler determines size is 4
    ```
  * **Designated Initializers (C99 and later):** You can initialize specific elements.
    ```c
    int arr[5] = {[2] = 100, [0] = 50}; // arr = {50, 0, 100, 0, 0}
    ```

#### **3. Accessing Array Elements**

Array elements are accessed using an **index** (or subscript). In C, array indices are **zero-based**, meaning the first element is at index 0, the second at index 1, and so on. If an array has `N` elements, their indices range from `0` to `N-1`.

**Syntax:**
`array_name[index]`

**Example:**

```c
int numbers[5] = {10, 20, 30, 40, 50};

printf("First element: %d\n", numbers[0]);   // Prints 10
printf("Third element: %d\n", numbers[2]);   // Prints 30

numbers[1] = 25; // Modify the second element
printf("Second element (modified): %d\n", numbers[1]); // Prints 25
```

**Important Note: Array Bounds Checking:** C does **not** perform automatic bounds checking at runtime. If you try to access `numbers[5]` or `numbers[-1]` in the example above, the compiler won't stop you, but it will lead to **undefined behavior** (your program might crash, produce incorrect results, or even introduce security vulnerabilities) because you're accessing memory outside the array's allocated space. It's the programmer's responsibility to ensure valid indices.

#### **4. Iterating Through Arrays**

Loops (especially `for` loops) are commonly used to process all elements of an array.

**Example:**

```c
#include <stdio.h>

int main() {
    int grades[] = {85, 90, 78, 92, 88};
    int num_grades = sizeof(grades) / sizeof(grades[0]); // Calculate number of elements

    printf("Student Grades:\n");
    for (int i = 0; i < num_grades; i++) {
        printf("Grade %d: %d\n", i + 1, grades[i]);
    }

    // Calculate sum
    int total_sum = 0;
    for (int i = 0; i < num_grades; i++) {
        total_sum += grades[i];
    }
    printf("Average grade: %.2f\n", (float)total_sum / num_grades);

    return 0;
}
```

**`sizeof` Operator for Array Size:**

  * `sizeof(array_name)` gives the total size of the array in bytes.
  * `sizeof(array_name[0])` gives the size of one element in bytes.
  * So, `total_size / size_of_one_element` gives the number of elements. This is a common idiom in C.

#### **5. Character Arrays and Strings**

In C, a **string** is simply a **character array terminated by a null character (`\0`)**. The null character signifies the end of the string.

**Declaring and Initializing Strings:**

  * **As a character array:**
    ```c
    char greeting[6] = {'H', 'e', 'l', 'l', 'o', '\0'}; // Explicit null terminator
    ```
  * **Using a string literal (most common):** The compiler automatically adds the null terminator.
    ```c
    char message[] = "World"; // Compiler determines size is 6 (W,o,r,l,d, \0)
    char city[10] = "Paris";   // Size 10, but only 6 bytes used, rest are \0
    ```

**Inputting Strings:**

  * **`scanf("%s", array_name)`:** Reads characters until whitespace is encountered. **Does not include spaces**. Puts a `\0` at the end. **Unsafe** if input is larger than array size.
  * **`fgets(array_name, size, stdin)`:** Reads a whole line (including spaces) up to `size-1` characters or until a newline. Includes the newline (`\n`) if read. **Safer**.
    ```c
    char name[50];
    printf("Enter your full name: ");
    fgets(name, sizeof(name), stdin); // Reads input including spaces and newline
    // Often you want to remove the trailing newline if fgets reads it:
    // name[strcspn(name, "\n")] = 0; // Requires #include <string.h>
    ```

**String-Handling Functions (from `<string.h>`):**

C provides a rich set of functions for working with strings in the `<string.h>` header file.

| Function            | Description                                                | Example                                     |
| :------------------ | :--------------------------------------------------------- | :------------------------------------------ |
| `strlen(s)`         | Returns the length of string `s` (excluding `\0`).       | `strlen("Hello")` is `5`                     |
| `strcpy(dest, src)` | Copies string `src` to `dest`. **Unsafe if `dest` is too small.** | `strcpy(name, "Alice");`                   |
| `strncpy(dest, src, n)` | Copies at most `n` characters from `src` to `dest`. **Safer, but be careful with `\0`.** | `strncpy(name, "Bob", 3);`                 |
| `strcat(dest, src)` | Appends string `src` to `dest`. **Unsafe if `dest` is too small.** | `strcat(s1, s2);` (`s1` becomes `s1s2`)    |
| `strncat(dest, src, n)` | Appends at most `n` characters from `src` to `dest`. **Safer.** | `strncat(s1, s2, 2);` (`s1` appends first 2 chars of `s2`) |
| `strcmp(s1, s2)`    | Compares strings `s1` and `s2` lexicographically.         | `0` if equal, `>0` if `s1 > s2`, `<0` if `s1 < s2` |
| `strncmp(s1, s2, n)` | Compares at most `n` characters of `s1` and `s2`.        |                                             |
| `strchr(s, c)`      | Returns a pointer to the first occurrence of character `c` in string `s`. |                                             |
| `strstr(s1, s2)`    | Returns a pointer to the first occurrence of string `s2` in string `s1`. |                                             |
| `strcspn(s1, s2)`   | Returns the length of the initial segment of `s1` which consists entirely of characters NOT in `s2`. Useful for removing `\n` after `fgets`. | `strcspn("Hello\n", "\n")` is `5`          |

-----

### **Relevant DSA (Conceptual Link):**

Arrays are one of the most fundamental **data structures** and are used in countless algorithms:

  * **Sequential Data Storage:** Arrays are perfect for storing lists of items (e.g., a list of student scores, a sequence of measurements, elements of a matrix).
  * **Implementing Other Data Structures:** Many other data structures are built *on top of* arrays, such as:
      * **Stacks:** Can be implemented using an array and a "top" index.
      * **Queues:** Can be implemented using an array and "front" and "rear" indices.
      * **Hash Tables:** Often use arrays internally to store elements.
  * **Algorithms:**
      * **Searching Algorithms (Linear, Binary):** Both operate on arrays.
      * **Sorting Algorithms (Bubble, Selection, Insertion, Merge, Quick Sort):** All perform their operations on arrays, rearranging elements.
      * **Dynamic Programming:** Often uses arrays (or multi-dimensional arrays) to store sub-problem solutions.
      * **Game Development:** Game maps can be represented as 2D arrays, inventories as 1D arrays, etc.
  * **Strings:** Essential for any text processing, parsing, or command-line interaction.

Understanding arrays is crucial because they are the building blocks for more complex data organization and almost every algorithm you'll encounter.

-----

### **Homework Project:**

**Project 8: Array Explorer**

1.  **Grade Analyzer:**

      * Declare an `int` array named `student_grades` of size 5.
      * Prompt the user to enter 5 grades, one by one, using a `for` loop and `scanf()`. Store them in the array.
      * After reading all grades, use another `for` loop to:
          * Calculate and print the **sum** of all grades.
          * Calculate and print the **average** grade (as a float, formatted to 2 decimal places).
          * Find and print the **highest** grade in the array. (Hint: Initialize a `max_grade` variable with a very small number or the first element).
          * Find and print the **lowest** grade in the array. (Hint: Initialize a `min_grade` variable with a very large number or the first element).

2.  **Reverse a String:**

      * Declare a `char` array `input_string` of size 100.
      * Prompt the user to "Enter a string (max 99 characters): ".
      * Read the string using `fgets()` and remember to remove the trailing newline (`name[strcspn(name, "\n")] = 0;`).
      * Calculate the length of the string using `strlen()`.
      * Create a new `char` array `reversed_string` of the same size.
      * Use a `for` loop to copy characters from `input_string` into `reversed_string` in reverse order. Make sure `reversed_string` is properly null-terminated.
      * Print both the original string and the reversed string.

3.  **Basic Contact List (Slightly Harder):**

      * Declare two `char` arrays:
          * `contact_names[3][50];` (A 2D array to store 3 names, each up to 49 characters + null)
          * `contact_numbers[3][15];` (A 2D array to store 3 phone numbers, each up to 14 characters + null)
      * Use a `for` loop to:
          * Prompt the user to enter a name for each of the 3 contacts. Read using `fgets()` and remove newline.
          * Prompt the user to enter a phone number for each of the 3 contacts. Read using `fgets()` and remove newline.
      * After collecting all data, use another `for` loop to print the entire contact list in a formatted way:
        ```
        Contact List:
        1. Name: [Name1], Phone: [Number1]
        2. Name: [Name2], Phone: [Number2]
        3. Name: [Name3], Phone: [Number3]
        ```

**Self-Check/Debugging:**

  * Are you accessing array elements using valid indices (0 to `size-1`)?
  * Are you correctly calculating the size of your arrays when iterating or when using `fgets`?
  * For strings, are you handling the null terminator (`\0`) correctly, especially when manually reversing or manipulating?
  * Did you include `<string.h>` for string functions like `strlen()` and `strcspn()`?
  * Are you using loops effectively to process array elements?
