### **Lesson 11: Dynamic Memory Allocation**
-----


Up until now, most of our variables (like `int x;` or `int arr[10];`) have been allocated on the **stack** or in the **static/global data segment**. This memory is automatically managed by the compiler and operating system.

  * **Stack:** Used for local variables, function parameters, and return addresses. Memory is allocated and deallocated automatically as functions are called and return. It's fast but limited in size.
  * **Static/Global Data Segment:** Used for global variables and static variables. Memory is allocated once when the program starts and remains throughout its execution.

However, sometimes you need memory for data whose size isn't known until the program is running (e.g., an array whose size depends on user input, or a linked list that grows and shrinks). For these cases, you use **dynamic memory allocation**, which allocates memory from the **heap**.

  * **Heap:** A large pool of free memory that can be allocated and deallocated at runtime by your program. This memory must be explicitly managed by the programmer.

The functions for dynamic memory allocation are part of the standard library (`<stdlib.h>`).

#### **1. `malloc()` (Memory Allocation)**

The `malloc()` function allocates a block of memory of a specified size in bytes and returns a `void` pointer to the beginning of the allocated block. If it fails to allocate memory, it returns `NULL`.

**Syntax:**
`void *malloc(size_t size);`

  * `size`: The number of bytes you want to allocate. `size_t` is an unsigned integer type often used for sizes and counts.
  * **Returns:** A `void *` pointer to the allocated memory, or `NULL` if allocation fails.

**Typical Usage:**
You usually cast the `void *` return value to the appropriate pointer type. It's good practice to multiply `sizeof(data_type)` by the number of elements you need.

```c
#include <stdio.h>
#include <stdlib.h> // Required for malloc, free

int main() {
    int *ptr;
    int num_elements = 5;

    // Allocate memory for 5 integers
    ptr = (int *)malloc(num_elements * sizeof(int));

    // Check if malloc was successful
    if (ptr == NULL) {
        printf("Memory allocation failed!\n");
        return 1; // Indicate an error
    }

    // Use the allocated memory (e.g., as an array)
    printf("Allocated memory for %d integers. Enter values:\n", num_elements);
    for (int i = 0; i < num_elements; i++) {
        printf("Enter element %d: ", i + 1);
        scanf("%d", &ptr[i]); // Use pointer like an array name
    }

    printf("Entered values: ");
    for (int i = 0; i < num_elements; i++) {
        printf("%d ", ptr[i]);
    }
    printf("\n");

    // Free the allocated memory when it's no longer needed
    free(ptr);
    ptr = NULL; // Good practice to set pointer to NULL after freeing
    printf("Memory freed.\n");

    return 0;
}
```

#### **2. `calloc()` (Contiguous Allocation)**

The `calloc()` function allocates a block of memory for a specified number of elements, each of a specified size, and **initializes all allocated bytes to zero**. It also returns a `void` pointer.

**Syntax:**
`void *calloc(size_t num_elements, size_t element_size);`

  * `num_elements`: The number of elements to allocate.
  * `element_size`: The size of each element in bytes.
  * **Returns:** A `void *` pointer to the allocated memory, or `NULL` on failure.

