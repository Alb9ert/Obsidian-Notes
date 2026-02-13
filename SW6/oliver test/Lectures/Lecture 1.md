# Lecture 1 — Foundations of the Synchronous Model

> [!note] Coverage
> Introduces reactive computation, the synchronous model, and the formal SRC definition.

---

1. [[Reactive vs Functional Computation]]
   - What makes reactive systems different from classical programs
   - Sequential vs concurrent; synchronous vs asynchronous

2. [[The Synchronous Model]]
   - Lock-step, round-based execution
   - The synchrony hypothesis and its practical justification

3. [[Synchronous Reactive Component]]
   - Formal definition: the 5-tuple $(I, O, S, \text{Init}, \text{React})$
   - How components are specified and what a reaction means

4. [[SRC Executions]]
   - How a component runs over time: execution traces
   - Nondeterminism and blocking
