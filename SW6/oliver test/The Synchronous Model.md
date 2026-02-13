# The Synchronous Model

Related: [[Reactive vs Functional Computation]] · [[Synchronous Reactive Component]]

## Core Idea
All components execute in a sequence of **logical rounds** in **lock-step** — every component reads its inputs, computes, and writes its outputs simultaneously.

> [!definition] Synchronous Model
> At each clock tick (round), every component executes its reaction. Time advances in discrete, globally synchronised steps.

Used in synchronous languages: Esterel, Lustre, VHDL, Verilog, Stateflow.

---

## Synchrony Hypothesis

> [!important] Synchrony Hypothesis
> The time needed to execute update code is **negligible** compared to the delay between successive input arrivals.
>
> As a logical abstraction:
> - Execution of update code takes **no time**
> - Production of outputs and reception of inputs occurs at the **same instant**
> - When multiple components are composed, all execute **synchronously and simultaneously**

This is a **design-time abstraction**. The implementation must ensure it actually holds.

---

## Time-Triggered Communication
Example: **Controller Area Network (CAN)** protocol in automobiles
- Time divided into slots
- In each slot, **exactly one component** sends a message over the shared bus
- Achieves synchronous communication over distributed hardware

---

## Benefit vs Challenge
| | |
|---|---|
| **Benefit** | Design is simpler — no need to reason about arbitrary interleaving |
| **Challenge** | Ensuring synchronous execution when the platform is not single-chip hardware |
