# Events

- An input/output variable can be of type **event**.
- An event can be **absent**, or **present** (in which case it has a value):
	- `event x` means $x$ ranges over $\{\text{absent}, \text{present}\}$
	- `event(bool) x` means $x$ ranges over $\{\text{absent}, 0, 1\}$
	- `event(nat) x` means $x$ ranges over $\{\text{absent}, 0, 1, 2, \ldots\}$

>[!tip] **Event-based communication:**
	>- If no value is assigned to an output event, then it is **absent** (by default).
	>- Event-triggered components execute only in rounds where input events are **present** (actual definition slightly more general; see textbook).
	>- Motivation: the notion of “clock” can be different for different components.

## Testing for presence

- **Syntax:** `x?` means “test for presence”.
- **Syntax:** `x!v` means the assignment $x := v$ (and `x!` means $x := \text{present}$).
	- minute! emits event?

### Example
![[Pasted image 20260210125303.png]]