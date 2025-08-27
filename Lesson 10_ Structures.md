### **Lesson 10: Structures**

-----

A **structure** (`struct`) in C is a user-defined data type that allows you to combine items of **different data types** into a single unit. Unlike arrays, which store elements of the same type, a structure can hold elements of various types (e.g., an `int`, a `float`, and a `char` array) that are logically related.

Think of it like a blueprint for a custom record or object. For instance, you could define a `struct` to represent a `Student` with members like `name` (a character array), `roll_number` (an integer), and `gpa` (a float).

#### **1. Defining a Structure**

You first define the "blueprint" of the structure. This definition does not allocate any memory; it just describes the template.

**Syntax:**

```c
struct tag_name {
    data_type member1;
    data_type member2;
    // ... more members
}; // Don't forget the semicolon!
```

  * **`struct`**: Keyword indicating a structure definition.
  * **`tag_name`**: An optional (but highly recommended) name for the structure. This name can then be used to declare variables of this structure type.
  * **`member1`, `member2`**: Variables (of any data type, including other structures or arrays) that belong to this structure. These are called **members** or **fields**.

**Example:**

```c
struct Book {
    char title[100];
    char author[50];
    int pages;
    float price;
}; // Structure definition - no memory allocated yet
```

#### **2. Declaring Structure Variables**

Once a structure type is defined, you can declare variables of that type.

**Syntax:**

```c
struct tag_name variable_name;
// Or multiple variables:
struct tag_name var1, var2, var3;
```

**Example:**

```c
struct Book book1; // Declares a variable 'book1' of type 'struct Book'
struct Book book2, book3; // Declares two more variables
```

You can also declare structure variables directly when defining the structure (though less common for reusability):

```c
struct Point {
    int x;
    int y;
} p1, p2; // p1 and p2 are variables of type struct Point
```

#### **3. Initializing Structure Variables**

Structure variables can be initialized during declaration, similar to arrays.

  * **Member-wise initialization (in order):**
    ```c
    struct Book book1 = {"The C Programming Language", "Kernighan & Ritchie", 272, 35.99};
    ```
  * **Designated initializers (C99 and later):** Recommended for clarity, especially with many members.
    ```c
    struct Book book2 = {
        .pages = 400,
        .title = "Effective C",
        .author = "Robert C. Seacord",
        .price = 45.00
    };
    ```

#### **4. Accessing Structure Members**

To access individual members of a structure variable, you use the **dot operator (`.`)**.

**Syntax:**
`structure_variable.member_name`

**Example:**

```c
struct Book my_book = {"1984", "George Orwell", 328, 12.50};

printf("Book Title: %s\n", my_book.title);
printf("Author: %s\n", my_book.author);
printf("Pages: %d\n", my_book.pages);
printf("Price: $%.2f\n", my_book.price);

// Modifying a member
my_book.price = 15.00;
printf("New Price: $%.2f\n", my_book.price);
```

#### **5. Structures and Pointers**

Pointers to structures are extremely common, especially when working with dynamic memory allocation or passing structures to functions efficiently.

  * **Declaring a pointer to a structure:**

    ```c
    struct Book *ptr_to_book;
    ```

  * **Assigning an address to the pointer:**

    ```c
    struct Book my_book = {"Dune", "Frank Herbert", 896, 20.00};
    ptr_to_book = &my_book; // ptr_to_book now holds the address of my_book
    ```

  * **Accessing members using a pointer (Arrow Operator `->`):**
    When you have a pointer to a structure, you use the **arrow operator (`->`)** to access its members. This is syntactic sugar for dereferencing the pointer first and then using the dot operator (i.e., `(*ptr_to_struct).member`).

    **Syntax:**
    `pointer_to_structure->member_name`

    **Example:**

    ```c
    struct Book *ptr_to_book;
    struct Book science_book = {"Cosmos", "Carl Sagan", 432, 18.75};
    ptr_to_book = &science_book;

    printf("Title via pointer: %s\n", ptr_to_book->title);
    printf("Author via pointer: %s\n", ptr_to_book->author);

    ptr_to_book->pages = 450; // Modify pages via pointer
    printf("New Pages via pointer: %d\n", science_book.pages); // Original struct also changed
    ```

#### **6. Structures as Function Arguments**

Structures can be passed to functions and returned by functions.

  * **Call by Value (passing a copy):** The entire structure is copied. This can be inefficient for large structures.
    ```c
    void printBookInfo(struct Book b) { // b is a copy
        printf("Title: %s\n", b.title);
        printf("Author: %s\n", b.author);
    }
    // Call: printBookInfo(my_book);
    ```
  * **Call by Reference (passing a pointer):** Only the address of the structure is passed. This is much more efficient for large structures and allows the function to modify the original structure. This is the **most common and recommended** way to pass structures to functions.
    ```c
    void updateBookPrice(struct Book *b_ptr, float new_price) { // b_ptr is a pointer
        b_ptr->price = new_price; // Modify original struct
    }
    // Call: updateBookPrice(&my_book, 25.00);
    ```

