### **Lesson 12: File Input/Output (File I/O)**

-----

File I/O in C involves interacting with the operating system to perform read and write operations on files. This allows programs to store data permanently, process existing data files, or share data with other applications.

#### **1. File Pointer ( `FILE *` )**

To perform any operation on a file, you first need to open it. When a file is opened successfully, the operating system associates it with a `FILE` pointer. This `FILE` pointer (often called a file handle or stream pointer) is declared as `FILE *` and is used by all file I/O functions to identify the specific file you're working with.

You must include `<stdio.h>` for file operations.

#### **2. Opening a File (`fopen()`)**

The `fopen()` function is used to open a file. It returns a `FILE *` pointer if successful, or `NULL` if the file cannot be opened (e.g., file not found, permission denied).

**Syntax:**
`FILE *fopen(const char *filename, const char *mode);`

  * **`filename`**: A string (character array or pointer) containing the name of the file to open. This can include the full path.
  * **`mode`**: A string indicating the mode in which the file is to be opened.

**Common File Modes:**

| Mode | Description                                                                        | If File Exists | If File Doesn't Exist |
| :--- | :--------------------------------------------------------------------------------- | :------------- | :-------------------- |
| `"r"`  | **Read mode.** Opens a text file for reading.                                      | Read from      | Error (returns `NULL`) |
| `"w"`  | **Write mode.** Opens a text file for writing.                                     | Truncates (empties) | Creates new         |
| `"a"`  | **Append mode.** Opens a text file for writing, appending to the end.              | Append to      | Creates new         |
| `"r+"` | **Read/Write mode.** Opens a text file for both reading and writing.               | Read/Write     | Error (returns `NULL`) |
| `"w+"` | **Write/Read mode.** Opens a text file for both reading and writing.               | Truncates      | Creates new         |
| `"a+"` | **Append/Read mode.** Opens a text file for both reading and writing (appending). | Append/Read    | Creates new         |

**Binary Modes:** For binary files (e.g., images, executables), append 'b' to the mode (e.g., `"rb"`, `"wb"`, `"ab+"`). Text mode performs character conversions (like newline translation, which can be OS-dependent), while binary mode processes raw bytes. For most structured data, binary mode is preferred to avoid unexpected conversions.

**Crucial Check:** Always check if `fopen()` returns `NULL` before proceeding with file operations\!

**Example:**

```c
FILE *fptr;
fptr = fopen("example.txt", "w"); // Open for writing, will create or overwrite
if (fptr == NULL) {
    printf("Error opening file!\n");
    return 1; // Indicate error
}
// File is open, proceed with operations
```

#### **3. Closing a File (`fclose()`)**

After you are done with file operations, it is **essential** to close the file using `fclose()`. This flushes any buffered data to the disk, releases the file handle, and frees up system resources. Failing to close files can lead to data loss, resource leaks, and even program crashes.

**Syntax:**
`int fclose(FILE *fptr);`

  * **`fptr`**: The `FILE *` pointer returned by `fopen()`.
  * **Returns:** `0` on success, `EOF` (End Of File) on error.

**Example:**

```c
// ... after opening file and operations ...
fclose(fptr);
fptr = NULL; // Good practice to set to NULL after closing
```

#### **4. Writing to a File**

There are several functions to write data to a file:

  * **`fputc()` / `putc()` (Write a character):** Writes a single character to the file.
    ```c
    fputc('A', fptr);
    ```
  * **`fputs()` (Write a string):** Writes a null-terminated string to the file. It does **not** append a newline.
    ```c
    fputs("Hello, File!\n", fptr);
    ```
  * **`fprintf()` (Formatted write):** Works just like `printf()`, but writes to the specified file stream instead of standard output.
    ```c
    int value = 123;
    fprintf(fptr, "The value is: %d\n", value);
    ```

#### **5. Reading from a File**

Similarly, there are functions to read data from a file:

  * **`fgetc()` / `getc()` (Read a character):** Reads a single character from the file. Returns `EOF` if the end of the file is reached or an error occurs.
    ```c
    char ch = fgetc(fptr);
    if (ch != EOF) {
        // Process character
    }
    ```
  * **`fgets()` (Read a line):** Reads characters from the file until a newline character is encountered, `size-1` characters have been read, or the end of the file is reached. It includes the newline character if read. Returns `NULL` on error or End-Of-File.
    ```c
    char buffer[100];
    if (fgets(buffer, sizeof(buffer), fptr) != NULL) {
        printf("Read line: %s", buffer);
    }
    ```
  * **`fscanf()` (Formatted read):** Works just like `scanf()`, but reads from the specified file stream instead of standard input.
    ```c
    int number;
    char name[50];
    fscanf(fptr, "%d %s", &number, name);
    ```

