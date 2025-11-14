# Sistema di Programmazione Generale della CEP (SPGC) [[4](./0_reference.md)]
The *Sistema di Programmazione Generale della CEP* (SPGC) is the collection of all auxiliary programs developed for translating into and from lower-level binary machine code the information the higher-level language used by users to define the program.

The SPGC is mainly composed of small programs dedicated to a particular phase of the traduction (e.g. one delagated in translating fixed point numbers from decimal to binary). These programs then must be called each time a program needs them during execution. The merging of the user's defined program and these auxiliary programs is delegated to a general assembler program, which is able to load and call all the required translation programs into memory and inbetween the user's code for immediate execution, or can store them along the code when printing the code into tape.

The SPGC is defined using an extension of the Fortran programming language. As of 1963 there were discussion in supporting also the Algol syntax, along side a translator for the language, but it was not yet implemented.

The SPGC divides is auxiliary programs into five main groups.

## Group A
A series of programming languages for formulating CEP's routines.
The programming languages handled by the CEP *(as of 1963)* are:

- **Linguaggio Programmativo Decimale CEP (LPDC)**.

    It is the programming language more close to the low level binary machine language. Its main abstraction is the use of the decimal notation for declaring instructions and program constants. 

    It was the first programming language defined for the CEP, but it was no longer used after the other higher level languages have been implemented in the computer. Still, it is used for debugging machine level programs.

- **Linguaggio Programmativo Simbolico CEP (LPSC)**.

    It is based on the structure of the CEP's instructions. It offers ways to refer to instructions and pseudoinstruction as mnemonics and for using symbolic reference for referring to memory locations pointing to instructions location, parameters and data. 

- **Linguaggio Programmativo Correttore CEP (LPCC)**.
    
    
    It is used for declaring at the binary level checking routines in the program.

    Both **LPSC** and **LPCC** permits to refer in the routines to numeric constants and literal constants.

- **Linguaggio Programmativo Fortran origin (LPFO)**. 
    
    The LPFO coincides with the Fortral language defined by IBM for the *704* computer and is described in the manuals *FORTRAN I* and *FORTRAN II* written by IBM. 

    The support for this language has been added in order to make the CEP compatible with all programs already written in that version of the programming language. Still, an user can extend programs written in **LPFO** by adding instructions defined in the custom Fortran CEP programming language.

- **Linguaggio Programmativo Fortran CEP (LPFC)**. 

    An extended version of the Fortran origin programming language. The two languages mainly differs in the possibility of defining in **LPFC** arithmetic expressions containing arithmetic variables of any kind <!-- Still not clear on what any kind means. --> and in some parts of the syntax which were modified to make the compilation of **LPFC** programs easier.

    Both **LPFC** and **LPFO** are the highest level language available in the CEP *(as of 1963)* and were used for defining scientific programs <!-- I think mainly for the computation of integrals-->.

    When compiling a **LPFO** program, the last step before binary translation is transforming it into a **LPFC** program. 
    
    In **LPFC** are defined library subprograms and pseudoinstruction highly optimized. 

Additionally, for notation sake we refer to the low level machine language of the CEP as **LBBC** <!-- As of know I haven't found what it stands for -->.

## Group B
A series of translation programs for translating the routines written in the higher language into the lower level binary machine language of the CEP.

For each programming language supported by the CEP it is associated a translator program which transforms the given program into the lower-level equivalent following the hierarchy of the programming languages of the CEP. The hierarchy is described in the below image:

![image](../resources/cep_language_hierarchy.png)

below each arrow is defined the program delagated to the translation process:

- **Traduttore Fortran Originario a fortran CEP (TFOC)**: translator from **LPFO** programs into **LPFC** programs.

- **Traduttore Fortran a Simbolico CEP (TFSC)**: translator from **LPFC** programs into **LPSC** programs.

- **Traduttore Simbolico a Binario CEP (TSBC)**: translator from **LPFC** to **LBBC**.

- **ASSGC**: is the main assembler of the CEP.

- **Caricatore Estrattore Generale CEP (GEGC)**.

Only **TFOC**, **TFSC**, and **TSBC** are real translators, **ASSGC** and **GEGC** are not in group B, but in group C. 

## Group C
A series of programs for organizing the *assemble* (from the italian *assemblamento*) of programs and for translating tapes containg pseudoinstructions.

These programs are:

1. **Assemblatore Generale CEP (ASSGC)**: It has the responsibility of composing the final executable by merging the program, its subprograms (both custom or from the library), and pseudoinstructions into a final binary executable. It also supports the writing of the composed program into a magnetic tape.

    It assumes all components of the program have been traduced to machine code prior to the assembly. 
    
    For **LPCC** programs it can translate it without requiring to call the translator. This is done mainly for defining more easily checkup routines.

2. **Compilatore dei nastri binari di gruppo delle pseudoistruzioni**: used for grouping pseudoinstructions into a single tape.

3. **Caricatore Estrattore Generale CEP**: the first auxiliary program being defined for the CEP. It implements the following functions:

    - conversion and loading of programs of single sequence instructions or program constants written in **LPDC**.

    - extraction of binary tapes of *autocaricamento* programs. 
    
    - perforation or printing of memory content in decimal instructions or octal characters.

## Group D
A series of static and dynamic programs for checking arithmetic calculations and the safety of programs. 

Static programs check spefic memory location; dynamic programs check the program in execution. Both allow a step-by-step debug of the program, printing information specified by the user.

## Group E
A large series of pseudoinstructions necessary for organizing logically the computer, for defining the I/O operations, and for handling subprogram's calls.
