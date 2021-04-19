![MikroE](http://www.mikroe.com/img/designs/beta/logo_small.png)

[GO BACK TO MAIN PAGE](../README.md)

---
## 4 Module Rules

---
### 4.1 Naming Conventions

**Rules:**
- a. All module names shall consist entirely of lowercase letters, numbers, and underscores. No spaces shall appear within the module’s header and source file names.
- b. All module names shall be unique in their first **8** characters and end with suffices **.h** and **.c** for the header and source file names, respectively.
- c. No module’s header file name shall share the name of a header file from the **C Standard Library** or **C++ Standard Library**. For example, modules shall not be named ```stdio``` or ```math```.
- d. Any module containing a ```main( )``` function shall have the word ```main``` as part of its source file name.

**Reasoning:**<br /> Multi-platform development environments (e.g., Unix and Windows) are the norm rather than the exception. Mixed case names can cause problems across operating systems and are also error prone due to the possibility of similarly-named but differently-capitalized files becoming confused by human programmers. The inclusion of ```main``` in a file name is an aid to code maintainers that has proven useful in projects with multiple software configurations.

**Enforcement:**<br /> An automated tool shall confirm that all file names that are part of a build are consistent with these rules.

---
### 4.2 Header Files

**Rules:**
- a. There shall always be precisely one header file for each source file and they shall always have the same root name.
- b. Each header file shall contain a preprocessor guard against multiple inclusion, as shown in the example below.
- c. The header file shall identify only the procedures, constants, and data types (via prototypes or macros, ```#define``` , and typedefs, respectively) about which it is strictly necessary for other modules to be informed.
  + i. It is a preferred practice that no variable ever be declared (via ```extern```) in a header file.
  + ii. No storage for any variable shall be allocated in a header file.
- d. No public header file shall contain a ```#include``` of any private header file.

*Example:*

```.c
#ifndef ADC_H
#define ADC_H

...

#endif  //  ADC_H
```

**Reasoning:**<br /> The **C** language standard gives all variables and functions global scope by default. The downside of this is unnecessary (and dangerous) coupling between modules. To reduce inter-module coupling, keep as many procedures, constants, data types, and variables as possible privately hidden within a module’s source file.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 4.3 Source Files

**Rules:**
- a. Each source file shall include only the behaviors appropriate to control one “entity”. Examples of entities include encapsulated data types, active objects, peripheral drivers (e.g., for a UART), and communication protocols or layers (e.g., ARP).
- b. Each source file shall be comprised of some or all of the following sections, in the order listed: comment block; include statements; data type, constant, and macro definitions; static data declarations; private function prototypes; public function bodies; then private function bodies.
  + i. Each file shall contain comments stating sections of code appropriately
- c. Each source file shall always ```#include``` the header file of the same name (e.g., file adc.c should #include “adc.h” ), to allow the compiler to confirm that each public function and its prototype match.
- d. Absolute paths shall not be used in include file names.
- e. Each source file shall be free of unused include files.
- f. No source file shall ```#include``` another source file.

**Reasoning:**<br /> The purpose and internal layout of a source file module should be clear to all who maintain it. For example, the public functions are generally of most interest and thus appear ahead of the private functions they call. Of critical importance is that every function declaration be matched by the compiler against its prototype.

**Enforcement:**<br /> Most static analysis tools can be configured to check for include files that are not actually used. The other rules shall be enforced during code reviews.

---
### 4.4 File Templates

**Rules:**
- a. A set of templates for header files and source files shall be maintained at the project level.

- [Library Header Template](../templates/header_file_template.md)
- [Library Source Template](../templates/source_file_template.md)

**Reasoning:**<br /> Starting each new file from a template ensures consistency in file header comment blocks and ensures inclusion of appropriate copyright notices.

**Enforcement:**<br /> The consistency of file formats shall be enforced during code reviews.

---

[GO BACK TO MAIN PAGE](../README.md)

---

Version : **1.0.0**

- 1.0.0 / Document initial version created by Strahinja Jacimovic

---
