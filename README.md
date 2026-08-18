# Compiler Design Lab Manual

This repository contains a complete set of experiments for the Compiler Design laboratory. Each experiment demonstrates a core principle of compiler construction such as lexical analysis, parsing, tokenization, syntax checking, intermediate code generation, and type analysis. The programs are implemented using Flex (Lex) and Yacc (Bison), which are standard tools used to generate scanners and parsers.

The project is organized experiment-wise, with each folder containing the source files, generated output, and the associated lab tasks for that specific exercise.

---

## 1. Course Objective

The goal of this lab is to understand the phases of a compiler and how they are implemented in practice. The experiments in this repository cover:

- Lexical analysis and token recognition
- Symbol table creation and lookup
- Parsing arithmetic and variable expressions
- Recognition of control structures in C-like languages
- Expression evaluation using parser-driven computation
- Intermediate code generation in three-address code format
- Type checking and assignment validation
- Code generation concepts for simple machine instructions

Through these projects, students learn how source code is converted from raw text into structured internal representations that can be interpreted or compiled.

---

## 2. Tools Used

This lab primarily uses:

- Flex / Lex for lexical analysis
- Yacc / Bison for syntax analysis and grammar parsing
- GCC for compiling generated C programs
- Standard C libraries such as stdio.h, stdlib.h, and string.h

Typical command flow:

```bash
flex file.l
bison -dy file.y
gcc lex.yy.c y.tab.c -o output -lfl -ly
./output
```

Depending on the experiment, some files may use only a `.l` file or a combination of `.l` and `.y` files.

---

## 3. Repository Structure

```text
CD_LAB/
├── exp1/
├── exp2/
├── exp3/
├── exp4/
├── exp5/
├── exp6/
├── exp7/
├── exp8/
├── exp9/
├── exp10/
├── README.md
└── output files and generated results
```

Each experimental folder contains:

- `.l` file: Lex/Flex source
- `.y` file: Yacc/Bison grammar source
- `output.txt`: sample output / execution result
- supporting lab notes if provided

---

## 4. Detailed Experiment Summary

### Experiment 1: Lexical Analyzer with Symbol Table

**Objective:**
To write a FLEX program that identifies lexical tokens in a C-like source file and stores identifiers in a symbol table.

**Concepts covered:**
- Token recognition
- Identifiers, constants, operators, and comments
- Symbol table insertion and lookup
- Lexical analysis in compiler design

**Implementation details:**
The program uses regular expressions for:

- identifiers
- integer constants
- operators such as `+`, `-`, `*`, `/`, `=`, `<`, `>`
- comments beginning with `//` or `/* ... */`

Whenever an identifier is encountered, the lexer inserts it into a symbol table if it is not already present. It prints the token category and the identifier name. At the end, the symbol table is displayed.

**Typical result:**
The scanner identifies tokens like:

- `Identifier : int`
- `Constant : 10`
- `Operator : =`
- `Comment : // sample comment`

This experiment establishes the basis for building real compilers where scanning is the first stage.

---

### Experiment 2: Tokenizer for Keywords, Identifiers, Delimiters, and Operators

**Objective:**
To implement a lexical analyzer that recognizes keywords, identifiers, numbers, operators, and separators used in a C-like language.

**Concepts covered:**
- Reserved keywords
- Identifier pattern recognition
- Delimiter detection
- Operator classification
- Header file recognition

**Implementation details:**
The lexer checks for:

- preprocessor directives such as `#include`
- header files like `<stdio.h>`
- C keywords including `int`, `float`, `char`, `double`, `if`, `else`, `for`, `while`, `switch`, `case`, etc.
- identifiers and numbers
- arithmetic and relational operators
- punctuation marks such as `;`, `,`, `()`, `{}`, `:`

It prints the type of each token it detects and skips whitespace.

**Why it matters:**
This is a basic but essential step in compiler design because a parser only receives valid tokens from the lexical analyzer.

---

### Experiment 3: Arithmetic Expression Parser

**Objective:**
To parse and validate arithmetic expressions using YACC.

**Concepts covered:**
- Context-free grammar
- Expression grammar
- Operator precedence
- Left associativity
- Syntax validation

**Implementation details:**
The grammar accepts expressions like:

```text
a + b
2 * (x - y)
- (a + b)
```

The grammar defines rules for:

- addition
- subtraction
- multiplication
- division
- unary minus
- parentheses
- numbers and identifiers

A YACC parser checks whether the expression is syntactically valid. If the syntax is wrong, it reports an error.

**Expected behavior:**

- Valid input produces: `Valid Expression`
- Invalid input produces: `Invalid Expression`

This experiment demonstrates how a parser can check the structure of source language statements using formal grammar rules.

---

### Experiment 4: Valid Variable Grammar Recognition

**Objective:**
To verify whether a variable name follows a valid format according to the language grammar.

**Concepts covered:**
- Grammar design
- Terminal and non-terminal symbols
- Recursive variable definitions
- Identifier validation

**Implementation details:**
The grammar accepts variable names built from letters and digits, such as:

```text
a
x1
var123
```

The program checks whether the variable name is valid according to the recursive production:

```text
var -> var DIG | var LET | LET
```

This ensures names begin with a letter and can include digits afterward.

**Expected behavior:**
If the input matches the defined pattern, the parser prints `Valid variable`; otherwise it prints `Invalid variable`.

This experiment introduces grammar-based validation of identifiers, which is fundamental in programming language design.

---

### Experiment 5: Parser for Control Structure Syntax

**Objective:**
To detect and validate C-style control structures such as `if`, `else`, `for`, `while`, and `switch` statements.

