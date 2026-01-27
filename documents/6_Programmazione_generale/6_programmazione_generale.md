# Sistema di Programmazione Generale della CEP (SPGC) [[4](./0_reference.md)]
The CEP low-level execution system represents both numbers and instructions in binary. This makes programming at the machine-level very difficult (if not impossible) for users. 

The computer support high-level languages by providing a complex system of service programs, called the *Sistema di Programmazione Generale della CEP* (SPGC for short). This collection of programs handle the automatic translation of code into machine-language and the conversion of numeric constants between decimal and binary.

Each service program is small, being dedicated to a particular phase of the translation (e.g. one delagated in translating fixed point numbers from decimal to binary). These services must be invoked each time an high-level program needs them during its execution. 

The incorporation of service routines into a user's defined program is delegated to a general assembler.

Most SPGC's programs are written in Fortran-CEP, an extension of Fortran developed especially for the CEP. 

As of 1963 there were discussion in adding support to the Algol language, with prelimary design decision regarding is translation routines.

# SPGC Organization

The SPGC divides its programs into five main groups.

## Group A
The set of programming languages supported by CEP, in which are coded all of its routines.

The programming languages handled by the CEP *(as of 1963)* are:

- **Linguaggio Programmativo Decimale CEP (LPDC)**.

    It is the programming language more close to the low level binary machine language, differing only in the use of the decimal notation for declaring instructions and program constants. 

    It was the first high-level programming language implemented for the CEP, but it was no longer supported after other higher level languages have been added in the computer.
    
    Still, it is used for debugging purposes.

- **Linguaggio Programmativo Simbolico CEP (LPSC)**.

    Its syntax follows the format of CEP's instructions. It allows:
     
    - to refer to instructions and pseudoinstruction as mnemonics.
    - to define symbolic references for memory locations pointing to instructions location, parameters and data. 

    The translation process of LPFO or LPFC programs returns as output code written in this language.[[7](../0_Additional_resources/0_reference.md)]

    Pseudoistruzioni and subprograms focussed on optimization are written in this language.[[7](../0_Additional_resources/0_reference.md)] 

- **Linguaggio Programmativo Correttore CEP (LPCC)**.
    
    Used for declaring checking routines, supporting the decimal or octal base for numbers.

    Theoretically it is possible to declare any kind of routine coded in LPCC, but its main usage is for checking routines.[[7](../0_Additional_resources/0_reference.md)]

    Both **LPSC** and **LPCC** permits to refer in the routines to numeric constants and literal constants (alphabetic sequences).

- **Linguaggio Programmativo Fortran Origin (LPFO)**. 
    
    The LPFO coincides with the Fortral language defined by IBM for the *704* computer and is described in the manuals *FORTRAN I* and *FORTRAN II* written by IBM. 

    The support for this language has been added in order to make the CEP compatible with all programs already written in Fortran. Still, an user can extend programs written in **LPFO** to the Fortran CEP programming language.

- **Linguaggio Programmativo Fortran CEP (LPFC)**. 

    An extended version of the LPFO, allowing the defition in **LPFC** of arithmetic expressions containing arithmetic variables of any kind. 

    Both **LPFC** and **LPFO** are the highest level language available in the CEP *(as of 1963)* and were used for defining scientific programs.

    When compiling a **LPFO** program, the last step before binary translation is transforming it into a **LPFC** program. 
    
    In **LPFC** are defined highly optimizied library subprograms and pseudoinstruction. 

Additionally, for notation sake we refer to the low level machine language of the CEP as **LBBC** (Linguaggio Base Binario CEP [[7](../0_Additional_resources/0_reference.md)]).


## Group B
A series of translator programs for translating from one of the supported higher language into either machine language or a lower-level language.

For each high-level programming language it is associated a translator program. The hierarchy is described in the below image:

![image](../../resources/cep_language_hierarchy.png)

below each arrow is indicated the translator program used for translating from the left-language into the right-language:

- **Traduttore Fortran Originario a fortran CEP (TFOC)**: translator from **LPFO** programs into **LPFC** programs.

- **Traduttore Fortran a Simbolico CEP (TFSC)**: translator from **LPFC** programs into **LPSC** programs.

- **Traduttore Simbolico a Binario CEP (TSBC)**: translator from **LPFC** to **LBBC**.

- **ASSGC**: is the main assembler of the CEP.

- **Caricatore Estrattore Generale CEP (GEGC)**.

Only **TFOC**, **TFSC**, and **TSBC** are real translators.

**ASSGC** and **GEGC** are not part of group B, being members of group C. 

## Group C
A series of programs for organizing the *assemble* (in italian "*assemblamento*") of programs and for translating tapes containing pseudoinstructions.

These programs are:

1. **Assemblatore Generale CEP (ASSGC)**: It has the responsibility of composing the final executable by merging the program, its subprograms (both custom or from the library), and pseudoinstructions into a final binary executable. It also supports writing the merged program into a magnetic tape.

    It assumes all components of the program have been traduced to machine code or in LPPC prior to the assembly process. 
    
    For **LPCC** programs the assembler support their translation. This feature was added mainly for defining more easily checkup routines.

2. **Compilatore dei nastri binari di gruppo delle pseudoistruzioni**: used for grouping into a magnetic tape all the routines associated to a group of pseudoistruzioni.

3. **Caricatore Estrattore Generale CEP**: the first auxiliary program being defined for the CEP. It implements the following functions:

    - conversion and loading of programs of single sequence instructions or program constants written in **LPDC**.

    - extraction of binary tapes of *autocaricamento* programs. 
    
    - perforation or printing of memory content in decimal instructions or octal characters.

## Group D
A series of static and dynamic programs for checking correctness of arithmetic calculations and general safety. 

Static programs check specific memory location; dynamic programs check the program in execution. Both allow a step-by-step debug of the program, printing information as requested by the user.

## Group E
A large series of pseudoinstructions necessary for organizing logically the computer, for defining the I/O operations, and for handling subprogram's calls.
