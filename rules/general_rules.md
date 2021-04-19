![MikroE](http://www.mikroe.com/img/designs/beta/logo_small.png)

[GO BACK TO MAIN PAGE](../README.md)

---
## 1 General Rules

---
### 1.1 Which C

**Rules:**
- a. All programs shall be written to comply with the **C89** version of the ISO C Programming Language Standard.<br />
- b. Whenever a C++ compiler is used, appropriate compiler options shall be set to restrict the language to the selected version of **ISO C**.<br />
- c. The use of proprietary compiler language keyword extensions, ```#pragma``` , and ```inline``` assembly shall be kept to the minimum necessary to get the job done and be localized to a small number of device driver modules that interface directly to hardware.<br />
- d. Preprocessor directive ```#define``` shall not be used to alter or rename any keyword or other aspect of the programming language.<br />

*Example:*

```.c
#define begin   {       //  Don’t do something like this...
#define end     }       //  ... nor this.
...

for ( int row = 0; row < MAX_ROWS; row++ ) begin
    ...
end

//  Let C be C, not some language you once loved.
```

**Reasoning:**<br /> To clearly define the rules in the rest of this standard, it is important that we first agree on the baseline programming language specification.

**Enforcement:**<br /> These rules shall be enforced via compiler setup and code reviews.

---
### 1.2 Line Widths

**Rules:**
- a. The width of all lines in a program shall be limited to a maximum of **80** characters.

**Reasoning:**<br /> From time-to-time, peer reviews and other code examinations are conducted on printed pages. To be useful, such print-outs must be free of distracting line wraps as well as missing (i.e., past the right margin) characters. Line width rules also ease on-screen side-by-side code differencing.

**Enforcement:**<br /> Violations of this rule shall be detected by an automated code format tool.

---
### 1.3 Braces

**Rules:**
- a. Braces shall always surround the blocks of code (a.k.a., compound statements), following ```if``` , ```else``` , ```switch``` , ```while``` , ```do``` , and ```for``` statements; single statements and empty statements following these keywords shall also always be surrounded by braces.<br />
- b. Each left brace ( ```{``` ) shall appear next to the block it opens. The corresponding right brace ( ```}``` ) shall appear by itself in the appropriate number of lines later in the file.<br />

*Example:*

```.c
{

    if ( depth_in_ft > 10 ) dive_stage = DIVE_DEEP;     // This is legal ...
    else if ( depth_in_ft > 0 )
        dive_stage = DIVE_SHALLOW;                      // ... as is this...
    else {                                              // but using braces is always safer.
        dive_stage = DIVE_SURFACE;
    }
    ...
}
```

**Reasoning:**<br /> There is considerable risk associated with the presence of empty statements and single statements that are not surrounded by braces. Code constructs like this are often associated with bugs when nearby code is changed or commented out. This risk is entirely eliminated by the consistent use of braces. The placement of the left brace on the following line allows for easy visual checking for the corresponding right brace.

**Enforcement:**<br /> The presence of a left brace after each ```if``` , ```else``` , ```switch``` , ```while``` , ```do``` , and ```for``` shall be enforced by an automated tool at build time. The same tool or another (such as a code beautifier) shall be used to enforce the alignment of braces.

---
### 1.4 Parentheses

**Rules:**
- a. Do not rely on C’s operator precedence rules, as they may not be obvious to those who maintain the code. To aid clarity, use parentheses (and/or break long statements into multiple lines of code) to ensure proper execution order within a sequence of operations.<br />
- b. Unless it is a single identifier or constant, each operand of the logical AND ( ```&&``` ) and logical OR ( ```||``` ) operators shall be surrounded by parentheses.<br />

*Example:*

```.c

if ( ( depth_in_cm > 0 ) && ( depth_in_cm < MAX_DEPTH ) ) {
    depth_in_ft = convert_depth_to_ft( depth_in_cm );
}

```

**Reasoning:**<br /> The syntax of the C programming language has many operators. The precedence rules that dictate which operators are evaluated before which others are complicated—with over a dozen priority levels—and not always obvious to all programmers. When in doubt it’s better to be explicit about what you hope the compiler will do with your calculations.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 1.5 Common Abbreviations

**Rules:**
- a. Abbreviations and acronyms should generally be avoided unless their meanings are widely and consistently understood in the engineering community.
- b. A table of project-specific abbreviations and acronyms shall be maintained in a version-controlled document.

[Abbreviation Table](abbreviation_table.md)

**Reasoning:**<br /> Programmers too readily use cryptic abbreviations and acronyms in their code. Just because you know what ZYZGXL means today doesn’t mean the programmer(s) who have to read maintain/port your code will later be able to make sense of your cryptic names that reference it.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 1.6 Casts

**Rules:**
- a. Each cast shall feature an associated comment describing how the code ensures proper behavior across the range of possible values on the right side.

*Example:*

```.c
int abs ( int arg ) {
    return ( ( arg < 0 ) ? -arg : arg );
}
...
    uint16_t sample = adc_read( ADC_CHANNEL_1 );

    result = abs( ( int ) sample );             // WARNING: 32-bit int assumed.
```

**Reasoning:**<br /> Casting is dangerous. In the example above, unsigned 16-bit “sample” can hold larger positive values than a signed 16-bit integer. In that case, the absolute value will be incorrect as well. Thus there is a possible overflow if int is only 16-bits, which the **ISO C** standard permits.

**Enforcement:**<br /> This rule shall be enforced during code reviews.

---
### 1.7 Keywords to Avoid

**Rules:**
- a. The ```auto``` keyword shall not be used.
- b. The ```register``` keyword shall not be used.
- c. It is a preferred practice to avoid all use of the ```goto``` keyword. If ```goto``` is used it shall only jump to a label declared later in the same or an enclosing block.
- d. It is a preferred practice to avoid all use of the ```continue``` keyword.

**Reasoning:**<br /> The ```auto``` keyword is an unnecessary historical feature of the language. The ```register``` keyword presumes the programmer is smarter than the compiler. There is no compelling reason to use either of these keywords in modern programming practice. The keywords ```goto``` and ```continue``` still serve purposes in the language, but their use too often results in spaghetti code. In particular, the use of ```goto``` to make jumps orthogonal to the ordinary control flows of the structured programming paradigm is problematic. The occasional use of ```goto``` to handle an exceptional circumstance is acceptable if it simplifies and clarifies the code.

**Enforcement:**<br /> The presence of forbidden keywords in new or modified source code shall be detected and reported via an automated tool at each build. To the extent that the use of goto or continue is permitted, code reviewers should investigate alternative code structures to improve code maintainability and readability.

---
### 1.8 Keywords to Frequent

**Rules:**
- a. The ```static``` keyword shall be used to declare all functions and variables that do not need to be visible outside of the module in which they are declared.
- b. The ```const``` keyword shall be used whenever appropriate. Examples include:
  + I. To declare variables that should not be changed after initialization,
  + II. To define call-by-reference function parameters that should not be modified (e.g., ```char const * param``` ),
  + III. To define fields in a ```struct``` or union that should not be modified (e.g., in a ```struct``` overlay for memory-mapped I/O peripheral registers)
  + IV. As a strongly typed alternative to ```#define``` for numerical constants.
- c. The ```volatile``` keyword shall be used whenever appropriate. Examples include:
  + I. To declare a global variable accessible (by current use or scope) by any interrupt service routine,
  + II. To declare a global variable accessible (by current use or scope) by two or more threads,
  + III. To declare a pointer to a memory-mapped I/O peripheral register set (e.g., ```timer_t volatile * const p_timer``` )
  + IV. To declare a delay loop counter.

*Example:*

```.c
typedef struct
{
    uint16_t        count;
    uint16_t        max_count;
    uint16_t const  _unused;                     // read-only register
    uint16_t        control;

} timer_reg_t;

timer_reg_t volatile *const p_timer = ( timer_reg_t * )HW_TIMER_ADDR;
```

**Reasoning:**<br /> C’s ```static``` keyword has several meanings. At the module-level, global variables and functions declared ```static``` are protected from external use. Heavy-handed use of ```static``` in this way thus decreases coupling between modules. The ```const``` and ```volatile``` keywords are even more important. The upside of using ```const``` as much as possible is compiler-enforced protection from unintended writes to data that should be read only. Proper use of ```volatile``` eliminates a whole class of difficult-to-detect bugs by preventing compiler optimizations that would eliminate requested reads or writes to variables or registers.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---

[GO BACK TO MAIN PAGE](../README.md)

---

Version : **1.0.0**

- 1.0.0 / Document initial version created by Strahinja Jacimovic

---
