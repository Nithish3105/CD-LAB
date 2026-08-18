# Experiment 8 — Type Checking using LEX and YACC

## Title
Implement type checking using LEX and YACC.

---

## Aim
To write a program using FLEX and BISON to implement type checking of variables in simple declarations and assignment expressions, using a symbol table built during parsing.

---

## Algorithm

### FLEX (`typecheck.l`)
1. Return keyword tokens `INT` and `FLOAT` for the type keywords.
2. Return `ID` (with `strdup`-ed text in `yylval.str`) for identifiers.
3. Return `NUM` for integer literals (treated as type `int`).
4. Return operator and punctuation characters directly.

### BISON (`typecheck.y`)
1. Maintain a symbol table array of `(name, type)` pairs.
2. `insert(name, type)` adds a variable on declaration; `typeOf(name)` looks it up.
3. Grammar rules:
   - `decl → INT ID ;` / `FLOAT ID ;` — inserts the variable with its declared type.
   - `assign → ID = expr ;` — compares the declared type of the LHS with the resolved type of the RHS.
   - `expr` — propagates type strings upward; returns `"mismatch"` if operand types differ.
4. Prints "No type mismatch", "Type mismatch", or "Undefined variable" accordingly.

---

## Files
| File | Description |
|------|-------------|
| `typecheck.l` | FLEX source — tokeniser |
| `typecheck.y` | BISON source — type-checking grammar + symbol table |
| `output.txt` | Sample terminal output |

---

## Compile & Run

```bash
# Step 1 — generate the lexer
flex typecheck.l

# Step 2 — generate the parser
bison -d typecheck.y

# Step 3 — compile
gcc lex.yy.c typecheck.tab.c -o typecheck -lfl

# Step 4 — run
./typecheck
```

---

## Sample Inputs to Try

```
int a; int b; int c;
a = b * c;
```
```
int a; float b; int c;
a = b + c;
```

---

## Expected Output
```
Enter declarations and expressions:
int a; int b; int c;
a = b * c;
No type mismatch in expression: a = ...

Enter declarations and expressions:
int a; float b; int c;
a = b + c;
Type mismatch in assignment to a
```

---

## Notes
- The program handles multiple declarations and assignments in one session.
- Numeric literals are assumed to be of type `int`.
- Using an undeclared variable prints "Undefined variable: <name>" without crashing.

---

## Result
Thus, the FLEX and BISON program for type checking was successfully implemented. The program builds a symbol table from declarations and checks type consistency in assignment expressions.
