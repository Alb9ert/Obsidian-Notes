# Design Paradigms

Related: [[Parallel Composition]] · [[Component Properties]] · [[Case Study - Cruise Controller]]

## Bottom-Up Design

> [!note] Bottom-Up Approach
> 1. Design basic/primitive components
> 2. Compose existing components in block diagrams to build new, larger ones
> 3. Maintain a **library of reusable components**
> 4. Reuse components at every step

Canonical example: **synchronous circuit design**

### Basic Circuit Library

| Component | I/O | Await | Update |
|---|---|---|---|
| **SyncNot** | `bool in → bool out` | `out` awaits `in` | `out := ~in` |
| **SyncAnd** | `bool in1, in2 → bool out` | `out` awaits `in1, in2` | `out := in1 & in2` |
| **SyncOr** | `bool in1, in2 → bool out` | `out` awaits `in1, in2` | Composed from SyncNot + SyncAnd |

**SyncOr** is built via De Morgan's law: $a \lor b = \neg(\neg a \land \neg b)$
→ `SyncNot(in1) \|\| SyncNot(in2) \|\| SyncAnd` + final `SyncNot`

### Synchronous Latch
- Inputs: `bool set`, `bool reset`; Output: `bool out`
- Initial state: `bool x := choose {0, 1}` (nondeterministic — initial bit value unknown)
- Behaviour: `set=1` → latch stores 1; `reset=1` → latch stores 0; else → holds value

> [!warning] Latch is nondeterministic (multiple initial states) and **not** deterministic when `set=1 & reset=1` simultaneously

### 1BitCounter → 3BitCounter
- `1BitCounter`: composed from SyncOr, SyncAnd, SyncNot, Latch
  - Inputs: `inc`, `start`; Outputs: `out` (current bit), `carry` (overflow)
- `3BitCounter` = three chained `1BitCounter` instances, carry of one feeds `inc` of next

---

## Top-Down Design

> [!note] Top-Down Approach
> 1. Start from the desired **inputs and outputs** of the whole system
> 2. Model the environment and its assumptions
> 3. Write informal/formal behaviour description
> 4. **Decompose** into sub-components with their own interfaces
> 5. Design each sub-component independently

Canonical example: **Cruise Controller** → see [[Case Study - Cruise Controller]]

---

## Comparison

| | Bottom-Up | Top-Down |
|---|---|---|
| Starting point | Primitive building blocks | System-level I/O specification |
| Flow | Build up from components | Break down from requirements |
| Reuse | High — library-oriented | Moderate |
| Example | Synchronous circuits | Cruise controller |
