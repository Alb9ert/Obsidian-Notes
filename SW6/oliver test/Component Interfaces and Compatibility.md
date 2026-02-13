# Component Interfaces and Compatibility

Related: [[Task Graphs]] · [[Parallel Composition]] · [[Feedback and Cyclic Dependencies]]

## Interface Definition

> [!definition] Interface
> The **interface** of component $C$ is:
> - Input variables $I$
> - Output variables $O$
> - **Await dependencies**: for each output $y \in O$, which inputs $x \in I$ does $y$ await?

The interface abstracts away internal details (state variables, local variables, code) — only the causal I/O dependencies matter for composition.

---

## Compatibility Definition

> [!definition] Compatible Components
> $C_1$ (outputs $O_1$, await relation $<_1$) and $C_2$ (outputs $O_2$, await relation $<_2$) are **compatible** if:
> 1. **Disjoint outputs**: $O_1 \cap O_2 = \emptyset$
> 2. **Acyclic combined await relation**: $(<_1 \cup\, <_2)$ restricted to I/O variables is **acyclic**

Parallel composition $C_1 \| C_2$ is only defined when $C_1$ and $C_2$ are compatible.

---

## Key Proposition

> [!important] Proposition 2.1
> If $C_1$ and $C_2$ are compatible (combined await-dependencies acyclic), then the task graph of the product $C_1 \| C_2$ is also **acyclic**.
>
> Interface compatibility **guarantees** a valid execution order for the product.

---

## Why Interfaces Are Sufficient
- Compatibility can be checked purely at the **interface level** — no need to look inside
- This enables **modular design**: design components independently, verify compatibility at boundaries, compose safely
- Interfaces capture exactly the information needed for this check

---

## Parallel Composition Definition (Task-Graph Level)
Given compatible $C_1$ and $C_2$, the product $C_1 \| C_2$ has React defined by merging task graphs:
- **Local variables**: $L_1 \cup L_2$
- **Tasks**: $\Pi_1 \cup \Pi_2$
- **Precedence edges**: all edges from $<_1$ and $<_2$, plus new edges where a task of one component reads a variable written by a task of the other

See [[Parallel Composition]] for the full I/O/S/Init definition.

---

## Example: Relay vs SplitDelay with Inverter

| Composition | Compatible? | Reason |
|---|---|---|
| Relay \|\| Inverter | ❌ No | `out` awaits `in` and `in` awaits `out` → cycle |
| SplitDelay \|\| Inverter | ✅ Yes | `out` does NOT await `in` → no cycle |
| Delay \|\| Inverter | ❌ No | Same cycle as Relay |
