# Micro-programming Control Unit (MCU) 
The main control unit of the Calcolatrice Elettronica Pisana is the most important component of the architecture, responsible for the execution of **micro-instructions**. Following the principles enstablished by Wilkes, The CEP utilizes a **Micro-programming Control Unit** (MCU), in which instructions are implemented as a sequences of lower-level micro-instructions. 

Each micro-instruction serves two purposes:

1. Determine which *"gates"* to open and close in the CPU, instrumenting data through the processor to perform a specific micro-operation.

1. Providing the address of the next micro-instruction to execute.

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

- the **termination address** $\epsilon_0$ = 111 $\ldots$ 1, which is always the last step of every micro-program. This fix location points to the start of the Fetch Phase micro-program.

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

