# Extended State Machines (ESMs)

Related: [[Synchronous Reactive Component]] · [[Component Properties]] · [[SRC Executions]]

## Motivation
Some components are naturally described with **discrete modes** (states like `on/off`, `idle/running`). ESMs make this explicit.

## Structure

> [!definition] ESM
> An ESM adds a special **mode variable** (ranging over a finite set of modes) to an SRC. Each reaction corresponds to executing a **mode-switch** (a labelled transition).

Each transition on a mode-switch has:
- A **guard** — boolean condition that must hold for the transition to fire; `(condition)?` syntax
- An **update** — code that runs when the transition fires; `-> expression` syntax

Self-loops (staying in the same mode) are valid transitions too.

---

## Example: Switch Component
```
Input: bool press
State: mode ∈ {off, on},  int x := 0

off --[press = 0]?--> off              (stay off)
off --[press = 1]?--> on              (turn on, x unchanged)
on  --[press = 0 & x < 10] -> x:=x+1--> on    (count)
on  --[press = 1 | x >= 10] -> x:=0--> off    (turn off, reset)
```

Initial state: `(off, 0)`

Sample execution:
$$(\text{off},0) \xrightarrow{0} (\text{off},0) \xrightarrow{1} (\text{on},0) \xrightarrow{0} (\text{on},1) \cdots \xrightarrow{0} (\text{on},10) \xrightarrow{0} (\text{off},0)$$

---

## Relation to SRC
ESMs are **syntactic sugar** — the mode variable is just another state variable in $S$. Guards and updates define the reactions in $[\text{React}]$.

> [!tip] Finite-State ESM
> If the mode set is finite **and** all other state variables range over finite types (e.g., `int[0,10]`), the ESM is a [[Component Properties|finite-state component]] and can be represented as a Mealy machine.
