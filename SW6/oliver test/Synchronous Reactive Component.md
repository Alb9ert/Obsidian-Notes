# Synchronous Reactive Component (SRC)

Related: [[The Synchronous Model]] · [[SRC Executions]] · [[Component Properties]]

## Definition

> [!definition] SRC
> A **Synchronous Reactive Component** $C = (I,\, O,\, S,\, \text{Init},\, \text{React})$ where:
> - $I$ — set of typed **input variables** → set of inputs $Q_I$
> - $O$ — set of typed **output variables** → set of outputs $Q_O$
> - $S$ — set of typed **state variables** → set of states $Q_S$
> - $\text{Init}$ — initialization code → defines $[\text{Init}] \subseteq Q_S$ (the initial states)
> - $\text{React}$ — reaction description → defines $[\text{React}]$ of reactions $s \xrightarrow{i/o} t$

$[\text{React}]$ is a subset of $Q_S \times Q_I \times Q_O \times Q_S$.

---

## The Block Diagram
Visually an SRC is a box:
- Inputs arrive from the left, outputs exit to the right
- The box contains: state variable declarations + init code, and update code

---

## The Five Parts

### Inputs ($I$)
- Each variable has a type: `bool`, `int`, `nat`, `real`, `{on, off}`, …
- An **input** is a valuation of all input variables

### Outputs ($O$)
- An **output** is a valuation of all output variables
- Output must be **explicitly assigned** in update code (syntax requirement)

### States ($S$)
- A **state** is a valuation of all state variables
- State is **internal** and persists across rounds

### Initialization ($\text{Init}$)
- Sequence of assignments to state variables
- May define a single state or multiple via `choose`
  - `bool x := 0` → $[\text{Init}] = \{0\}$
  - `bool x := choose {0, 1}` → $[\text{Init}] = \{0, 1\}$

### Reactions ($\text{React}$)
- Sequence of assignments and conditionals that update output and state variables
- Each reaction: $(s, i, o, t)$ written as $s \xrightarrow{i/o} t$

> [!warning] Syntax Requirements
> - Types of variables and expressions must match
> - Output variables must be **written before being read**
> - Output variables must be **explicitly assigned** a value
>
> If code cannot be executed → no reaction possible → that $(s,i)$ entry in $[\text{React}]$ is empty.

---

## Example: Delay
```
bool x := 0
out := x;  x := in
```
- $I = \{\text{in}\}$, $O = \{\text{out}\}$, $S = \{x\}$, $[\text{Init}] = \{0\}$
- 4 reactions:

$$0 \xrightarrow{0/0} 0 \quad 0 \xrightarrow{1/0} 1 \quad 1 \xrightarrow{0/1} 0 \quad 1 \xrightarrow{1/1} 1$$

---

## Semantic Equivalence
Two syntactically different components are **semantically equivalent** if they have identical $[\text{React}]$.

> [!example]
> `out := in1² − in2²` and `x:=in1+in2; y:=in1−in2; out:=x*y` — different syntax, same reactions.

A compiler may transform code as long as semantics is preserved.