#### **6. Checking for End-Of-File (`feof()`) and Errors (`ferror()`)**

  * **`feof(FILE *fptr)`**: Returns a non-zero value if the End-Of-File indicator has been set for the given stream. This is useful for checking if you've reached the end *after* an attempted read.
  * **`ferror(FILE *fptr)`**: Returns a non-zero value if the error indicator has been set for the given stream.

**Example of reading an entire file:**

```c
#include <stdio.h>
#include <stdlib.h> // For exit()

int main() {
    FILE *fptr;
    char ch;

    fptr = fopen("read_me.txt", "r");
    if (fptr == NULL) {
        perror("Error opening file"); // perror prints descriptive error based on errno
        exit(EXIT_FAILURE); // Defined in stdlib.h
    }

    printf("Contents of read_me.txt:\n");
    while ((ch = fgetc(fptr)) != EOF) { // Read character by character until EOF
        printf("%c", ch);
    }
    printf("\n");

    if (ferror(fptr)) {
        printf("Error reading file.\n");
    } else if (feof(fptr)) {
        printf("End of file reached.\n");
    }

    fclose(fptr);
    return 0;
}
```

#### **7. Standard Streams (`stdin`, `stdout`, `stderr`)**

C automatically opens three standard file streams for every program:

  * **`stdin`**: Standard input (usually keyboard). `scanf()` and `getchar()` read from `stdin`.
  * **`stdout`**: Standard output (usually console). `printf()` and `puts()` write to `stdout`.
  * **`stderr`**: Standard error (usually console). Used for error messages. `fprintf(stderr, ...)` is often used for errors so they aren't redirected if `stdout` is.

These are pre-opened `FILE *` pointers you can use directly.

#### **8. Random Access File I/O (`fseek()`, `ftell()`, `rewind()`)**

By default, file operations proceed sequentially. However, you can move the file pointer to a specific location within a file for random access.

  * **`fseek(FILE *fptr, long offset, int origin)`**: Moves the file pointer to a new location.
      * `fptr`: The file pointer.
      * `offset`: Number of bytes to move.
      * `origin`: Starting point for the offset. Can be `SEEK_SET` (beginning of file), `SEEK_CUR` (current position), or `SEEK_END` (end of file). (These are macros defined in `<stdio.h>`).
  * **`ftell(FILE *fptr)`**: Returns the current position of the file pointer (offset from the beginning of the file in bytes). Returns `-1L` on error.
  * **`rewind(FILE *fptr)`**: Sets the file pointer to the beginning of the file. Equivalent to `(void)fseek(fptr, 0L, SEEK_SET);`.

**Example:**

```c
#include <stdio.h>
#include <stdlib.h>

int main() {
    FILE *fptr;
    fptr = fopen("data.txt", "w+"); // Open for read/write
    if (fptr == NULL) return 1;

    fputs("ABCDEFGHIJKLMNOPQRSTUVWXYZ", fptr); // Write 26 characters
    long current_pos = ftell(fptr); // Should be 26
    printf("Current position: %ld\n", current_pos);

    // Move to 5th character (index 4) from beginning
    fseek(fptr, 4, SEEK_SET);
    char ch = fgetc(fptr); // Reads 'E'
    printf("Char at 5th position: %c\n", ch);

    rewind(fptr); // Go back to beginning
    ch = fgetc(fptr); // Reads 'A'
    printf("Char at beginning after rewind: %c\n", ch);

    fclose(fptr);
    return 0;
}
```

-----

### **Relevant DSA (Conceptual Link):**

File I/O is crucial for persisting and managing data in almost all real-world applications and often interacts with DSA concepts:

  * **Persistent Storage:** Any data structure (linked list, array, tree) that needs to exist beyond the program's runtime must be saved to a file. File I/O provides the mechanism for **serialization** (writing data structures to a file) and **deserialization** (reading them back).
  * **Database Systems:** At their core, databases rely on efficient file I/O to store and retrieve massive amounts of structured data.
  * **Logging and Auditing:** Programs often log their activities or errors to files for debugging or historical records.
  * **Data Processing Pipelines:** Reading large datasets from files, processing them using various algorithms and data structures in memory, and then writing results back to other files.
  * **Game Save/Load:** Saving game state (player position, inventory, world status) to a file and loading it back is a direct application of file I/O.
  * **File Formats:** Implementing custom file formats (e.g., for images, configurations, specific application data) requires precise control over reading and writing bytes or structured records using file I/O.
  * **Text Processing:** Reading text files line by line, parsing content, searching for patterns, and writing modified text are common tasks.

Without File I/O, programs would be limited to ephemeral in-memory data, greatly reducing their utility.

-----

### **Homework Project:**

**Project 12: File Manipulator**

