# Fortran CEP
The Fortran CEP is a dialect of Fortran made during the CEP development. It differs from the common Fortran II mainly for [[6](./0_reference.md)]:

1. It extends the variety of modes for real quantities.

2. It allows the addressing of a greater number of input-output equipments.

3. Removes the limitations on the list of quantities trasmitted between the magnetic core memory and the drum of the magnetic tape units.

A program in Fortran II should be compiled by the Fortran CEP compiler  

## Program syntax of Fortran CEP [[6](./0_reference.md)]
A program in Fortran CEP is a sequence of statements.
Legal characters in the language are:

- Capital letters of the English alphabet.
- The decimal digits.
- The special characters: $+ \ * \ - \  / \ ( \ ) \ . \ : \ , \ =$
- the *blank* character.

Each statement of a program in punched into a portion of a paper tape with a maximum of $60$ characters. Actually is possible to extends the limit of characters for a statement to $660$, by dividing each portion by using the sequence $\# \gamma \ \nu \#$. 

The beginning of a statement is signed by $\#$, while the end by the pair $\gamma \ \nu$. The beginning statement character $\#$ can be preceded by a statement number between $0$ and $10^5$ or by a letter followed or not by a statement number up to a maximum of six characters.

The statements must be punched in the same order as they are in the program.

Blank characters are considered only when typing alphanumeric field, otherwise they are ignored.

## Quantities [[6](./0_reference.md)]
A quantity is a constant, a variable a subscripted variable, or a function. Generally the mode (i.e. type) of a constant is described by a *mode indicator*. The *mode indicator* can be putted adjacent to the starting statement character $\#$ either to its right or to its left. The indicator is composed of a single letter and can be followed or not by the character $:$. 

Some *mode indicator*s are:

- $B$: indicates *boolean quantities*.
- $V$: floating point single precision quantities with an absolute value equal to zero or included between $10^{-38}$ and $10^{+38}$.
- $W$: floating point double precision quantities with an absolute value equal to zero or between $10^{-4932}$ and $10^{+4932}$.
- $S$: fixed point single precision quantities in the interval $-1$ and $1$.
- $D$: fixed point double precision quantities in the interval $-1$ and $1$.
- $J$: integers in the range $2^{-35}$ and $2^{35}$.

Other mode indicators needs to be introduced by the MODE statement, requiring to work a suitable set of pseudoinstructions which implements their operations. 

Note that a mode indicator has an unique meaning in both the main program and its subprograms.

## Constants [[6](./0_reference.md)]

We refer to decimal digits by $\delta$ and $\omega$ for octal digits. 

In a program we can define:
- Integer constants in the form: $\delta\delta\delta\ldots\delta$ 

    where the sign is optimal and the maximum number of digits is $5$.

- booleans in the form: $\omega\omega\omega\ldots\omega$

    where the maximum number of digits is $12$.

- floating point (both single precision and double precision) in the form:

    $+/- \delta\delta\delta\ldots\delta.\delta\delta\delta\ldots\delta$

    The sign symbols are optionals. For single precision floats the maximum number of digits is $8$, whereas in double precision are $16$.

- fixed point (both single and double precision) in the form:
    - if the value is in (-1, 1) :
    $+/- . \delta\delta\ldots\delta$ if the value is 
    - if the value is -1: $-1.000\ldots0$

    For single precision the maximum number of digits is $10$, whereas for double precision in $20$. 

    In the $-1$ case the zeroes can be omitted.

## Variables [[6](0_reference.md)]
Variables are names composed of letters and digits and they may be integers or non-integers. Integers are assumed between the range $2^{-35}$ and $2^{35}$.

A non-integer variable can express any constant expressible in variable mode.

It is possible to assign to a variable a matrix, containing either intergers or non-integers values. The type (integer or non-integer) of the matrix depends on the type of the values it contains. The indices of the matrix are called *subscripts* and they may be at most three.
Matrix elements are called *subscripted variables*. To refer to one element, use the name of the matrix, followed by a parenthesis in which are enclosed the subscripts, separated by commas.

