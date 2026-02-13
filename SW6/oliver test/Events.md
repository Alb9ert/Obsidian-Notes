# Events and Event-Triggered Components

Related: [[Synchronous Reactive Component]] · [[Component Properties]] · [[Case Study - Cruise Controller]]

## The Event Type

> [!definition] Event
> An input/output variable of type **event** is either **absent** or **present** (optionally carrying a value).

| Type declaration | Range |
|---|---|
| `event x` | $\{\text{absent},\, \text{present}\}$ |
| `event(bool) x` | $\{\text{absent},\, 0,\, 1\}$ |
| `event(nat) x` | $\{\text{absent},\, 0,\, 1,\, 2,\, \ldots\}$ |

---

## Syntax

| Syntax | Meaning |
|---|---|
| `x?` | Test for **presence** of $x$ |
| `x!v` | Emit $x$ with value $v$ |
| `x!` | Emit $x$ as present (no value needed) |

---

## Event-Based Communication

> [!tip] Default Absence
> If no value is assigned to an output event variable in update code, it is **absent** by default.

> [!tip] Event-Triggered Execution
> An event-triggered component only executes in rounds where its **triggering input events are present**. It skips rounds where those events are absent.
>
> Motivation: different components can effectively have different "clocks."

---

## Example: SecondToMinute
```
int x := 0

if second? then {
    x := x + 1;
    if x == 60 then { minute!; x := 0; }
}
```
- Inputs: `event second`
- Output: `event minute`
- Fires `minute!` every 60th time `second` is present
- This is an **event-triggered** component — does nothing in rounds where `second` is absent
