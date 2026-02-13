# Component Properties

Related: [[Synchronous Reactive Component]] · [[Extended State Machines]] · [[Parallel Composition]]

## Finite-State Components

> [!definition] Finite-State
> A component where **all variables** (input, output, state) range over **finite types**.

Finite types: `bool`, enumerated types like `{on, off}`, bounded integers `int[-5, 5]`.

- **Delay** is finite-state; **DiffSquare** is not (unbounded `int`)
- Finite-state components can be represented as **Mealy machines** and are amenable to exact algorithmic analysis

### Mealy Machines
A graph where nodes = states, edges labelled $i/o$ = transitions. Good for visualising finite-state components.

---

## Combinational Components

> [!definition] Combinational
> A component with **no state variables** ($S = \emptyset$). Output depends only on the current input — no memory across rounds.

Examples: DiffSquare, logic gates (SyncNot, SyncAnd, SyncOr).
**Delay** is NOT combinational.

---

## Deterministic vs Nondeterministic

> [!definition] Deterministic
> A component where:
> 1. It has a **single** initial state
> 2. For every state $s$ and input $i$, there is a **unique** $(t, o)$ such that $s \xrightarrow{i/o} t$

- Same input sequence → same output sequence every time (predictable, repeatable)
- **Delay** is deterministic; **LossyDelay** (`x := choose {in, x}`) is not
- Nondeterminism ≠ probabilistic — it models **unknown / uncertain** behaviour

---

## Input-Enabled Components

> [!definition] Input-Enabled
> For **every** state $s$ and input $i$, there exists at least one valid reaction $s \xrightarrow{i/o} t$.

- Component can handle any input in any state — never blocks
- **Delay** is input-enabled
- **BlockingDelay** (`if x != in then {out:=x; x:=in}`) is NOT — it blocks when `x == in`

> [!warning]
> A non-input-enabled component implicitly **assumes** something about its environment. When composing, you must verify these assumptions are satisfied.

---

## Closure Under Parallel Composition
When composing ([[Parallel Composition]]):
- Both finite-state → product is finite-state ($n_1 \times n_2$ states)
- Both deterministic → product is deterministic