## Functions [[6](./0_reference.md)]
Fnunctions are divided into library, arithmetic statements and Fortran functions.

A function is called by naming it in an arithmetic or boolean expression, following the name with inside parenthesis its arguments. 

Only one result can be returned by a function call. The result can me an integer or a non-integer.

### Library functions
These functions are predefined as closed subroutines which may be on library paper tapes or magnetic tapes.

There are $26$ library functions.
<!-- As of the paper 1963-IS-003_0 -->

![image](../resources/library_functions.png)

### Arithmetic statement functions
These function are defined by the statement $F = E$ where $F$ is the name of the function followed by parentheses, and $E$ is either an arithmetic or boolean expression.

Each argument of $F$ must be a non-subscripted variable. $E$ must not contain subscripted variables, but may use variables, library function, and function defined in the preceding statements.

Function $F$ assumed the mode of the expression $E$.

An arithmetic statement is represented in the form $V = E$
where $V$ is a generical variable and $E$ and expression. The symbol $=$ does not indicate equality between the two sides, rather an assignment between the two.

- $V$ is a generical real variable and may be preceded immediately by a mode indicator to specify its mode.

- $E$ is an arithmetic expression with a mode equal or different to $V$, as long as one of the two has mode $J$ and the other $S$ or $D$.

If the mode of $E$ is expressed in a mode within the expressability of $V$, then $E$ is interpreted as $V$, otherwise it is assigned the best approximated value of $E$ within the limits of $V$.

### Control statements

- UNCONDITIONAL GO TO, COMPUTED GO TO, ASSIGNED GO TO and ASSIGN.

- IF where the expression may be arithmetic or boolean.

- SENSE LIGHT, IF (SENSE LIGHT), IF (SENSE SWITCH), IF ACCUMULATOR OVERFLOW, IF QUOTIENT OVERFLOW, and IF DIVIDE CHECK.

- DO.

    Note that no IF-type or GO TO-type  transfer is allowed into the range of a DO from outside its range. The last statement in the range of a DO must not be one of the following statements: FORMAT, DIMENSION, EQUIVALENCE, COMMON, FREQUENCY, and MODE.

- CONTINUE, PAUSE, and STOP.

- END
    It is used only to signal the Fortran CEP translator that the end of the program has been reached.
    END must always be the last statement of a program and must never be omitted.

### Subprogram statements
A subprogram can call any other subprograms except itself and the one who called it.

A FUNCTION subprogram may be referred to by an arithmetic expression or a boolean type which contain the name of a Fortran function. a SUBROUTINE subprogram may be referred only by a CALL statement.

### Function statement
In a FUNCTION statement, each dummy argument may be a name of an non-subscripted variable, the name of a SUBROUTINE subprogram, the name of a Fortran function, or the name without the terminal $F$, of a library function.

The mode of a function referred to is that belonging to the name of it in the body of the FUNCTION subprogram.

The mode of a dummy argument is that belonging to it in the body of the FUNCTION subprogram.

A dummy argument, which is the name of a matrix, must appear in a DIMENSION statement with the same dimension and mode of the name matrix that correspond to the actual argument.


### Subroutine statement
In a SUBROUTINE statement, the mode of a dummy argument is that belonging to it in the SUBROUTINE subprogram. A dummy argument, which is the name of a matrix, must appear in a DIMENSION statement.

### Call
The actual arguments are set as those of the FORTRAN functions.

For example:
``` Fortran
CALL RIS(SIN, X, A, B, 6HRESULT)
CALL SPUR (C, 50, D: T) 
CALL QUIBUS (B:G*E+F, W:253.256764357856, LOGW, S: A+B/J, X1)
```

### Subroutine names as arguments
A list of names of Fortran functions, of library functions without the terminal $F$ and of SUBROUTINE subprograms, which are actual arguments of FUNCTION or SUBROUTINE subprogram, must be punched with the letter $F$ before the beginning starter $\#$.

