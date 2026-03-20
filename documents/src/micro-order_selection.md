# Micro-Order Selection [[1](../0_Additional_resources/0_reference.md)]
The **Micro-programming Control Unit** (MCU) of the Calcolatrice Elettronica Pisana (CEP) is the most important component of the architecture, following the principles established by Wilke. In this design, high-level machine instructions, also indicated as **macro-instruction**, are concretely implemented in the architecture as a sequence of lower-level **micro-instruction**.

Each micro-instruction serves two purposes:

1. Determine which logical *"gates"* to open and close in the CPU, directing the flow of data through the processor to perform specific tasks.

1. Providing the address of the next micro-instruction to execute, allowing for conditional branching and loops within the control logic.

## Micro-Orders and Micro-Operations

- **Micro-Orders**: the **functional components** of a micro-instruction, indentifying the physical **control signals (i.e. electrical pulses)** emitten by the MCU's diode matrices when executing it.

- **Micro-Operations:** physical events in the hardware that occur when a micro-order is received.

A micro-instruction is a collection of micro-orders that work together to trigger the necessary micro-operations within a single operative cycle.

### Example

Recall that the CEP is a 1-address machine, i.e. the user can specify the address for only **one** operand. 

Consider a simple **ADD** instruction involving an explicit integer constant (provided by the user as an address in memory) and an implicit value in a register (usually the **Accumulator**).

First the machine needs to retrieve the instruction and initialize its micro-program (i.e. the sequence of micro-instructions implementing it).
This is executed by the **Fetch Phase**, which in the micro-programming control unit is a micro-program itself. The phase also copies the integer constant from main memory into the **Memory Buffer Register**.

Retrieved the micro-program, the computer passed to the **Execution Phase**, which consists in executing it. Today doing a simple addition seems trivial, but it still can be broken down into elementary steps:

1. Opening the path to move the constant from the Memory Buffer Register to the ALU input.

1. Signaling the ALU to perform the addition with the value already held in the Accumulator.

1. Branching the micro-sequence back to Fetch Phase micro-program to retrieve the next machine-level command.

## Determining the next micro-instruction
As we have said, a micro-instruction is responsible for both executing an elementary step of a high-level machine-instruction and determing the next micro-instruction to execute.

Below we provide a schematic of the MCU, focussed on this particular problem:

![image](../../resources/cep_maincontrolunit.svg)

## Register $0$
Register $0$ contains the address of the next **micro-instruction** to execute. 

## The Decoder $DC$
Receives as input the **micro-instruction** from register $0$ which transforms as the associated **micro-operation**, i.e. the set of control signals concretely implementing the request.

## The Matrix $MC$
The Matrix $MC$ distributes the control signals representing the micro-operation across the architecture. 

### Micro-instruction matrices
The MCU can be abstracted as a set of smaller matrices, each having a particular role in the determination of the next micro-instruction:

- **Matrix I:** its horizontal lines determine the **unconditional address instruction** $\mu_1$, the default one to pick.

- **Matrix II:** its horizontal lines determine the **conditional address** $\mu_2$, selected only when a given condition is valid.

The vertical lines of both matrices will determine the control signals used to decide the next address among the possible choices.

### Control circuits $K_1$, $K_2$ and $K_3$
Three Parallel AND-OR switching circuits will instrument the control signals responsible in determing which address will be picked for the next micro-instruction.

$K_1$ will decide which address should be picked between $\mu_1$ and $\mu_2$.

$K_2$ decides if the special address $\epsilon_0$ should be picked.

![image](../../resources/k2_circuit.svg)

$K_3$ implements the special behaviour for **repeating the previous micro-intructions**. When set, the output produced by $K_0$ is discarded and and the program counter keeps the previous address, indicated as $\mu_0$. 

$K_1$ and $K_3$ share the same structure:

![image](../../resources/k1_k3_circuit.svg)

**NOTE:** In the diagram of all circuits there could be more than two vertical conditions.

### Branching circuit $K_0$
The next micro-instruction is decided by the parallel AND-OR switching circuit $K_0$ between:

- the **unconditional address** $\mu_1$. 

- the **conditional address** $\mu_2$.  

- the **termination address** $\epsilon_0$ = 111 $\ldots$ 1, which is always the last step of every micro-program. It is the address of the micro-program $E_0$, pointing to the start of the Fetch Phase micro-program.

- the **previous address** $\mu_0$, indicating that the computer must execute the same operation.

