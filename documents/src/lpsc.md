<!-- ==================================== I'M HERE ================================== -->

# CEP Symbolic Programming Language (LPSC)
LPSC stands for *CEP's Symbolic Programming Language* (from the italian "*Linguaggio Programmativo Simbolico CEP*) and at its core is an extension of the CEP's machine-langauge, adding support for declaring numerical and alphabetical types using mnemonics (i.e. symbols). [[7](../helper_resources/references.md)]

## Symbolic instructions
Programs in LPSC are written as a sequence of symbolic instructions. They share the same structure as their binary equivalent, permitting the user to specify its various components as mnemonics.

- The **automatic check flag** must be specified explicitly by the user (i.e. if the translator needs to set the control bit to one or not).

- The **operative code** can be represented as a mnemonic. 

- The two consecutives **parametric addresses** can be provided both as mnemonics or normally. A notation is available for parametric addresses, which is used to distinguish to which parametric group should a symbolic parametric reference belong. Assignign of symbolic parametric addresses is done automatically by the service programs.

- The **operand address** can be symbolically represented both as an alphanumeric sequence or a sequence of constants.

The symbolic instruction can be passed either through the teleprinter ora by reference using a constant table, provided during the assembly phase. The conversion from a relative symbolic address to a unique absolute one is done by using the $63$-th parametric cell (i.e. the last member of group $\{H^{1}\}$). In order to guarantee the conversion, it must be preceded by a conventional symbol. [[7](../helper_resources/references.md)]

## Symbolic tables
To generate a binary executable from a program written in LPSC, the assembler needs to know the map between the symbolic names and their associated constructs in the code. [[7](../helper_resources/references.md)]

- A map between symbolic names and subprograms.
- The **constant table**, i.e. the map between constants and their binary equivalent.
- Attributes of the work areas (?).
- The map between symbolic names and pseudoinstructions.
- The map between symbolic names and groups of pseudoinstructions.
- The list of pseudoinstruction groups used in each subprogram.
- The map between the symbolic names of the arguments passed to each subprogram and their binary equivalent.

## Constant table
Each routine has its own constant table, specifying its valid constants. There could be an arbitrary number of attributes, given that the programmer provides for each of them its name and dimension. [[7](../helper_resources/references.md)]

## Work areas 
Work areas are regions of memory dedicated to a given set of routines. Structure wise, they are similar to constant tables, having an arbitrary number of attributes. The main difference is that in the work area, each entry of the table can be dynamically modified by one of its subprograms. [[7](../helper_resources/references.md)]

To save memory space, two common work areas are declared by default. the first is for keeping track of all available pseudoinstructions, the second is for specifying the local variables of each available subprogram. [[7](../helper_resources/references.md)]

## Symbolic subprogram call
To call a subprogram using its previously declared name, we just need to append its name to the name of the pseudoinstruction call-handler. [[7](../helper_resources/references.md)]