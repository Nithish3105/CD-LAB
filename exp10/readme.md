# Experiment 10 — Compiler Back-End: TAC to 8086 Assembly

## Title
Implement the back-end of the compiler for which the three-address code is given as input and the 8086 assembly language code is produced as output.

---

## Aim
To write a program using FLEX and BISON to implement the back-end of a compiler which takes three-address code (TAC) as input and generates equivalent 8086 assembly language code.

---

## Algorithm

### FLEX (`backend.l`)
1. Return `ID` for identifiers (variables and temporaries), storing matched text via `strdup`.
2. Return operator and punctuation characters directly.
3. Skip whitespace and newlines.

### BISON (`backend.y`)
1. Define grammar for TAC assignment statements: `id = expr ;`
2. Emit 8086 instructions during reduction:
   - First operand of `expr` — emit `MOV AX, operand`.
   - `expr + ID` — emit `ADD AX, operand`.
   - `expr - ID` — emit `SUB AX, operand`.
   - `expr * ID` — emit `MUL operand`.
   - `expr / ID` — emit `MOV DX, 0` / `MOV BX, operand` / `DIV BX`.
3. On completing a full statement, emit `MOV result, AX` and a blank line.
4. Repeat for every TAC statement until input ends.

---

## Files
| File | Description |
|------|-------------|
| `backend.l` | FLEX source — tokeniser |
| `backend.y` | BISON source — 8086 code generation grammar |
| `output.txt` | Sample terminal output |

---

## Compile & Run

```bash
# Step 1 — generate the lexer
flex backend.l

# Step 2 — generate the parser
bison -d backend.y

# Step 3 — compile
gcc lex.yy.c backend.tab.c -o backend -lfl

# Step 4 — run (terminate input with Ctrl+D)
./backend
```

---

## Sample Input
```
t1 = a + b;
t2 = t1 - c;
t3 = t2 * d;
t4 = t3 / e;
x = t4;
```

---

## Expected Output
```
Enter TAC statements (end with Ctrl+D):
MOV AX, a
ADD AX, b
MOV t1, AX

MOV AX, t1
SUB AX, c
MOV t2, AX

MOV AX, t2
MUL d
MOV t3, AX

MOV AX, t3
MOV DX, 0
MOV BX, e
DIV BX
MOV t4, AX

MOV AX, t4
MOV x, AX
```

---

## Notes
- The grammar restricts the right-hand side of each TAC statement to a chain of binary operations on identifiers only (no numeric literals), matching typical TAC output.
- The `AX` register is used as the accumulator throughout; `BX` and `DX` are used only for division.
- Each emitted instruction block is separated by a blank line for readability.

---

## Result
Thus, the back-end of the compiler was successfully implemented using FLEX and BISON to translate three-address code into equivalent 8086 assembly language code.
