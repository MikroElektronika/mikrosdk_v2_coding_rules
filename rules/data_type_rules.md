![MikroE](http://www.mikroe.com/img/designs/beta/logo_small.png)

[GO BACK TO MAIN PAGE](../README.md)

---
## 5 Data Type Rules

---
### 5.1 Naming Conventions

**Rules:**
- a. The names of all new data types, including structures, unions, and enumerations, shall consist only of lowercase characters and internal underscores and end with ```_t```.
- b. All new structures, unions, and enumerations shall be named via a typedef .
- c. The name of all public data types shall be prefixed with their module name and an underscore.

*Example:*

```.c
typedef struct
{
    uint16_t    count;
    uint16_t    max_count;
    uint16_t    _unused;
    uint16_t    control;
} timer_reg_t;
```

**Reasoning:**<br /> Data type names and variable names are often appropriately similar. For example, a set of timer control registers in a peripheral calls out to be named ‘ timer_reg ’. To distinguish the structure definition that defines the register layout, it is valuable to create a new type with a distinct name, such as ‘ timer_reg_t ’. If necessary this same type could then be used to create a shadow copy of the timer registers, say called ‘ timer_reg_shadow ’.

**Enforcement:**<br /> An automated tool shall scan new or modified source code prior to each build to ensure that the keywords ```struct```, ```union``` and ```enum``` are used only within typedef statements or in anonymous declarations. Code reviews shall be used to enforce the naming rules for new types.

---
### 5.2 Fixed-Width Integers

**Rules:**
- a. Whenever the width, in bits or bytes, of an integer value matters in the program, one of the fixed width data types shall be used in place of ```char```, ```short```, ```int```, ```long```, or ```long long```. The signed and unsigned fixed-width integer types shall be as shown in the table below.

| Integer Width | Signed Type   | Unsigned Type |
|---------------|---------------|---------------|
| 8 bits        | int8_t        | uint8_t       |
| 16 bits       | int16_t       | uint16_t      |
| 32 bits       | int32_t       | uint32_t      |
| 64 bits       | int64_t       | uint64_t      |

- b. The keywords ```short``` and ```long``` shall not be used.
- c. Use of the keyword ```char``` shall be restricted to the declaration of and operations concerning strings.

**Reasoning:**<br /> The **C90** standard purposefully allowed for implementation-defined widths for ```char``` , ```short``` , ```int``` , ```long``` , and ```long long``` types, which has resulted in code portability problems. The **C99** standard did not resolve this but did introduce the type names shown in the table, which are defined in the ```stdint.h``` header file. In the absence of a **C99** compatible compiler, it is acceptable to define the set of fixed-width types in the table above as typedefs built from underlying types. If this is necessary, be sure to use compile-time checking (e.g., static assertions).

**Enforcement:**<br /> At every build an automated tool shall flag any use of keywords ```short``` or ```long```. Compliance with the other rules shall be checked during code reviews.

---
### 5.3 Signed and Unsigned Integers

**Rules:**
- a. Bit-fields shall not be defined within signed integer types.
- b. None of the bitwise operators (i.e., ```&``` , ```|``` , ```~``` , ```^``` , ```<<``` , and ```>>``` ) shall be used to manipulate signed integer data.
- c. Signed integers shall not be combined with unsigned integers in comparisons or expressions. In support of this, decimal constants meant to be unsigned should be declared with a ```u``` at the end.

*Example:*

```.c
uint16_t unsigned_a = 6U;
int16_t signed_b = -9;

if ( ( unsigned_a + signed_b ) < 4 ) {
    // Execution of this block appears reliably logical, as -9 + 6 is -3
    ...
}

// ... but compilers with 16-bit int may legally perform (0xFFFF – 9) + 6.
```

**Reasoning:**<br /> Several details of the manipulation of binary data within signed integer containers are implementation-defined behaviors of the **ISO C** standards. Additionally, the results of mixing signed and unsigned integers can lead to data-dependent outcomes like the one in the code above. Beware that the use of **C99**'s fixed-width integer types does not by itself prevent such defects.

**Enforcement:**<br /> Static analysis tools can be used to detect violations of these rules.

---
### 5.4 Floating Point

**Rules:**
- a. Avoid the use of floating point constants and variables whenever possible. Fixed-point math may be an alternative.
- b. When floating point calculations are necessary:
  + i. Use the **C99** type names ```float32_t```, ```float64_t```, and ```float128_t```.
  + ii. Append an ```f``` to all single-precision constants (e.g., pi = 3.141592f ).
  + iii. Ensure that the compiler supports double precision, if your math depends on it.
  + iv. Never test for equality or inequality of floating point values.
  + v. Always invoke the ```isfinite( )``` macro to check that prior calculations have resulted in neither INFINITY nor NAN.


*Example:*

```.c
#include <limits.h>

//  Ensure the compiler supports double precision.

#if ( DBL_DIG < 10 )
#   error “Double precision is not available!”
#endif
```

**Reasoning:**<br /> A large number of risks of defects stem from incorrect use of floating point arithmetic. By default, **C** promotes all floating-point constants to double precision, which may be inefficient or unsupported on the target platform. However, many microcontrollers do not have any hardware support for floating point math. The compiler may not warn of these incompatibilities, instead performing the requested numerical operations by linking in a large (typically a few kilobytes of code) and slow (numerous instruction cycles per operation) floating-point emulation library.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 5.5 Structures and Unions

**Rules:**
- a. Appropriate care shall be taken to prevent the compiler from inserting padding bytes within ```struct``` or ```union``` types used to communicate to or from a peripheral or over a bus or network to another processor.
- b. Appropriate care shall be taken to prevent the compiler from altering the intended order of the bits within bit-fields.

*Example:*

```.c
typedef struct
{
    uint16_t    count;                  // offset 0
    uint16_t    max_count;              // offset 2
    uint16_t    _unused;                // offset 4

    uint16_t    enable      : 1;        // offset 6 bits 15-14
    uint16_t    b_interrupt : 1;        // offset 6 bit  13
    uint16_t    _unused1    : 7;        // offset 6 bits 12-6
    uint16_t    b_complete  : 1;        // offset 6 bit  5
    uint16_t    _unused2    : 4;        // offset 6 bits 4-1
    uint16_t    b_periodic  : 1;        // offset 6 bit  0
} timer_reg_t;

// Preprocessor check of timer register layout byte count.

#if ((8 != sizeof(timer_reg_t))
#   error “timer_reg_t struct size incorrect (expected 8 bytes)”
#endif
```

**Reasoning:**<br /> Owing to differences across processor families and loose definitions in the **ISO C** language standards, there is a tremendous amount of implementation-defined behavior in the area of structures and unions. Bit-fields, in particular, suffer from severe portability problems, including the lack of a standard bit ordering and no official support for the fixed-width integer types they so often call out to be used with. The methods available to check the layout of such data structures include static assertions or other compile-time checks as well as the use of preprocessor directives, e.g., to select one of two competing struct layouts based on the compiler.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 5.6 Booleans

**Rules:**
- a. Boolean variables shall be declared as type ```bool```.
- b. Non-Boolean values shall be converted to Boolean via use of relational operators (e.g., < or != ), not via casts.

**Reasoning:**<br /> The **C90** standard did not define a data type for Boolean variables and **C** programmers have widely treated any non-zero integer value as ```true```. The **C99** language standard is backward compatible with this old style, but also introduced a new data type for Boolean variables along with new constants ```true``` and ```false``` in the ```stdbool.h``` header file.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---

[GO BACK TO MAIN PAGE](../README.md)

---

Version : **1.0.0**

- 1.0.0 / Document initial version created by Strahinja Jacimovic

---
