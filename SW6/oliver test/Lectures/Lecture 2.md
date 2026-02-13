# Lecture 2 — Structure and Composition

> [!note] Coverage
> Extends the model with state machines, composition, task graphs, and design methodology.

---

1. [[Extended State Machines]]
   - Adding mode variables, guards, and updates to components
   - How ESMs encode into the SRC framework

2. [[Component Properties]]
   - Finite-state, combinational, deterministic, input-enabled
   - Mealy machines as the finite-state special case

3. [[Events]]
   - The event type: absent or present
   - Event-triggered execution and default absence

4. [[Parallel Composition]]
   - Combining components with the product operator $C_1 \| C_2$
   - I/O/state of the product; output hiding

5. [[Feedback and Cyclic Dependencies]]
   - When composition creates cycles — and why that's a problem
   - Await dependencies and the condition for safe feedback

6. [[Task Graphs]]
   - Splitting reaction code into tasks with read/write sets
   - The five requirements for a valid task graph

7. [[Component Interfaces and Compatibility]]
   - Interfaces as a summary: inputs + outputs + await dependencies
   - Compatibility condition and Proposition 2.1

8. [[Design Paradigms]]
   - Bottom-up: building from a component library
   - Top-down: decomposing from a desired I/O spec

9. [[Case Study - Cruise Controller]]
   - Full worked example: MeasureSpeed, SetSpeed, ControlSpeed
   - How top-down design plays out in practice
