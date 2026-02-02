# Traduttore Fortran CEP [[7](../0_Additional_resources/0_reference.md)]
Recall that the language FORTRAN CEP is an extension of the first iteration of FORTRAN, with the main difference being the possibility of writing arithmetic expressions having numbers in any representation.

The compilation procces is divided in two steps. 

## First step - Traduttore Fortran I

### Overview
The first phase, called Traduttore Fortran I, translates all FORTRAN's instructions into *"linguaggio contratto"*, an intermediate representation. Additionally, it also constructs numerous help tables. 

These tables will be used for computing addresses, quantities, and to keep useful information necessary for the second phase. 
    
The phase ends as soon as we reach, for all declared programs and subprogram, the special `END` proposition.

In total, this initial phase utilizes $46$ subprograms and $17$ distinct tables to manage the program state.

### Implementation [[7](../0_Additional_resources/0_reference.md)]
The compiler operates under the assumption that the FORTRAN source program is accessible via the attached punched tape reader. The process begins with a call to the *"inizializzazione"* subprogram, which initialize some counters.

Subsequently, the *"letture"* program transfers the next instruction from the punched tape into main memory. During this transfer, the routine performs a preliminary transformation of the instruction to for facilitating its scanning:

If the statement is a declaration, but not an arithmetic statement function or the special instruction `END`, then it is translated into table entries and/or symbolic declarations. [[8](../0_Additional_resources/0_reference.md)]

If the statement is of executable type or an arithmetic statement functon or `END`, it is transalted into a string of intermediate statements and, when it is the case, into table entries. [[8](../0_Additional_resources/0_reference.md)]


the compiler evaluates the operation code $XXX$:

- If the opcode corresponds to the `END` instruction, the system invokes the $\pi/ASIND$ subprogram to finalize the current compilation phase and transition to the next. During this finalization, the compiler references data stored in the auxiliary *"help tables"* to compute and assign memory addresses to all variables and quantities utilized throughout the program.

- Otherwise, control is dispatched to the specific subprogram delegated to that opcode, designated as $\pi/XXX$.

    The traslation subprogram must be aware if the current instruction is within a `DO` loop. If an instruction resides within a cycle, the compiler must invoke the $\pi/CHDO$ subprogram. This routine is responsible for generating the *"istruzione contratta"* (contracted instruction) required to close the loop and for defining the boundary constraints for matrix index combinations accessed within that scope.

### Intermediate Representation Structure
A statement in *"linguaggio contratto"* has the single variable, the matrix elements, the **Hollerith** arguments and noninteger constants are replaced by internal names which specify their kind and relative address in the corresponding tables. [[8](../0_Additional_resources/0_reference.md)]

### Translation of expressions [[8](../0_Additional_resources/0_reference.md)]

Expressions and the input/output lists are compiled from FORTRAN CEP to the *"linguaggio contratto"*.

Each expression is scanned from left to right and the compilation is done only when, according to the well known operator hierarchy, an operator with precedence lower than the last one met is found. The generated intermediate expressions and formular are without parentheses.

These generated expressions can be of three kinds, having all the same hierarchy number except the **unary minus**:

- **First kind:** contains expressions where where appears at leat one operation sign.

- **Second kind:** indefies library or arithmetic statement function names. 

- **Third kind:** are FORTRAN function names.

A mode indicator is determined by the kind of the expression, as it indicates if the variable is a integer/noninteger variable or a function.

### Translation of Lists [[8](../0_Additional_resources/0_reference.md)]
In the context of the CEP FORTRAN compiler the term "list" **does not refer to a data structure**, but to a **syntactic sequence** of variables and expressions that the compiler must process in a row.

For example in a function call `FOO (X, Y, Z)` the variables inside the parenthesis define an **argument list**. In general every collection of items inside parentheses, regardless of coming from a matrix or a function, define a list for the compiler to scan

A subscript is an element of a list. For example for `M (X, Y, Z)` the elements `X`, `Y`, `Z` are subscripts.

Parentheses used within a subscript do not define a list, so the compiter ignores them during the translation process. For example in the statement `A( (X + Y), Z)` the parentheses `(X + Y)` do not define another list, just a subscript.

