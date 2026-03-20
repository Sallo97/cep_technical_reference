#  CEP General System Programs (SPGC) [[4](./0_reference.md)]
SPGC stands for "*CEP General System Programs*" (from the italian "*Sistema di Programmazione Generale della CEP*") and it identifies the base set of helper programs written for the CEP. [[4](../0_Additional_resources/0_reference.md)]

The CEP low-level machine-language represents both numbers and instructions in binary. This makes programming very difficult (if not impossible) for users. 

For this reason, the computer offers a number of high-level programming languages, which are supported by using a complex system of service programs called the **SPGC**, i.e. the *General Programming System of the CEP* (from the italian "*Sistema di Programmazione Generale della CEP*"). This collection of programs handle the automatic translation of code into machine-language and the conversion of numeric constants between decimal and binary.

Each service program is small, being dedicated to a particular phase of the translation (e.g. one delagated in translating fixed point numbers from decimal to binary). These services must be invoked each time an high-level program needs them during its execution. 

The incorporation of service routines into a user's defined program is delegated to a general assembler.

Most SPGC's programs are written in Fortran-CEP, an extension of Fortran developed especially for the CEP. 

As of 1963 there were discussion in adding support to the Algol language, with prelimary design decision regarding is translation routines. Sadly, the compiler was never finished.

# SPGC Organization

The SPGC divides its programs into five main groups.

## Group A - Programming Languages
The set of programming languages supported by CEP, in which are coded all of its routines.

The programming languages handled by the CEP are:

### CEP Decimal Programming Language  (LPDC)

The first developed programming language is LPDC, which stands for *"CEP's Deciaml Programming Language"* (from the italian "*Linguaggio Programmativo Decimale CEP*"). This language is almost identical to the low level machine language, with the main difference being the possibility to define constants and instructions in the decimal numeral system.[[9](../0_Additional_resources/0_reference.md)]

It was the first high-level programming language implemented for the CEP, but it was quickly delegated to simple debugging routines after other more powerful languages were introduced.[[4](../0_Additional_resources/0_reference.md)]

### CEP Symbolic Programming Language (LPSC)
The second developed programming language is LPSC, which stands for "Linguaggio Programmativo Simbolico CEP" (in english "CEP Symbolic Programming Language"). This language, like the previous one, is really close to machine-level, with the main difference being the possibility to use mnemonics for declaring instructions, pseudoinstruction, addresses and parametric cells. [[9](../0_Additional_resources/0_reference.md)]

Its syntax follows the format of CEP's instructions. It allows:     
- to refer to instructions and pseudoinstruction as mnemonics.
- to define symbolic references for memory locations pointing to instructions location, parameters and data. [[7](../0_Additional_resources/0_reference.md)] 

This language is mostly used for writing pseudoinstruction and programs focused on optimization. [[7](../0_Additional_resources/0_reference.md)] 

### CEP Checker Programming Language (LPCC)
LPCC, which stands for "*CEP Checker Programming Language*" (from the italian "*Linguaggio Programmativo Correttore CEP*"), is a special programming language used for writing checking routines. The language permitted to declare numbers both in base-10 and base-8. [[4](../0_Additional_resources/0_reference.md)]

### FORTRAN Origin Programming Language (LPFO)
LFPO, which stands for "*Fortran Origin Programming Language*" (from the italian "Linguaggio Programmativo Fortran Origin"), corresponds to the FORTRAN II language developed by IBM during the late '50s for the $704$-computer. This language was supported to add compatibility to all programs written in FORTRAN II. [[4](../0_Additional_resources/0_reference.md)]

### FORTRAN CEP Programming Language (LPFC)
LPFC, which stands for *"Fortan CEP Programming Language"* (from the italian *"Linguaggio Programmativo Fortran CEP"*), is an extension of FORTRAN II, introducing additional formats for real quantities, plus the possibility of declaring in expression quantities of different formats.[[7](../0_Additional_resources/0_reference.md)]. There are also some syntactic differences, introduced for parsing proposition more easily during the compilation process. [[4](../0_Additional_resources/0_reference.md)]

