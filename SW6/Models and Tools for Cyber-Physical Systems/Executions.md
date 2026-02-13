# Definition of Executions

- Given [[Synchronous Reactive Component|Component]]$C = (I, O, S, Init, React)$, what are its executions?
- Initialize the state to some state $s_0 \in [Init]$.
- Repeatedly execute rounds. In each round $n = 1, 2, 3, \ldots$:
  - Choose an input value $i_n \in Q_2^I$.
  - Execute $React$ to produce output $o_n$ and change the state to $s_n$, i.e.

    $$
    s_{n-1} \xrightarrow{i_n/o_n} s_n \in [React].


$$
- Sample execution:
  
$$

  s_0 \xrightarrow{i_1/o_1} s_1 \xrightarrow{i_2/o_2} s_2 \xrightarrow{i_3/o_3} s_3 \xrightarrow{\ \cdots\ } \cdots

$$

## Example
![[Pasted image 20260203142954.png]]

- Define initial states
- Compute the Reaction
	- Say input is 1. What is the output