A list is scanned from left to right and for each left parenthesis a `DO` is generated. Indexing information regarding the loop are inserted immediately before the matching right parenthesis. The range of the `DO` extends up to that indexing information.

Each list variable is translated into a intermediate statement, having a mode indicator associated to it.

### Translation of Subscripts [[8](../0_Additional_resources/0_reference.md)]
In FORTRAN a **subscripted variable** is a variable whose value is obtained by an array/matrix access.

For example `M(X, Y, Z)` is a subscripted variable and the compiler should be able to to compile them.


Consider the most general form of a subscripted variable $S$:
```
M (a1 * I1 ± b1, a2 * I2 ± b2, a3 * I3 ± b3)
```

Where:

- `M` is an array name.

- `I1`, `I2`, `I3` are integer single variables.

- `a1`, `a2`, `a3`, `b1`, `b2`, `b3` are integer constants without sign.

If the array $M$ has precision $p$, row-dimension $d_1$, column-dimension $d_2$, layer-dimension (i.e. third-dimension) $d_3$, and its elements are stored by columns in increasing locations, then the address of $S$ can be computed as:

$M + p (a_1 \times I_1, \ a_2 \times d_1 \times I_2, \ a_3 \times d_1 \times d_2 \times I_3) \\ \ \ \ \ \  + p \ [\  (\pm b_1 - 1) + (\pm b_2 - 1)d_1 +  (\pm b3 - 1)d_1d_2 \ ]$

which we break down into:

- **variable part:** $p (a_1 \times I_1, \ a_2 \times d_1 \times I_2, \ a_3 \times d_1 \times d_2 \times I_3)$

    Within it we declare the following **multiplicative constants**:
        
    - $p \times a_1$ as multiplicative constant of $I_1$.
    - $p \times a_1 \times d_1$ as multiplicative constant of $I_2$.
    - $p \times a_3 \times d_1 \times d_2$ as multiplicative constant of $I_3$.

- **constant part:** $ p \ [\  (\pm b_1 - 1) + (\pm b_2 - 1)d_1 +  (\pm b3 - 1)d_1d_2 \ ]$

### The `PDO` Special Construct [[8](../0_Additional_resources/0_reference.md)]
In FORTRAN, within a `DO` block there could be a matrix access operation where at least one of the indices is a constant offset.

Consider now the case where we have a matrix access outside of any loop:

```Fortran
B(ROW, COL) = "something"
```

We could interpret it as being inside a `DO` cycle only having one statement as range:

```Fortran
DO I = 0, 1
    B(ROW, COL) = "something"
```

CEP's developers defined the special proposition `PDO`, short for PSEUDO-DO, which defines a `DO` for an index $I$ having only one statement as range, indexing parameters equal to the index, and no statement number in its struction. 

By wrapping each free (i.e. not within a `DO`) subscript variable inside a distinct `PDO`,  we can "force" each matrix access in the code to be within a cycle.

But why we want to threat all matrix accesses as being within a loop? Because **in this way the compiler can treat all matrix accesses using constant offset in the same way, regardless of if they are in a loop or not**.

### Translation of a `DO` [[8](../0_Additional_resources/0_reference.md)]
A `DO` is translated into table entries and into an intermediate statement where the **statement number** of range is replaced by the **serial number** of the `DO` in the source program.

A **statement number** is a logical "bookmark" used to *logically* indentify components of a program (instruction, loops, subprograms). It is equivalent to what modern day prgorammer call **labels**.

A **serial number** in FORTRAN (and early computers in general) was a sequence of digits used to keep the deck of punched cards storing a program in the correct order. 
Because a program could consist of thousands of physical cards, if a tray of cards was dropped or shuffled, the serial number allowed a mechanical sorter or a programmer to restore the original sequence. So it indetifies where *physically* start a component of the program.

When a `CONTINUE` statement occurs or a statement that clases the associated `DO` has been compilerd, then the compiler generates an intermediate statement of closure of the `DO`.

### Translation of a `PDO` [[8](../0_Additional_resources/0_reference.md)]
A `PDO` is translated into table entries and into an intermediate statement which has the same structure as the original one. After the unique statement in the range of the `PDO` has been compiled, an intermediate statement of closure of `PDO` is generated.

### Tables Produced by the first phase

- **MODT table:** Each entry in the table contains a letter denoting a mode and the corresponding precision.