Such a list may be punched between any two statements of a program containing references to the FUNCTION and SUBROUTINE subprograms.


Example:
```Fortran
F # SIN, COS, MAP
```

### Input/Output statements
Used to trasmit information during program execution between the magnetic core memory and an external memory device such as magnetic drum, magnetic tapes, paper tape readers, high speed punches, online typewriters and online printers.

#### List of quantities
Some input/output statements contain a list of quantities to be transmitted. In the list several mode indicator may appear, each of which may be introduced before any list element and also before any variable which is not any part of the subscript or of an indexing information.

A mode indicator defines the mode of each non-integer variable following it, until another mode indicator is reached.

To pass an entire matrix it is only required to write its name.
The elements of the matrix are stored in order of increasing storage location.

### Fortran CEP
Same function offered by Fortran II, only following the rules of CEP's functions.

## Actual arguments of a function and modes
The actual argument of a Library or arithmetic statement functions is an arithmetic expression or a boolean depending on the type of the function.

- An arithmetic expression.
- A boolean.
- A matrix name.
- The name of a Library function without the terminal $F$.
- The name of a Fortran function.
- The name of a subroutine program.
- A Hollerith field.

## Expressions [[6](./0_reference.md)]
An expression can be either arithmetic or boolean. In is composed of operands (constants, generical variables or functions), operators (arithmetic or logical), and parenthesis according to the respective formation rules.

Since an expression can be an argument of a function contained in a longer expression, we will refer to the one passed as argument as a *argument expression*, whereas the latter as a *total expression*.

### Arithmetic expressions
Arithmetic operators are $+ \ - \ * \ / ** $. The last one denotes exponentiation. Operands can be integers or  not.

In a division, every dividend must lay on a line with its divisor.

In a exponentiation, every base must lay on a line with its exponent.

Two operators cannot appear consecutively.


#### Hierarchy of operators

- first: $**$
- second: $*$ and $/$
- third: $+$ and $-$

For every expression not following this hierarchy, the default behavior is an evaluation from left to right.

For example the expression $A ** B  ** C$ will be evaluated as $(A ** B) ** C$.


#### Operands and modes
The mode of an expression (both total or argument) must be specified by the suitable mode indicator immediately preceding the expression. If not specified, every component (non-integer constants, generical variabels and functions) is suppose to share the same mode as the expression.

It is not necessary to specify the mode of a integer total arithmetic expression.

![image](../resources/operands_and_modes_examples.png)

#### Operations and modes
The evaluation if an arithmetic operation is considered an integer only if both operands are integers, otherwise the mode depends of the operands whose mode differs.

The result of the division of an integer quantity by another is the integer part of the real quotient.

The addition and subtraction between two operands $A$ and $B$ where $A$ is an integer and $B$ is a fixed point (either single precision or double precision) is not valid. Also for the division the same rule apply.

### Boolean expressions
Boolean expressions define rules similar to those of arithmetic expressions.

The following may be interpreted as boolean expressions:
- A positive octal constant with no more than $12$ digits.
- A generical variable with name indicating a non-integer mode.
- A function with a name indicating a non-integer mode.

If $\beta_1$ and $\beta_2$ are interpretable as boolean expressions and if the first character of $\beta_2$ is not the symbol $-$, then the following are intepretable as boolean expressions:

- $- \beta_2$
- $\beta_1 * \beta_2$
- $\beta_1 + \beta_2$
- $(\beta_1)$

where the arithmetic symbols $- \ * \ +$ are interpreted as:

- $- \rightarrow \lnot$
- $* \rightarrow \land$
- $+ \rightarrow \lor$

Additionally an expression can be forced to be interpreted as boolean if it is prepended by mode $B$.

#### Hierarchy of operators

- first : $-$
- second: $*$
- third: $+$

Again, if the hierarchy is not deterined by these rules, it is evaluated from left to right.

#### Operations
All logical operations are performed upon the full $36$-bit words.


