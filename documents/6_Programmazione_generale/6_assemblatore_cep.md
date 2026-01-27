# Assemblatore CEP
The assembly phase of a CEP program consists in merging the main program with its subprograms and pseudoistruzioni, generating the final program ready for execution.

This phase is handled by the programma assemblatore ASSGC. The ASSGC has the ability to decide where to store the produced result:

- load it in main memory, eventually storing part of the subprograms in the magnetic drum.

- store it in a binary punched tape which can be automatically loaded in main memory by a single move in the control panel.

    The binary punched tape can be of two types:

    - **absolute:** the stored addresses of instructions are always absolute. During loading this permits the CEP to retrieve subprograms and pseudoistruzioni more easily.

    - **invariant:** all stored addresses are relative to a parameter, which must be setted over the control panel's keyboard.

        This allows more flexibility of memory distribution for service programs.

We can distinguis three types of assembly: programmed, free, and mixed.

## Programmed assembly
Here the programmer specify the starting addresses for each subprogram, which can be stored either in main memory or in the magnetic drum. 

The order in which subprograms are loaded does not matter.

## Free assembly
Here the assembler will decide where to store the subprograms. In this case the order in which subprograms are loaded depends on the subprograms' level. 

If the ordering is not consistent, some errors can occur, causing the assembler to abort.

## Mixed assembly
In this scenario the programmer can set the starting address of some subprograms, with the rest being handled by the assembler.

## Schema di assemblamento
Routines used throught the code must be provided to the assembler by writing them into the "schema di assemblamento".

The schema di assemblamento in broken in three parts:

1. At the start the programmer must specify the names of the subprograms and, if it wants them to be fixed, their starting adresses. 

    If the start address has not been specified, the assembler will determine it dynamically.

1. Specify the start address in memory for :
    - the group $HU$ of shared celle parametriche.
    - the jump table containing the references to all pseudoistruzioni.
    - the reserved memory for each pseudoistruzioni' code.
    -  the reserver memory for the pseudoistruzioni's routines
    - the length of shared space reserved to pseudoistruzioni and subprograms.

1. In the last line of the module, the programmer must specify the groups of pseudoistruzioni used throught the code.

## Constructing $H0$
When reserving memory to each subprogram, the assembler will set the start of the local group $H0$ of celle parametriche at the beginning.

## Assemblatore's execution

1. The ASSGC starts by reading the "schema di asssemblamento" and all other information in the control panel, determining if the produced result must be loaded directly into memory or printend into a binary punched tape.

2. The assembler asks throught the teleprompter the names of the subprograms to assemble in the order in which the must be loaded. Here we must specify also the name of used pseudoistruzioni.

3. At the end of execution, the Assemble will print over the teleprompter all the determined memory locations.

### Error Handling
If we introduce a different tape than the one requested, the Assembler will notice it and request to insert the correct one.

The assembler will stop and tell an error in the case the number of groups of passed pseudoistruzioni goes overboard or the program will call a subprogram not specified in the schema di assemblamento.