# Linguaggio Programmativo Simbolico CEP [[7](../0_Additional_resources/0_reference.md)]
At its core the Linguaggio Programmativo Simbolico CEP can be though as an extension of the CEP's machine-language, adding support for symbols (i.e. mnemonics) in numerical and alphabetical types.

## Symbolic instructions

LPSC uses symbolic instructions, which share the same structure with their binary equivalent, having (from left to right):

- an operation symbol. A user needs to specify explicitly if the operation should be automatically controlled or not (i.e. if the translator needs to set the control bit to $1$).

- two consecutives parametric addresses, which can be provided both as mnemonics or normally.

    A notation is available for parametric addresses, which is used to distinguish to which parametric group should a symbolic parametric reference belong.

    Assignign of symbolic parametric addresses is done automatically by the service programs.

- one operation address. 

An address can be symbolically represented as an alphanumeric sequence of constants.

It is possible to pass to the symbolic instructions both numeric and alphabetical constants:
- directly, through the teleprinter 

- by reference through a constant table, provided during the assembly phase.

In order to guarantee that a relative symbolic address can be converted through an absolute one using the special cella parametrica $63$, it must be preceded by a convential symbol.

## Additional declarations
During the assembly phase, a program written in LPSC needs to provide the name declarations for list of used subprograms, attributes of a constant table, and attributes of work areas. 

Additionally we need to provide:

- the mapping between names of pseudoistruzioni and groups

- the pseudoistruzioni' groups used in each routine, an

- a list of the names of the arguments passed to each subprogram.

## Constant tables and Work areas [[7](../0_Additional_resources/0_reference.md)]
A constant table stores the constants valid in the associated routines. The table can have an arbitrary number of attributes, given that the programmer provides for each of them its name and dimension.

Work areas are memory areas exclusive to a given set of routines. They are similar to constant tables, having the possibility of containing an arbitrary number of attributes. The main difference is that in a work area, each entry can be dynamically modified by the associated subprograms.

To save memory, two common work areas are available:
- one for pseudoistruzioni.

- one for local variables of subprograms. This area avoids during subprogram's call to transmit constant as parameters.

References to work areas and constant tables can be retrieved through the name associated to one of their attributes, to which we could add some constant.

## Symbolic subprogram call
To call a subprogram using its previously declared name, we just need to append its name to the name of the call-handler pseudoistruzione.