- **SAT table:** The table maps an entry to a defined subprogram such that each entry contains a dummy argument of the `SUBROUTINE`/`FUNCTION`-type subprogram associated.

- **FDT table:** Maps for each arithmetic statement function an entry containing its distinct name.

- **FDAT table:** Contains in each entry the dummy argument of the arithmetic statement function associated.

- **SVT table:** For each single variable it stores in its entry its precision, and further informations when the single variable is index of a `DO`.

- **NET table:** an entry is make in this table for each `DO`/`PDO` of a nest: the relative address of the entry is the same as the level number $l$ that starts from $0$ increasing or decreasing by $1$ at any opening or cloasing of a `DO` or `PDO` of the nest.

    Each entry contains:
    
    1. the statement number or the last statement in the range or a mark respectively.
    
    1. the relative address in **SVT** containing the **index** associated with the `DO`.
    
    1. the serial number $z$.

- **DIMT table:** For each distinct matrix it contains its matrix name and the numbers $p$, $p \times d_1$, $p \times d_1 \times d_2$ , $p \times d_1 \times d_2 \times d_3$, 

    If the matrix is one-dimensional $d_2 = d_3 = 1$, If is two-dimensional $d_3 = 1$.

- **MET table:** The table maps **distinct** subscripted variables into entries. Two subscripted variables formally equal are still distinct if contained in different ranges of `DO`/`PDO` cycles.

    Each entry stores:

    1. The relative address of the DIMT entry containing the name of the array of which the subscripted variable belongs.

    1. The relative address of the VPT entry concerning the subscripted variable.

    3. The value of the constant part of the relative address of the subscripted variable.

- **VPT table:** An entry maps to a **distinct variable part**. A distinct variable part identifies a portion of a subscripted variable's address that changes with every iteration of a `DO`/`PSEUDO` loop. Two variable parts are considered distinct if they have different lists for their $c_1, c_2, c_3, l_1, l_2, l_3, z$.
    
    An entry contains:
    
    1. The multiplicative constants $c_1, c_2, c_3$ of the subscript variables controller by `DO`/`PDO` of levels $l_1, l_2, l_3$ in order of increasing magnitude.

    1. The serial number $z$ of the innermost `DO`/`PDO` containing in its range the subscripted variable. 

        If the matrix is one dimension then $c_1 = c_2 = l_1 = l_2 = 0$; if its two-dimensional then $c_1 = l_1 = 0$.

- **DOT table:** For each `DO`/`PDO` an entry stores:
    
    1. The relative address that specify the zone of VPT containing the variable parts concerning the matrix elements in the range of that loop.
    
    1. marks if the current value of the cycle's index must be saved after a normal exit.

- **SNT table:** For each statement it contains:
    
    1. its statement number.

    1. if the statement labeled by it is in the range of a `DO`, also the elvel of this `DO`.

### Other Translation Operations
- Statement numbers (i.e. unique numeric identifier associated to an instruction) are mapped and stored alongside the corresponding nesting level of the proposition.

- Control flow instructions are expanded into their primitive logical components.

- The compiler identifies and tracks `DO` loops that are eligible for optimization during the subsequent *"asteriscamento"* phase.

## Second step - Traduttore Fortran II

The compiler translates the intermediate representation (*"linguaggio contratto"*) into the Linguaggio Programmativo Simbolico CEP (LPSC). 

Simultaneously, it generates metadata for the Traduttore Simbolico Binario CEP (TSBC), using a new set of auxiliary tables, some of which comes from the previous phase.

In total the second phase uses $40$ subprogram and $19$ table, $14$ of which are in common with the first phase.

### Detailed Translation Operations [[8](../0_Additional_resources/0_reference.md)]

Given a statement in the intermediate representation, its translation into a symbolic statement depends on its kind:

1. If the intermediate statement is of the **first kind**, then the translation is straightforward since very operation once combined with the mode indicator returns the code of the right CEP's instruction/pseudoistruction.

1. If the intermediate statement is of the **second kind**, a search in the FDT table returns the name of the function declared in the statement, determining:

    - if it is an arithmetic statement function, then the program generate a return-jump to the entry of the subroutine performing the evalutation of the function, and a group of instructions to transmit the actual arguments.

    - if it is a library function, the program behaves in the same manner as when it parses an intermediate statement of the **third kind**.

