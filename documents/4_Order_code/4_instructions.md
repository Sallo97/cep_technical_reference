# Instructions[[7](./0_reference.md)]
CEP's instructions follow a 1+1 address scheme: each instruction has two operands, one explicit and one implicit, and an implicit return location.

All instructions occupy 36-bits and are subdivided (from left-to-right) into:

- the operation symbol (9-bits). 

    The first bit will determine if it is a pseudoistruzione or an instruction (more on this later). 
    
    The last bit will determine if we want the CEP to automatically check correctness on the instruction [[3](./0_reference.md)].

- two consecutives parametric addresses (6+6-bits), used for manipulating the operand address.

<!-- I'm not sure if parametric addresses strictly refer to addresses referring to celle parametriche. I though so at the beginning, but the distinction between instructions, specifying that special instructions are used for working with celle parametriche has made me dubious -->

- operand address (15-bits).

<!-- still not sure what the 15-bits address contains -->

![image](../resources/cep_instruction.svg)
<!-- fix the name "Operand Symbol" to "Operation Symbol" -->

For each instruction there are two additional implicit 15-bit addresses, each stored in a distinct reserved region of main memory [[2](./0_reference.md)].

There are in total $128$ normal instructions [[4](./0_reference.md)].

Instructions are grouped in two classes:

- **ordinary:** allow the modification of both parametric addresses.

    Most arithmetic instructions belong to this class. As an example consider an instruction used for scanning through a matrix, which require as parameters:
    - an accumulator.
    - the index of the current element of the matrix.
    - the start location of the matrix, which remains fixed throught the computation.

<!-- I'm not sure if the instructions over matrices are special or ordinary instructions. In theory they need to keep the start location fixed, but the other two parameters are updated at each iteration.
Their explanation is found at page 10 of 1964-IS-001 -->

- **special:** the user can only update the second parametric address. The first one is fixed and is used during instruction execution for different reasons depending on the instruction's type. Special instructions make the CEP behave like a two-address machine with only one parametric address. Note that the two addresses are relative, to get the absolute associated address they must be summed with the origin addresses stored in two disting registers of the Unità degli Indirizzi.

    The special instructions are divided into:

    - Those used for operating over the content in the cella parametrica.

    - jump instructions (both normal and conditional) where the cella parametrica stores the start address. They are especially convenient for jump instructions that, after completion, need to set back the previous instruction address (similar to how nowadays we have the return procedure from a callee to the caller). 

    - instructions for handling the transfer of blocks between main memory and auxiliary memory, being able to set the length of the block to send.

    - The query instruction over a table, being able to store in the cella parametrica the table's length.


