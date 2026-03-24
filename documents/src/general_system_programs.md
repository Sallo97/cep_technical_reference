#  CEP General System Programs (SPGC) 
SPGC stands for "*CEP General System Programs*" (from the italian "*Sistema di Programmazione Generale della CEP*") and it identifies the base set of support programs written for the CEP. This collection of programs mainly handle the compilation of high-level of code into machine-language and the conversion of numeric constants from and to binary representation.
 [[4](../helper_resources/references.md)]

The CEP low-level machine-language works entirely in binary, making programming for it very difficult. For this reason, the computer supports various high-level programming languages though the use of the **SPGC**. Each service program is small, being dedicated to a particular phase of the translation (e.g. one delagated in translating fixed point numbers from decimal to binary). These services must be invoked each time an high-level program needs them during its execution. The incorporation of service routines into a user's defined program is delegated to a general assembler. [[4](../helper_resources/references.md)]

Most SPGC's programs are written in Fortran-CEP, an extension of Fortran developed especially for the CEP. As of 1963 there were discussion in adding support to the Algol language, with prelimary design decision regarding is translation routines. Sadly, the compiler was never finished. [[4](../helper_resources/references.md)]

# SPGC Organization

The SPGC divides its programs into five main groups.

## Group A - Programming Languages
Identifies the set of programming languages supported by CEP. Most helper programs are written in these higher-level languages. [[4](../helper_resources/references.md)]

### CEP Decimal Programming Language  (LPDC)
The first developed programming language is LPDC, which stands for *"CEP's Decimal Programming Language"* (from the italian "*Linguaggio Programmativo Decimale CEP*"). This language is almost identical to the low level machine language, with the main difference being the possibility to define constants and instructions in base-ten.[[9](../helper_resources/references.md)]

It was the first high-level programming language implemented for the CEP, being reduced for defining simple debugging routines after other more powerful languages were introduced.[[4](../helper_resources/references.md)]

### CEP Symbolic Programming Language (LPSC)
The second developed programming language is LPSC, which stands for  *CEP's Symbolic Programming Language* (from the italian "*Linguaggio Programmativo Simbolico CEP*"). Like the previous one, it is really close to machine-level, with the main difference being the possibility to use mnemonics for declaring instructions, pseudoinstruction, addresses and parametric cells. [[9](../helper_resources/references.md)]

Its syntax follows the format of CEP's instructions, allowing to refer to instruction and pseudoinstructions as mnemonics, and the define symbolic references for memory locations (usually containing instructions), parameters and data. This language is mostly used for writing highly optimized pseudoinstruction and programs. [[7](../0_Additional_resources/0_reference.md)] 

### CEP Checker Programming Language (LPCC)
LPCC, which stands for "*CEP Checker Programming Language*" (from the italian "*Linguaggio Programmativo Correttore CEP*"), is a special programming language used for writing debug routines. The language permitted to declare numbers both in base-10 and base-8. [[4](../helper_resources/references.md)]

### FORTRAN Origin Programming Language (LPFO)
LFPO, which stands for "*FORTRAN Origin Programming Language*" (from the italian "Linguaggio Programmativo FORTRAN Origin"), corresponds to the FORTRAN II language developed by IBM during the late '50s for the $704$-computer. This language was supported to add compatibility to all programs written in FORTRAN II. [[4](../helper_resources/references.md)]

### FORTRAN CEP Programming Language (LPFC)
LPFC, which stands for *"FORTRAN CEP Programming Language"* (from the italian *"Linguaggio Programmativo FOTRAN CEP"*), is an extension of FORTRAN II, introducing additional formats for real quantities, plus the possibility of declaring within expressions quantities of different formats.[[7](../0_Additional_resources/0_reference.md)]. 

Compared to FORTRAN II, there some syntactic differences, introduced for parsing proposition more easily during the compilation process. Both **LPFC** and **LPFO** are the highest level language available in the CEP and were used for coding scientific programs and msot of the library subprograms and pseudoinstructions. [[4](../helper_resources/references.md)]


### CEP Base Binary Language (LBBC)
We refer to the machine-level language of the CEP as **LBBC**, i.e. "*CEP Base Binary Language"* (from the italian "*Linguaggio Base Binario CEP*"). [[7](../helper_resources/references.md)]

## Group B - Compilers
Identifies the suit of programs specilized in translating code for each of the high-level programming language supported. These languages follow an hierarchy, such that each compiler translates the input code into an equivalent one written for a lower-level language.

The hierarchy is described by the following graph:

![image](../../resources/cep_language_hierarchy.png)

Each arrow represents a compiler, which receives in input a program in the language at its left, and produces as output code for the language to its right.

Only **TFOC**, **TFSC**, and **TSBC** are real translators belonging to the group, with **ASSGC** and **GEGC** being members of group C. [[4](../helper_resources/references.md)]

### FORTRAN Origin to FORTRAN CEP Translator (TFCO)
TFCO stands for "*FORTRAN Origin to FORTRAN CEP Translator*" (from the italian "*Traduttore FORTRAN ORIGIN a FORTRAN CEP*"). As the name implies, it translates code written in FORTRAN Origin into FORTRAN CEP. [[4](../helper_resources/references.md)]

### FORTRAN CEP to Symbolic CEP Translator (TFSC) 
TFSC stands for "*FORTRAN CEP to Symbolic CEP Translator*" (from the italian "*Traduttore FORTRAN a Simbolico CEP*"). It translates code written in FORTRAN CEP into the Symbolic CEP Language LPSC. [[4](../helper_resources/references.md)]

### Symbolic CEP to Binary CEP Translator (TSBC)
TSBC stands for "*Symbolic CEP to Binary CEP Translator*" (from the italian "*Traduttore Simbolico CEP a Binario CEP*"). It translates code written in Symbolic CEP into Binary CEP. [[4](../helper_resources/references.md)]

## Group C
A series of helper routines specialized in organizing the *assemble* process of the final executable, merging routine programs and pseudoinstructions into the written code.

### CEP General Assembler
The ASSGC, which stands for "*CEP General Assembler*" (from the italian "*ASSemblatore Generale CEP*"), is the assembler program responsible for merging all routines into the final binary executable. It also supports writing the final binary program into a magnetic tape. Before execution, it assumes all code has been traduced into either machine code or LPPC. The support for the LPPC language was added to facilitate writing checkup routines. [[4](../helper_resources/references.md)]

### Compiler of binary tapes of a group of pseudoinstructions
Used for grouping into a magnetic tape all the routines associated to a group of pseudoinstructions. [[4](../0_Additional_resources/0_reference.md)]

### CEP General Extractor Loader
From the italian "*Caricatore Estrattore Generale CEP*", it was the first auxiliary program being defined for the CEP and offers [[4](../helper_resources/references.md)]:

1. Conversion and loading of programs of single sequence instructions or program constants written in **LPDC**.

2. Extraction of binary tapes of *autoloading* (from the italian "*autocaricamento*") programs. 

3. Perforation or printing of memory content in decimal instructions or octal characters.


## Group D
A series of static and dynamic programs for checking correctness of arithmetic calculations and other kinds of instructions. Static programs check specific memory location, while dynamic programs check the current machine-state. Both allow a step-by-step execution of the program, printing debug information as requested by the user. [[4](../helper_resources/references.md)]

## Group E
A large series of pseudoinstructions necessary for the logical organization of the computer. Mainly defines I/O operations and the management of subprogram's calls. [[4](../helper_resources/references.md)]