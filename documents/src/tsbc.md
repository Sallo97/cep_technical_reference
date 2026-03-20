# Traduttore Simbolico Binario CEP [[7](../0_Additional_resources/0_reference.md)]
The Traduttore Simbolico Binario CEP (TSBC for short) has two responsabilities:

1. translation of routines written in the Linguaggio Programmativo Simbolico CEP to binary. 

1. actively monitor during the translation process that the program is syntactically correct, returning useful error messages otherwise. 

    It does not just print the first error it ecounters and aborts the translaton process, rather it will still scan the program, finding all remaining errors.

If the program is correctly translated, it can return, if requested, some additional information useful for debugging or by the assembler.


A programmer can request:

- the generation of a binary punched tape for the assembler. 

    This punched tape contains:

    - the original definiton of each routine, specifying additionally where it came from.

    - the pseudoistruzioni' mask and the definition of the program.

    - the list of the subprograms' names used in the program.

    - the binary code produced.

- the printing (through the teleprinter) or punching (through the generation of a punched tape) of the original program side by side to its octal traduction.

- printing or punching of the tables created by TSBC during the translation process. 

    The TSBC constructs the following tables:

    - the table listing all the parametric references used.

    - the table listing all subprograms' names used.

    - the table listing all symbolic references.

    - the table listing all the costant tables.

    - the table listing all the work areas.

    - the table listing all pseudoistruzioni used.



