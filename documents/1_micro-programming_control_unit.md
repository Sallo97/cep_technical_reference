# Micro-programming Control Unit (MCU) 
The main control unit of the Calcolatrice Elettronica Pisana is the most important component of the architecture, responsible for the execution of instruction. The processor follows Wilkes' micro-programming principle and thus is also called a **micro-programming control unit**. 

The MCU uses a 1+1 address scheme, meaning that the Order Code defines only instruction having **exactly one explicit operand and one implicit operand**.

![image](../resources/cep_maincontrolunit.svg)

# Components

## Register 0
The register 0 is what can now be referred as the **program counter** : it stores the address of the next **macro-instruction**.

## Micro-instruction matrices
For each micro-instruction, the MCU determines two possible next micro-instructions: the **unconditional address** $\mu_1$, which by default is the one picked; and a **conditional address** $\mu_2$, which is selected only when a given condition is valid.

The production of these micro-instructions is delegated to two matrices: $I$ for $\mu_1$ and $II$ for $\mu_2$.

The choice for the next micro-instruction is decided by the circuit $K_0$.

## Branching Circuit $K_0$
A parallel AND-OR switching circuit whose output decides which micro-instruction to execute in the next cycle between:

- the conditional address $\mu_1$. 
- the unconditional address $\mu_2$.  
- $m$, an address coming form the Main Memory $M$.
- the fixed special address $\epsilon_0$ = 111 $\ldots$ 1, which is the address of the last micro-order.
<!-- ⚠️⚠️⚠️
    I'm not sure it if is the last micro-order of the current micro-program, or a special micro-order which is the last in the Order Code which forces program termination.
-->
 
It receives as input the content of: the matrices $I$ ($\mu_1$) and $II$ ($\mu_2$), the circuits $K_1$ ($k_1$) and $K_2$ ($k_2$), and the control-signal $c_1$. 

## Rules of $K_0$

- if $k_1$ = 0 $\land$ $k_2$ = 0 $\land$ $c_1$ = 0 $\rightarrow$ $\mu_1$ 
- if $k_1$ = 1 $\land$ $k_2$ = 0 $\land$ $c_1$ = 0 $\rightarrow$ $\mu_2$
- if $k_1$ = 0 $\land$ $k_2$ = 0 $\land$ $c_1$ = 1 $\rightarrow$ $m$
- if $k_1 = 0/1$ $\lor$ $k_2$ = 1 $\lor$ $c_1$ = 0/1 $\rightarrow$ the fixed address $\epsilon_0$ = 111 $\ldots$ 1

## Helper Circuits $K_1$, $K_2$ and $K_3$
the three circuits are two input AND gates whose outputs are used to determine the next micro-instruction. $K_1$ and $K_2$ are provide their output directly to $K_0$, which uses them to select a new operation for the next cycle. 

$K_1$ is responsible to determine which address to choose between $\mu_1$ and $\mu_2$.

$K_2$ determines to retrieve the special address $\epsilon_0$.

$K_3$ implements a special behaviour for handling **repeating micro-intructions**. When set the output produced by $K_0$ is discarded and and the program counter keeps the same instruction, indicated as $\mu_0$. 

## Next micro-instruction priority
The current micro-order is determined by the current control-signals being activated by the diode-matrix in the architecture. We say the control signals $m_1$, $m_2$ and $m_3$ refers respectively to those produced by $K_1$, $K_2$ and $K_3$

| Addresses     |   Addresses   |   Addresses | Conditional Control Signals|
| ------------- | ------------- | ------------- | ------------- |
| $\mu_1$                  |  $\epsilon_0$      |              |none |
| $\mu_1 \mu_2$            | $\mu_1 \epsilon_0$ |$\mu_1 \mu_0$ |one  |
| $\mu_1 \mu_2 \epsilon_0$| $\mu_1 \mu_2 \mu_0$ |              |two  |
| $\mu_1 \mu_2 \epsilon_0 \mu_0$|               |              |three| 

The first group refers to micro-order addresses selected by unconditional control signals.

The second, third and fourth group refers to micro-order which deliver one, two and three conditional control signals respectively.

The priority of the next address is:

$\mu_1 \prec \mu_2 \prec \epsilon_0 \prec \mu_0$ 

For example, if the control-signals set to true addresses $\mu_2, \epsilon_0, \mu_0$, among them $\mu_0$ is selected.

# Order Code
The CEP offers $2^8$ distinct micro-order. Usually the number of micro-order composing a high-level instruction is two.