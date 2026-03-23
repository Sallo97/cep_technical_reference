# Micro-Order
The CEP offers $2^8$ distinct microinstruction, meaning their operative code occupies one byte (i.e. 8-bits). Usually, a machine-instruction is implemented as two microinstructions. [[1](../helper_resources/references.md)]

## Microinstructions using the main memory

### Microinstructions for normal memory accesses
For every instruction, their first microinstruction consists in moving their operative code in the Control Unit. The last 15-bits of the instruction, representing the base operand address, are moved in register $C$ of the Arithmetic Unit ($X^{'}$), while the implicit parametric addresses are stored in $R$ of $Y^{'}$. The final memory address is then stored in $N$ of ($Y^{'}$). The memory is accessed using the content in $N$, which is then returned to $X$ by the link $9x$. [[2](../helper_resources/references.md)]

<!-- Here there is a contraddiction! From other documents, register `Z` is used to access memory content, while `N` is used as the program counter -->

### Microinstructions for special instructions or jumps
Recall that a special instruction interprets the CEP as a two address machine, one being the base operand address and the second being the address of the first parametric cell $P$. Micro-instruction which needs to modify the current memory access using the implicit parametric cells, then is obtained as the sum of $H_0$, $H_1$ with $R$ of $Y^{'}$. The final address is obtained as the sum between the complete parametric address with the address in $C$ and is stored in $C$. To do this operation the logic network $X_D$ is used. [[2](../helper_resources/references.md)]

### Microinstruction which are executing the requested action
Microinstructions not responsible for the prologue or epilogue of a microprogram usually execute an operation over the content of register $C$, which stores the address of the current operand. We refer with $m$ as the content of the entry in Main Memory pointed by $C$. This value will be retrieved and stored in register $Z$ (physically stored in the Arithmetic Unit) for applying the operation. [[2](../helper_resources/references.md)]

#### Possible execution cases for $m$

1. $m$ is computed in $X$ and stored in register $A$ and/or $B$ of the Arithmetic Unit.

1. $m$ is summed through network $X_D$ with the content of $A$ and/or $B$ and the result is stored in $A$ and/or $B$.

1. $m$ is computed in $X$ and stored in memory at location $m$.

1.  $m$ is summed through network $X_D$ with the content of $A$ and/or $B$ and the result is stored in memory at the same address $m$.

### Microinstruction classes
Microinstructions are grouped into four classes, each determining how their member use or not the Main Memory. Each class is associated to a temporal cycle. [[2](../helper_resources/references.md)]

#### Cycle I-1
Identifies microinstructions in microprograms where it is always read a value $m$ from memory, which is used by the Arithmetic Unit. Two subgroups are present:

- Those applying a computation with or without $X$'s parallel adder, and the result is written into $X^{'}$.
- Those applying a computation **without using** $X$'s parallel adder and the result is written back to the same Main Memory's entry.

Note that these microinstructions never use $Y$'s parallel adder.

![image](../resources/cep_microinstructions_cycle-I1.png)

#### Cycle I-2
Identifies microinstructions in microprograms where a value $m$ is read from Main Memory for use by the Arithmetic Unit. The main computation is done through $X$'s parallel adder, and the result is always written back to Main Memory at the same address as the operand.

Note that these microinstructions never use $Y$'s parallel adder.

![image](../resources/cep_microinstructions_cycle-I2.png)

#### Cycle I-3
Identifies microinstructions behaving similarly to those in I-1 group, with the exception that they use $Y$'s parallel adder.

![image](../resources/cep_microinstructions_cycle-I3.png)

#### Cycle I-4

Identifies microinstructions behaving similarly to those in I-2 group, with the exception that they use $Y$'s parallel adder.

![image](../resources/cep_microinstructions_cycle-I4.png)

## Microinstructions using only the Arithmetic Unit and Address Unit

Identifies microinstructions that extend only withing the Arithmetic Unit and the Address Unit. They could produce conditions either during or at the end of their execution. They could use either $X$ and $Y$'s parallel adder for computing. Within the  group we identify normal microinstructions and cyclic microinstructions. Each type of microinstruction has its own set of subclasses.

1. **Normal microinstructions:** when called they execute only one time.

2. **Cyclic microinstructions:** when called they are executed a certain number of time in a row.

### Normal microinstructions
Identifies microinstructions that when called are executed only one time.

#### Cycle II-1
Microinstructions using the Arithmetic Unit and/or the Address Unit, which do not need $X$'s or $Y$'s parallel adders for their computation. They **do not** produce any control signal whatsoever.

#### Cycle II-2
Microinstructions which either use only the Arithmetic Unit, or use both the Arithmetic Unit and the Address Unit simultaneously. They do not need $Y$'s parallel adder, and could produce conditions "*during*" or at "*the end*" of their execution.

#### Cycle II-3
Microinstructions which either use only the Address Unit, or use both the Arithmetic Unit and tha Address Unit simultaneously. They need $Y$'s parallel adder, and could produce conditions "*during*" or at "*the end*" of their execution.

### Cyclic microinstructions
Identifies microinstruction that when called are executed more than one time.

#### Cycle 3-1
Microinstruction which use only the Arithmetic Unit and compute data withouth needing $X$'s parallel adder.

#### Cycle 3-2
Microinstruction which use only the Arithmetic Unit and compute data with $X$'s parallel adder.