- the **starting address** $m$, pointing to the first micro-instruction of the macro-program associated to the current instruction to execute. This address is setted at the end of the Fetch Phase to **move into the Execution Phase**.
 
The decision is determined by the control signals $k_1$, $k_2$, and $c_1$ according to the following rules:

- if $k_1$ = 0  $\land$ $c_1$ = 0 $\rightarrow$ $\mu_1$ 

- if $k_1$ = 1 $\land$ $c_1$ = 0 $\rightarrow$ $\mu_2$

- if $k_1$ = 0 $\land$ $c_1$ = 1 $\rightarrow$ $m$

- if $k_2$ = 1 $\rightarrow$ $\epsilon_0$ 

- if $k_3$ = 1 $\rightarrow$ $\mu_0$

**NOTE:** In some documents the control signals $k_1, k_2, k_3$ are also referred as $m_1, m_2, m_3$.

If multiple addresses are eligible, the following priority hierarchy decides the one selected, form less important, to more important:

$\mu_1 \prec \mu_2 \prec \epsilon_0 \prec \mu_0$ 

For example, if the considitions for $\mu_2, \epsilon_0, \mu_0$, are all valid, among them $\mu_0$ is **always** selected.

A possible diagram of the $K_0$ is the following:

![image](../../resources/k0_circuit.png)
 
# General Case
A general scheme is also considered, where one of four addresses $\mu_1, \mu_2, \mu_3, \mu_4$ may occur at the end of a micro-operation, but contrary to the previous approach they can select any micro-order.

Below we provide a schematic:

![image](../../resources/general_micro-order_selection.png)

## Address selection
At the start of a micro-program, register $0$ will always retrieve the first address from Main Memory.

Circuits $K_1^{'}$ and $K_2^{'}$ are **identical** to $K_1$ and $K_3$ of the previous configuration. Their produced outputs $k_1^{'}$ and $k_2^{'}$ are passed as input to decoder $DF$.

$DF$ decodes these signals to select one of the four possible addresses ($\mu_1, \mu_2, \mu_3, or \mu_4$). The picked address is forwarded to $K_0$.

When the control signal $c_1$ is asserted (i.e. set to $1$), it inhibits the $DF$ decoding process, forcing it to output $0$. $K_0$ interprets this special address as a **request to fetch the new address from Main Memory**.

## Micro-Program Sequence
In a micro-program, the entry-point address **dictates the whole execution path of it**. The sequence needs to be thought out by the computer right from the start, considering all possible branches and conditional jumps from the outset. This is because in register $0$ the first micro-instruction will be overwritten with the address of the next one, so we cannot use it anymore.

