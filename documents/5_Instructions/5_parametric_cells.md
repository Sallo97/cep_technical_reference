# Index registers [[3](./0_reference.md)]
With index register we indentify registers whose responsability is to point to an operand's address. In this way we can use the register to interate over contiguous data structures (e.g. arrays or vectors).

Generally, index registers contain the base address of the data structure we want to iterate. At each iteration the pointer is added or subtracted by an offset, usually  specified by the instruction or stored in another register.

The first computer to introduce the concept of index register was the **Manchester Mark I** developed by the Victoria University of Manchester in 1949. In this machine there were only two index registers, known as a **B-lines**.

# Parametric cells
CEP's *parametric cells* are similar to the *B-lines* of the Manchester Mark I, being memory cells used for handling ciclic programs and storing invariant addresses for a memory routine. [[7](0_reference.md)].

Additionally parametric cells (as their name implies) are used to hold the parameters passed to a sub-program. [[5](./0_reference.md)]

## History [[7](./0_reference.md)]

### First implementation
Parametric Cells where considered since the very start of development, with their main goal to allow the automatic modification of instuctions in two ways:

- **an everlasting modification** which is applied only at the first call of the instruction. This functionality was considered for allowing programmers to declare a parameter's value.

- **a temporary modification**, valid only in the current execution of the instruction. This functionality was considered in cyclic computations for updating the index at each iteration step.

By using parametric cells, the need to the need to redefine a constant parameter each time the instruction was called in a cycle was avoided. However this initial implementation didn't supported the case where the parameter's value changed frequently at each iteration.

In total were considered only 16 parametric cells.

### Second implementation
In this second version, two distinct sets of parametric cells where considered:

- the first batch was used for addresses modification in instructions and for registers update.

- the second batch was used exclusively for the execution of special instructions.

This new implementation didn't require to update a parameter if it is kept constant during the whole execution. However at the first update, the programmer needed to modify its address at each iteration.

The total number of parametric cells increased from $16$ to $64$ and they where moved from Auxilary Memory to the Main Memory. Additionally, each program could define their own group of parametric cells. The user just needed to specify the location in Main Memory of the first parametric cell, then the $63$ consecutive cells were automatically considered part of the group.

This solution allowed programs to have an arbitrary number of parameters.

## Final implementation [[11](./0_reference.md)]
In the final design of the CEP, the number of parametric cells is $64$ (like the second implementation), being numbered from 0 to 63. Parametric cells are divided into **two groups**, each program has their own distinct pair of sets. Parametric cells are stored in Main Memory and design-wise they do not differ from any other memory cell, occupying a memory word each. In fact, parametric cells can be accessed and modified using normal memory access instructions.

### Parametric groups
Registers $H^{0}$ and $H^{1}$ of 15-bits each (the same amount dedicated to the address in the instructions) contain **for the current program** the base addresses $h^{0}$ and $h^{1}$ of its **two parametric groups**. 

The two groups are declared as:

### Local Group $\{H^{0}\}$
Contains the first $32$ cells. It identifies cells **reserved** to the associated program, meaning that any program needs to have a distinct $\{H^{0}\}$ group.

Given $h^{0}$, the base address of the group, then the set of addresses belonging to the group is: 
$$\{H^{0}\} = \{h^0 + 0 + h^0 + 1 + h^0 + 2 + \ldots + h^0 + 31\}$$

The first member of the group (the one at address $h^{0} + 0$) when interpreted as parametric cell **is assumed by the CEP to be immutable and have value $0$**. In practice it can be modified with any other number when treated as a normal memory cell, but operations using it as a parametric cell will follow the above description.

### Global Group $\{H^{1}\}$
Contains the last $32$ cells. It identifies cells **shared** with the rest of the programs in the current execution. The set of addresses belonging to the group is:
$$\{H^{1}\} = \{h^1 + 0 + h^1 + 1 + h^1 + 2 + \ldots + h^1 + 31\}$$

The first member of the group (the one at address $h^{1} + 0$) when interpreted as parametric cell **is assumed by the CEP to be immutable and have value $0$**. In practice it can be modified with any other number when treated as a normal memory cell, but operations using it as a parametric cell will follow the above description.

## Parametric cells usage
The parametric cells are mainly used for modifying the address of instructions and for calculations.

## Identifying a parametric cell
In each instruction, two sequences of 6-bits are used to identify two distinct parametric cells used by it. Each sequence is broken down into a **group index** and a **relative address**; the combination of the two will determine the parametric cell.

### Group index
We define the **group index** as the first bit of the sequence. It identifies the parametric group of the cell:

- if $i = 0 \rightarrow$ it refers to the parametric group $\{H^{0}\}$ 
- if $i = 1 \rightarrow$ it refers to the parametric group $\{H^{1}\}$ 

### Relative parametric address
The last 5 bits of the sequence represent an integer number from $0$ to $32$ called the **relative address**. This number is used as the entry of the parametric cell in the group specified by the group index.