**Concepts covered:**
- Structured programming constructs
- Parsing control flow syntax
- Conditional statements
- Loop constructs
- Switch-case grammar

**Implementation details:**
The lexer extracts tokens for:

- keywords: `if`, `else`, `for`, `while`, `switch`, `case`, `default`
- identifiers
- numbers
- comparison operators
- braces and parentheses
- statements and conditions

The parser grammar validates:

- if-else blocks
- while loops
- for loops
- switch-case blocks

This experiment shows how syntactic rules are used to ensure the program structure follows valid control-flow patterns.

**Expected behavior:**
The parser prints a success message when the control structure grammar is valid and an error message otherwise.

---

### Experiment 6: Calculator Using Yacc and Lex

**Objective:**
To build a calculator that evaluates arithmetic expressions entered at runtime.

**Concepts covered:**
- Arithmetic expression evaluation
- Operator precedence
- Recursive descent-like grammar in YACC
- Semantic actions in parsing

**Implementation details:**
The grammar includes:

- addition
- subtraction
- multiplication
- division
- unary minus
- parentheses

Each grammar rule has an associated semantic action that computes the value of the expression. The program reads expression input and prints the result, such as:

```text
Enter the expression:
5 + 3 * 2
Answer: 11
```

This experiment is a practical introduction to parser-generated evaluation of arithmetic expressions.

---

### Experiment 7: Three-Address Code (TAC) Generation

**Objective:**
To generate intermediate three-address code for expressions used in assignment statements.

**Concepts covered:**
- Intermediate representation
- Three-address code (TAC)
- Expression tree breakdown
- Temporary variable generation

**Implementation details:**
The program takes expression statements of the form:

```text
a = b + c * d
```

and converts them into TAC-like instructions such as:

```text
t1 = b + c
t2 = t1 * d
a = t2
```

The lexer recognizes identifiers and numbers, and the parser generates temporary variables to store intermediate results. This representation is significant in compiler optimization and code generation.

**Importance:**
Three-address code is an intermediate stage between the source program and target machine code, making optimization and translation easier.

---

### Experiment 8: Type Checking of Variables and Expressions

**Objective:**
To detect whether variable declarations and assignments match the correct type rules.

**Concepts covered:**
- Type systems
- Variable declaration
- Type inference
- Type mismatch detection

**Implementation details:**
The parser recognizes declarations such as:

```text
int a;
float b;
```

and assignments such as:

```text
a = b + 4;
```

The symbol table stores each variable name and its declared type. The program checks whether an expression uses variables of compatible types. If not, it reports a type mismatch.

**Typical output:**

- `No type mismatch in expression`
- `Type mismatch in assignment to a`
- `Undefined variable: x`

This experiment introduces one of the core features of semantic analysis: checking correctness of data types.

---

### Experiment 9: Semantic Analysis and Type Validation

**Objective:**
To validate whether expressions and assignments follow the type rules of the language.

**Concepts covered:**
- Semantic analysis
- Variable lookup
- Type compatibility
- Expression checking

**Implementation details:**
The grammar allows declarations and assignments, and a table tracks each variable’s type. While parsing an assignment, the program compares the type of the left-hand side variable with the type of the expression on the right-hand side. If they are compatible, the expression is accepted; otherwise, a warning or error is reported.

This experiment is foundational for understanding how compilers ensure that code is not only syntactically valid but also semantically meaningful.

---

### Experiment 10: Simple Code Generation for Assembly-Like Instructions

**Objective:**
To translate expressions into simplified assembly-style operations.

**Concepts covered:**
- Code generation
- Machine-level instruction mapping
- Simple register-based translation
- Expression-to-INSTRUCTION conversion

**Implementation details:**
The parser handles assignments and arithmetic operations and prints instructions analogous to machine code, for example:

```text
MOV AX, a
ADD AX, b
MOV x, AX
```

or for multiplication and division:

```text
MUL b
DIV c
```

This is a very simplified form of code generation where expressions are converted into operations that resemble assembly instructions.

**Why it matters:**
This experiment introduces the final phase of a compiler pipeline where high-level language constructs are converted into lower-level machine-oriented operations.

---

## 5. Learning Outcomes

After completing this lab series, students are expected to understand:

- how lexical analysis splits source code into tokens
- how parsers validate syntax using grammar rules
- how symbol tables help in scanning and semantic checking
- how arithmetic expressions are evaluated and transformed
- how control structures are recognized in source programs
- how intermediate code is generated and used for optimization
- how type checking ensures semantic correctness
- how code generation produces low-level instructions from source programs

---

## 6. General Compilation Workflow

For most experiments, the typical workflow is:

```bash
flex expN.l
bison -dy expN.y
gcc lex.yy.c y.tab.c -o expN -lfl -ly
./expN
```

If the experiment contains only a Lex file, the compilation is simpler:

```bash
flex expN.l
gcc lex.yy.c -o expN -lfl
./expN
```

---

## 7. Practical Significance

Compiler design is a core subject in computer science and software engineering. These experiments model how real compilers operate in practice. The concepts learned here support the development of:

- programming languages
- translators
- interpreters
- code optimizers
- static analyzers
- compiler front ends and back ends

---

## 8. Conclusion

This lab set provides a complete foundation in compiler construction using Lex and Yacc. From lexical analysis to syntax parsing, semantic checking, intermediate code generation, and machine-oriented code generation, the experiments collectively demonstrate the complete pipeline of modern compiler design.

The repository is intended to help students understand both the theory and the implementation of translation from source code to executable machine instructions.
