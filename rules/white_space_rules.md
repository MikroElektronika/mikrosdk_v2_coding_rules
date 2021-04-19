![MikroE](http://www.mikroe.com/img/designs/beta/logo_small.png)

[GO BACK TO MAIN PAGE](../README.md)

---
## 3 White Space Rules

---
### 3.1 Spaces

**Rules:**
- a. Each of the keywords ```if``` , ```while``` , ```for``` , ```switch``` , and ```return``` shall be followed by one space on the same line.
- b. Each of the assignment operators ```=``` , ```+=``` , ```-=``` , ```*=``` , ```/=``` , ```%=``` , ```&=``` , ```|=``` , ```^=``` , ```~=``` , and ```!=``` shall always be preceded and followed by one space.
- c. Each of the binary operators ```+``` , ```-``` , ```*``` , ```/``` , ```%``` , ```<``` , ```<=``` , ```>``` , ```>=``` , ```==``` , ```!=``` , ```<<``` , ```>>``` , ```&``` , ```|``` , ```^``` , ```&&``` , and ```||``` shall always be preceded and followed by one space.
- d. Each of the unary operators ```+``` , ```-``` , ```++``` , ```--``` , ```!``` , and ```~``` , shall be written without a space on the operand side.
- e. The pointer operators ```*``` and ```&``` shall be written with white space on left side within declarations.
- f. The ```?``` and ```:``` characters that comprise the ternary operator shall each always be preceded and followed by one space.
- g. The structure pointer and structure member operators ( ```->``` and ```.``` , respectively) shall always be without surrounding spaces.
- h. The left and right brackets of the array subscript operator ( ```[``` and ```]``` ) shall be without surrounding spaces, except as required by another white space rule.
- i. Expressions within parentheses shall always have no spaces adjacent to the left and right parenthesis characters.
- j. The left and right parentheses of the function call operator shall always be without surrounding spaces, except that the function declaration shall feature one space between the function name and the left parenthesis to allow that one particular mention of the function name to be easily located.
- k. Except when at the end of a line, each comma separating function parameters shall always be followed by one space.
- l. Each semicolon separating the elements of a for statement shall always be followed by one space.
- m. Each semicolon shall follow the statement it terminates without a preceding space.

**Reasoning:**<br /> In source code, the placement of white space is as important as the placement of text. Good use of white space reduces eyestrain and increases the ability of programmers and reviewers of the code to spot potential bugs.

**Enforcement:**<br /> These rules shall be followed by programmers as they work as well as reinforced via a code beautifier, e.g., **GNU Indent**.

---
### 3.2 Alignment

**Rules:**
- a. The names of variables within a series of declarations shall have their first characters aligned.
- b. The names of ```struct``` and ```union``` members shall have their first characters aligned.
- c. The assignment operators within a block of adjacent assignment statements shall be aligned.
- d. The ```#``` in a preprocessor directive shall always be located at the start of a line, though the directives themselves may be indented within a ```#if``` or ```#ifdef``` sequence.

*Example:*

```.c
#ifdef USE_UNICODE_STRINGS
#   define BUFFER_BYTES     128
#else
#   define BUFFER_BYTES     64
#endif

...

typedef struct
{
    uint8_t    buffer[BUFFER_BYTES];
    uint8_t    checksum;

} string_t;
```

**Reasoning:**<br /> Visual alignment emphasizes similarity. A series of consecutive lines each containing a variable declaration is easily seen and understood as a block of related lines of code. Blank lines and differing alignments should be used as appropriate to visually separate and distinguish unrelated blocks of code that happen to be located in proximity.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 3.3 Blank Lines

**Rules:**
- a. No line of code shall contain more than one statement.
- b. There shall be a blank line before and after each natural block of code. Examples of natural blocks of code are loops, ```if```...```else``` and ```switch``` statements, and consecutive declarations.
- c. Blank line shall always surround block comments.
- d. Each source file shall terminate with a comment marking the end of file followed by a blank line.

**Reasoning:**<br /> Appropriate placement of white space provides visual separation and thus makes code easier to read and understand, just as the white space areas between paragraphs of this coding standard aid readability. Clearly marking the end of a file is important for human reviewers looking at printouts and the blank line following may be required by some older compilers.

**Enforcement:**<br /> These rules shall be enforced during code reviews.

---
### 3.4 Indentation

**Rules:**
- a. Each indentation level should align at a multiple of **4** characters from the start of the line.
- b. Within a ```switch``` statement, the ```case``` labels shall be aligned; the contents of each case block shall be indented once from there.
- c. Whenever a line of code is too long to fit within the maximum line width, indent the second and any subsequent lines in the most readable manner possible.

*Example:*

```.c

void sys_error_handler ( int err ) {
    switch ( err ) {
        case ERR_THE_FIRST:
            ...
        break;

        default:
            ...
        break;
    }

    //  Purposefully misaligned indentation; see why?

    if ( ( first_very_long_comparison_here
         && second_very_long_comparison_here )
        || third_very_long_comparison_here )
    {
        ...

    }
}
```

**Reasoning:**<br /> Fewer indentation spaces increase the risk of visual confusion while more spaces increases the likelihood of line wraps.

**Enforcement:**<br /> A tool, such as a code beautifier, shall be available to programmers to convert indentations of other sizes in an automated manner. This tool shall be used on all new or modified files prior to each build.

---
### 3.5 Tabs

**Rules:**
- a. The ```TAB``` character **ASCII 0x09** shall never appear within any source code file.

*Example:*

```.c
// When tabs are needed inside a string, use the ‘\t’ character.

#define COPYRIGHT “Copyright (c) 2018 Barr Group.\tAll rights reserved.”

//  When indents are needed in the source code, align via spaces instead.

void main ( void ) {
    // If not, you can encounter
  // all sorts
       // of weird and
            // uneven
          // alignment of code and comments... across tools.
}
```

**Reasoning:**<br /> The width of the tab character varies by text editor and programmer preference, making consistent visual layout a continual source of headaches during code reviews and maintenance.

**Enforcement:**<br /> Each programmer should configure his or her code editing tools to insert spaces when the keyboard’s **TAB** key is pressed. The presence of a tab character in new or modified code shall be flagged via an automated scan at each build or code check-in.

---
### 3.6 Non-Printing Characters

**Rules:**
- a. Whenever possible, all source code lines shall end only with the single character ```LF``` **ASCII 0x0A**, not with the pair ```CR```-```LF``` **0x0D 0x0A**.
- b. The only other non-printable character permitted in a source code file is the form feed character ```FF``` **ASCII 0x0C**.

*Example:* It’s not possible to demonstrate non-printing characters in print.

**Reasoning:**<br /> The multi-character sequence ```CR```-```LF``` is more likely to cause problems in a multi-platform development environment than the single character ```LF```. One such problem is associated with multi-line preprocessor macros on Unix platforms.

**Enforcement:**<br /> Whenever possible, programmer’s editors shall be configured to use ```LF```. In addition, an automated tool shall scan all new or modified source code files during each build, replacing each ```CR```-```LF``` sequence with an ```LF```.

---

[GO BACK TO MAIN PAGE](../README.md)

---

Version : **1.0.0**

- 1.0.0 / Document initial version created by Strahinja Jacimovic

---
