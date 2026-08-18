# Experiment 7 — Three Address Code Generation

## Title
Generate three address code for a simple program using LEX and YACC.

---

## Aim
To write a program using FLEX and BISON to generate three-address code (TAC) for a simple arithmetic expression.

---

## Algorithm

### FLEX (`tac.l`)
1. Include required headers and the BISON-generated token header.
2. Define patterns to return `ID` for identifiers and `NUM` for numeric constants, storing the matched text in `yylval.str`.
3. Skip whitespace and pass all other single characters directly to BISON.

### BISON (`tac.y`)
1. Declare tokens `ID` and `NUM` with `%union` using a `char *str` field.
2. Set operator associativity: `+` and `-` are left-associative; `*` and `/` are left-associative.
3. Define grammar:
   - `stmt → ID = expr` — prints the final assignment.
   - `expr → expr op expr` — generates a new temporary `t1`, `t2`, … and prints the TAC instruction.
   - `expr → ID | NUM` — returns the operand as-is.
4. Maintain a global `tempCount` counter to name temporaries sequentially.
5. `main()` prompts and calls `yyparse()`; `yyerror()` reports errors.

---

## Files
| File | Description |
|------|-------------|
| `tac.l` | FLEX source — tokeniser |
| `tac.y` | BISON source — TAC generation grammar |
| `output.txt` | Sample terminal output |

---

## Compile & Run

```bash
# Step 1 — generate the lexer
flex tac.l

# Step 2 — generate the parser
bison -d tac.y

# Step 3 — compile
gcc tac.tab.c lex.yy.c -o tac -lfl

# Step 4 — run
./tac
```

---

## Expected Output
```
Enter the expression:
a = b + c * d
t1 = c * d
t2 = b + t1
a = t2
```

---

## Notes
- Temporaries are generated in the order BISON reduces sub-expressions (bottom-up), so the innermost operation (higher precedence) gets `t1` first.
- `strdup()` is used to preserve token strings across reductions since BISON may overwrite `yylval`.

---

## Result
Thus, the program to generate three-address code using FLEX and BISON was executed and verified successfully.