### Handling Branching
Consider the case where the architecture is executing micro-order $a$ of sequence $s^{'}$. This operation jumps to another sequence $s^{''}$ of micro-order $b$, which can be the beginning of another micro-program or an operation in-between. 

The computer needs to execute only a portion of $s^{''}$, up until a generic micro-instruction $M_u$. Reached $M_u$ the hardware should be able to exit from $s^{''}$ and automatically jump back to $s^{'}$ or to a whole new sequence $s^{'''}$.

There are two solutions to this problem:

1. During our initial sequence $s^{'}$, one of its operations sets a specific condition in the machine. When $s^{''}$ reaches the generic micro-order $M_u$, the setted flag will instruct the hardware to perform a conditional branch. 

    While the branch occurs at $s^{''}$, the decision was made during $s^{'}$, making it a **fictituous branch** of $s^{''}$.

1. During initialization of $s^{'}$, a portion of of bits from its micro-order code (differing in value from the micro-order code of $s^{''}$), is copied in register $O^{'}$. 

    Register $O^{'}$ will output signals $\alpha_1$ and $\alpha_2$, corresponding to the two least significant bits of the original code. The conditions are by $K_1^{'}$ and $K_2^{'}$ for implementing the fictituous branch of $s^{''}$.

Solution $2$ is the better approach, allowing for a single shared sequence $s^{''}$ to be utilized by multiple different parent sequences, each providing its own *"return coordinates"* via register $O^{'}$.

### Branch Conditions
Conditions $\alpha_1, \alpha_2, \alpha_3, \ldots, \alpha_n$ are send to $K^{'}$ and $K^{''}$ to let the architecture handle branching. More specifically we will refer to them as:

- $\alpha_1^{'}, \alpha_2^{'}, \alpha_3^{'}, \ldots, \alpha_n^{'}$ when passed to $K_1$.

- $\alpha_1^{''}, \alpha_2^{''}, \alpha_3^{''}, \ldots, \alpha_n^{''}$ when passed to $K_2$.

The condition are distributed in two groups:

- $\forall j \in \{1,2\}.a_j$ is a direct output of $O^{'}$ corresponding to the first or second least significant bit in the macro-order code of the parent branch.

- $\forall i \in \{3,4,\ldots, n\}.a_i$ is a signal coming directly or indirectly from register or flip-flops in the architecture, whose value depend of the content of the component generating them.

#### General Address selection logic
Let $m_1^{'}, m_2^{'}, m_3^{'}, \ldots, m_n^{'}$ and $m_1^{''}, m_2^{''}, m_3^{''}, \ldots, m_n^{''}$ designate output signals from the matrix rows, intersected by a generic micro-order applied to the second input of the AND-gates in $K_1$ and $K_2$ respectively.

Let $\alpha_h, \alpha_k, \alpha_l$ stand for three generic distinct conditions.

When $m_h^{'} = m_k^{''} = m_l^{'} = m_l^{''} = 1$, addresses $\mu_1, \mu_2, \mu_3, \mu_4$ are selected according to the table below:

| Conditions                  | $k_1^{'}$ | $k_2^{'}$ | Address |
|-----------------------------|-----------|-----------|---------|
| $a_h = a_k = a_l = 0$       | 0         | 0         | $\mu_1$  | 
| $a_h = 1 ,\ a_k = a_l = 0$  | 1         | 0         | $\mu_2$  |
| $a_k = 1, \ a_h = a_l = 0$  | 0         | 1         | $\mu_3$  |
| $a_h = a_k = 1, \  a_l = 0$ | 1         | 1         | $\mu_4$  |
| $a_l = 1, \ a_h = a_k = 0$  | 1         | 1         | $\mu_4$  |

#### Priority Hierarchy
If more conditions are set s.t. more than one address could be chosen in the same cyle, one is picked according to the following priority rules:

$\mu_1 \prec \mu_2 \lor \mu_3 \prec \mu_4 (\alpha_l = 0) \prec \mu_4(\alpha_l = 1)$

### Branching due to $a_i \land a_j$
The previous considerations considered branching due to $a_i$.

Consider the case when, at the end of a generic micro-order $M_u$ in sequence $s^{''}(a_1^{''}, a_2^{''})$, we want to originate a fictituous branch belonging to $s^{'}(a_1^{'}, a_2^{'})$.

Assuming $m_h^{'} = m_k^{'} = m_l ^{'} = m_l^{''} = 1$ with $m_h = m_k = a_i \land m_l = a_j$:

- if $\alpha_1^{''} = \alpha_2^{''} = 1 \rightarrow $ no fictituous branch to avoid ambiguity.

- if $a_1^{''} = a_2^{''} = 0 \rightarrow $ fictituous branch only with $\mu_4$ having $\alpha_l = a_1^{'} \lor \alpha_2^{'} = 1$

- if $s^{''}$ with $(\alpha_1^{''}, \alpha_2^{''}) \equiv s^{''}(00) \rightarrow$ there could be two possible fictituous branches:

    - $(\alpha_1^{'}, \alpha_2^{'}) = (10) \land (\alpha_1^{'}, \alpha_2^{'}) = (11)$

    - $(\alpha_1^{'}, \alpha_2^{'}) = (01) \land (\alpha_1^{'}, \alpha_2^{'}) = (11)$

- if $s^{''} (\alpha_1^{''}, \alpha_2^{''}) = s^{''}(01) \lor s^{''}(\alpha_1^{''}, \alpha_2^{''}) = (10)$ there could be two fictituous branches:

    - $(\alpha_1^{'}, \alpha_2^{'}) = (10) \land (\alpha_1^{'}, \alpha_2^{'}) = (11)$
    
    - $(\alpha_1^{'}, \alpha_2^{'}) = (01) \land (\alpha_1^{'}, \alpha_2^{'}) = (11)$

### Branching due to $a_j$
Assuming:

- $m_1^{'} = m_2^{''} = 1$

- $\alpha_h = \alpha_1$

- $\alpha_k = \alpha_2$

- $\alpha_l = 0$

At the end of a micro-order, up to three fictituous branches are possible, with addresses picked among $\{\mu_1, \mu_2, \mu_3, \mu_4\}$.

Be aware that the addresses chosed by $\alpha_1^{''} \land \alpha_2^{''}$ of $s^{''}(alpha_1^{''}, \alpha_2^{''})$ belonging to $M_u$ is excluded.

