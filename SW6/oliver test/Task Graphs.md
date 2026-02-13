# Task Graphs

Related: [[Synchronous Reactive Component]] · [[Component Interfaces and Compatibility]] · [[Feedback and Cyclic Dependencies]]

## Motivation
To reason about the ordering of computations within a component's reaction — especially for composition — we split the reaction code into atomic **tasks** and represent their dependencies as a directed acyclic graph.

---

## What Is a Task?

> [!definition] Task
> A task $A = (R,\, W,\, \text{Update})$ where:
> - **Read-set** $R \subseteq I \cup S \cup O \cup L$ — variables the task reads
> - **Write-set** $W \subseteq O \cup S \cup L$ — variables the task writes
> - **Update** — code that computes values in $W$ based on values in $R$

A **task graph** is a DAG of tasks with **precedence edges** $<$:
- $A_1 < A_2$ means $A_1$ must execute before $A_2$

---

## Requirements on a Valid Task Graph

> [!note] R1 — Acyclicity
> The precedence relation $<$ must be **acyclic**. This guarantees at least one valid schedule exists.

> [!note] R2 — Single Writer
> Each output variable is in the write-set of **exactly one** task.

> [!note] R3 — Write Before Read
> If output/local variable $y$ is in the read-set of task $A$, then $y$ must be in the write-set of some task $A'$ such that $A' <^+ A$ (i.e., $A'$ precedes $A$).

> [!note] R4 — Write-Conflict Ordering
> Tasks $A$ and $A'$ have a **write-conflict** if they share a variable (one writes what the other reads or writes). Conflicting tasks must be ordered: either $A <^+ A'$ or $A' <^+ A$.

> [!note] R5 — Schedule Independence
> The set of reactions must **not depend** on which valid schedule is chosen. All valid orderings produce the same result.

---

## Schedules
A **task schedule** is a total ordering of all tasks consistent with precedence edges ($A' < A \Rightarrow A'$ appears before $A$). Multiple valid schedules may exist — they must all give the same result (R5).

Notation: $A' <^+\! A$ means there is a path from $A'$ to $A$ in the task graph.

---

## I/O Await Dependencies
The task graph determines the **await dependencies** exported in the component's interface:

> Output $y$ awaits input $x$ (written $x < y$) if the task writing $y$ directly reads $x$, or some predecessor task reads $x$.

These await dependencies are what the [[Component Interfaces and Compatibility|interface]] exposes to the outside world.

---

## Example: SplitDelay
```
State: bool x := 0

A1: R={x},     W={out}    →   out := x
A2: R={in},    W={x}      →   x := in

Precedence: A1 < A2
```
- Only valid schedule: A1, then A2
- `out` does NOT await `in` (A1 reads only `x`)
- I/O await dependency: none — output is independent of current-round input

This is what makes SplitDelay composable in a feedback loop (see [[Feedback and Cyclic Dependencies]]).
