## Synchronous Reactive Component

### Definition
SRC component is a tuple of C = (I, O, S, Init, React)

- Set $I$ of typed input variables: gives set $Q_I$ of inputs
- Set $O$ of typed output variables: gives set $Q_O$ of outputs
- Set $S$ of typed state variables: gives set $Q_S$ of states
- Initialization code $Init$: defines set [Init] of initial states
- Reaction description $React$: defines set $[React]$ of reactions of the form  $s \xrightarrow{i/o} t$, where $s,t$ are states, $i$ is an input, and $o$ is an output.

![[Synchronous Reactive Components.png]]

---

## 01 Inputs

- Each component has a set $I$ of **input variables**.
	- Each variable has a type (bool, int etc.)
- **Input:** Valuation of all input variables.
	- This set of inputs is denoted $Q_I$

---

## 02 Outputs
- Each component has set O if **typed output variables**
- **Output**: valuation of all **output variables**
	- Denoted: $Q_O$

---

## 03 States
The set of **states** is denoted $Q_s$
For delay:
▪ The **set of states** is {0, 1}

---

## 04 Initialization
The set $[Init]$ of **initial states**, which is a **subset** of $Q_S$
For delay The set $[Init]$ of initial states is {0}

---

## 05 Reactions

For **delay**, there are 4 reactions:
$$
0 \xrightarrow{0/0} 0,\quad
0 \xrightarrow{1/0} 1,\quad
1 \xrightarrow{0/1} 0,\quad
1 \xrightarrow{1/1} 1.
$$

**Notation:** current state $s$, followed by input/output $i/o$, and finally the next state $t$; i.e. $s \xrightarrow{i/o} t$.

---

### Syntax error
 If update code cannot be executed, then no reaction possible 
 
Update code expected to satisfy a number of requirements:
 ▪ Types of variables and expressions should match 
 ▪ Output variables must first be written before being read ▪ Output variable must be explicitly assigned a value
## Examples: 
#todo

