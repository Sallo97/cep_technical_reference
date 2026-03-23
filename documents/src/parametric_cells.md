# Index registers 
Index registers are registers which point to the current operand's address. They are mainly used to keep the start entry of a contiguous data structure (e.g. arrays or vectors). At each iteration the start address is added or subtracted by a certain offset, usually specified by either an instruction or another memory location.[[3](../helper_resources/references.md)]

Index registers where first introduced by the **Manchester Mark I**, a computer developed by the Victoria University of Manchester in 1949. This machine had only two index registers, referred to as **B-lines**.

# Parametric cells
CEP's *parametric cells* are similar to the *B-lines* of the Manchester Mark I, being memory cells mainly used for cyclic programs or storing invariant addresses for a routine. [[7](../helper_resources/references.md)].

The name derives from the fact that they are also used to hold a parameter of a program [[5](../helper_resources/references.md)].

## History 

### First implementation
Parametric cells where considered since the very start of development, with their main goal to allow the automatic modification of instuctions in two ways.

- **An everlasting modification** , that is one which is applied only at the first call of the instruction. This functionality was considered for allowing programmers to declare a parameter's value.

- **A temporary modification**, that is one valid only in the current execution of the instruction. This functionality was considered in cyclic computations for updating the index at each iteration step.

By using parametric cells, there was no need to redefine a constant parameter each time the instruction was called in a cycle. However this initial implementation didn't supported the case where the parameter's value changed at each iteration. In total were considered only $16$ parametric cells. [[7](../helper_resources/references.md)]

### Second implementation
The initial idea was expanded by introducing two distinct sets of parametric cells. The first batch was used for addresses modification or register update; the second batch was reserved to the execute of  special instructions. This new implementation didn't require to update a parameter if it is kept constant during the whole execution; however after the first update the programmer needed to modify it at each iteration. [[7](../helper_resources/references.md)]

The total number of parametric cells was increased from $16$ to $64$ and they where moved from Auxilary Memory to the Main Memory. Additionally, each program could define their own group of parametric cells. The user just needed to specify the location in Main Memory of the first parametric cell, then the $63$ consecutive cells were automatically considered part of the group. This solution allowed programs to have an arbitrary number of parameters. [[7](../helper_resources/references.md)]

## Final implementation 
In the final design of the CEP, the number of parametric cells is kept to $64$ , being numbered from $0$ to $63$. Parametric cells are divided into **two groups**, with each program having their own distinct pair of sets. Parametric cells are stored in Main Memory and they do not differ from any other memory cell, occupying a memory word each. In fact, parametric cells can be accessed and modified using normal memory access instructions. [[11](../helper_resources/references.md)]

### Parametric groups
For the current program in execution, registers $H^{0}$ and $H^{1}$ of 15-bits each contain the base addresses $h^{0}$ and $h^{1}$ of their associated parametric group. 

### The first group $\{H^{0}\}$
Contains the first $32$ cells. Given the start address $h^{0}$, the set of addresses belonging to the group is: 
$$\{H^{0}\} = \{h^0 + 0 + h^0 + 1 + h^0 + 2 + \ldots + h^0 + 31\}$$

The first member of the group (the one at address $h^{0} + 0$) **is assumed by the CEP to be immutable and have value $0$**. In practice its content can be modified freely by the user, but during execution if its accessed as a parametric cell, the CEP will assume that its value is always zero.

### The second group $\{H^{1}\}$
Contains the last $32$ cells. Given the start address $h^{1}$, the set of addresses belonging to the group is:
$$\{H^{1}\} = \{h^1 + 0 + h^1 + 1 + h^1 + 2 + \ldots + h^1 + 31\}$$

The first member of the group (the one at address $h^{1} + 0$) **is assumed by the CEP to be immutable and have value $0$**. In practice its content can be modified freely bu the suer, however during execution if its accessed as a parametric cell, the CEP will assume that its value is always zero.

## Parametric cells usage
Parametric cells are mainly used for address modification and for calculations.

## Identifying a parametric cell
In each instruction, two contiguous subsequences of 6-bits identify itstwo distinct parametric cells. Each sequence is broken down into a **group index** and a **relative address**, with the combination of the two determining the parametric cell.

### Group index
We define the **group index** $i$ as the first bit of the sequence. It identifies the parametric group of the cell:

- if $i = 0 \rightarrow$ it refers to the parametric group $\{H^{0}\}$ 
- if $i = 1 \rightarrow$ it refers to the parametric group $\{H^{1}\}$ 

### Relative parametric address
The last 5 bits of the sequence represent an integer number from $0$ to $32$ called the **relative address**. This number is used as the entry of the parametric cell in the group specified by the group index.
