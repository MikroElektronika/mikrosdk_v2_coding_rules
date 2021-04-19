![MikroE](http://www.mikroe.com/img/designs/beta/logo_small.png)

[GO BACK TO MAIN PAGE](../README.md)

---
## 2 Comment Rules

---
### 2.1 Acceptable Formats

**Rules:**
- a. Single-line comments in the **C++** style (i.e., preceded by ```//``` ) should be used for single line comments instead of **C** style (i.e., ```/* ... */``` ).
- b. **C** style  (i.e., ```/* ... */``` ) comments should be used only for block comments.
- c. Comments shall never contain the preprocessor tokens ```/*```, ```//```, or ```\```.
- d. Code shall never be commented out, even temporarily.
  + I. To temporarily disable a block of code, use the preprocessor’s conditional compilation feature (e.g., ```#if 0``` ... ```#endif``` ).
  + II. Any line or block of code that exists specifically to increase the level of debug output information shall be surrounded by ```#ifndef NDEBUG``` ... ```#endif``` .

*Example:*

```.c
/* The following code was meant to be part of the build...
...

safety_checker();

...
/* ... but an end of comment character sequence was omitted. */
```

**Reasoning:**<br /> Whether intentional or not, nested comments run the risk of confusing source code reviewers about the chunks of the code that will be compiled and run. Our choice of negative-logic ```NDEBUG``` is deliberate, as that constant is also associated with disabling the ```assert()``` macro. In both cases, the programmer acts to disable the verbose code.

**Enforcement:**<br /> The use of only permitted comment formats can be partially enforced by the compiler or static analysis. However, only human code reviewers can tell the difference between commented-out code and comments containing descriptive code snippets.

---
### 2.2 Locations and Content

**Rules:**
- a. All comments shall be written in clear and complete sentences, with proper spelling and grammar and appropriate punctuation.
- b. The most useful comments generally precede a block of code that performs one step of a larger algorithm. A blank line shall follow each such code block. The comments in front of the block should be at the same indentation level.
- c. Avoid explaining the obvious. Assume the reader knows the C programming language. For example, end-of-line comments should only be used where the meaning of that one line of code may be unclear from the variable and function names and operations alone but where a short comment makes it clear. Specifically, avoid writing unhelpful and redundant comments, (e.g., ```numero <<= 2; // Shift numero left 2 bits. ```).
- d. The number and length of individual comment blocks shall be proportional to the complexity of the code they describe.
- e. Whenever an algorithm or technical detail is defined in an external reference—e.g., a design specification, patent, or textbook—a comment shall include a sufficient reference to the original source to allow a reader of the code to locate the document.
- f. Whenever a flow chart or other diagram is needed to sufficiently document the code, the drawing shall be maintained with the source code under version control and the comments should reference the diagram by file name or title.
- g. All assumptions shall be spelled out in comments.
- h. Each module and function shall be commented in a manner suitable for automatic documentation generation, e.g., via Doxygen.
  + i. All documentation which should be used as Doxygen input should be placed inside header (.h) file.
  + ii. Each public functions must be documented, at least with short description and documentation of function parameters and return value.
- i. Use the following capitalized comment markers to highlight important issues:
  + i. ```WARNING:``` alerts a maintainer there is risk in changing this code. For example, that a delay loop counter’s terminal value was determined empirically and may need to change when the code is ported or the optimization level tweaked.
  + ii. ```NOTE:``` provides descriptive comments about the “why” of a chunk of code—as distinguished from the “how” usually placed in comments. For example, that a chunk of driver code deviates from the datasheet because there was an errata in the chip. Or that an assumption is being made by the original programmer.
  + iii. ```TODO:``` indicates an area of the code is still under construction and explains what remains to be done. When appropriate, an all-caps programmer name or set of initials may be included before the word TODO (e.g., “ MJB TODO: ”).
- j. Each comment except single line comment should be preceded and followed by empty line.
- k. Each start of the block comment ( ```/*``` ) shall appear by itself on the separated line. The corresponding end of block comment ( ```*/``` ) shall appear by itself in the same position the appropriate number of lines later in the file.

*Example:*

```.c

//  Step 1: Batten down the hatches.

for ( int hatch = 0; hatch < NUM_HATCHES; hatch++ ) {
    if ( hatch_is_open( hatches[hatch] ) ) {
        hatch_close( hatches[hatch] );
    }
}

/*
    Step 2: Raise the mizzenmast.
    TODO: Define mizzenmast driver API.
*/
```

**Reasoning:**<br /> Following these rules results in good comments. And good comments correlate with good code. It is a best practice to write the comments before writing the code that implements the behaviors those comments outline. Unfortunately, it is easy for source code and documentation to drift over time. The best way to prevent this is to keep the documentation as close to the code as possible. Likewise, anytime a question is asked about a section of the code that was previously thought to be clear, you should add a comment addressing that issue nearby. Doxygen is a useful tool to generate documentation describing the modules, functions, and parameters of an API for its users. However, comments are also still necessary inside the function bodies to reduce the cost of code maintenance.

**Enforcement:**<br /> The quality of comments shall be evaluated during code reviews. Code reviewers should verify that comments accurately describe the code and are also clear, concise, and valuable. Automatically generated documentation should be rebuilt each time the software is built.

---

[GO BACK TO MAIN PAGE](../README.md)

---

Version : **1.0.0**

- 1.0.0 / Document initial version created by Strahinja Jacimovic

---
