# Unità di Calcolo

### Handling the sum operation [[2](./0_reference.md)]
The sum operation uses as operand the memory address specified explicitly by the instruction and that already present in **an** arithmetic register (*the document does not specify if the register is always the same, just that it is an arithmetic one*). 

For handling this operation are required both a memory *read*, for loading the content of the requested address, and a memory *write*, for keeping the content in memory. The memory write is done in parallel to the actual addition in the Unità di Calcolo. The addition starts immediately after the requested content is read. 

#### Timing of the sum operation
The time required for executing an addition requires two cycles and is divided in the following time slice:

- $\Delta T_1$ is the time required for reading the content in the specified memory address. It depends on the maximum time for guaranteeing that the memory is stabilized (i.e. its content is valid for reading) + the time required for **reaching** that memory address. Be aware that the time ends right after we have reached the address, **not** read it.

- $\Delta T_2$ is the time required for propagating the signals from the specific memory address to the *Unità di Controllo*. Be aware that it is not the time for reading the content, rather the time required for moving it physically from memory.

    This time slice ends right away an auxiliary $\Delta T_{2}^{'}$ starts. 
$\Delta T_{2}^{'}$ identifies the time required for the memory to be stabilized for the next cycle.

$\Delta T_1, \Delta T_2,$ are all within a single cycle. $\Delta T_{2}^{'}$  is the time required for starting the next cycle, thus it is not considered within any cycle.

- $\Delta T_3$ is the time required for reading the memory. It starts right after the next cycle and ends when the content is stored in the register delegated to storing the current memory content.

- $\Delta T_4$ is the maximum time required by the *Unità di Calcolo* for applying the addition, **while at the same time** the memory is being rewritten. 

    The time is dominated by the memory write, so it is required that the time for the addition does not surpass the time for the memory. This requirement is more of a formality, realistically there is never a case where an operation within the Unità di Controllo requires more time than one in memory.  

![image](../resources/cep_sum_time.png)