1. If the intermediate statement is of the **third kind**, a group of presudo-instructions is generated for the subroutine call and for the transmission of the actual arguments. 

    For each subroutine, its dummy argument's address is associated to  a parametric cell of a group, in order to eliminate the prologue.

1. If the intermediate statement concerns the **I/O of a quantity**, then:
    
    - If it is an array, a loop on a pseudo-istruction is generated.

    - Otherwise a single pseudo-instruction is generated.


    The generated pseudo-instruction has a code terminating (generally) with a letter, denoting the mode of the transmitted quantity. Additionally inside its macro-program it stores:
    
    - Conversion functions from one numerical system into another.

    - Reading, printing punching the quantity.

    - Transmission between the main and auxilary memory.

1. If the intermediate statement is a `DO`, then are generated the instructions for initializing, testing and (if necessary) calculating the values of the variable parts.

    If the intermediate statment is a `PDO`, then are generated the instructions for initializing and (if necessary) calculating the values of the variable parts.

    The compiler retrieves the **distinct variable parts** of the loop from the VPT table (by first looking for the index of the entry, stored in the DOT table). When scanning each distinct variable part, if the current part has $l_3$ equal to the level of the `DO` or `PDO`, **then it is necessary to compute the value of the variable part**. The computed values are stored in memory cells whose names are memorized in the associated entries of VPT.

1. If the intermediate statement is **a closure of `DO`**, the instructions for advancing and jumping to th test instructions of the `DO` are generated.

    If the intermediate statement is a **closure of a `PDO`**, nothing is generated.

### Mapping of quantities in instructions [[8](../0_Additional_resources/0_reference.md)]

An instruction or a pseudo-instruction which operates on a quantity has the form  `L # C, P, Q, A `, where:

- `L` contains the symbolic location of the instruction or pseudo-instruction.

- `C` contains the symbolic code fo the instructions or pseudo-instruction.

- `P` and `Q` contain the symbolic or number address of a parametric cell. When one of the two is missing, it is substituted by the special character `-`.

- `A` contains a symbolic address, made up by a name followed in case by an integer with sign, or an integer, or the character `-`. 

    An integer constant appears without its sign in `A`; no search over the tables is necessary, the constants are always appear explicitly within the intermediate statement.

    Every other constant, occurring in an expression, is reffer to by its internal name placed in `A`.

    A single variable that is not a dummy argument is referred to by its own name in `A`.
    
    A dummy argument is referred to by its associated parametric cell address, symbolically indicated by the own name of the argument if it is listed in a `FUNCTION`/`SUBROUTINE` statement. A search over SVT and SAT tables allows the variable name used and the field where the name was placed to be retrieved.

    A subscripted variable is referred by:

    1. The name of the associated array, which is in `P` if the matrix is not a dummy argument, otherwise in `A`. 

    1. The value of the variable part in a parametric cell in `Q`.

    1. The number, which expresses the value of the constant part, in `A` afer the array name (if there is one).

    1. The array name, the memory cells where the value of the variable part is stored and the number which expresses the value of the castant part can be retrieved by searching over tables MET, DIMT, VPT.

    1. The name of the array is interpreted as a dummy argument or not depending on a search over SAT.

### Symbolic Representation Structure [[8](../0_Additional_resources/0_reference.md)]
The obtained symbolic statements, when referring to quantities mentioned in the source program, retain the names of those quantities in their addresses. [[8](../0_Additional_resources/0_reference.md)]

### Development curiosity [[8](../0_Additional_resources/0_reference.md)]
During development the Traduttore Fortran CEP was written in a programming language totally independent from the CEP's architecture.

# Main Problems 
During the implementation of the Fortran CEP translator, the main problems tacked during development were:

1. The compilation of arithmetic expressions 

1. The automatic chaining of subprograms.

1. The traslation of cycles within a program in a way that is optimized in respect to the CEP's ability of modifying addresses.

## Compiling arithmetic expressions
The translation of a given arithmetic expression is done in two phases:

