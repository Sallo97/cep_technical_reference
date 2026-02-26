# Index Register [[3](./0_reference.md)]
With index register we indentify processor's registers whose responsability is to point to an operand's address. In this way we can use the register to implement interations over contiguous data structures like arrays or vectors.

Generally the index register contains the base address of the data structure we want to iterate. At each iteration the pointer is added/subtracted by an offset (specified by the instruction or stored in another register) to obtain the complete address to the current memory location.

The first computer to implement the concept of index register was the Manchester Mark I developed by the Victoria University of Manchester in 1949. In this computer there were only two index registers,  to as a *B-lines*.

# CEP's celle parametriche

At their core, CEP's *celle parametriche* are similar to the *B-lines* of the Manchester Mark I, being memory cells used for handling ciclic programs and storing invariant addresses for a memory routine. [[7](0_reference.md)] Additionally parametric cells, as their name implies, are also used to hold the parameters passed to a sub-program. [[5](./0_reference.md)]

## First implementation [[7](./0_reference.md)]
Since the very start of development, the concept of celle parametriche was considered, with their main goal to allow the automatic modification of instuctions in two ways:

- an everlasting modification which is applied only at the first call of the instruction, for declaring a parameter value.

- a temporary modification, valid only in the current execution of the instruction, used for updating an index at each iteration.

This solution avoided the need to redefine a constant parameter each time the instruction was called in a cycle; however it didn't support the case where the parameter's value changes frequently at each iteration.

In total were considered only 16 celle parametriche.

## Second implementation [[7](./0_reference.md)]
In this second version, the instruction is accompained with two distinct sets of celle parametriche:

- the first one reserved for the modification of addresses involving the instruction, but also the registers.

- the second used only in the execution of special instructions.

In this new implementation if there is no update for a parameter, it is kept constant; but at the first update it is need to keep modify the address at each iteration. 

The registers increased from 16 to 64 and where moved from auxilary memory to the main one. Additionally for each program it was possible to fix per program the group of memory cells being celle parametriche.

The user just needed to specify the first memory cell being a cella parametrica, then all the 63 consecutive cells were considered being celle parametriche.

This allowed different programs to have a different number of parameters.

## Final implementation [[7](./0_reference.md)]
In the final version the number of celle parametriche remain 64, being numbered from 0 to 63, and are divided into two groups, each having 31 consecutive cells. 

Like in the second iteration, each program can define the group of memory cells being celle parametriche, by specifying the first cell being a cella parametrica. 

Note how we have stated that each of the two groups has 31 celle parametriche, making a total of 62 cells out the 64 total. This is because the cell $0$ and the cell $63$ are special purpose, more precisely:

- cell $0$ stores the fixed value $0$. Each time an operation has as operand the cell $0$, the processing unit knows implicitly that its value is $0$ and uses it, without making a read operation.

- cell $63$ contains the instruction counter. The CEP will add the content of this cell to the starting address of the program in order to retrieve the current instruction to execute.

The two groups of celle parametriche are called $H0$ and $HU$:

- $HO$ contains the *celle parametriche* reserved by the current program in execution. This means that every program, being a subprogram or not, refers to a distinct $H0$ group.

- $HU$ contains the *celle parametriche* in common to the system's organization the programs in execution.