**Usage:** `calloc` is useful when you want the allocated memory to be pre-cleared to zero.

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *ptr;
    int num_elements = 3;

    // Allocate memory for 3 integers and initialize them to 0
    ptr = (int *)calloc(num_elements, sizeof(int));

    if (ptr == NULL) {
        printf("Memory allocation failed!\n");
        return 1;
    }

    printf("Values after calloc (should be 0):\n");
    for (int i = 0; i < num_elements; i++) {
        printf("%d ", ptr[i]); // Prints 0 0 0
    }
    printf("\n");

    free(ptr);
    ptr = NULL;

    return 0;
}
```

#### **3. `realloc()` (Re-allocation)**

The `realloc()` function is used to change the size of a previously allocated memory block. It can expand or shrink the block. It attempts to extend the existing block in place. If that's not possible, it allocates a new block, copies the contents of the old block to the new one, and then frees the old block.

**Syntax:**
`void *realloc(void *ptr, size_t new_size);`

  * `ptr`: A pointer to the memory block previously allocated by `malloc`, `calloc`, or `realloc`. If `ptr` is `NULL`, `realloc` behaves like `malloc`.
  * `new_size`: The new desired size of the memory block in bytes. If `new_size` is 0, `realloc` behaves like `free`.
  * **Returns:** A `void *` pointer to the reallocated memory block, or `NULL` if re-allocation fails (in which case the original block is *not* freed).

**Usage:** `realloc` is common when you have a dynamic array that needs to grow (or shrink).

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    int *arr;
    int initial_size = 2;
    int new_size = 5;

    // Allocate initial memory for 2 integers
    arr = (int *)malloc(initial_size * sizeof(int));
    if (arr == NULL) return 1;

    arr[0] = 10;
    arr[1] = 20;
    printf("Initial array: %d %d\n", arr[0], arr[1]);

    // Reallocate memory to hold 5 integers
    int *new_arr = (int *)realloc(arr, new_size * sizeof(int));

    if (new_arr == NULL) {
        printf("Reallocation failed!\n");
        free(arr); // Free original block if realloc failed
        return 1;
    }

    arr = new_arr; // Update arr to point to the potentially new memory location

    // Initialize new elements (old ones are preserved)
    arr[2] = 30;
    arr[3] = 40;
    arr[4] = 50;

    printf("Reallocated array: ");
    for (int i = 0; i < new_size; i++) {
        printf("%d ", arr[i]);
    }
    printf("\n");

    free(arr);
    arr = NULL;

    return 0;
}
```

**Crucial `realloc` Tip:** Always assign the result of `realloc` to a *temporary pointer first* (`new_arr` in the example). If `realloc` fails and returns `NULL`, your original pointer (`arr`) still points to the valid memory block, preventing a memory leak. If you directly assigned `arr = realloc(...)` and it returned `NULL`, you'd lose the reference to the original block.

#### **4. `free()` (Memory Deallocation)**

The `free()` function deallocates the memory block pointed to by `ptr`, returning it to the heap. It's crucial to `free` memory that you've dynamically allocated when it's no longer needed to prevent **memory leaks**.

**Syntax:**
`void free(void *ptr);`

  * `ptr`: A pointer to a memory block previously allocated by `malloc`, `calloc`, or `realloc`.
  * **Important:**
      * **Only `free` memory that was dynamically allocated.** Don't try to `free` stack variables or static variables.
      * **Don't `free` the same memory twice** (double free). This leads to undefined behavior.
      * After `free(ptr);`, it's good practice to set `ptr = NULL;` to prevent a "dangling pointer" (a pointer that points to deallocated memory).

#### **Summary of Memory Management Principles:**

  * **Allocate what you need, when you need it.**
  * **Always check for `NULL` return values** from `malloc`, `calloc`, `realloc`.
  * **Always `free` memory you've allocated** when it's no longer required. For every `malloc` (or `calloc`, `realloc`), there should be a corresponding `free`.
  * **Don't access freed memory** (dangling pointers). Set pointers to `NULL` after freeing.

-----

### **Relevant DSA (Conceptual Link):**

Dynamic memory allocation is the backbone for building flexible **dynamic data structures**:

  * **Linked Lists:** Each node in a linked list is typically a `struct` that is dynamically allocated using `malloc`. This allows the list to grow or shrink in size as needed, unlike arrays with fixed sizes.
  * **Trees (Binary Search Trees, AVL Trees, etc.):** Similar to linked lists, each node in a tree is usually a dynamically allocated `struct` containing data and pointers to its child nodes.
  * **Graphs:** Vertices and edges in a graph, especially adjacency lists, often rely on dynamic memory allocation.
  * **Dynamic Arrays (Vectors):** While C doesn't have a built-in dynamic array type like C++'s `std::vector`, you can implement one yourself using `malloc`, `realloc` to grow the underlying array, and `free`. This is essentially what the Challenge Problem from Lesson 10 asked you to do\!
  * **Generic Data Structures:** When you create data structures that can hold any type of data (using `void *`), dynamic allocation is used to size the memory correctly for the specific data being stored.
  * **Runtime Flexibility:** Allows algorithms to adapt to input data sizes not known at compile time, improving resource utilization.

