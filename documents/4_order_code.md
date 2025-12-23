# Order Code
The CEP uses for all of its operations a 1+1 address scheme, meaning that the Order Code defines only instruction having:
- exactly one explicit operand.
- one implicit operand.

There are in total 128 normal instructions [[4](./0_reference.md)] and 219 pseudo-instructions ([6](./0_reference.md)).  
For each instruction there two additional implicit 15-bit addresses, each stored in a distinct reserved region of main memory [[2](./0_reference.md)].

Finally these is an implicit return location.


## Instruction structure
All instructions are 36-bit long and contain (moving from the left-most bits to the right):

- operation symbol (9 bits):
    
    - The first bit determines if it is a instruction or a pseudoinstruction.
    
    - If it is an instruction, 7 bits determine it. [[4](./0_reference.md)] 

        If it is a pseudoinstruction those 7 bits determine the entry in the jump table where it is defined. [[3](./0_reference.md)]
    
    - 1 bit is a control bit to specify if we want automatic check on the instruction. [[3](./0_reference.md)]

- first parametric address (6 bits).

- second parametric address (6 bits).

- operand address (15 bits).

![image](../resources/cep_instruction.svg)


## Instruction classes [[2](./0_reference.md)]
Instructions in the CEP can be grouped in two main classes: *istruzioni ordinarie* and *istruzioni speciali*. Each class interprets the two implicit 6-bit addresses of the *celle parametriche* differently.

### Istruzioni ordinarie
The content of the implicit 6-bit addresses is used for modifying the current instruction. 

### Istruzioni speciali
Here only the second *cella parametrica* is used for modifying the operand's address, the first one is used in different cases depending on the instruction. 

Thus the CEP for this kinds of instructions behaves like a two-address machine, with only one parametric address is parametric.

The two addresses used by the instruction are relative. To make them absolute they are they are summed with the origin addresses in two distinct registers in the *Unità degli Indirizzi* where all memory addresses are stored.

#### Arithmetic instructions

Among special instruction there are also those which are able to operate and/or use the *celle parametriche*. An example of this group of operations are arithmetic operations which have addresses composed of a parameter, an index and (eventually) a constant component.

#### Jump instructions
All kinds of instructions involving jumps in which the second *cella parametrica* is used to store the start address. 

The *cella parametrica* used mainly in implementing loops which must return to the start of the cycle.

#### Move instructions
Instructions for moving data from the main memory to the auxiliary memory and instructions for defining the length of the block to move. 

In this way we can handle the transmission of blocks of various length.

<!-- I assume the cella parametrica is used to store the length of the block, but the paper doesn't state it explicitly. -->

#### Search instruction over table
In instructions for searching over a table the *cella parametrica* is used for specifying the table's size.