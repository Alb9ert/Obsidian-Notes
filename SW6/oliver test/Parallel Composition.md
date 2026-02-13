# Parallel Composition

Related: [[Synchronous Reactive Component]] · [[Component Interfaces and Compatibility]] · [[Feedback and Cyclic Dependencies]]

## Motivation
Complex components are built by **composing** simpler ones. The parallel composition operator $C_1 \| C_2$ formalises "run both concurrently."

---

## Instantiation / Renaming
Before composing, each instance must have **distinct variable names**:
- Explicitly rename I/O variables: `Delay[out -> temp]`
- State variables are implicitly renamed to avoid conflicts
- Always use **fresh names** per instance

**Example:** `DoubleDelay = (Delay1 \|\| Delay2) \setminus \text{temp}` where:
- `Delay1 = Delay[out -> temp]`
- `Delay2 = Delay[in -> temp]`

---

## Compatibility Conditions
$C_1$ and $C_2$ can be composed only if they are **compatible**:

> [!definition] Compatibility (structural)
> - May share **input variables** (both read the same input)
> - Must **not** share **output variables** (one component "owns" each output)
> - Must **not** share **state variables** (resolved by implicit renaming)
> - The combined await-dependency relation $(<_1 \cup <_2)$ must be **acyclic**

See [[Component Interfaces and Compatibility]] for the full interface-level definition.

---

## Product: I, O, S, Init

| | Definition |
|---|---|
| **Outputs** | $O_1 \cup O_2$ |
| **Inputs** | $(I_1 \cup I_2) \setminus (O_1 \cup O_2)$ — inputs not produced internally |
| **States** | $S_1 \cup S_2$ — state is a pair $(s_1, s_2)$ |
| **Init** | $\text{Init}_1;\, \text{Init}_2$ — order doesn't matter; $[\text{Init}] = [\text{Init}_1] \times [\text{Init}_2]$ |

> [!tip] Inputs of the Product
> A variable is an input of the product if it is an input of one component **and not an output of the other**. Internal wires disappear from the external interface.

---

## Reactions of the Product
The product's task graph merges both components' task graphs:
- **Local variables**: $L_1 \cup L_2$
- **Tasks**: $\Pi_1 \cup \Pi_2$
- **Precedence edges**: all edges from $<_1$ and $<_2$, **plus** an edge $A_1 \to A_2$ whenever $A_2$ reads a variable written by $A_1$

---

## Properties

> [!note] Closure Properties
> - **Commutative**: $C_1 \| C_2 = C_2 \| C_1$
> - **Associative**: order of grouping doesn't matter
> - Both **finite-state** → product is finite-state ($n_1 \times n_2$ states)
> - Both **deterministic** → product is deterministic

---

## Output Hiding / Encapsulation

> [!definition] Output Hiding
> $C \setminus y$ — output variable $y$ is demoted to a **local variable**: it is no longer visible to the outside world.

Used to hide internal wires after composition. Example: `temp` in `DoubleDelay = (Delay1 \|\| Delay2) \setminus temp`.
