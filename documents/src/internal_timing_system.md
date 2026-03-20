# Internal Timing System
The internal timing system is responsible for managing the consistency for operations within the main kernel of the CEP (i.e. not involving external devices). Microinstructions whose operations are limited between the registers and main memory are called **internal microinstruction**. [[10](../helper_resources/reference.md)]

Nine time cycles are possible:

### Group $I$
- $I-I$ ![image](I-I)
- $I-II$ ![image](I-II)
- $I-III$ ![image](I-III)
- $I-IV$ ![image](I-IV)

### Group $II$
- $II-I$ ![image](II-I)
- $II-II$ ![image](II-II)
- $II-III$ ![image](II-III)

### Group $III$
These cycles identify cyclic micro-instructions.
- $III-I$ ![image](III-I)
- $III-II$ ![image](III-II)

⚠︎ Be aware that $T_1$ is only considered for the first cycle of the cyclic micro-instruction.

After each delay $T$, the circuit is composed of tension levels for dispatching pulses to registers in the computers. All tension levels governing gates come from the Control Unit and are denoted as $\psi$. They change at the next micro-instruction. [[10](../helper_resources/reference.md)]

## Time considerations for micro-instruction execution
Each microinstruction begins execution when the pulse $\epsilon_1$ is dispatched. The pulse can be set either by pressing starting button of the CEP (logically defined as the pulse $\epsilon_4$), or by the end of a cycle.

When activated, $\epsilon_1$ moves through the gates opened by the delay $T_1$ and travels inside it with a delay $R_C$. Then the Control Unit sends the commands $\psi_i$ which combined determine the micro-operation, its temporal cycle and the registers used by it. 

After the exit and entry levels for the registers are stabilizied, it is possible to end the micro-instruction by sending the stable pulse command $\epsilon$ to the registers we would like to update with the data coming from their input. [[10](../helper_resources/reference.md)]

At the end of a micro-instruction two cases can occur:

- **Registers used by the current micro-instruction in $X^{'}$, $Y^{'}$ and $M$ have been written by the precedent micro-instruction.** Then we need to consider only the transmission time of commands $\psi$ and the elaboration and stabilization of data at the exit of networks $X$ and $Y$. All employed registers can be written at the end of the micro-instruction, after all information has been stabilized.

- **Main Memory $M$ is read and one of its data will be used by the micro-instruction.** Then there are two temporal phases:

    1. The first phase begins with the instant in which the Control Unit is being impulsed. It ends with the stabilization of the levels of the requested memory address and the dispatch of the reading pulse of $M$.

    1. Starts in the instant in which the exit register of $M$ has been written with the data coming from Main Memory. It ends with the stabilization of $X^{'}$ and $Y^{'}$ levels and the dispatch of the writing pulses to the interested registers.

For a correct execution, considering gates $A_1$ through $A_6$ we need to set:

- $R_{C_{min}} \geq $ maximum time required for a pulse cycle.

- $T_1 \geq (R_{C_{max}} + stabilization_{max})$ where $stabilization_{max}$ is the maximum time required to stabilize the $\psi_i$ commands which govern gates $A_7, \ldots, A_{11}$.

## Internal micro-instructions cycles
The time required for each internal micro-instruction is defined by the delay circuits in its associated temporal cycle. Internal micro-instructions are distinguished according to their usage or not of the main memory. [[10](../helper_resources/reference.md)]

### Micro-instruction not using the main memory
Here belongs cycles $II-I, II-II, II-III, III-I,$ and $III-II$, which do no have exits for dispatching both the reading pulse $\epsilon_2$ and writing pulse $\epsilon_3$ of the Main Memory. These operations only write into the registers through pulses $\epsilon_x$ and $\epsilon_y$. At the same time the starting pulse $\epsilon_1$ is send to the Control Unit. Logically the Main Memory $M$ is connected by $I_M$ to the Computer Unit e with $2_M$ to the Address Unit. [[10](../helper_resources/reference.md)]

We distinguish this set further in:

#### Micro-instruction for calling (i.e. starting) a CEP instruction
While the operation code goes to the Control Unit:

- The memory address specified by the instruction is dispatched in register $C$ (of group $X^{'}$) through connection $2x^{'} - Y - 3y$. The content $m$ of the specified memory cell is used by the Compute Unit.

    There are four cases:

    1. $m$ is processed in $X$ and its value copied in one of the arithmetic registers ($A$ or $B$) through connection $I_M - X - 2x$.

    1. $m$ is summed in $X_D$ with the content of one of the arithmetic registers ($A$ or $B$). The result is then written in one of the arithmetic registers (not necessary the same one used previously).

    1. $m$ is processed in $X$ and written in the same memory cell through $I_X$.

        The maximum delay + maximum stabilization time of connection  $I_X - X - I_X$ is minor than the difference between the start of the writing pulse in main memory and the writing pulse for the register storing the read content from memory.

    1. $m$ is summed in $X_D$ with the content of one of the arithmetic registers ($A$ or $B$). The result is written back in the same memory cell through connection $I_X$.

        Its necessary to add to $T_2$ a delay $T_2^{'}$ for delaying the start of the writing process until the levels in $I_X$ are stabilized.

    The previous four cases can be further divided in $8$, depending on wheter for the addressing of the memory it is used the parallel adder of $Y$ or not.

- The parametric addresses are dispatched to register $R$ (of group $Y^{'}$).

During all of this the memory address is present in register $N$ (of group $Y^{'}$) through the connection $I_M - X -I_x$. [[10](../helper_resources/reference.md)]

#### Micro-instructions for modifying an instruction or for starting a special instruction. 

The memory is read at the addresse determined by summing $H^{1}$ or $H^{0}$ with $R$ through the connection $I_X^{'} - Y_0 - 3y$.

The content of the selected cell in main memory is summed in $X_D$ (i.e. the logical network X employing its parallel adder) with the content of register $C$ (of group $X^{'}$). The result is written in $C$.[[10](../helper_resources/reference.md)]


#### Cyclic cycles
Within this set, cycles $III-I$ and $III-II$ are sssociated to cyclic micro-instructions. When the micro-instruction reaches its end, the starting pulse $\epsilon_1$ is not sent to the Control Unit by closing the gate $A_1$. This does not allow the computer to move to the next micro-instruction, repeating the same one. 

Ending the repetition is decided by a condition coming from registers either in $X^{'}$ or $Y^{'}$, which will open gate $A_1$, letting the $\epsilon_1$ reach the Control Unit.


### Micro-instruction using the Main Memory
Cycles $I-I, I-II, I-III,$ and $I-IV$ have exits for reading pulse $\epsilon_2$ and writing pulse $\epsilon_3$ of the Main Memory. 

All of the cycles in the set have three fundamental delays: 

- $T_1$ or $T_1 + T_1^{'}$: begins with the dispatch of the starting pulse $\epsilon_1$ to the Control Unit. It corresponds to the time necessary for stabilizing the memory address (coming from registers $N$, $C$, $R$, $H^{0}, and $H^{1}$) before sending the reading pulse.

- $T_2$ or $T_2 + T_2^{'}$: starts at dispatch of the reading pulse to Main Memory and the ends at the beginning of the inhibition pulse of the Main Memory.

- $T_3$: is the time necessary to maintain constant the levels of the memory addres and the levels for writing of the Main Memory, while the inhibilition pulse and writing pulse of the Main Memory are dispatched. 

    These levels are at the wires $3x$ and $Ix$.