1. the expression is unpacked into a sequence of reduced formulas. The produced formulas can be of two kinds:

    - **Arithmetic reduced formulas:** the operation symbols used in each of them shares the same hierarchical number ("numero gerarchico").

    **NOTE:** *I have no idea what an hierarchical number ("numero gerarchico") means. Maybe is the arity of the arguments?*

    - **Functional reduced formulas:** each functional formula is composed of a function's name and a list specifying the name of all its arguments.

    To each reduced formula we map a representation id ("indicatore di rappresentazione), which specifies the type of the variables within.

    The technique used for unpacking a formula is **"compiling and recording"**, which was a pretty stardard practice at the time. The technique optimizes memory by finding for each expression the sub-expression(s) it is composed that have already been compiled.

2. Each reduced formula is translated into symbolic language (i.e. LPSC). For arithmetic reduced formula, its operation sign and representation id can be easily combined to obtain the symbolic code ("codice simbolico") of the associated CEP's symbolic instruction/pseudoistruzione. 

    Search over a *"table"* permits to map the symbolic 
    address to the variables in the reduced formula.

    **NOTE:** *the document does not provide any explaination of what this "table" is.*

    For functional reduced formulas its necessary to distinguish if the function within comes from the library or is defined within the program. 
    
    Regardless, each argument's name of the function will have associated addresses from the local group $H0$ of the subprogram containing the formula. 


### Quick Fortran Notation Refresher
- A **Loop Control Variable** is the variable declared at the start of the loop, which is updated at each iteration.

- a **constant offset** is an expression which can be represented in the general formula $I + const$ where `I` is a loop control variable and `const` is a constant value (for example `5`).

For example in the following code:
```Fortran
DO I = 2, 9
    B(I) = ( A(I - 1) + A(I) + A(I + 1) ) / 3.0
END DO
```

- `I` is the loop control variable
- `I - 1` and `I + 1` are constant offsets.

## Strength Reduction Optimization
An optimization technique used by the compiler is **"Strength Reduction"**, also used for computing the memory addresses of matrices.

First recall that the indices of a matrix determine the associated entry, assuming that the matrix is implemented as a contiguos array, as:

`A(I, J) = I * NUM_OF_COLUMNS + J`


The bottleneck of the formula above is the multiplication `I * NUM_OF_COLUMNS`, which is expensive. We want to finda way to substitute it with a series of more simple additions.

### Constant multiplicative factors
A **constant multiplicative factor** (also called a **stride** or a **multiplier**) is a constant value being multiplied inside an expression. 

Consider the case of determining the entry of a matrix using the row and column indices, the formula is `A(I, J) = I * NUM_OF_COLUMNS + J`, so the constant multiplicative factor is `NUM_OF_COLUMNS`.

### Distributing DO cycles into levels
Let us consider a simple program that reads all elements within a 5 x 10 matix:

```FORTRAN
DO ROW = 1, 5
    DO COL = 1, 10
        A(ROW, COL) = something
```

The scan is implemented by two cycles, such that:

- The innermost cycle is the one which is updated more often ($10 \times 5 = 50$ times in the example) and **usually** does not require multiplications.

    For each iteration over a row, we do a column cycle. In one of these column cyles, for computing the entry `A(ROW, COL)`, only `COL` is updated at each iteration. 
    
    `ROW` stays the same, making `COL_OFFSET = ROW * NUM_OF_COLUMNS` a constant. This means we only need to compute the sum:
    
    `COL_OFFSET + COL`

- The outermost cycle is updated less frequently ($5$ times in the examples), but each time we update `ROW`, we need to update `COL_OFFSET = ROW * NUM_OF_COLUMNS`. Since `ROW` is incremented by $1$ at each iteration, we can compute the result inductively:

    $ 
    Col\_Offset_{i} = Row_i \times NUM\_OF\_COLUMNS \\
    Col\_Offset_{i+1} = Col\_Offset_{i} + NUM\_OF\_COLUMNS
    $

    Reducing an expensive multiplication into a simple addition.

This reasoning can be generalized to any series of nested `DO` cycles in which there are computations dependent over the loop control variables.


### Storing precomputed results into tables
The previous approach assumed that we keep somewhere the constants of each `DO` cycle. The compiler stores them in a table, often called **Symbol Table** or **Offset Table**.

The filling works as follows:

1. Analize the level hierarchy of the `DO` cycles.

1. Checks which indices contains loop control variables.

1. Precompute the necessary constant for each level and stores them in the Symbol Table.

### Asteriscamento delle proposizioni Do
First, some notation:

- Each matrix access is indicated as $M(x_1, x_2, x_3)$, with $x_1, x_2, x_3$ being the **indices of the matrix**. 

- Each index con be represented in the general expression:
    
    $\forall k \in \{1,2,3\}. x_k = C_k \times I_k \pm D_k$ where:

    - $C_k$ is the **number of columns**.
    - $D_k$ is the **column's index**.

    **NOTE**: here I'm assuming, the document just state the expression without explicitly specifying which parameter does what.

- $I_1, I_2, I_3$ denotes the name of a **loop control variable** and:

    $\forall k \in \{1,2,3\}. (I_k) = $ the content of $I_k$.

- With {$a_1, a_2, a_3$} we refer to the **constant multiplicative factors** of respectively $I_1, I_2, I_3$.

- With $I_a$ we refer to the **global constant additive factor** of matrix $M$, where $a$ is obtained as:

    $a = a_1 \times (I_1) + a_2 \times (I_2) + a_3 \times (I_3)$



The CEP's FORTRAN translation phase dedicated in implementing **Strength Reduction** was called "Asteriscamento delle proposizioni Do".

The phases were:

1. The translator adds `PSEUDO-DO` cycles for each matrix access not within a loop.

1. Phase two is the most complex and is subdivided in subphases:
    1. For each `DO` loop it creates all possible indices combinations for matrix accesses that will be done within.
        
        In practice the number  of combination we can consider for each loop depends on the number of free celle parametrice in $H0$ not occupied by parameters.


    1. Determine the constant multiplicative factors of each possible index, storing them in a special table. The organization of the table (i.e. the order of its entries) depends on the level of the `DO` or `PSEUDO-DO` loop.

    1. To each matrix access $M(x_1, x_2, x_3)$, we map a symbolic address, which can be determined in two ways:

        1. If $M$ is a matrix's name which is not a received as parameter:
            | P Field  | Q Field | Address Field |
            | -------  | ------- | ------------- |
            | /        | $, n$   | $M + \beta$   | 


        1. If $M$ is a matrix's name which is received as parameter.
            | P Field  | Q Field | Address Field |
            | -------  | ------- | ------------- |
            | $0, \mu$ | $0, n$  | $\beta$       |

            where:
            - $0, n$ refers to the address in group $H0$ containing the number $a$.

            - $0, \mu$ refers to the address in group $H0$ which contain the base address of matrix $M$.
    1. For each cycle add to them the instructions "Inizializzazione" and "Incremento".

Concretely, this compilation steps tries to optimize all instuctions within a loop $S$ which refer to a matrix access $M(x_1, x_2, x_3)$ where al least one of the indices is setted to the loop control variable $I_S$. The number $a$ is stored in a parametric cell of the local group $H0$.

Recall that $a = a_1 \times (I_1) + a_2 \times (I_2) + a_3 \times (I_3)$. We define a "component of $a$" in respect to loop control variable $I_S$ the sum $\epsilon_1 \times a_r \times (I_r) + \epsilon_2 \times a_k \times (I_t)$ where $\epsilon_1$/$\epsilon_2$ can be either:

- $0$ if the level of the `DO` is **minor** to the level of the `DO` associated to $I_r/I_k$.

- $1$ if the level of the `DO` is **major** to the level of the `DO` associated to $I_r/I_k$.

Each "component of $a$" will be stored in a parametric cell of the group $HU$, which was created and used only during the execution of adiajcent instruction *"Inizializzazione"* and *"Incremento"* of each `DO`.

### Optimizing the "Asteriscamento ..."
Recall that the number of possible indices combination we can consider in each loop depends strictly on the number of free cells in $H0$ (i.e. those not occupied for storing parameters).

We can overcome this limitation by expoiting two properties of the CEP:

- Being able to use the  parametric cells in $HU$ for a double modify of the "inizializzazione" instruction.

- Being able to chane the origin of a group of celle parametriche.

    **NOTE:** *the document does not specify how the second property is used to optimize the "Asteriscamento". I think this is done to "enlarge" the $H0$ number of cells, but I'm not convince (recall a group $H0$ will always have 1 + 31 cells, with the first for the special cell $0$).*