#### **7. Nested Structures**

A structure can contain members that are themselves other structures.

**Example:**

```c
struct Date {
    int day;
    int month;
    int year;
};

struct Person {
    char name[50];
    struct Date dob; // Nested structure
};

struct Person p;
p.dob.day = 15;
p.dob.month = 6;
p.dob.year = 1990;
strcpy(p.name, "John Doe");

printf("%s was born on %d/%d/%d\n", p.name, p.dob.day, p.dob.month, p.dob.year);
```
#### **8. Advanced Techniques**

  *  **Flexible Array Members (C99+):**
    How do you create a struct that can hold a string of any length without wasting space with a fixed-size char name[256]? A flexible array member is the answer. It must be the last member of a struct and is declared with empty brackets.
```c
struct String {
    int length;
    char data[]; // Flexible array member
};
```
You cannot declare a variable of this type directly. You must allocate memory for it dynamically using `malloc`, allocating space for the struct itself *plus* the desired size of the flexible array.
```c
#include <stdlib.h>
#include <string.h>

char *my_string = "Hello Flexible World";
int len = strlen(my_string);

// Allocate space for the struct + the string data + null terminator
struct String *flex_str = malloc(sizeof(struct String) + len + 1);

flex_str->length = len;
strcpy(flex_str->data, my_string); // Copy the string into the flexible part

printf("'%s' has length %d\n", flex_str->data, flex_str->length);

free(flex_str); // Don't forget to free the allocated memory!
```
* **Bit-Fields:**
For very low-level work (like interacting directly with hardware registers), you might need to control individual bits. A struct allows you to define members that are only a certain number of bits wide.
```c
// Represents an 8-bit hardware control register
struct ControlRegister {
    unsigned int enable_feature_A : 1; // Use 1 bit
    unsigned int enable_feature_B : 1; // Use 1 bit
    unsigned int reserved         : 2; // 2 unused bits
    unsigned int config_value     : 4; // Use 4 bits
};
// Total: 1+1+2+4 = 8 bits = 1 byte.
```
This gives you a highly readable way to manipulate specific bits without manual bitwise `&` and `|` operations. `sizeof(struct ControlRegister)` will be just 1 byte (or the size of an `int` depending on the compiler, but the bit layout is what matters).

#### **9. `typedef` with Structures (Optional but Common)**

The `typedef` keyword creates an alias (a new name) for an existing data type. It's commonly used with structures to make code more readable by avoiding the `struct` keyword every time you declare a variable.

**Syntax:**

```c
typedef struct tag_name {
    // members
} TypeName; // TypeName is the new alias
```

**Example:**

```c
// Without typedef:
// struct Book { /* ... */ };
// struct Book my_book;

// With typedef:
typedef struct { // Tag name is optional here if only using typedef name
    char title[100];
    char author[50];
    int pages;
    float price;
} Book; // 'Book' is now an alias for 'struct Book'

Book my_book; // Declare variable using the alias
Book *ptr_book; // Declare pointer using the alias
```

-----

### **Relevant DSA (Conceptual Link):**

Structures are arguably the most crucial user-defined data type in C and form the basis for almost all complex **data structures**:

  * **Custom Data Types:** They allow you to model real-world entities (e.g., a `Student`, a `Car`, a `Pixel`, a `Node` in a tree).
  * **Linked Lists, Trees, Graphs:** These fundamental dynamic data structures are built from "nodes," where each `node` is typically a `struct` containing data and one or more pointers to other `node` structures.
  * **Object-Oriented Concepts (in C):** While C is not an object-oriented language, structures with function pointers can be used to simulate basic object-like behavior (e.g., storing data and "methods" that operate on that data).
  * **Databases & Records:** Structures directly map to the concept of a "record" in a database, where each record is a collection of related fields.
  * **Game Development:** Game objects (player, enemy, item) are often represented as structures, holding properties like position, health, type, etc.
  * **Efficient Data Passing:** Using pointers to structures allows large data objects to be passed to functions efficiently, avoiding costly copying.

Structures are the gateway to building more abstract and complex programs that manage diverse types of information in a coherent way.

-----

### **Homework Project:**

**Project 10: Structuring Your Code**

