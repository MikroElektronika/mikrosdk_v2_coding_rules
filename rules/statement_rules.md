![MikroE](http://www.mikroe.com/img/designs/beta/logo_small.png)

[GO BACK TO MAIN PAGE](../README.md)

---
## 8 Statement Rules

---
### 8.1 Variable Declarations

**Rules:**
- a. The comma operator ```,``` shall not be used within variable declarations.

**Example:**

```.c
char * x, y;        // Was y intended to be a pointer also? Don’t do this.

...

char *x, *y;        // This is correctly done.

```

**Reasoning:**<br /> The cost of placing each declaration on a line of its own is low. By contrast, the risk that either the compiler or a maintainer will misunderstand your intentions is high.

**Enforcement:**<br /> This rule shall be enforced during code reviews.

---
### 8.2 Conditional Statements

**Rules:**
- a. It is a preferred practice that the shortest (measured in lines of code) of the if and else if clauses should be placed first.
- b. Nested ```if```...```else``` statements shall not be deeper than two levels. Use function calls or switch statements to reduce complexity and aid understanding.
- c. Assignments shall not be made within an ```if``` or ```else if``` test.
- d. Any ```if``` statement with an ```else if``` clause shall end with an ```else``` clause.

**Example:**

```.c
if ( NULL == p_object ) {
    result = ERR_NULL_PTR;
} else if (p_object = malloc(sizeof(object_t))) {       // No assignments!
    ...
} else {
    // Normal processing steps, which require many lines of code.
    ...
}
```

**Reasoning:**<br /> Long clauses can distract the human eye from the decision-path logic. By putting the shorter clause earlier, the decision path becomes easier to follow. (And easier to follow is always good for reducing bugs.) Deeply nested ```if```...```else``` statements are a sure sign of a complex and fragile state machine implementation; there is always a safer and more readable way to do the same thing.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 8.3 Switch Statements

**Rules:**
- a. The ```break``` for each ```case``` shall be indented to align with the associated ```case```, rather than with the contents of the ```case``` code block.
- b. All ```switch``` statements shall contain a ```default``` block.
  + i. The ```default``` case shall have one empty line beforehand.
- c. Any ```case``` designed to fall through to the next shall be commented to clearly explain the absence of the corresponding ```break```.

**Example:**

```.c
switch (err)
{
    case ERR_A:
        ...
    break;
    case ERR_B:
    case ERR_C:             // Also perform the steps for ERR_C.
        ...
        ...
    break;

    default:                // One empty line before `default` case.

        ...
    break;
}
```

**Reasoning:**<br /> C’s ```switch``` statements are powerful constructs, but prone to errors such as omitted ```break``` statements and unhandled cases. By aligning the ```case``` labels with their ```break``` statements it is easier to spot a missing break.

**Enforcement:** These rules shall be enforced during code reviews.

---
### 8.4 Loops

**Rules:**
- a. Magic numbers shall not be used as the initial value or in the endpoint test of a ```while``` , ```do```...```while```, or ```for``` loop.
- b. With the exception of the initialization of a loop counter in the first clause of a for statement and the change to the same variable in the third, no assignment shall be made in any loop’s controlling expression.
- c. Infinite loops shall be implemented via controlling expression ```while( 1 );```.
- d. Each loop with an empty body shall feature a set of braces enclosing a comment to explain why nothing needs to be done until after the loop terminates.

**Example:**

```.c
// Why would anyone bury a magic number (e.g., “100”) in their code?

for (int row = 0; row < 100; row++) {
    // Descriptively-named constants prevent defects and aid readability.

    for (int col = 0; col < NUM_COLS; col++) {
        ...
    }
}
```

**Reasoning:**<br /> It is always important to synchronize the number of loop iterations to the size of the underlying data structure. Doing this via a descriptively-named constant prevents defects that result when changes in one part of the code, such as the dimension of an array, are not matched in other areas of the code.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 8.5 Jumps

**Rules:**
- a. The use of ```goto``` statements shall be restricted.
- b. C Standard Library functions ```abort()```, ```exit()``` , ```setjmp()```, and ```longjmp()``` shall not be used.

**Reasoning:**<br /> Algorithms that utilize jumps to move the instruction pointer can and should be rewritten in a manner that is more readable and thus easier to maintain.

**Enforcement:**<br /> These rules shall be enforced by an automated scan of all modified or new modules for inappropriate use of forbidden tokens. To the extent that the use of ```goto``` is permitted, code reviewers should investigate alternative code structures to improve code maintainability and readability.

---
### 8.6 Equivalence Tests

**Rules:**
- a. When evaluating the equality of a variable against a constant, the constant shall always be placed to the left of the equal-to operator (```==```).

**Example:**

```.c
if ( p_object == NULL ) {           // This is not OK!!!
    return ( ERR_NULL_PTR );
}

...

if ( NULL == p_object ) {           // This is OK!!!
    return ( ERR_NULL_PTR );
}
```

**Reasoning:**<br /> It is always desirable to detect possible typos and as many other coding defects as possible at compile-time. Defect discovery in later phases is not guaranteed and often also more costly. By following this rule, any compiler will reliably detect erroneous attempts to assign (i.e., = instead of ==) a new value to a constant.

**Enforcement:**<br /> Many compilers can be configured to warn about suspicious assignments (i.e., located where comparisons are more typical). However, ultimate responsibility for enforcement of this rule falls to code reviewers.

---

[GO BACK TO MAIN PAGE](../README.md)

---

Version : **1.0.0**

- 1.0.0 / Document initial version created by Strahinja Jacimovic

---
