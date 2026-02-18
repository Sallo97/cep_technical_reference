# Instructions [[9](../0_Additional_resources/0_reference.md)]
CEP's instructions (in italian "instruzioni" or "ordini" ) follow a 1+1 address scheme: **only one explicit address** which can be modified through the usage of parametric cells.

Each instruction $i$ occupies a memory word (i.e. 36-bits) 

The sequence $i = i_0 i_1 i_2 \ldots i_{35}$ is divided as:

- The first 9-bits ($i_0 i_1 \ldots i_8$) represents the **operation code** $O$. 

    The operation code can refer to $347$ distinct instructions, where :
    
    - $128$ are associated to fixed micro-programs of the MCU.
    
    - $219$ refer to **pseudo-instructions**.


- The successive 6-bits + 6-bits ($i_9 i_{10} \ldots i_{14}$ and $i_{15} i_{16} \ldots i_{20}$) indentify the **addresses for the two parametric cells** $P$ and $Q$.

- The last 15-bits ($i_{21} i_{22} \ldots i_{35}$) indentify the **explicit address**.

Instruction are divided into **ordinary** and **special** depending on how their final absolute address is computed. 

## Ordinary Instructions
Ordinary instructions compute the absolute address by summing the explicit relative address $s$ with the two parametric cells's addresses ($p$ and $q$): $absolute \ address = s + p + q$.

## Special Instructions
Special instructions compute the absolute address by summing the explicit address $s$ with only the parametric cell's address $q$: $ absolute \ address = s + q $. 

The remaining parametric cell address $p$ is used in different ways depending on the special instruction.

## Parametric Cells Organizations
Parametric cells are subdivided in two groups, each having $31$ consecutive cells + $1$ fixed parametric cell:

- The **local group** had cells numbered from $0$ to $31$ and starting address $H^{0}$. This group refers to parametric cells exclusive to the current call. 

Cell $0$ is fixed, having its content is **always set to $0$**. 

- The **global group** had cells numbered from $32$ to $63$, and starting address $H^{1}$ *(in some documents is also referred as $H_U$)*. This group refers to parametric cells shared among all programs and subprograms. 

Cell $63$ is fixed and works as the **instruction counter.**

## Instruction Classes
Instructions in the CEP are divided in **six classes**, each group identifying a set of instruction whose automatic checking works in the same manner (i.e. can be checked using the same helper subprogram).

- **Class 1 - Fixed Point Overflow**: identifies all instructions for fixed point arithmetic, which can be automatically checked for overflow.

- **Class 2 - Floating Point Overflow**: identifies all isntructions for floating point arithmetic, which can be automatically checked for overflow.

- **Class 3 - Parametric Cells**: identifies all instructions working with parametric cells.

- **Class 4 - Jumps**: identifies all instructions implementing jump operations.

- **Class 5 - I/O**: identifies all instructions managing input or output operations with external devices.

- **Class 6 - Unchecked**: indentifies all instructions which have no automatic checked program associated to them.

### Activating automatic checking
For letting the CEP automatically check an instruction in a program, the user needs to do the following two things:

1. In the control panel, it needs to turn the key associated to the class containing the instruction.

1. In the instruction itself, the second bit of the operation code must be set to $1$ (i.e. $i_1 = 1$).

Then when the CEP reaches the instruction, after completing it, program's execution stops and automatically jumps to the associated control program.

## Pseudo-instructions
Pseudo-instructions extend the base set. A pseudo-instruction is treated as a normal instruction by the CEP, with the main difference that, instead of using the operation code to instrument the micro-program associated to it in the Micro-program Control Unit, it is used to determine the address in Main Memory containing the macro-program (i.e. a subprogram) implementing the pseudo-instruction. 

Managing the call to the macro-program is automatically handled by the CEP, making the user unaware of the difference between executing the two types of instructions.

The computer distinguish between instruction and pseudo-instruction by checking the first bit of it (i.e. $i_0$):

- if $i_0 = 0 \rightarrow$ it is a **normal instruction**.

- if $i_0 = 1 \rightarrow$ it is a **pseudo-instruction**.

## Used Registers
Instructions can read and write the following registers:

- Registers $A$ and $B$ of the arithmetic unit. Each occupies a memory word (i.e. 36-bits).

- Registers $H^{0}$ and $H^{1}$. Each stores the starting address of their associated parametric cells' group.

- Register $N$ used as both a **numerator or instruction counter**. It occupies 15-bit, the same length dedicated to the explicit address in an instruction. 

    Usually $N$ is incremented by after each instruction cycle, except in the case of jumps which set it to a new value.

- Register $G$ is used as the **overflow flag**. It contains just one bit, which if set to $1$ tells the CEP that an overflow occurred.

- Registers $G_0$ and $G_1$ are registers of 1-bit each, used by the CEP during comparison operations.

- Registers $P$ and $Q$ store the relative address of the two parameteric cell present in the instruction at position $i_9 i_{10} \ldots i_{14}$ and $i_{15} i_{16} \ldots i_{20}$ respectively. Both occupy 6-bits.

- Register $Z$ is used to **store the content pointed by the absolute address of the current normal instruction**. Recall that the **normal** absolute address $c$ is computed as $c = s + q + r$.

- Register $Z^{'}$ **is used to store the content pointed by the relative address of the current special instruction**. Recall that the **special** absolute address $c^{'}$is computed as $c^{'} = i + q$.

As notation, small letters refer to the valute of the register with the same letter, only in block. As an example: $a$ refers to the content of register $A$.

## List of Instructions

### Addition and Subtraction to Main Memory Instructions

| Mnemonic Symbol | Meaning               |
| --------------- | --------------------- |
| $AZ$            | $a \rightarrow Z$     |
| $Z + A$         | $z + a \rightarrow Z$ |
| $-Z$            | $-z \rightarrow Z$    |
| $BZ$            | $b \rightarrow Z$     |
| $-AZ$           | $-a \rightarrow Z$    |
| $Z - A$         | $z - a \rightarrow Z$ |
| $0Z$            | $0 \rightarrow Z$     |
| $PZ$            | $p \rightarrow Z^{'}$ |

### Instructions for swapping content between Main Memory and Registers

| Mnemonic Symbol | Meaning                            |
| --------------- | ---------------------------------- |
| $ZAZ$           | $z \rightarrow A, a \rightarrow Z$ |
| $ZBZ$           | $z \rightarrow B, b \rightarrow Z$ |

### Instructions for copying content from/to Main Memory and Registers

| Mnemonic Symbol | Meaning                            |
| --------------- | ---------------------------------- |
| $ZA$            | $z \rightarrow A$                  |
| $-ZA$           | $-z \rightarrow A$                 |
| $A + Z$         | $a + z \rightarrow A$              |
| $A - Z$         | $a - z \rightarrow A$              |
| $-A$            | $-a \rightarrow A$                 |
| $A + M$         | $a + \|z\| \rightarrow A$          |
| $A - M$         | $a - \|z\| \rightarrow A$          |
| $CA$            | $c \rightarrow A$                  |
| $-CA$           | $-c \rightarrow A$                 |
| $A + C$         | $a + c \rightarrow A$              |
| $A - C$         | $a - c \rightarrow A$              |
| $ZAB$           | $z \rightarrow A, a \rightarrow B$ |
| $ZB$            | $z \rightarrow B$                  |
| $-ZB$           | $-z \rightarrow B$                 |
| $B + Z$         | $b + z \rightarrow B$              |
| $B - Z$         | $b - z \rightarrow B$              |
| $-B$            | $-b \rightarrow B$                 |
| $CB$            | $c \rightarrow B$                  |
| $-CB$           | $-c \rightarrow B$                 |
| $B + C$         | $b + c \rightarrow B$              |
| $B - C$         | $b - c \rightarrow B$              |
| $ZBA$           | $z \rightarrow B, b \rightarrow A$ |

### Instructions for working with Parametric Cells
Parametric cells where mainly introduced for handling vectors, i.e. contiguos blocks of values in Main Memory. 

Suppose we want to scan a vector, to do so we need the address to the first element of the block, referred to as the **base address**. Instead of updating this content at each step, which can be expensive, we can instead keep just the base address and at each step, sum it with an offset. This technique was already used by I.B.M for their /360 computers. The CEP implements it by storing the base address in a parametric cell.

| Mnemonic Symbol | Meaning                                |
| --------------- | -------------------------------------- |
| $ZP$            | $z^{'} \rightarrow P$                  |
| $-ZP$           | $-z^{'} \rightarrow P$                 |
| $P + Z$         | $p + z^{'} \rightarrow P$              |
| $P - Z$         | $p - z^{'} \rightarrow P$              |
| $ACP$           | $a + c^{'} \rightarrow P$              |
| $CPQ$           | $c^{'} \rightarrow P, p \rightarrow Q$ |
| $CP$            | $c^{'} \rightarrow P$                  |
| $-CP$           | $-c^{'} \rightarrow P$                 |
| $P + C$         | $p + c^{'} \rightarrow P$              |
| $P - C$         | $p - c^{'} \rightarrow P$              |
| $BCP$           | $b + c^{'} \rightarrow P$              |

Additionally there are avalable instructions for changing the group of parametric cells:

| Mnemonic Symbol | Meaning                                        |
| --------------- | ---------------------------------------------- |
| $CHO$           | $c^{'} \rightarrow H^{0}, h^{0} \rightarrow P$ |
| $CHI$           | $c^{'} \rightarrow H^{1}, h^{1} \rightarrow P$ |

### Instruction for doing Arithmetic Shifts

These are **special instructions** using the arithmetic registers $A$ and $B$. Their bit sequences are referred as $a_0 a_1 \ldots a_{35}$ and $b_0 b_1 \ldots b_{35}$ respectively.

| Mnemonic Symbol | Meaning                                |
| --------------- | -------------------------------------- |
| $VS$            | $a_1 a_2 \ldots a_{35}0 \rightarrow A$ (short left arithmetic shift repeated $c^{'}$ times) |
| $VD$            | $a_0 a_0 \ldots a_{35} \rightarrow A$ (short right arithmetic shift repeated $c^{'}$ times) |
| $WS$            | $a_1 a_2 \ldots a_{35} b_1 \rightarrow A, \  0 b_2 b_3 \ldots b_{35} 0 \rightarrow B$ (full left arithmetic shift repeated $c^{'}$ times)                           |
| $WD$            | $a_0 a_0 a_1 \ldots a_{34} \rightarrow A, 0 a_{35} b_1 b_2 \ldots b_{34} \rightarrow B$ (full right arithmetic shift repeated $c^{'}$ times)                  |
| $NOR$           | $0 \rightarrow b_0$ and following normalizations of $A$ and $B$ through left shift. The number of normalization shift is stored in $P$.

### Instructions for Fixed Point Multiplication and Division Operations

| Mnemonic Symbol | Meaning                                       |
| --------------- | --------------------------------------------- |
| $ME$            | $x_0 x_1 \ldots x_{35} \rightarrow A, 0 x_{36} \ldots x_{70} \rightarrow B$ where $x = a \times z$ (exact multiplication)                                                   |
| $AVZ$           | $x_0 x_1 \ldots x_{35} \rightarrow A, 0 x_{36} \ldots x_{70} \rightarrow B$ where $x = a\times z + 2^{-36}$ (approximated multiplication)                                                   |
| $MCU$           | $x_0 x_1 \ldots x_{35} \rightarrow A, 0 x_{36} \ldots x_{70}$ where $x = a \times z + 2^{-35}b$ (cumulative multiplication)                                                   |
| $A/Z$           | $a/z \rightarrow A$ (rounded division)        |
| $DDR$           | $(a,b)/z \rightarrow A$ rounded long division |
| $DDE$           | $(a, b)/z \rightarrow A$ (stores the exact quotient), $(a,b)/z \rightarrow B$ (stores the rest) (exact long division)                                                         |

### Instructions for Fixed Point Arithmetics
In these instructions we use the following notations:

- $D$ is the abstract register obtained by combining $A$ and $B$.

- $Z$ refers to the pair of cells $Z^{1}$ and $Z^{2}$ of address $c$ and $c+1$/

- $(x, y)$ is the number $x + 2^{-35} y$ 

| Mnemonic Symbol | Meaning                                       |
| --------------- | --------------------------------------------- |
| $ZD$            | $(z^{1}, z^{2}) \rightarrow A,B$              |
| $D + Z$         | $(a, b) + (z^{1}, z^{2}) \rightarrow A,B$     |
| $DZ$            | $(a,b) \rightarrow (Z^{1}, Z^{2})$            |
| $-ZD$           | $-(z^{1}, z^{2}) \rightarrow A, B$            |
| $D - Z$         | $(a, b) - (z^{1}, z^{2}) \rightarrow A,B$     |
| $COR$           | if $c=0$ then $a + b_0 \times 2^{-35} \rightarrow A, 0 \rightarrow b_0$; else $c = 1$ then $a - b_0 \times 2^{-35} \rightarrow A, 0 \rightarrow b_0$                                 |

### Instructions for Address Operations
| Mnemonic Symbol | Meaning                                       |
| --------------- | --------------------------------------------- |
| $BIZ$           | The last 15-bits of $Z$ are replaced with the last 15-bits of $B$                                                    |
| $ZIB$           | The last 15-bits of $B$ are repaced with the last 15-bits of $Z$                                                    |
| $PIZ$           | The last 15-bits of $Z^{'}$ are replaced by the last 15-bits of $P$                                               |
| $ZIP$           | The las 15-bits of $P$ are replaces with the last 15-bits of $Z^{'}$                                                |

### Instructions for Floating Point Arithmetics
In these instructions $a$ and $z$ are always assumed to be fixed point numbers and **$B$ is always modified**.

| Mnemonic Symbol | Meaning                                       |
| --------------- | --------------------------------------------- |
| $-F$            | $-a \rightarrow A$                            |
| $F + Z$         | $a + z \rightarrow A$                         |
| $F + M$         | $a + \|z\| \rightarrow A$                     |
| $F * Z$         | $a \times z \rightarrow A$                    |
| $-ZF$           | $-z \rightarrow A$                            |
| $F - Z$         | $a - z \rightarrow A$                         |
| $F - M$         | $a - \|z\| \rightarrow A$                     |
| $F / Z$         | $a / z \rightarrow A$                         |

### Search over Tables Instruction
The instruction $RT$ searches for the first occurrence of a word in a table. It is implemented in-hardware, making it really fast.

The table is determined by:

- $c^{'}$, which is its **start address**.

- $p$, which is its **length**.

The word is determined by:

- $a$, the content of the word.

- $B$,  referring to its configuration, i.e. how to interpret the content $a$.

The searched worked as follows: starting from the base address, at each step the current word is compare against $a$ by checking if $z \land b = a$. If so, the matching address is copied from $Z$ to $B$, otherwise the process interates to the next word. After scanning the whole table, then $111 \ldots 1 \rightarrow B$.

### Instructions for compare operations
Compare instrucitons will set the value of registers $G_0$ and $G_1$. Their value is determined by two internal routines:

- $r_0$, corresponding to the $<$ function. 

    Given two numbers $x$ and $y$, $r_{0}(x, y) = 1$ when $x < y.$

- $r_1$, corresponding to the $\neq$ function.

    Given two numbers $x$ and $y$, $r_{1}(x,y) = 1$ when $x \neq y$.

| Mnemonic Symbol | Meaning                                       |
| --------------- | --------------------------------------------- |
| $RAZ$           |  $r_0(a,z) \rightarrow G_0, r_1(a,z) \rightarrow G_1, a - z^{'} \rightarrow P$                                     |
| $RAC$           | $r_0(a, c^{'}) \rightarrow G_0, r_1(a, c^{'}) \rightarrow G_1, a - c^{'} \rightarrow P$                         |
| $RZP$           | $r_0(z^{'}, p) \rightarrow G_0, r_1(z^{'}, p) \rightarrow G_1$                                                  |
| $RCP$           | $r_0(c^{'}, p) \rightarrow G_0, r_1(c^{'},p) \rightarrow G_1, c^{'} \rightarrow Q$                             |

### The STOP Instruction
The instruction $ALT$ stops the CEP after setting the instruction counter as:

- if $c \neq 0$ then $c \rightarrow N$

- if $c = 0$ then $n + 1 \rightarrow N$

## Jump Instructions
| Mnemonic Symbol | Meaning                                            |
| --------------- | -------------------------------------------------- |
| $IP+$           | $p + 1 \rightarrow P$; if $p + 1 \geq 0$ then jump |
| $IP-$           | $p + 1 \rightarrow P$; if $p + 1 < 0$ then jump    |
| $DP+$           | $p - 1 \rightarrow P$; if $p - 1 \geq 0$ then jump |
| $DP-$           | $p - 1 \rightarrow P$; if $p - 1 < 0$ then jump    |
| $S$             | unconditional jump                                 |
| $SR+$           | if $g_0 = 0$ then jump                             |
| $SR=$           | if $g_1 = 0$ then jump                             |
| $SA+$           | if $a \geq 0$ then jump                            |
| $SA=$           | if $a = 0$ then jump                               |
| $SB+$           | if $b \geq 0$ then jump                            |
| $SB=$           | if $b = 0$ then jump                               |
| $SD=$           | if $(a,b) = 0$ then jump                           |
| $SGI$           | if $g = 1$ then jump                               |
| $SR-$           | if $g_0 = 1$ then jump                             |
| $SR\neq$        | if $g_1 = 1$ then jump                             |
| $SA-$           | if $a < 0$ then jump                               |
| $SA \neq$       | if $a \neq 0$ then jump                            |
| $SB-$           | if $b < 0$ then jump                               |
| $SB \neq$       | if $b \neq 0$ then jump                            |
| $SD \neq$       | if $(a,b) \neq 0$ then jump                        |
| $SGO$           | if $g = 0$ then jump                               |

Excluding instructions $IP+, IP-, DP+. DP-$, all other listed instructions when doing a jump will update the value $P$ to $n + 1 \rightarrow P$  if $P \neq 0$ and $P \neq 63$. $n$ is the position of the executed instruction. All registers used in the jump stay the same.

### I/O Instructions
| Mnemonic Symbol | Meaning                                            |
| --------------- | -------------------------------------------------- |
| $EAP$           | Reads character by character a punched tape. When reading a character, its value is stored to the last 6-bits of both $A$ and $P$. The previous content of the two registers is eraser (i.e. the first 30-bits of both are all set to $0$).                                              |
| $ELZ$           | Reads a punched tape by blocks. A block's length is specified by the first word of it.                                                |
| $EAU$           | Outputs a character. Depending on $c$'s value, the character obtained by retrieving the last 6-bits of $A$ is sent to a tape punches, or to the teletype, or to cell $c$ of the auxiliary memory $MAU$. $MAU$ has $102$ cells, each of 6-bits (i.e. it can contain a character).                          |
| $ESP$           | Prints the content of $MAU$ and frees it. After printing it executes a jump in the paper depending of the address of $c$.             |

## Instructions for communicating with the Magnetic Drum
The magnetic drum can be connected to the CEP only through the Main Memory, with which it can transfer blocks from or to it. 

| Mnemonic Symbol | Meaning                                            |
| --------------- | -------------------------------------------------- |
| $ITM$           | Sets the magnetic drum to the start of the requested block.                                                       |
| $EZT$           | Sets if the drum is read from or write to.         |
| $ETZ$           | Sets the type of transfer.                         |
| $EZK$           | Specifies the origin of the block in Main Memory.                                                                |
| $EKZ$           | Specifies the length of the block.                 |

To start the communication, an user needed first to call instruction $ITM$ to set the magnetic drum to the correct memory location. Then he can call all other instructions in the list.

## Instructions for using the magnetic tapes
| Mnemonic Symbol | Meaning                                            |
| --------------- | -------------------------------------------------- |
| $EZN$           | Starts the writing of data in the tape. It specified the length of each block.                                    |
| $BNM$           | Sets the magnetic tape to the start of the requested block.                                                       |
| $EZN$           | Specifies if the tape is read from.                |
| $ENZ$           | Specifies if the tape is write to. It also needs to specify the start position of the block in Main Memory and the lenght of the block.                                                          |