Both **LPFC** and **LPFO** are the highest level language available in the CEP *(as of 1963)* and were used for defining scientific programs. [[4](../0_Additional_resources/0_reference.md)]

In **LPFC** are defined highly optimizied library subprograms and pseudoinstruction. [[4](../0_Additional_resources/0_reference.md)] 

### CEP Base Binary Language (LBBC)
We refer to the low level machine language of the CEP as **LBBC**, i.e. "*CEP Base Binary Language"* (from the italian "*Linguaggio Base Binario CEP*"). [[7](../0_Additional_resources/0_reference.md)]

## Group B - Compilers
For each high-level programming language supported, the CEP provided a translator program. This group follows an hierarchy, such that each compilers translates the input code into an equivalent one written for a lower-level language. 

The hierarchy is described by the following graph:

![image](../../resources/cep_language_hierarchy.png)

Each arrow represents a compiler, which receives in input a program in the language at its left, and produces as output code for the language to its right.

Only **TFOC**, **TFSC**, and **TSBC** are real translators, with **ASSGC** and **GEGC** being members of group C. [[4](../0_Additional_resources/0_reference.md)]

### FORTRAN Origin to Fortran CEP Translator (TFCO)

TFCO stands for "*FORTRAN Origin to Fortran CEP Translator*" (from the italian "*Traduttore FORTRAN ORIGIN a FORTRAN CEP*"). As the name implies, it translates code written in FORTRAN Origin into FORTRAN CEP. [[4](../0_Additional_resources/0_reference.md)]

### FORTRAN CEP to Symbolic CEP Translator (TFSC) 
TFSC stands for "*FORTRAN CEP to Symbolic CEP Translator*" (from the italian "*Traduttore FORTRAN a Simbolico CEP*"). It translates code written in FORTRAN CEP into the Symbolic CEP Language LPSC. [[4](../0_Additional_resources/0_reference.md)]

### Symbolic CEP to Binary CEP Translator (TSBC)
TSBC stands for "*Symbolic CEP to Binary CEP Translator*" (from the italian "*Traduttore Simbolico CEP a Binario CEP*"). It translates code written in Symbolic CEP into Binary CEP. [[4](../0_Additional_resources/0_reference.md)]

## Group C
A series of helper routines specialized in organizing the *assemble* (in italian "*assemblamento*") of programs and for translating tapes containing pseudoinstructions.

These programs are:

### CEP General Assembler
The ASSGC, which stands for "*CEP General Assembler*" (from the italian "*Assemblatore Generale CEP*"), is the assembler program responsible for merging all routines into the final binary executable. It also supports writing the final binary program into a magnetic tape. [[4](../0_Additional_resources/0_reference.md)]

It assumes all components of the program have been traduced to machine code or in LPPC prior to the assembly process. [[4](../0_Additional_resources/0_reference.md)]
    
For **LPCC** programs the assembler support their translation. This feature was added mainly for defining more easily checkup routines. [[4](../0_Additional_resources/0_reference.md)]

### Compiler of binary tapes of a group of pseudoinstructions
Used for grouping into a magnetic tape all the routines associated to a group of pseudoistruzioni. [[4](../0_Additional_resources/0_reference.md)]

### CEP General Extractor Loader
From the italian "*Caricatore Estrattore Generale CEP*", it implements the following features:

1. Conversion and loading of programs of single sequence instructions or program constants written in **LPDC**.

2. Extraction of binary tapes of *autoloading* (from the italian "*autocaricamento*") programs. 

3. Perforation or printing of memory content in decimal instructions or octal characters.

It was the first auxiliary program being defined for the CEP. [[4](../0_Additional_resources/0_reference.md)]
## Group D
A series of static and dynamic programs for checking correctness of arithmetic calculations and other kinds of instructions. Static programs check specific memory location, while dynamic programs check the program in execution. Both allow a step-by-step debug of the program, printing information as requested by the user. [[4](../0_Additional_resources/0_reference.md)]

## Group E
A large series of pseudoinstructions necessary for the logical organization the computer, for defining the I/O operations, and for handling subprogram's calls. [[4](../0_Additional_resources/0_reference.md)]