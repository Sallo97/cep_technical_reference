**IMPORTANT:** *as of now we are not sure if this circuit is actually a part of the CEP, it could be just a general example regarding how registers are implemented in the machine. Still I decided to add it, since it provides useful informations on how data is stored in registers.*

*Update: from the control panel there are nominated registers $A,B,C$, so maybe they **do** exist.*

# Combinatorial & Sequential Circuits A, B, C [[1](../0_Additional_resources/0_reference.md)]
A **Combinatorial Circuit** is one in which the output depends only on the current input. **It has no memory**.

A **Sequential Circuit** is one in which the output depends on the current inputs plus the previous state. **It has memory**.

In practice there could not exist a purely combinatorial or purely sequential circuit.

In the CEP, the **Combinatorial & Sequential Circuits** in the **central processing unit (CPU)** $A,B,C$ manage information flow inside the Micro-programming Control Unit (MCU), determining the current value for registers and coordinate the micro-program in execution.

These units are described as being both combinatorial and sequential because:

- A block of logic gates compute the *"next state"* based on current inputs.

- The *"retention of information"* is delegated to an attached register.

During execution the register hold the value of the previous state and feeds it back into the combinatorial logic input. The computed next state will update the register's content.


Before explaining the structure, we provide some nomenclature:

- $x_c, \alpha_i, \beta_i$ are **control signals**.

- $R_i, R_k, R_l$ are **registers**, having associated the pulses $P_i, P_k, P_l$.

- $d$ defines a **delay gate**.

- $(p)$ describes the **degree of parallelism** of a component, i.e. the number of bits associated to it.

![image](../../resources/sequential_circuits.png)


## Circuit and register communication
Each combinatorial-sequential circuit pair operates using the same logic. To illustrate it, we will examine circuit $A$ and its associated register $R_k$.

During the determination of the next state, circuit $A$ generates output signal $Y_k^{(p)}$, representing the next value of register $R_k$. When the timing pulse $P_k$ arrives, the input gates of register $R_k^{(p)}$ open, allowing it to capture and store the new value. 

The stored value is then emitted as the "current state" signal, first processed by the delay component $d$ on order to ensure no race condition happens. It eventually reaches the circuit inputs as $y_k^{(p)}$.

## Inputs
For each circuit their **previous state** $y_x^{(p)}$ and the signals $x^{(p)}$ are used as inputs.

Control signals $x_c$ are used as inputs for circuits $A$ and $B$.

Circuit $C$ receive as output part of the next state produced by $A$ and $B$.

## Generated outputs
Each circuit will produce for themself the output $Y_x^{(p)}$ which determines the **next state** of their attached register.

Circuit $C$ will produce output $z^{(p)}$.

Circuit $A,B,C$ will generate outputs $\alpha_i \beta_i = c$ passed as inputs to circuits $K_1, K_2, K_3$.




