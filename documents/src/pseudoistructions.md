# Pseudoistruzioni [[3](./0_reference.md)]
Pseudoistruzioni extend the instructions offer by the CEP, by allowing users to declare new ones by defining an associated macroprogram. 

In each program, the user needs to define a map between custom-added instructions and subprograms.

## History of pseudoistruzioni [[7](./0_reference.md)]
Pseudoistruzioni were not considered in the original design of the CEP. During development they were introduced momentarily as a need to facilitate the programming of floating point operations, but where discarded as soon as the introduction of floating point operations in the microprogram of the computer. 

In 1958 the concept was reintroduced after it was found a satisfying technical solution for implementing them.

## Pseudoinstruzioni's structure

They share the same 36-bit structure as normal instructions. From a theoretical point of view they are equivalent as normal instructions, the main difference is in **how** the machine executes them: 

- a normal instruction is executed by running is associated fixed microprogram, stored in the control unit.

- the CEP retrieves the associated **macro-program**, a sequence of normal instructions and pseudoistruzioni, and loads it into main memory.

The instruction decoder distinguish between instructions and pseudoistruzioni by reading the first bit of the operation symbol (first 9-bits of the instruction): 

- if the bit is set to '$0$', then it is a normal instruction.
- if the bit is set to '$1$', then it is a pseudoistruzione.

## Executing the macroprogram
When the decoder detects a pseudoistruzione:

1. copies the address in a specific cella parametrica of the group $HU$. Recall that $HU$ is the group of celle parametriche which will be shared among all operations.

2. sets the CEP in a special mode which does not update the instruction counter. This is done in order to separate the global instruction counter from the counter of the macroprogram.

3. jumps to the starting address of the associated macroprogram, which is specified by the last 8-bits of the operation symbol. The jump instruction stores the current instruction counter into a cella parametrica of the $HU$ group.

The map between pseudoistruzioni e macroprograms is defined by a jump table stored in main memory.

## Special pseudoistruzioni
Most pseudoistruzioni behave as normal instructions, being able to modify both of its parametric addresses. 

Still, it is possible to define special pseudoistruzioni, having a fixed parametric address and a modifiable one.

## Jump table
The number of available pseudoistruzioni in a program depends, ignoring memory capacity, to the size of the table storing the map between pseudoistruzioni and macroprograms. 

8-bits are used to determine the requested pseudoistruzione, meaning we potentially have at most $2^8 = 256$ pseudoistruzioni (again, ignoring the limit of memory capacity).

In practice, since in the CEP design it is not possible to define fixed location for any particular group of cells, the jump table location dynamically changes in each program, beginning always at the start of the group $HU$ of celle parametriche. 

This causes an overlap between the jump table, $HU$ (composed of 31-consecutive cells), and the special cella parametrica $63$, meaning the first 31 + 1 cells cannot be used by the table. The actual number of entries in the jump table is reduced to $256 - 32 = 224$.

Those $224$ consider also the five routines associated to the control-bit. [[7](../0_Additional_resources/0_reference.md)]

Note that the jump table declares the pseudoinstructions mapping available at the current moment in main memory. This means we can have potentially infinite pseudoistruzioni stored among auxiliary memories. To use them we need to load their content into main memory and dedicate them an entry in the jump table.

# Types of pseudoistruzioni [[7](../0_Additional_resources/0_reference.md)]
Pseudoistruzioni are used for implementing useful features throught the CEP's libraries. 

We distinguish the library-defined pseudoistruzioni in two fields:

- **Arithmetic**, offering complex arithmetic operations not available as normal instructions, or for handling numbers in base different than binary.

    Some of them are:

    - root operations.
    
    - exponential operations.
    
    - conversion operations from binary to decimal and viceversa.
    
    - arbitrary-precision arithmetic operations.

- **Logic**, are responsible to manage the program's execution. They have been mainly used by translators and the assembler.

    Some of them are:
    
    - operations for operating over single bits, or group of bits, or over variable-length words.

    - for handling the prolog and epilogue of calls to subprograms.

    - a unique pseudoinstruction for defining special pseudoinstruction, by interpreting the address of another pseudoinstruction.

## Groups of pseudoistruzioni [[7](../0_Additional_resources/0_reference.md)]
Since we can create an infinite number of pseudoistruzioni, it is not possible to have a fixed identifier for each of them. 

To overcome this problem we subdivide pseudoistruzioni into groups according to how probable is to use them together. Each group has $32$ entries. Each program can use pseudoistruzioni coming from at most seven different groups. 

These groups do not take into account pseudoistruzioni being explicitly merged with the program's code during the assembly phase.

If a pseudoistruzione share a good part of its code with other members of its group, they can be aggregated in a "block". Think of a block as a subprogram with different entry points. 

The Linguaggio Programmativo Simbolico CEP (LPSC) during compilation will assign a distinct binary code to each pseudoistruzione. The identifier is determined by attaching the group number of the pseudoistruzione (between $1$ and $7$) and its numbered entry within (between $0$ and $31$).

An additional recodification of the codes is applied by the assembler to handle possible differences in the ordering of the groups in the subprograms.

# Compilatore dei nastri di gruppo delle pseudoistruzioni (CONGBX) [[7](../0_Additional_resources/0_reference.md)]
During the assembly phase we need to load the routines used by the pseudoistruzioni in the program. The process of translating to binary the requested routines is handled by the Compilatore dei nastri di Gruppo delle Pseudoistruzioni (CONGBX for short).

For each group CONGBX retrieves the associated routines, translates them into binary and mount them over a magnetic tape.