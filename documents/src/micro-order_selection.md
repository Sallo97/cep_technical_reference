# Micro-Order Selection
The **Microprogramming Control Unit** (MCU) of the Calcolatrice Elettronica Pisana (CEP) is the most important component of the architecture. Its design follows the principles established by Wilkes, where machine instructions are executed as a sequence of lower-level **microinstructions**. [[1](../helper_resources/reference.md)]

Each microinstruction serves two purposes:

1. Determine which logical *"gates"* to open and close in the CPU, directing the flow of data through the computer to perform specific tasks.

1. Providing the address of the next micro-instruction to execute, allowing for conditional branching and loops within the control logic.

## Micro-Orders and Micro-Operations

**Micro-Orders** are the **functional components** of a micro-instruction, they determine the control signals (i.e. the physical electrical pulses) emitted by the MCU's diode matrices during execution. [[1](../helper_resources/reference.md)]

**Micro-Operations:** are the physical events in the hardware that occur when a micro-order is received.

A microinstruction is a collection of micro-orders that work together to trigger the necessary micro-operations within a single operative cycle. [[1](../helper_resources/reference.md)]

### Example

Recall that the CEP is a 1-address machine, i.e. the user can specify the address for only **one** operand.  Consider a simple **ADD** instruction involving an explicit integer constant and an implicit value in a register. First, the machine needs to retrieve the instruction and initialize its microprogram. This is the **Fetch Phase**, which is implemented in the MCU as a micro-program itself. The phase also copies the integer constant from main memory into the **Memory Buffer Register** $Z$.

The computer then passes to the **Execution Phase**, which executes the command. A simple addition seems trivial, but it can be broken down into elementary steps:

1. Opening the path to move the constant from the Memory Buffer Register $Z$ to the ALU input.

1. Signaling the ALU to perform the addition with the value already held in the Accumulator.

1. Branching the microsequence back to fetch phase microprogram to retrieve the next machine-level command.

## Determining the next micro-instruction
As we have said, a micro-instruction is responsible for both executing an elementary step of a high-level machine-instruction and determing the next micro-instruction to execute.

Below we provide a schematic of the MCU:

![image](../../resources/cep_maincontrolunit.svg)

## Register $0$
Register $O$ contains the address of the next **micro-instruction** to execute. [[1](../helper_resources/reference.md)] 

## The Decoder $DC$
Receives as input the **micro-instruction** in register $O$, which decodes into the associated **micro-operation**. [[1](../helper_resources/reference.md)] 

## The Matrix $MC$
The Matrix $MC$ distributes the generated control signals across the architecture. [[1](../helper_resources/reference.md)]  

### Micro-instruction matrices
The main matrix can be abstracted as a set of smaller matrices, each having a particular role:

- **Matrix I:** its horizontal lines determine the **unconditional address instruction** $\mu_1$, the default one to pick.

- **Matrix II:** its horizontal lines determine the **conditional address** $\mu_2$, selected only when a given condition is valid.

The vertical lines of both matrices will determine the control signals used to decide the next address among the possible choices. [[1](../helper_resources/reference.md)] 

### Control circuits $K_1$, $K_2$ and $K_3$
$K_1, K_2, K_3$ are three switching circuits responsible for instrumenting the control signals determing the next microinstruction to execute. [[1](../helper_resources/reference.md)] 

- $K_1$ decides which address should be picked between the conditional or unconditional address $\mu_1$ and $\mu_2$.

- $K_2$ determines if the special address $\epsilon_0$ should be chosen.

- $K_3$ implements the special case in which we need to repeat the previous micro-intruction. When set, it discards the output produced by $K_0$ and the microinstruction register $O$ keeps the previous address, named $\mu_0$.

    Construction-wise, $K_1$ and $K_3$ share the same logical structure.

![image](../../resources/k1_k3_circuit.svg)

![image](../../resources/k2_circuit.svg)

### Branching circuit $K_0$
The output produced by $K_1$ and $K_2$ are fed into switching circuit $K_0$. The possible candidates for the next microinstruction are:

- the **unconditional address** $\mu_1$. 

- the **conditional address** $\mu_2$.  

- the **termination address** $\epsilon_0$ = 111 $\ldots$ 1. It is the address of the microprogram $E_0$, pointing to the start of the fetch phase microprogram and for this reason is always the last step of every micro-program.

- the **previous address** $\mu_0$, indicating that the computer must repeat the previous micro-operation.

- the **starting address** $m$, pointing to the first microinstruction of the current instruction to execute. This address is setted at the end of the fetch phase.
 
