# Feedback and Cyclic Dependencies

Related: [[Parallel Composition]] · [[Task Graphs]] · [[Component Interfaces and Compatibility]]

## The Problem
When composing $C_1 \| C_2$, an output of $C_1$ may feed into $C_2$, and an output of $C_2$ may feed back into $C_1$. This raises the question: **in what order do we execute React$_1$ and React$_2$?**

---

## Await Dependencies
Each output variable may **await** input variables — it cannot be produced until those inputs are available.

> [!definition] Await Dependency
> Output $y$ **awaits** input $x$ (written $x < y$) if the task writing $y$ reads $x$, or some predecessor task (via $<^+$) reads $x$.

---

## When Is Feedback Problematic?

> [!warning] Combinational Cycle
> If $C_1$'s output awaits $C_2$'s output **and** $C_2$'s output awaits $C_1$'s output → **cycle** → no valid execution order exists.
> Such a composition is **disallowed**.

**Example (not allowed):**
- **Relay**: `out := in` → `out` awaits `in`
- **Inverter**: `in := ~out` → `in` awaits `out`
- Composed: `out` awaits `in` awaits `out` → **cycle → incompatible**

---

## When Is Feedback OK?

Feedback is fine when a component can produce its output **without waiting for the feedback value** — typically when it reads its own state first.

**Example (allowed):**
- **SplitDelay**: Task A1 (`out := x`) runs before Task A2 (`x := in`)
  - `out` does NOT await `in` — A1 reads only state `x`
- **Inverter**: `in` awaits `out`
- Composed: `in` awaits `out`, and `out` does NOT await `in` → **no cycle → compatible**

> [!tip] Key Insight
> A **delay element** in the feedback path breaks the combinational cycle. The delayed output depends on the *previous* round's input, not the current one.

---

## Formal Rule

> [!definition]
> Composition $C_1 \| C_2$ is **only allowed** when $C_1$ and $C_2$ are **compatible** — i.e., the combined await relation $(<_1 \cup <_2)$ is **acyclic**.

See [[Component Interfaces and Compatibility]] for the full definition.
