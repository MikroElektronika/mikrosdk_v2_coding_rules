![MikroE](http://www.mikroe.com/img/designs/beta/logo_small.png)

[GO BACK TO MAIN PAGE](../README.md)

---
## 6 Procedure Rules

---
### 6.1 Naming Conventions

**Rules:**
- a. No procedure shall have a name that is a keyword of any standard version of the **C** or **C++** programming language. Restricted names include ```interrupt``` , ```inline``` , ```class``` , ```true``` , ```false``` , ```public``` , ```private``` , ```friend``` , ```protected``` , and many others.
- b. No procedure shall have a name that overlaps a function in the **C Standard Library**. Examples of such names include ```strlen``` , ```atoi``` , and ```memset``` .
- c. No procedure shall have a name that begins with an underscore.
- d. No procedure name shall be longer than **31** characters.
- e. No function name shall contain any uppercase letters.
- f. No macro name shall contain any lowercase letters.
- g. Underscores shall be used to separate words in procedure names.
- h. Each procedure’s name shall be descriptive of its purpose. Note that procedures encapsulate the “actions” of a program and thus benefit from the use of verbs in their names (e.g., ```adc_read( )``` ); this “noun-verb” word ordering is recommended. Alternatively, procedures may be named according to the question they answer (e.g., ```led_is_on( )``` ).
- i. The names of all public functions shall be prefixed with their module name and an underscore (e.g., ```sensor_read( )``` ).

**Reasoning:**<br /> Good function names make reviewing and maintaining code easier (and thus cheaper). The data (variables) in programs are nouns. Functions manipulate data and are thus verbs. The use of module prefixes is in keeping with the important goal of encapsulation and helps avoid procedure name overlaps.

**Enforcement:**<br /> Compliance with these naming rules shall be established in the detailed design phase and be enforced during code reviews.

---
### 6.2 Functions

**Rules:**
- a. All reasonable effort shall be taken to keep the length of each function limited to one printed page, or a maximum of **100** lines.
- b. Whenever possible, all functions shall be made to start at the top of a printed page, except when several small functions can fit onto a single page.
- c. It is a preferred practice that all functions shall have just one exit point and it shall be via a return at the bottom of the function.
- d. A prototype shall be declared for each public function in the module header file.
- e. All private functions shall be declared static.
- f. Each parameter shall be explicitly declared and meaningfully named.

*Example:*
```.c
int state_change ( int event ) {
    int result = ERROR;

    if ( EVENT_A == event ) {
        result = STATE_A;
    } else {
        result = STATE_B;
    }

    return ( result );
}
```

**Reasoning:**<br /> Code reviews take place at the function level and often on paper. Each function should thus ideally be visible on a single printed page, so that flipping papers back and forth does not distract the reviewers. Multiple return statements should be used only when it improves the readability of the code.

**Enforcement:**<br /> Compliance with these rules shall be checked during code reviews.

---
### 6.3 Function-Like Macros

**Rules:**
- a. Parameterized macros shall not be used if a function can be written to accomplish the same behavior.
- b. If parameterized macros are used for some reason, these rules apply:
  + i. Surround the entire macro body with parentheses.
  + ii. Surround each use of a parameter with parentheses.
  + iii. Use each parameter no more than once, to avoid unintended side effects.
  + iv. Never include a transfer of control (e.g., ```return``` keyword).

**Example:**

```.c

//  Don’t do this ...

#define MAX(A, B) ((A) > (B) ? (A) : (B))

//  ... when you can do this instead.

inline int max ( int num1, int num2 );
```

**Reasoning:**<br /> There are a lot of risks associated with the use of preprocessor defines, and many of them relate to the creation of parameterized macros. The extensive use of parentheses (as shown in the example) is important, but does not eliminate the unintended double increment possibility of a call such as ```MAX(i++, j++)```. Other risks of macro misuse include comparison of signed and unsigned data or any test of floating-point data. Making matters worse, macros are invisible at run-time and thus impossible to step into within the debugger. Where performance is important, note that **C99** added **C++**’s ```inline``` keyword.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 6.4 Threads of Execution

**Rules:**
- a. All functions that encapsulate threads of execution (tasks, processes) shall be given names ending with ```_thread``` (or ```_task``` , ```_process``` )

**Example:**

```.c
void alarm_thread ( void * p_data ) {
    alarm_t alarm = ALARM_NONE;
    int err = OS_NO_ERR;

    while ( 1 ) {
        alarm = OSMboxPend( alarm_mbox, &err );

        // Process alarm here.
    }
}
```

**Reasoning:**<br /> Each task in a real-time operating system (**RTOS**) is like a ```mini-main()```, typically running forever in an infinite loop. It is valuable to easily identify these important, asynchronous functions during code reviews and debugging sessions.

**Enforcement:**<br /> This rule shall be followed during the detailed design phase and enforced during code reviews.

---
### 6.5 Interrupt Service Routines

**Rules:**
- a. Interrupt service routines (ISRs) are not ordinary functions. The compiler must be informed that the function is an ISR by way of a ```#pragma``` or compiler-specific keyword, such as ```__interrupt```.
- b. All functions that implement ISRs shall be given names ending with ```_isr```.
- c. To ensure that **ISRs** are not inadvertently called from other parts of the software (they may corrupt the CPU and call stack if this happens), each **ISR** function shall be declared static and/or be located at the end of the associated driver module as permitted by the target platform.
- d. A stub or default **ISR** shall be installed in the vector table at the location of all unexpected or otherwise unhandled interrupt sources. Each such stub could attempt to disable future interrupts of the same type, say at the interrupt controller, and ```assert()```.


**Example:**

```.c
#pragma irq_entry

void timer_isr ( void ) {
    uint8_t static prev = 0x00;         // prev button states
    uint8_t curr = *gp_button_reg;      // curr button states

    //  Compare current and previous button states.

    g_debounced |= (prev & curr);       // record all closes
    g_debounced &= (prev | curr);       // record all opens

    //  Save current pin states for next interrupt

    prev = curr;

    // Acknowledge timer interrupt at hardware, if necessary.
}
```

**Reasoning:**<br /> An **ISR** is an extension of the hardware. By definition, it and the straight-line code are asynchronous to each other. If they share global variables or registers, those singleton objects must be protected via interrupt disables in the straight-line code. The **ISR** must not get hung up inside the operating system or waiting for a variable or register to change value. Note that platform-specific **ISR** installation steps vary and may require ISRs functions to have prototypes and in other ways be visible to at least one other function. Although stub interrupt handlers don’t directly prevent defects, they can certainly make a system more robust in real-world operating conditions.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---

[GO BACK TO MAIN PAGE](../README.md)

---

Version : **1.0.0**

- 1.0.0 / Document initial version created by Strahinja Jacimovic

---
