# Experiment 9 — Code Optimization Techniques

## Title
Implement simple code optimization techniques (Constant Folding, Strength Reduction and Algebraic Transformation).

---

## Aim
To write a program using FLEX and BISON to implement simple code optimization techniques — constant folding, strength reduction, and algebraic simplification — applied while parsing three-address code style assignment statements.

---

## Algorithm

### FLEX (`optimize.l`)
1. Return `ID` for identifiers and `NUM` for integer literals, storing matched text via `strdup`.
2. Return operator and punctuation characters directly.
3. Skip whitespace.

### BISON (`optimize.y`)
1. Define grammar for assignment statements: `id = expr ;`
2. While reducing an `expr` production, apply optimizations:
   - **Constant Folding** — if both operands are numeric literals, compute the result at compile time.
   - **Algebraic Simplification** — apply identities: `x+0→x`, `0+x→x`, `x-0→x`, `x*1→x`, `x/1→x`.
   - **Strength Reduction** — replace `x*2` with `x+x`.
3. Print a comment indicating which optimization fired, then print the optimized statement.

---

## Files
| File | Description |
|------|-------------|
| `optimize.l` | FLEX source — tokeniser |
| `optimize.y` | BISON source — optimization grammar + actions |
| `output.txt` | Sample terminal output |

---

## Compile & Run

```bash
# Step 1 — generate the lexer
flex optimize.l

# Step 2 — generate the parser
bison -d optimize.y

# Step 3 — compile
gcc lex.yy.c optimize.tab.c -o optimize -lfl

# Step 4 — run (terminate input with Ctrl+D)
./optimize
```

---

## Sample Input
```
a = 2 + 4;
b = d * 1;
c = s * 2;
```

---

## Expected Output
```
Enter Three Address Code statements (end with Ctrl+D):
a = 2 + 4;
// Constant Folding: 2+4 -> 6
a = 6

b = d * 1;
// Algebraic Simplification: x*1 -> x
b = d

c = s * 2;
// Strength Reduction: x*2 -> x+x
c = s + s
```

---

## Notes
- `isdigit()` is used to distinguish numeric string operands from identifier operands.
- The `strdup()` calls ensure string values survive across multiple BISON reductions.
- Operator precedence (`%left`) is declared so that BISON builds the correct parse tree before applying optimizations.

---

## Result
Thus, the FLEX and BISON program for simple code optimization techniques — constant folding, strength reduction, and algebraic simplification — was successfully implemented and tested with various inputs.
