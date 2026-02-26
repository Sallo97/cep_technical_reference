# Instructions[[11](./0_reference.md)]
CEP's instructions all are long a memory word and follow a 1-address scheme, i.e. they can refer to one memory address used as ope

The sequence of bits of an instruction $(s_0 s_1 s_2 \ldots s_8 s_9 \ldots s_{14} s_{15} \ldots s_{20} s_{21} \ldots s_{35})$ is divided into six groups:

- **the pseudoinstruction flag (1-bits):** $s_0$ determines if the current instruction is a pseudoinstruction or not[[3](./0_reference.md)].
    
- **the automatic check flag (1-bits):** $s_1$ determines if we want execution to automatically check correctness of the instruction [[3](./0_reference.md)].

- **the operation code (7-bits):** $(s_2 \ldots s_{14})$ defines the command to execute.

- **first parametric address (6-bits):** $(s_p = s_9 \ldots s_{14})$ refers to the parametric cell $P$. Recall that the first bit is the group index and the last five bits are the relative address. We designate $p$ as the content of $P$.

- **second parametric address (6-bits):** $(s_q = s_{15} \ldots s_{20})$ refers to the parametric cell $Q$. Recall that the first bit is the group index and the last five bits are the relative address. We designate $1$ as the content of $Q$.

- **memory address (15-bits):** $(s = s_{21} \ldots s_{35})$ is the integer number representing the address of the instruction.

![image](../../resources/cep_instruction.svg)

The total number of normal instruction, i.e. those implemented through micro-programs of the Micro-program Control Unit, is $128$[[4](./0_reference.md)]. From now on we will call normal instructions simply instructions.

## Instruction classes
Before execution, the addres $s$ of the instruction is modified. There are two ways to modifies an address, identifying two classes of instructions:

### Ordinary instructions
In **ordinary instructions** the modification is computed by applying two modifications depending on both parametric cells $P$ and $Q$. The final address of the operation is $c = s + q + p$. Most arithmetic instructions belong to this class.

⚠︎ Note that it is not important the order in which $c$ is evaluated (i.e. if we sum first by $q$ or $p$).

### Special instructions
In **special instructions** the modification only uses the parametric cell $Q$; the final address of the operation is $c = s + q$. The parametric cell $P$ is used in different ways depending on the instruction. For this instructions the CEP works as a 2-address machine.

Special instructions are divided into:

- Operations over parametric cells.

- Jump instructions. 

- Transfer operations between Main Memory and Auxiliary Memory.

- Query instruction over a table.


## Using the final address
The instruction defines which type of memory is referred by the final address $c$:

- **If it refers to Auxiliary Memory** then all 15-bits of $s$ are used to determine the cell.

- **If it refers to Main Memory** then all 12-bits of $s$ are used to determine the cell.

## Timing considerations
Each operation done to compute the final address $c$ requires a memory access, which costs $10 \mu sec$. However if one of the parametric address refers to the first cell of its parametric group, then the CEP assumes its value is always zero, and thus does not sum its content and goes one without any time wasted.