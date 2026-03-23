# Pseudoinstructions 
Pseudoinstructions at their core are subprograms which can be called in the same way as instructions. Users can declare new ones by mapping their associated **macroprogram**, i.e. an ordered sequence of normal instructions and pseudoinstructions stored in memory. The map between pseudoinstructions e macroprograms is defined by a jump table stored in main memory.
[[3](../helper_resources/references.md)] 

## History
The initial design of the CEP didn't consider pseudoinstructions. They where introduced momentarily to facilitate the implementation of floating point operations, but where discarded as soon as said instruction where added as microprograms. In 1958 the concept was reconsidered after it was found a satisfying way for implementing them. [[7](../helper_resources/references.md)]

## Pseudoinstruction's structure
Structure and behaviour wise, they are identical to legit instructions, thei differ in how the machine executes them. Normal instructions are implemented by an associated fixed microprogram in the Control Unit, whereas pseudoinstruction are mapped to a macroprogram stored in Main Memory. [[3](../helper_resources/references.md)]

Given a sequence $S$ of 36-bit representing an instruction ($S = s_0s_1s_2\ldots s_{35}$), the Instruction Decoder determines if its an instructions or a pseudinstruction by reading the first bit $s_0$: 

- if $s_0 = 0 \rightarrow S$ is a legit instruction.
- if $s_0 = 1 \rightarrow S$ is a pseudoinstruction.

## Executing the macroprogram
When the Instruction Decoder detects a pseudoinstruction, the following operations are done:

When the CEP detects a pseudoinstruction, a particular jump is done. The previous content of $o$ and $n$ (more precisely $n+1$) are kept in some memory location. The address of the jump is obtained by the first bits of the pseudoinstruction. [[11](../helper_resources/references.md)]

### How the jump is done
When $s_0 = 1$ then we identify as $s^{*}$ the sequences of subsequent eight bits ($s^{*} = s_1 s_2 \ldots s_8$ s.t. $0 \leq s^{*} \leq 255$). 

The current memory address $o$ (i.e. the address of the operand) is copied in the cell of index $1$ in the global parametric group $\{H^{1}\}$. The instruction counter $n$ is incremented and copied in $N$. Then a special jump instruction is called, **which does not modify N**. The address pointed by the jump is computed as $h^{1} + s^{*}$, with the pointed location always being an instruction which calls mapped macroprogram. Also, the content of register $N$ is stored in a parametric cell. In this way the called subprogram can easily retrieve $o$, for using it as its operand, and $n+1$, to call the next instruction. [[11](../helper_resources/references.md)]

### Properties of $s^{*}$
The address of the subprogram is computed as $h^{1} + s^{*}$. Recall that $h^{1}$ is the start address of parametric group $\{H^{1}\}$, which has reserved 32 cells. These cells are reserved, so $s^{*}$ must not refer to them. Additionally the three cells after $\{H^{1}\}$ are also reserved for the bit $s_1$ and thus cannot be touched by $s^{*}$.

This means that $35 \leq s^{*} \leq 255$. [[11](../helper_resources/references.md)]

## Special pseudoinstructions
Recall that an instruction can behave as an ordinary instruction, by using both parametric cells $P$ and $Q$ to compute the operand address, or as a special instruction, using only $Q$ for the operand-modification, while $P$ is used as a second address. Pseudoinstructions can behave in both ways. [[7](../helper_resources/references.md)]

## Jump table
The mapping between pseudoinstructions and macroprograms is stored in the **jump table**. The size of the table limits the number of pseudoinstruction available in the current program. Ignoring memory capacity, the table identifies each entry by an 8-bit code, meaning that theoretically there could be at most $256$ pseudoinstruction.

The jump table location dynamically changes in each program, beginning always at the start of group $\{H^1\}$.  This causes an overlap, meaning the first 31 + 1 cells are reserved to the parametric group. Thus maximum number of entries in the jump table is reduced to $256 - 32 = 224$. Those $224$ consider also the five routines associated to the control-bit, reducing the number of custom pseudoinstructions further to $224 - 5 = 219$. [[7](../helper_resources/references.md)]

Note that the jump table declares the pseudoinstructions available at the current moment in Main Memory, potentially we could have an infinite number of pseudoinstructions available among Auxilitary Memories. To use one of them we need to load their content into Main Memory and overwrite an entry in the jump table.

# Types of pseudoinstructions 
Pseudoinstructions implement useful features throught CEP's librarie, being distinguished into two fields: arithmetic pseudinstructions and logic pseudoinstructions. [[7](../helper_resources/references.md)]

### Arithmetic pseudoinstructions
Implement either complex arithmetic operations beyond the base machine-level scope, or traslators for handling numbers in representations beyond binary. For example, in this group belongs the exponential operation and the conversion from base-two to base-ten and viceversa. [[7](../helper_resources/references.md)]

### Logic pseudoinstructions
Instructions managing the program's execution. They are mainly used during compilation and assembling of programs written in supported high-level languages. For example, in this group belongs the instruction for handling the prologue and epilogue of subprograms' calls, and a unique pseudoinstruction for defining other pseudoinstructions though the usage of the address of another pseudoinstruction. [[7](../helper_resources/references.md)]

## Groups of pseudoinstructions 
Since we can potentially define an infinite number of pseudoinstructions, it is impossible to map each of them to an unique identifier . To overcome this problem, we subdivide pseudoinstructions into groups, pairing them according to how probable is to call them together. Each group has $32$ entries. Each program can use pseudoinstructions coming from at most seven different groups. [[7](../helper_resources/references.md)]

Note that these groups do not take into account pseudoinstructions being explicitly merged with the program's code during the assembly phase. [[7](../helper_resources/references.md)]

During compilation of the program written in the LPSC (i.e. CEP's Symbolig Programming Language), the compiler will assign a distinct binary code to each pseudoinstruction. The computed identifier is determined by attaching the group number of the pseudoinstruction (between $1$ and $7$) and its numbered entry within (between $0$ and $31$). An additional recodification of the codes is applied by the assembler to handle possible differences in the ordering of the groups in the subprograms. [[7](../helper_resources/references.md)]

## Common block of code
If a pseudoinstruction shares a good part of its code with other members of its group, they can be aggregated into a "block". Think of a block as a subprogram with different entry points. [[7](../helper_resources/references.md)]



