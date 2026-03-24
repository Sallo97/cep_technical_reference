# CEP's assembler
The assembly phase of a CEP program consists in merging the main code with its support subprogrms and pseudoistructions, generating the final binary executable. This phase is handled by ASSGC. The produced program can be written into either Main Memory, with eventually part of the subprograms in Auxiliary Memory, or on a punched tape. [[7](../helper_resources/references.md)]

Binary punched tapes can be of two types:

- **absolute:** the stored addresses of instructions are always absolute. During loading this permits the CEP to retrieve subprograms and pseudoistruzioni more easily.

- **invariant:** all stored addresses are relative to a parameter, which must be setted over the control panel's keyboard. This allows more flexibility of memory distribution for service programs.

## Assemble process type
We can distinguis three types for the assembly process: programmed, free, and mixed. [[7](../helper_resources/references.md)]

## Programmed assembly
In this mode the programmer specifies the starting addresses for each subprogram, which can be written either in Main Memory or in the auxiliary memory. Note that the order in which subprograms are loaded does not matter. [[7](../helper_resources/references.md)]

## Free assembly
In this mode the the assembler will decide where to store the subprograms. The order in which subprograms are loaded depends on the subprograms' level. If the ordering is not consistent, some errors can occur, causing the assembler to abort. [[7](../helper_resources/references.md)]

## Mixed assembly
In this mode the programmer can set the starting address of some subprograms, with the rest being handled by the assembler. [[7](../helper_resources/references.md)]

## Assembly scheme
Routines used throught the code must be provided to the assembler by writing them into the "assembly scheme" (*from the italian "schema di assemblamento*"). It is broken in three parts. [[7](../helper_resources/references.md)]

1. At the start, the programmer must specify the names of the subprograms and optionally, if it wants them to be fixed, their starting addresses. For programs missing the start address, the assembler will determine it dynamically.

1. Specifies the start address in memory for:
    - the group $\{H^{1}\}$ of parametric cells.
    - the jump table containing the references to all pseudoinstructions.
    - the reserved memory for each pseudoinstructions' code.
    - the reserver memory for the pseudoinstructions' routines
    - the length of shared space reserved to pseudoinstructions and subprograms.

1. The last line of the module, the programmer must specify the groups of pseudoinstructions used throught the code.

## Constructing $\{H^0\}$
When reserving memory to each subprogram, the assembler will set the start of the their $\{H^{0}\}$ group  at the beginning.

## Assemblatore's execution

1. The ASSGC starts by reading the Assembly Scheme and additional information from the control panel. This data determines if the produced result must be loaded directly into Main Memory or printend into a binary punched tape.

2. The assembler asks throught the teleprinter the names of the subprograms to assemble in the order in which they must be loaded. Here we must specify also the name of all used pseudoinstructions.

3. At the end of execution, the assembler will print over the teleprinter all the determined memory locations.

### Error Handling
If we introduce a different tape than the one requested, the assembler will notice it and request to insert the correct one. [[7](../helper_resources/references.md)]

The assembler will stop and tell an error in the case the number of groups of passed pseudoistruzioni goes overboard or the program will call a subprogram not specified in the schema di assemblamento. [[7](../helper_resources/references.md)]