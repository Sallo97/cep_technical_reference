# Traduttore Fortran CEP [[7](../0_Additional_resources/0_reference.md)]
Recall that the language FORTRAN CEP is an extension of the first iteration of FORTRAN, with the main difference being the possibility of writing arithmetic expressions having numbers in any representation.

The compilation procces is divided in two steps. 

## First step - Traduttore Fortran I

### Overview
The first phase, called Traduttore Fortran I, translates all FORTRAN's instructions into *"linguaggio contratto"*, an intermediate representation. Additionally, it also constructs numerous help tables. 

These tables will be used for computing addresses, quantities, and to keep useful information necessary for the second phase. 
    
The phase ends as soon as we reach, for all declared programs and subprogram, the special `END` proposition.

### Implementation
The compiler operates under the assumption that the FORTRAN source program is accessible via the attached punched tape reader. The process begins with a call to the *"inizializzazione"* subprogram, which initialize some counters.

Subsequently, the *"letture"* program transfers the next instruction from the punched tape into main memory. During this transfer, the routine performs a preliminary transformation of the instruction to for facilitating its scanning.

During the scanning phase, the compiler evaluates the operation code $XXX$:

- **Termination:** if the opcode corresponds to the `END` instruction, the system invokes the $\pi/ASIND$ subprogram to finalize the current compilation phase and transition to the next. During this finalization, the compiler references data stored in the auxiliary *"help tables"* to compute and assign memory addresses to all variables and quantities utilized throughout the program.

- **Translation:** otherwise, control is dispatched to the specific subprogram delegated to that opcode, designated as $\pi/XXX$.

    The traslation subprogram must be aware if the current instruction is within a `DO` loop. If an instruction resides within a cycle, the compiler must invoke the $\pi/CHDO$ subprogram. This routine is responsible for generating the *"istruzione contratta"* (contracted instruction) required to close the loop and for defining the boundary constraints for matrix index combinations accessed within that scope.

    ### Detailed Translation Operations

    - Arithmetic expressions are decomposed into a sequence of simplified, reduced formulas.

    - The lists for input and output propositions are developed.
        
        **NOTE:** *I did not understood what is means, so I just translated from the italian: "si sviluppano le liste di proposizioni di entrata e di uscita".*

    - If a matrix is accessed outside of a loop, the compiler attaches a special `PSEUDO-DO` construct to manage the memory reference (detailed in the following section).

    - The system constructs specialized tables to track all possible combinations of indices for matrix access patterns.

    - Statement numbers (i.e. unique numeric identifier associated to an instruction) are mapped and stored alongside the corresponding nesting level of the proposition.

    - Control flow instructions are expanded into their primitive logical components.

    - The compiler identifies and tracks DO loops that are eligible for optimization during the subsequent *"asteriscamento"* phase.

In total, this initial phase utilizes $46$ subprograms and $17$ distinct tables to manage the program state.

## Second step - Traduttore Fortran II

The compiler translates the intermediate representation (*"linguaggio contratto"*) into the Linguaggio Programmativo Simbolico CEP (LPSC). 

Simultaneously, it generates metadata for the Traduttore Simbolico Binario CEP (TSBC), using a new set of auxiliary tables, some of which comes from the previous phase.

### Detailed Translation Operations

- For each CEP instruction address, the compiler embeds the corresponding reduced formula.

- The compiler manages subprogram calls by resolving parameter addresses and handling their transfer into the local $H0$ group of the specific subprogram.

- The system generates I/O pseudoinstructions, for managin reading, printing, and punching between the main memory and auxiliary storage.

- Loop structures are finalized. To handle FORTRAN control jumps, the compiler implements a *"double jump"* technique.

    **NOTE:** *I need to check what the "double jump" technique is.*

In total the second phase uses $40$ subprogram and $19$ table, $14$ of which are in common with the first phase.

### Development curiosity
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

## Translation of cycles
In FORTRAN, within a `DO` block there could be a matrix access operation where at least one of the indices is a constant offset.

Consider now the case where we have a matrix access outside of any loop:

```Fortran
B(ROW, COL) = "something"
```

We could interpret it as being inside a `DO` cycle of only one iteration:

```Fortran
DO I = 0, 1
    B(ROW, COL) = "something"
```

For sake of abbreviation we define the special proposition `PSEUDO-DO` which is equivalent to writing `DO I = 0, 1`. By using the `PSEUDO-DO` we can "force" each matrix access in the code to be within a cycle.

But why we want to threat all matrix accesses as being within a loop? Because **in this way the compiler can treat all matrix accesses using constant offset in the same way, regardless of if they are in a loop or not**.

The compiler thus defines a common formula for computing the memory address associated to a matrix access:

`Address of A(I, J) = Base + ((I * NUM_OF_ROWS + J) * Dimension_Type)`

Another optimization technique used by the compiler is **"Strength Reduction"**, also used for computing the memory addresses of matrices.

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