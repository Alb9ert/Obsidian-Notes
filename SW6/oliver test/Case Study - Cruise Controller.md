# Case Study: Cruise Controller

Related: [[Design Paradigms]] · [[Events and Event-Triggered Components]] · [[Parallel Composition]]

## System Context (Top-Level Block Diagram)
The **CruiseController** interacts with:

| Component | Signal |
|---|---|
| **Clock** | → `event second` |
| **Sensor** | → `event rotate` (wheel rotation pulse) |
| **Driver** | → `event cruise`, `event inc`, `event dec` |
| **ThrottleController** | ← `event(real) F` (throttle force) |
| **Display** | ← `nat speed`, `event(nat) cruiseSpeed` |

---

## Decomposition
```
CruiseController = MeasureSpeed || SetSpeed || ControlSpeed
```

### MeasureSpeed
- **Inputs**: `event rotate`, `event second`
- **Output**: `nat speed`
- **Behaviour**: Counts `rotate` events per `second` interval → current speed

### SetSpeed
- **Inputs**: `event cruise`, `nat speed`, `event inc`, `event dec`
- **Output**: `event(nat) cruiseSpeed`
- **Behaviour**: Maintains the desired cruise speed target

```
nat s := minSpeed;  bool on := 0

if cruise? then {
    on := ¬on;
    if (speed < minSpeed) then s := minSpeed
    else if (speed > maxSpeed) then s := maxSpeed
    else s := speed
}
else if [dec? ∧ on ∧ (s > minSpeed)] then s := s − 1
     else if [inc? ∧ on ∧ (s < maxSpeed)] then s := s + 1;
if on then cruiseSpeed := s
```

- **cruise** toggles cruise mode on/off and captures current speed as the target
- **inc / dec** adjust the target by 1 (only when cruise is on and within bounds)
- `cruiseSpeed` is an event — only emitted while cruise is active

### ControlSpeed
- **Inputs**: `nat speed`, `event(nat) cruiseSpeed`
- **Output**: `event(real) F`
- **Behaviour**: Computes throttle pressure to make actual speed equal desired speed
- Relies on **dynamical systems / control theory** (e.g., PID controller)

---

## Design Decisions Made

> [!tip] Top-Down Choices
> - **Safety bounds**: `SetSpeed` clamps the cruise target to `[minSpeed, maxSpeed]`
> - **Simultaneity**: only one of `cruise`, `inc`, `dec` is assumed active per round (precedence in the if-chain handles multiple)
> - **Separation of concerns**: `ControlSpeed` is cleanly separated — it implements control theory independently of the mode logic in `SetSpeed`

> [!note] Event-Triggered Design
> All three sub-components use event inputs — they act only when relevant events arrive. See [[Events and Event-Triggered Components]].
