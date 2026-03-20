# Handling the sum operation 
The sum operation uses as operands the memory address specified explicitly by the instruction and an arithmetic register. For handling this operation are required both a read from memory (for loading the content of the requested address), a write to memory (for keeping the content in memory) and the actual addition. The write and addition are done in parallel. The two processes start immediately after the requested content is read. [[2](../helper_resources/reference.md)]

## Timing of the sum operation
The time required for executing an addition takes two cycles and is divided in the following time slices:

- $\Delta T_1$ is the time required for reading the content in the specified memory address. It depends on the maximum time for guaranteeing that the memory is stabilized (i.e. its content is valid for reading), plus the time required for *physically reaching* the entry. Be aware that the time ends right after we arrive at the memory cell.

- $\Delta T_2$ is the time required for propagating the content of the cell to the CEP's kernel. Be aware that it is not the time taken for reading the content, rather the time required for moving it physically from memory.

    This time slice ends right away an auxiliary $\Delta T_{2}^{'}$ starts. 
$\Delta T_{2}^{'}$ identifies the time required for the memory to be stabilized for the next cycle.

$\Delta T_1, \Delta T_2,$ are all within a single cycle. $\Delta T_{2}^{'}$  is the time required for starting the next cycle, thus it is not considered within any cycle.

- $\Delta T_3$ is the time required for reading the memory. It starts right after the next cycle and ends when the content is stored in register $Z$.

- $\Delta T_4$ is the maximum time required by the arithmetic unit for applying the addition, concurrently the memory is being rewritten. 

    The time is dominated by the memory write, so it is required that the time for the addition does not surpass the time for the memory. This requirement is more of a formality, realistically there is never a case where an operation within the arithmetic unit requires more time than one in memory. [[2](../helper_resources/reference.md)]

![image](../../resources/cep_sum_time.png)