## Format

### Unit record
A unit record may be:

1. A printed line with a maximum of $68$ or $102$ characters depending on whether the output produced by a typewriter or printer.

2. A portion of punched paper tape ending in the form $\gamma \ \nu$ of at most $68$ characters.

3. A record of a BCD magnetic tape with a maximum of $132$ characters.

### Numerical fields
The conversion available are showed here:
![image](../resources/numerical_conversion.png)

### Repetition of field format, repetition of group 
A FORMAT statement may include up to three levels of parentheses.

For example the following statement is allowed:
```Fortran
FORMAT (2(3(2(F14, 7, E16.6), I3/) , 3E20.8//))
```

### Data input at execution time
The blank characters in numerical fields are ignored. The numbers, controlled by the numerical specification of type I, are not treated modulo.

Relaxation of input data format, is permitted in any precision.

Numbers effected by writing errors or magnitude errors will not be converted and the computer will stop signalling the error type.

### Read
The READ statement has the general form $U/R, L$ where:

- $U$ is a positive integer constant or an integer variable which specifies the number of the paper tape reader to be used. $U$ may assume the values $1, 2, 3$.

- $R$ is the number $0$ or a reference to a FORMAT statement, made by means of a statement number or a matrix name. If set to $0$, reading is not done under the control of the FORMAT statement; otherwise reading is controlled by the FORMAT statement. 

In the first case the variables of the list may correspond only to numerical data and ended by blank characters and/or by the pair of characters $\gamma \ \nu$.

- $L$ is the list of quantities.

The READ statement elicits the reading and (if necessary) conversion of data punched on paper tape, up to the end of the list.


### Read Input Tap, Read Tape, Read Drum, Print, Write, Output Tape, Write Tape and Write Drum

The magnetic tape units are numbered from $1$ to $8$. Note that this does not mean that there are $8$ magnetic drums, rather the drum memory space is divided into $8$ regions.

### Punch

A PUNCH statement has the general form $PUNCH \ U/R, L$ where:

- $U$ is a positive integer constant or an integer variable that specifies the number of the high speed punch to be used.

- $R$ is the reference to a FORMAT statement, made by means of a statement number or matrix name.

- $L$ is a list of quantities.

The PUNCH statement elicit (if necessary) the conversion of the passed list arguments and the punching of the quantities on a paper tape.

### TYPE

A TYPE statement has the general form $TYPE \ R, L$ where:

- $R$ is the reference to a FORMAT statement, made by means of a statement number or matrix name.

- $L$ is a list of quantities.

The TYPE statements causes (if necessary) the conversion of the passed list arguments and the printing of them on an online typewriter.

### Equivalence and Common
In an EQUIVALENCE statement the subscripted variable $C(p)$ with $p > 0$ means the $(p-1)_{th}$ location after the higher-order word of the variable $C$ or the first element of the matrix $C$. If $p$ is not specified it is taken to be $1$.

The COMMON area is assigned beginning at location $8000$ and continuing downward in the main memory. The elements of each matrix are stored in order of increasing storage locations.

Dummy variable names placed in COMMON statement to force correspondence in main memory locations between two variables which otherwise will occupy different relative positions

### Frequency
May be used but is simply ignored.

### Mode
MODE is a statement which has the general form $MODE \ M_1, M_2, \ldots, M_n$ where each $M_i$ is a letter in {$G, K, L, M, N, P, Q, R, T, U, Y$}, followed by a positive integer constant between parentheses. 

Every letter indicates the real mode of which the integer constant, following it, makes explicit the number of words, a quantity occupies under that node.

The MODE statement introduces real modes and must precede the other statements in which they appear.

### Comments
are recognized by putting a $C$ before the beginning character $\#$.

### Hand-coded subprograms
Subprograms written in symbolic language LPSC (Linguaggio Programmativo Simbolico CEP) may be called by a FORTRAN CEP program as either a subprogrm, function or subroutibe, provided that it is coded in a suitable way.