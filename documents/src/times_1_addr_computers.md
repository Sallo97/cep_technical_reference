# Time considerations for 1-address machines 
The time required for retrieving data from main memory is far greater than the time to execute an instruction using only registers. This can become a problem for 1-address machines like the CEP, where each instruction has an explicit memory address (i.e. an access to main memory). [[10](../helper_resources/reference.md)]

## Execution of an instruction
Let's consider the sum operation. This command is implemented by reading the explicit operand from memory, and then add the content to an implicit arithmetic register. The execution needs to consider:

1. The cycle time for reading the data and rewriting it in the memory cell.

1. The cycle time for adding the read data to the implicit arithmetic register. 

To optimize the total amount of time taken, an overlap can occur while the data in being rewritten into its memory cell and the arithmetic unit is adding the content to one of its registers. [[10](../helper_resources/reference.md)]

### Cycle time considerations for the Main Memory
The cycle time of the main memory is set as $\Delta T_1 + \Delta T_2$, where:

- $\Delta T_1$ is the **selection time**, i.e. maximum stabilization time plus the maximum delay of the selection levels of the addresses of Main Memory. $\Delta T_1$ starts the same instant where the Control Units begins its cycle.

- $\Delta T_2$ is the time starting when the machine begins  sending of the reading current and ending at the instant that preceeds $\Delta T_2^{'}$.
    
    $\Delta T_2^{'}$ is the minimum delay for computing and propagating the selection levels of the Main Memory, starting at the same moment in them where the Control Units begins the **next cycle**. Is the minimum time where the data from the previous cycle is still precent in the next one.

Below a diagram of the temporal cycles of the Main Memory:

![image](ADD "Cicli temporali della memoria")

$\Delta T_1$ and $\Delta T_2$ are the same defined previously, with additionally:

- $\Delta T_3$ is the **reading time**, .e. the time (starting at the begin of the cycle) necessary for copying the content of the requested Main Memory's cell into the memory register.

- $\Delta T_4$ is the maximum time employed by the Compute Unit. This amount is **never** greater than the cycle time of the Main Memory


### Cycle time considerations for the Compute Unit
The cycle time of the Compute Unit (previously defined as $\Delta T_4$) **deeply** depends on the propagation time  of the carry overs in the parallel adder.

### Optimal Cycle time for an instruction
Since all instructions consider a memory access, the best time can be reached by setting $\Delta T_4$ (the cycle time of the Compute Unit) equal to $\Delta T_1 + \Delta T_2$ (the cycle time of the Main Memory.)

## Parametric addresses
Parametric addresses are relative addresses that. To retrieve the full address of the parametric cell, the CEP sums them with the base full address of their parametric group (for local parametric cell it is contained in register $H^{0}$; for global parametric cells in register $H^{1}$).

## Writing data in registers
For correctly writing data in registers, it is necessary that the information at their entrance stays stabilized over all the time $R$ of the writing pulse.

We identify two times $R$:

- $R_1:$ the time taken for writing in a normal register.

- $R_2:$ the time taken from the start of the reading pulse and the end of the inhibition pulse in the Memory.

# CEP Main Kernel

The CEP main kernel is composed of three primary components: the Compute Unit (from the italian *"Unità di Calcolo"*), Main Memory, and Address Unit (from the italian *"Unità degli indirizzi"*).

Below a diagram of the Main Kernel:

<!-- ![image]("Schema a blocchi del nucleo centrale della CEP")-->

- $X$ and $Y$ are logical networks.

    Logical network $Y$ encompass the decoder of memory addresses.

    The elaborations of the two logical networks can be done with or without two parallel adders, one for each network. Depending on the usage of a parallel adder, the elaboration time differs considerably. 
    
    For this reason when we refer to $X_D$ and $Y_D$ we assume to use the adders, while with $X$ and $Y$ we assumet to not use them.

- $X^{'}$ and $Y^{'}$ are registers. 

    In $X^{'}$ are included the arithmetic registers $A$ and $B$ and register addressing register $C$ . $C$ contains the memory address of the current instruction.

    In $Y^{'}$ are present the register $N$, which addresses the memory instruction, register $R$, which provides the relative addresses, and the registers containing the base addresses of the parametric cells groups $H^{0}$ and $H^{1}$.

- $M$ is an abstraction of the main memory as a single register.

- $\psi_{i}$ are selection commands (define the static control).

- $\epsilon_i$ are writing pulses of registers (define the dynamic control).