1.  **Simple Log File:**

      * Open a file named `"activity.log"` in **append mode (`"a"`)**.
      * **Check for `NULL` return.**
      * Prompt the user to enter a short activity description (e.g., "Started program", "Processed data", "User logged out"). Use `fgets()` and remove newline.
      * Write the current date and time (you can just hardcode something like "2025-07-17 09:30:00 - " for now, or use `time()`/`localtime()` for a real timestamp if you want to be extra, but not required yet) followed by the user's activity description into the log file using `fprintf()`.
      * Close the file.
      * Run the program a few times to see entries accumulate in the log file.

2.  **Character Counter (Medium):**

      * Prompt the user to enter the name of a text file (e.g., `my_text.txt`).
      * Prompt the user to enter a single character to count.
      * Open the specified file in **read mode (`"r"`)**.
      * **Check for `NULL` return.** If the file doesn't exist or can't be opened, print "Error: File not found or cannot be opened." and exit.
      * Read the file character by character using `fgetc()` in a `while` loop until `EOF` is reached.
      * Count how many times the specified character appears in the file (case-sensitive).
      * Print the total count of the character.
      * Close the file.

3.  **Product Catalog to/from File (Challenge Problem - continuation of Lesson 10/11):**

      * **Objective:** Enhance your dynamic product catalog to save and load products from a file. This is crucial for persistent data.
      * Use your `struct Product` and `struct Catalog` definitions from the previous homework.
      * Add two new options to your main menu:
          * `4. Save Catalog to File`
          * `5. Load Catalog from File`
      * Modify your main `do-while` loop to handle these new choices.
      * **Implement `saveCatalog(const struct Catalog *catalog_ptr, const char *filename)` function:**
          * Opens `filename` in **write binary mode (`"wb"`)**.
          * **Checks for `NULL` return.**
          * Writes `catalog_ptr->num_products` as an `int` to the file first (so you know how many products to read back).
          * Then, use a `for` loop and `fwrite()` to write each `struct Product` from `catalog_ptr->products` directly to the file.
              * `size_t fwrite(const void *ptr, size_t size, size_t count, FILE *stream);`
              * `ptr`: Address of the data to write (e.g., `&catalog_ptr->products[i]`).
              * `size`: Size of each item to write (e.g., `sizeof(struct Product)`).
              * `count`: Number of items to write (e.g., `1` for one product at a time).
          * Close the file.
          * Print "Catalog saved successfully\!" or an error message.
      * **Implement `loadCatalog(struct Catalog *catalog_ptr, const char *filename)` function:**
          * Opens `filename` in **read binary mode (`"rb"`)**.
          * **Checks for `NULL` return.** If file not found, print "File not found. Creating empty catalog." and return `0` (failure but clean exit).
          * Reads the number of products (`num_products`) from the file using `fread()`.
              * `size_t fread(void *ptr, size_t size, size_t count, FILE *stream);`
              * `ptr`: Address to store read data (e.g., `&catalog_ptr->num_products`).
          * If `catalog_ptr->products` already has allocated memory (from a previous `addProduct` or `loadCatalog`), `free()` it first to avoid a memory leak.
          * Dynamically allocate memory for `catalog_ptr->products` using `malloc()` based on the `num_products` read from the file. Set `catalog_ptr->capacity = catalog_ptr->num_products`.
          * **Check for `NULL` from `malloc`.**
          * Use `fread()` in a loop to read all the `struct Product` data into `catalog_ptr->products`.
          * Close the file.
          * Print "Catalog loaded successfully\!" or an error.
          * Return `1` on success, `0` on failure.
      * **Important:** Remember to handle `free(catalog.products)` when the program exits (e.g., in your `Exit` case or `main`'s return).

**Self-Check/Debugging:**

  * Are you checking `fopen()` for `NULL` returns *every time* you open a file?
  * Are you `fclose()`ing files after you're done with them?
  * For the character counter, are you correctly looping until `EOF`?
  * For the challenge problem:
      * Are you opening files in the correct **binary mode** (`"wb"`, `"rb"`) for `fwrite` and `fread`?
      * Are `fwrite` and `fread` arguments (`ptr`, `size`, `count`) correctly specified?
      * Are you handling dynamic memory (`malloc`, `realloc`, `free`) correctly within the load/save functions? Especially `free`ing old `products` memory before `malloc`ing new in `loadCatalog` if needed.
      * Did you save the `num_products` count *before* saving the actual product data, so you know how much to read back?

This lesson opens up a whole new dimension for your C programs, allowing them to truly persist data. The challenge problem is significant, as it ties together almost everything we've learned\!

Once you've tackled this, we'll officially have covered the core C language fundamentals that form the bedrock of almost all C programming. After this, we can discuss branching out into more advanced C features (like `enums`, `unions`, `bit-wise` operations) or directly into some fundamental Data Structures and Algorithms if you prefer.