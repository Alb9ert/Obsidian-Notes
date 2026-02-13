# SRC Executions

Related: [[Synchronous Reactive Component]] · [[The Synchronous Model]]

## Definition

> [!definition] Execution
> Given $C = (I, O, S, \text{Init}, \text{React})$:
> 1. Choose initial state $s_0 \in [\text{Init}]$
> 2. Repeatedly execute rounds $n = 1, 2, 3, \ldots$:
>    - Choose input $i_n \in Q_I$
>    - Execute React: produce output $o_n$ and next state $s_n$ such that $s_{n-1} \xrightarrow{i_n / o_n} s_n \in [\text{React}]$

Sample execution trace:

$$s_0 \xrightarrow{i_1/o_1} s_1 \xrightarrow{i_2/o_2} s_2 \xrightarrow{i_3/o_3} s_3 \;\cdots$$

---

## Nondeterministic Executions
If for some $(s, i)$ there are multiple valid $(o, t)$ pairs, multiple executions are possible — modelling **uncertainty** or **lossy** behaviour.

Example: `x := choose {in, x}` — the component may or may not update its state.

---

## Blocking
If no reaction exists for a given $(s, i)$, the component **blocks** — it cannot proceed.
A blocking component makes implicit **assumptions about the environment** (it will never receive that input in that state).

See [[Component Properties]] for the formal definition of *input-enabled* vs *blocking*.

---

> [!warning] Variable Naming
> Everything within a round executes "simultaneously" (synchrony hypothesis). Do **not** reuse variable names across composed components — each instance needs distinct names.
> See [[Parallel Composition]] for the instantiation/renaming operation.