The logic rules for choosing the next microinstruction depends on control signals $k_1$, $k_2$, and $c_1$ and are:

| $k_1$ | $k_2$ | $k_3$ | $c_1$ | microinstruction picked |
| ----- | ----- | ----- | ----- | ----------------------- |
| $0$   | /     | /     | $0$   | $\mu_1$                 |
| $1$   | /     | /     | $0$   | $\mu_2$                 |
| $0$   | /     | /     | $1$   | $m$                     |
| /     | $1$   | /     | /     | $\epsilon_0$            |
| /     | /     | $1$   | /     | $\mu_0$                 |

Where "/" indicates that the value of that circuit can be whatever.

**NOTE:** In some documents the control signals $k_1, k_2, k_3$ are also referred as $m_1, m_2, m_3$.

When multiple addresses are eligible, the following priority hierarchy is used to decide which signal to select (it is ordered form less important, to more important):

$\mu_1 \prec \mu_2 \prec \epsilon_0 \prec \mu_0$ 

For example, if the conditions for $\mu_2, \epsilon_0, \mu_0$, are all valid, among them $\mu_0$ is **always** selected. [[1](../helper_resources/reference.md)]

A possible diagram of the $K_0$ is the following:

![image](../../resources/k0_circuit.png)
 
# General Case
In paper [[1](../helper_resources/reference.md)] a generalization of the microinstruction selection approach has been proposed. For completion sake we illustrate it. Here, four addresses are considered $\mu_1, \mu_2, \mu_3, \mu_4$. Each address may occur at the end of a micro-operation, but contrary to the previous approach they can select any micro-order.

Below a schematic of this scenario.

![image](../../resources/general_micro-order_selection.png)

## Address selection
At the start of a micro-program, register $O$ will always retrieve the first address from Main Memory.

Circuits $K_1^{'}$ and $K_2^{'}$ are **identical** to $K_1$ and $K_3$ of the previous configuration. Their produced outputs $k_1^{'}$ and $k_2^{'}$ are passed as input to decoder $DF$. $DF$ decodes these signals to select one of the four possible addresses ($\mu_1, \mu_2, \mu_3, or \mu_4$). The picked address is forwarded to $K_0$.

When the control signal $c_1$ is asserted (i.e. set to $1$), it inhibits the $DF$ decoding process, forcing it to output $0$. $K_0$ interprets this special address as a request to fetch the new address from Main Memory.[[1](../helper_resources/reference.md)]

## Micro-Program Sequence
In a microprogram, the entry point address dictates the whole execution path. The sequence needs to be thought out by the computer right from the start, considering all possible branches and conditional jumps from the outset. This is because in register $O$ the first microinstruction will be overwritten with the address of the next one, so we cannot use it anymore.

### Handling Branching
Consider the case where the architecture is executing micro-order $a$ of sequence $s^{'}$. This operation jumps to another sequence $s^{''}$ of micro-order $b$, which can be the beginning of another microprogram or an operation in-between. 

The computer needs to execute only a portion of $s^{''}$, up until a generic micro-instruction $M_u$. Reached $M_u$ the hardware should be able to exit from $s^{''}$ and automatically jump back to $s^{'}$ or to a whole new sequence $s^{'''}$.

There are two solutions to this problem:

1. During our initial sequence $s^{'}$, one of its operations sets a specific condition in the machine. When $s^{''}$ reaches the generic micro-order $M_u$, the setted flag will instruct the hardware to perform a conditional branch. 

    While the branch occurs at $s^{''}$, the decision was made during $s^{'}$, making it a **fictituous branch** of $s^{''}$.

1. During initialization of $s^{'}$, a portion of of bits from its micro-order code (differing in value from the micro-order code of $s^{''}$), is copied in register $O^{'}$. 

    Register $O^{'}$ will output signals $\alpha_1$ and $\alpha_2$, corresponding to the two least significant bits of the original code. The conditions are by $K_1^{'}$ and $K_2^{'}$ for implementing the fictituous branch of $s^{''}$.

Solution $2$ is the better approach, allowing for a single shared sequence $s^{''}$ to be utilized by multiple different parent sequences, each providing its own *"return coordinates"* via register $O^{'}$. [[1](../helper_resources/reference.md)]

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

Be aware that the addresses chosed by $\alpha_1^{''} \land \alpha_2^{''}$ of $s^{''}(alpha_1^{''}, \alpha_2^{''})$ belonging to $M_u$ is excluded. [[1](../helper_resources/reference.md)]

