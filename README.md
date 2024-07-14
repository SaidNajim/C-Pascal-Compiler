# C-Pascal-Compiler
This repository contains the source code for a simple Pascal to C compiler. The compiler can generate intermediate code (p-code) from a Pascal program. The p-code is then executed by an interpreter.

## Features

* **Lexical analysis**: Converts the input source code into a sequence of tokens.
* **Syntax analysis**: Checks the tokens against the grammar rules to form a parse tree.
* **Semantic analysis**: Ensures the parse tree adheres to the language's semantic rules, like type checking.
* **P-code generation**: Translates the validated source code into intermediate code (p-code).
* **P-code interpretation**: Executes the p-code.

## Project Structure

* `compiler.c`: Contains the source code of the compiler (lexical analyzer, syntax analyzer, semantic analyzer, and p-code generator).
* `tokens.h`: Definition of language tokens
* `errors.h`: Definition of language errors
* `run.c`: Code to execute the compiler

## Example Execution

1. Write a Pascal program in the `\tests` folder (e.g., `code1.p`).
2. Compile the compiler with the command `gcc run.c -o run`.
3. Run the compiler with the command `run code1.p`.

## Definitions of Non-terminal Symbols
- `PROGRAM` ::= program ID ; BLOCK .
- `BLOCK` ::= CONSTS VARS INSTS
- `CONSTS` ::= const ID = NUM ; { ID = NUM ; } | ε (epsilon)
- `VARS` ::= var ID { , ID } ; | ε
- `INSTS` ::= begin INST { ; INST } end
- `INST` ::= INSTS | ASSIGN | IF | WHILE | WRITE | READ | ε
- `ASSIGN` ::= ID := EXPR
- `IF` ::= if COND then INST
- `WHILE` ::= while COND do INST
- `WRITE` ::= write ( EXPR { , EXPR } )
- `READ` ::= read ( ID { , ID } )
- `COND` ::= EXPR RELOP EXPR
- `RELOP` ::= = | <> | < | > | <= | >=
- `EXPR` ::= TERM { ADDOP TERM }
- `ADDOP` ::= + | -
- `TERM` ::= FACT { MULOP FACT }
- `MULOP` ::= * | /
- `FACT` ::= ID | NUM | ( EXPR )
- `ID` ::= letter { letter | digit }
- `NUM` ::= digit { digit }
- `digit` ::= 0 |..| 9
- `letter` ::= a | b |..| z | A |..| Z

Non-terminal symbols are in uppercase and represent syntactic constructs, while terminal symbols are in lowercase and represent basic elements such as keywords, operators, identifiers, and numbers. The symbol ε (epsilon) represents an empty production.

## **Semantic Rules**

- **Rule 1**: All declarations in `CONSTS` and `VARS` must be unique.
- **Rule 2**: NO DUPLICATE DECLARATIONS
- **Rule 3**: After `BEGIN`, all symbols must be already declared
- **Rule 4**: A constant cannot change value in the program
- **Rule 5**: The program ID cannot be used within the program

## Instruction Set

### Arithmetic Instructions

- `ADD`: Adds the second-to-top value on the stack to the top value, and leaves the result on the top of the stack.
- `SUB`: Subtracts the top value of the stack from the second-to-top value, and leaves the result on the top of the stack.
- `MUL`: Multiplies the second-to-top value on the stack by the top value, and leaves the result on the top of the stack.
- `DIV`: Divides the second-to-top value on the stack by the top value, and leaves the result on the top of the stack.

### Comparison Instructions

- `EQL`: Leaves 1 on the top of the stack if the second-to-top value is equal to the top value, otherwise leaves 0.
- `NEQ`: Leaves 1 on the top of the stack if the second-to-top value is not equal to the top value, otherwise leaves 0.
- `GTR`: Leaves 1 on the top of the stack if the second-to-top value is greater than the top value, otherwise leaves 0.
- `LSS`: Leaves 1 on the top of the stack if the second-to-top value is less than the top value, otherwise leaves 0.
- `GEQ`: Leaves 1 on the top of the stack if the second-to-top value is greater than or equal to the top value, otherwise leaves 0.
- `LEQ`: Leaves 1 on the top of the stack if the second-to-top value is less than or equal to the top value, otherwise leaves 0.

### Stack Manipulation Instructions

- `PRN`: Prints the top value of the stack and pops it.
- `INN`: Reads an integer and stores it at the address found on the top of the stack, then pops the stack.
- `INT c`: Increments the stack pointer by the constant `c` (the constant `c` can be negative).
- `LDI v`: Pushes the value `v` onto the stack.
- `LDA a`: Pushes the address `a` onto the stack.
- `LDV`: Replaces the top of the stack with the value found at the address indicated by the top of the stack (dereference).
- `STO`: Stores the top value of the stack at the address indicated by the second-to-top value, then pops the stack twice.

### Branching Instructions

- `BRN i`: Performs an unconditional branch to instruction `i`.
- `BZE i`: Branches to instruction `i` if the top of the stack is equal to 0, then pops the stack.

### Other Instructions

- `HLT`: Stops the execution of the program.

## Contributing

If you find this project helpful, please give it a star ⭐. Contributions are welcome; fork the repo and submit a pull request. Thank you!
