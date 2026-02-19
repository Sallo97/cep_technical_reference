# Programming Languages [[9](../0_Additional_resources/0_reference.md)]
The CEP supported a variety of high-level programming languages. For executing them it offers special programs for their compilation and an assembler to generate the final binary executable. Additionally are available some helper programs for debugging.

### CEP Decimal Programming Language (LPDC) 
The first developed programming language is LPDC, which stands for "Linguaggio Programmativo Decimale CEP" (in english "CEP Decimal Programming Language"). This language is almost identical to the low level machine language, with the main difference being the possibility to define constants and instructions using the decimal base.

### CEP Symbolic Programming Language (LPSC)
The second developed programming language is LPSC, which stands for "Linguaggio Programmativo Simbolico CEP" (in english "CEP Symbolic Programming Language"). This language, like the previous one, is really close to machine-level, with the main difference being the possibility to use mnemonics for instructions, pseudoinstruction, addresses and parametric cells.

### CEP Checker Programming Language (LPCC) [[4](../0_Additional_resources/0_reference.md])]
LPCC, which stands for "Linguaggio Programmativo Correttore CEP" (in english "CEP Checker Programming Language"), is a special programming language used for writing checking routines. The language permitted to declare numbers both it decimal and octal base.

### Fortran Origin Programming Language (LPFO) [[4](../0_Additional_resources/0_reference.md)]
LFPO, which stands for "Linguaggio Programmativo Fortran Origin" (in english "Fortran Origin Programming Language"), corresponds to the FORTRAN II language developed by IBM during the late '50s for the $704$-computer. This language was introduced to add compatibility to all programs written in FORTRAN II.

### Fortran CEP Programming Language (LPFC)
LPFC, which stands for "Linguaggio Programmativo Fortran CEP" (in english "Fortran CEP Programmig Language"), is an extension of FORTRAN II, introducing additional formats for real quantities, plus the possibility of placing in expressions and lists quantities of different formats.

## Compilers
For each high-level programming language supported, the CEP provided a translator program. These programs follow an hierarchy, translating the input code into an equivalent one written for a lower-level language. 

The hierarchy is described by the following graph:

![image](../../resources/cep_language_hierarchy.png)

Each arrow indicates the translator program used for translating from the left higher-level language into the right lower-level language.

The compiler programs are:

- **Fortran Origin to Fortran CEP Translator (TFCO)**: translates code written in Fortran Origin into Fortran CEP. 

- **Fortran CEP to Symbolic CEP (TFSC) Translator**: translates code written in Fortran CEP into Symbolic CEP.

- **Symbolic CEP to Binary CEP (TSBC) Translator**: translates code written in Symbolic CEP into Binary CEP.

- **ASSGC**: assembler program.

## Assembler
The ASSGC, which stands for "Assemblatore Generale CEP" (in english "CEP General Assembler"), is the assembler program, responsible for merging all routines of the compiled program to execute.


## Control Programs
The CEP offered a suite of control programs for checking correctness during an execution. These programs are:

- A program which prints partial results, instruction per instruction.

- A program which prints intermediate results in specified program points.

- Some programs for printing the state of the main memory.

- Some programs for automatically checking possible overflows and for checking the correctness of I/O operations.