1.  **Student Management System (Basic):**

      * Define a `struct Student` with the following members:
          * `char name[50];`
          * `int roll_number;`
          * `float marks[3];` (an array to store marks in 3 subjects)
          * `float average_marks;`
          * `char grade;`
      * In your `main()` function:
          * Declare a `struct Student` variable.
          * Prompt the user to enter the student's name, roll number, and marks for 3 subjects.
          * Use `fgets()` for the name (remember to remove newline).
          * Use a loop for marks input.
          * Write a function `calculateStudentStats(struct Student *s_ptr)` that takes a pointer to a `Student` structure.
          * Inside this function, calculate `average_marks` (sum of 3 marks divided by 3) and store it in `s_ptr->average_marks`.
          * Based on `average_marks`, assign a `grade` ('A' for \>=90, 'B' for \>=80, 'C' for \>=70, 'D' for \>=60, 'F' for \<60) and store it in `s_ptr->grade`.
          * Back in `main()`, call `calculateStudentStats()` with the address of your student variable.
          * Finally, print all the student's details (name, roll number, 3 marks, average marks, and grade) in a well-formatted way.

2.  **Point and Distance Calculator (Medium):**

      * Define a `struct Point` with members `int x;` and `int y;`.
      * Define a `struct Circle` with members `struct Point center;` (a nested structure) and `float radius;`.
      * In your `main()` function:
          * Declare variables: `struct Point p1, p2;` and `struct Circle c1;`.
          * Prompt the user to enter coordinates for `p1` and `p2`.
          * Prompt the user to enter coordinates for the center of `c1` and its radius.
          * Write a function `calculateDistance(struct Point pA, struct Point pB)` that takes two `Point` structures (passed by value) and returns the Euclidean distance between them as a `float`. (Formula: `sqrt((x2-x1)^2 + (y2-y1)^2)`) - you'll need `#include <math.h>` and link with `-lm` if compiling with gcc.
          * Write a function `isPointInCircle(struct Point p, struct Circle c)` that takes a `Point` structure (by value) and a `Circle` structure (by value). It should return `1` (true) if the point `p` is inside or on the boundary of the circle `c`, and `0` (false) otherwise. (Hint: Calculate the distance from the point to the circle's center and compare it with the radius).
          * In `main()`, call `calculateDistance` for `p1` and `p2` and print the result.
          * In `main()`, call `isPointInCircle` for `p1` and `c1`, and print whether `p1` is inside `c1`.

3.  **Dynamic Product Catalog (Challenge Problem - integrates Arrays, Pointers, Structs, Basic I/O, Functions):**

      * **Objective:** Create a simple product catalog where you can add products and display them. This will require dynamically allocating memory for products, which structures are perfect for.
      * Define a `struct Product` with members:
          * `char name[50];`
          * `float price;`
          * `int quantity;`
      * Define a `struct Catalog` with members:
          * `struct Product *products;` (A pointer that will point to an array of `Product` structures)
          * `int num_products;`
          * `int capacity;` (The current maximum number of products the `products` array can hold)
      * In your `main()` function:
          * Initialize a `struct Catalog` variable. Set `num_products = 0`, `capacity = 2`.
          * Dynamically allocate initial memory for `catalog.products` using `malloc(catalog.capacity * sizeof(struct Product));` (You'll need `malloc` and `free` - covered briefly now, more deeply in a mini-lesson later. Just use `malloc` and remember to `free` at the end).
          * Implement a menu-driven program using a `do-while` loop:
            ```
            Product Catalog Menu:
            1. Add Product
            2. View All Products
            3. Exit
            Enter your choice:
            ```
          * **Case 1: Add Product:**
              * Prompt for product name, price, and quantity.
              * Read inputs.
              * **Crucially:** If `catalog.num_products == catalog.capacity`, you need to **double the capacity** and use `realloc()` to resize the `products` array. If `realloc` fails, print an error and exit.
              * Copy the new product details into `catalog.products[catalog.num_products]`. Increment `catalog.num_products`.
          * **Case 2: View All Products:**
              * If `catalog.num_products` is 0, print "Catalog is empty."
              * Otherwise, loop through `catalog.products` (from `0` to `num_products - 1`) and print each product's name, price, and quantity in a formatted way.
          * **Case 3: Exit:**
              * Print "Exiting..."
              * **Important:** Before exiting, `free(catalog.products);` to release dynamically allocated memory.
          * **Default:** Handle invalid choices.

**Self-Check/Debugging:**

  * Are you using the dot operator (`.`) for structure variables and the arrow operator (`->`) for structure pointers correctly?
  * For the `Student` problem, are you passing the structure to `calculateStudentStats` by pointer (`&student_var`) and using the arrow operator inside the function?
  * For the `Point` and `Circle` problem, are you correctly calculating the distance (remember `sqrt` and `pow` from `<math.h>`)?
  * For the **Challenge Problem**:
      * Are you using `malloc` and `realloc` correctly? (Remember to check if `malloc` or `realloc` returns `NULL`).
      * Are you handling the `capacity` and `num_products` logic correctly for growing the array?
      * **Most importantly, are you freeing the dynamically allocated memory before the program exits?** (`free()` is essential to prevent memory leaks).