Mastering dynamic memory allocation is essential for building efficient, scalable, and robust C applications that manage data effectively.

-----

### **Homework Project:**

**Project 11: Dynamic Memory Manager**

1.  **Dynamic Integer Array:**

      * Prompt the user to enter the desired number of integers (`N`).
      * Dynamically allocate an array of `N` integers using `malloc()`.
      * **Crucially, check if `malloc` returned `NULL`. If so, print an error message and exit the program.**
      * Use a `for` loop to prompt the user to enter `N` integer values and store them in the dynamically allocated array.
      * Print all the elements of the array.
      * Finally, `free()` the allocated memory.

2.  **String Duplicator:**

      * Write a function named `duplicateString()` that takes a `const char *source` (a pointer to the original string) as an argument.
      * Inside the function:
          * Calculate the length of the `source` string using `strlen()`.
          * Dynamically allocate memory for a new string using `malloc()` that is large enough to hold a copy of the `source` string **plus the null terminator**.
          * **Check for `NULL` return from `malloc`.**
          * Copy the `source` string into the newly allocated memory using `strcpy()`.
          * Return the `char *` (pointer) to the new duplicated string.
      * In your `main()` function:
          * Declare a `char` array (e.g., `char original[] = "Hello World";`).
          * Call `duplicateString()` to get a duplicated string.
          * Print both the original and duplicated strings.
          * **Remember to `free()` the memory returned by `duplicateString()` when you are done with it.**

3.  **Dynamic Contact List (Enhanced - Challenge Problem):**

      * **This builds directly on the Lesson 10 Challenge Problem.**
      * Use the `struct Product` (or `struct Contact` from earlier) definition.
      * Modify your menu-driven program to allow adding and viewing products/contacts.
      * **Refine the `realloc` logic:**
          * Start with an initial `capacity` of `1` or `2`.
          * When `num_products` equals `capacity`, **double the `capacity`** and use `realloc` to resize the `products` array. Implement `realloc` safely (assign to a temporary pointer first).
          * Implement a function `addProduct(struct Catalog *catalog_ptr)` that takes a pointer to your `Catalog` structure and handles adding a new product, including the `realloc` logic. This function should return `1` on success and `0` on failure (e.g., if `realloc` fails).
          * Implement a function `viewAllProducts(const struct Catalog *catalog_ptr)` that prints all products. Use `const` to indicate it doesn't modify the catalog.
      * **Add a "Delete Product by Name" Option (Harder still):**
          * Implement a function `deleteProduct(struct Catalog *catalog_ptr, const char *name_to_delete)`.
          * Inside this function, loop through the `products` array. If you find a product matching `name_to_delete` (using `strcmp`), you'll need to "remove" it. The common way in a dynamic array is to shift all subsequent elements one position back to "overwrite" the deleted one. Then decrement `num_products`.
          * Consider adding `realloc` to **shrink** the array if `num_products` falls significantly below `capacity` (e.g., if `num_products` is less than `capacity / 4`). This is more complex and optional for now.
          * Handle cases where the name is not found.
      * Ensure all dynamically allocated memory is `free()`d before exiting the program, especially in the `deleteProduct` function if you implement shrinking.

**Self-Check/Debugging:**

  * Are you including `<stdlib.h>` for `malloc`, `calloc`, `realloc`, `free`?
  * Are you **always checking the return value of `malloc` and `realloc` for `NULL`**?
  * Are you **`free`ing every block of memory that you `malloc` (or `calloc`/`realloc`)**? (This is a common source of memory leaks).
  * Are you assigning the result of `realloc` to a temporary pointer before assigning it back to the original pointer?
  * Are you avoiding "double free" or trying to `free` non-dynamically allocated memory?
  * For the string duplicator, did you allocate `strlen + 1` bytes for the null terminator?
  * For the challenge problem, is your `realloc` growth strategy sound? Is your deletion logic correct (shifting elements and decrementing `num_products`)?
