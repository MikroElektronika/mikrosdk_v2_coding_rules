![MikroE](http://www.mikroe.com/img/designs/beta/logo_small.png)

[GO BACK TO MAIN PAGE](../README.md)

---
## 7 Variable Rules

---
### 7.1 Naming Conventions

**Rules:**
- a. No variable shall have a name that is a keyword of **C**, **C++**, or any other well-known extension of the C programming language, including specifically **K&R C** and **C99**. Restricted names include interrupt, inline, restrict, class , true , false, public, private, friend, and protected.
- b. No variable shall have a name that overlaps with a variable name from the **C Standard Library** (e.g., ```errno```).
- c. No variable shall have a name that begins with an underscore.
- d. No variable name shall be longer than **31** characters.
- e. No variable name shall be shorter than **3** characters, excluding loop counters where widely used single character variable names should be used (```i```,```j```,```k```,```m```,```n```).
- f. No variable name shall contain any uppercase letters.
- g. No variable name shall contain any numeric value that is called out elsewhere, such as the number of elements in an array or the number of bits in the underlying type.
- h. Underscores shall be used to separate words in variable names.
- i. Each variable’s name shall be descriptive of its purpose.

**Reasoning:**<br /> The base rules are adopted to maximize code portability across compilers. Many **C** compilers recognize differences only in the first 31 characters in a variable’s name and reserve names beginning with an underscore for internal names. The other rules are meant to highlight risks and ensure consistent proper use of variables. For example, all code relating to the use of global variables and other singleton objects, including peripheral registers, needs to be carefully considered to ensure there can be no race conditions or data corruptions via asynchronous writes.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 7.2 Initialization

**Rules:**
- a. All variables shall be initialized before use.
- b. If project- or file-global variables are used, their definitions shall be grouped together and placed at the top of a source code file.
- c. Any pointer variable lacking an initial address shall be initialized to ```NULL```.

**Example:**

```.c
uint32_t matrix[NUM_ROWS][NUM_COLS] = { ... };

...

for ( int col = 0; col < NUM_COLS; col++ ) {
    matrix[row][col] = ...;
}
```

**Reasoning:**<br /> Too many programmers assume the **C** run-time will watch out for them, e.g., by zeroing the value of uninitialized variables on system startup. This is a bad assumption, which can prove dangerous in a mission-critical system. For readability reasons it is better to declare local variables as close as possible to their first use, which **C99** makes possible by incorporating that earlier feature of **C++**.

**Enforcement:**<br /> An automated tool shall scan all of the source code prior to each build, to warn about variables used prior to initialization; static analysis tools can do this. The remainder of these rules shall be enforced during code reviews.

---

[GO BACK TO MAIN PAGE](../README.md)

---

Version : **1.0.0**

- 1.0.0 / Document initial version created by Strahinja Jacimovic

---
