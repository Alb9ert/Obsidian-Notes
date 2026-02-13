# Reactive vs Functional Computation

## Functional Computation
- A program takes inputs and produces outputs — behaviour described as a mathematical function
- Canonical models: Turing machines, lambda calculus
- Examples: sorting names, shortest path in a graph

## Reactive Computation
- System **continuously** interacts with its environment via inputs and outputs
- Desired behaviour: which sequences of I/O interactions are acceptable?
- Examples: cruise controller, coffee machine, pacemaker

> [!tip] Key Distinction
> Functional = runs to completion once. Reactive = runs forever (ongoing interaction).

## Sequential vs Concurrent Computation

| | Description |
|---|---|
| **Sequential** | Instructions executed one at a time (Turing machines) |
| **Concurrent** | Multiple components exchanging information, evolving together |

- **Logical concurrency** — components appear to act simultaneously at the logical level
- **Physical concurrency** — components run simultaneously on different hardware
- Key distinction within concurrent systems: **Synchronous vs Asynchronous**

> [!note] Links
> - [[The Synchronous Model]] — how we model synchronous reactive systems
> - [[Synchronous Reactive Component]] — the formal language for components
