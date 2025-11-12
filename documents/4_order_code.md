# Order Code
The CEP uses for all of its operations a 1+1 address scheme, meaning that the Order Code defines only instruction having:
- exactly one explicit operand.
- one implicit operand.

Additionally for each instruction there two additional implicit 5-bit addresses, each stored in a distinct reserved region of main memory.,

Finally these is an implicit return location.

### Micro-Order
The CEP offers $2^8$ distinct micro-order, meaning each associated micro-instruction will be represented by a byte. Usually the number of micro-order composing a high-level instruction is two.

## Instruction Classes [[2](./0_reference.md)]
Instructions in the CEP can be grouped in two main classes: *istruzioni ordinarie* and *istruzioni speciali*. Each class interprets the two implicit 5-bit addresses differently.

### Istruzioni Ordinarie
The content of the implicit 5-bit addresses is used for modifying the current instruction. 

### Istruzioni Speciali
Here one of the two addresses is used by the instruction for its execution. Thus the CEP for this kinds of instructions behaves like a two-address machine, were only one address is parametric.

The two addresses used by the instruction are relative. To make them absolute they are they are summed with the origin addresses in two distinct registers in the *Unità degli Indirizzi* where all memory addresses are stored.

## Microinstructions using the main memory [[2](./0_reference.md)]

### Micro-instructions for normal memory accesses
The first micro-instruction of an instruction moves the opcode in the control unit; the explicit relative memory address is moved in $C$ of $X^{'}$, while the implicit parametric addresses are stored in $R$ of $Y^{'}$. The final memory address is then stored in $N$ of $Y^{'}$. The memory is accessed using the content in $N$, which is then returned to $X$ by the link $9x$.

### Micro-instructions for special instructions or jumps
Micro-instruction which needs to modify the current memory access using the implicit parametric cells, then is obtained as the sum of $H_0$, $H_1$ with $R$ of $Y^{'}$.

<!-- Maybe H_0 and H_1 have length smaller than R_1, s.t. they can be summed to separate contiguous groups of bits of R. -->

The final address is obtained as the sum between the complete parametric address with the address in $C$ and is stored in $C$. To do this operation the logic network $X_D$ is used.

### Micro-instruction which are executing the requested action
Micro-instruction delegated to the execution of a step of an instruction have at register $C$ a concrete memory address and the content $m$ pointing to associated memory cell is moved to the *Unità di Calcolo*.

#### Possible execution cases for $m$

1. $m$ is computed in $X$ and stored in register $A$ and/or $B$ of $X$ (Unità di Calcolo).

1. $m$ is summed through network $X_D$ with the content of $A$ and/or $B$ and the result is stored in $A$ and/or $B$.

1. $m$ is computed in $X$ and stored in memory at location $m$.

1.  $m$ is summed through network $X_D$ with the content of $A$ and/or $B$ and the result is stored in memory at the same address $m$.

### Micro-instruction classes
Considering the possible types of micro-instructions using the main memory: we can group micro-instructions in four classes, named by the associated temporal cycles for executing them:

1. **Cycle I-1**: micro-instructions which implement instructions with a single explicit memory address $m$ whose content is read by the *Unità di Calcolo* for either:
    - computing it with or without $X$'s parallel adder, and the result is written into $X^{'}$.
    - computing it **without using** $X$'s parallel adder and the result is written back into $m$

    Note how these micro-instructions **do not use** $Y$'s parallel adder.

    ![image](../resources/cep_microinstructions_cycle-I1.png)

1. **Cycle I-2**: micro-instructions which implement instructions with a single explicit ememory address $m$ whose contente is read by the *Unità di Calcolo*, computed **using** $X$'s parallel adder, and the result is written back to memory ad address $m$.

    ![image](../resources/cep_microinstructions_cycle-I2.png)

1. **Cycle I-3**: all micro-instructions that behaves like those of the cycle I-1 group, with the exception that they **do use** $Y$'s parallel adder.

    Note how these micro-instructions **do not use** $Y$'s parallel adder.

    ![image](../resources/cep_microinstructions_cycle-I3.png)

1. **Cycle I-4**: similar to the micro-instructions of cycle I-2 with the exception that they do use $Y$'s parallel adder.

    ![image](../resources/cep_microinstructions_cycle-I4.png)

## Micro-instructions only using the "Unità di Calcolo" [[2](./0_reference.md)]

Within this group are considered micro-instruction which satisfy the following properties:

- they use exclusively or simultaneously the "Unità di Calcolo" or "Unità degli Indirizzi" for computing data.

- the presence or not of conditions produced  both "during" or "at the end" of the micro-instruction.

- the usage or not of $X$'s and $Y$'s parallel adders for computing.

Then these micro-instructions are subdivided into:

1. **normal micro-instructions:** those doing typical operations (e.g. add, subtraction, ect...).

2. **cyclic micro-instructions:** those which are executed a certain number of time.

### Subclasses for normal micro-instructions

- **Cycle 2-1**: those which use the *Unità di Calcolo* and/or *Unità degli Indirizzi*, without using $X$'s or $Y$'s parallel adders for their computation, and they **do not** produce any control signal "during" and "at the end" of execution.

- **Cycle 2-2**: those which use either only the *Unità di Calcolo* or also simultaneously the *Unità degli Indirizzi*, which do not use the $Y$'s parallel adder, and which **do or do not** produce any condition both "during" and "at the end" of execution.

- **Cycle 2-3**:  those which use either only the *Unità di Calcolo* or also simultaneously the *Unità degli Indirizzi*, which use the $Y$'s parallel adder, and which **do or do not** produce any condition both "during" and "at the end" of execution.

### Subclasses for cyclic micro-instructions

- **Cycle 3-1**: those which use the *Unità di Calcolo* and compute data **without** using $X$'s parallel adder.

- **Cycle 3-2**: those which use the *Unità di Calcolo* and compute data **using** $X$'s parallel adder.
