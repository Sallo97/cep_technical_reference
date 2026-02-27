# General notation
Here are shown all the symbols and notation used to describe the instructions. Specific notations for particular group of instructions are provided at the beginning of their section.

- $A$ and $B$ are the two **arithmetic registers**, each occupying a memory word (i.e. $36$-bits).

- $H^0$ and $H^1$ are the two **parametric registers**, each occupying 15-bits (i.e. an address).

- $N$ is the **instruction counter** (also called the **numerator**). It provides to the Control Unit the address of the instruction to execute. Excepts for jumps, at the end of each instruction its content is always incremented by $1$. Jumps will update the content altogether.

- $G$ is a **flag register** (in italian *"indicatore"*), i.e. a 1-bit register used for checking possible overflows.

- $\tau$ is the **overflow value** of an arithmetic operation between fixed numbers. 

    - if $\tau = 0 \rightarrow$ no overflow occurred.
    - if $\tau = 1 \rightarrow$ an overflow occurred.

- $P$ and $Q$ are the parametric cell referred by $s_p$ and $s_q$.

- $o$ is the **memory cell** referred by the final modified address of the instruction.

- $Z$ is the **address' content register**, i.e. a 36-bit register containing the content of the memory cell at the instruction address.

- $C$ is the **address register**, i.e. a 15-bit register containing the instruction address.

- $O$ is the **micro-operation code** register, i.e. it stores the current operation code of the micro-instruction to execute.

- We refer with the lower-case counterpart of a register\divparametric cell name as its content **before** the current instruction has been executed. For example with $a$ we refer to the content of the arithmetic register $A$ before the instruction has been executed.

- We refer with the lower-case counterpart of a register\divparametric cells as its content **after** the current instruction has been executed. For example with $a^{'}$ we refer to the content of the arithmetic register $A$ after the instruction has been executed.

- In the Notes of an instruction's actions, only the updated registers are specified. Any register\divcontent not writen is assumed to have stayed unchanged.

- Ordinary instructions are recognized by their mnemonics being composed only by letters, whereas special instructions are always preprended by a $\star$.

## Fixed point transfers, additions, and subtractions

### Instructions from $Z$ to $A$
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $Z\rightarrow A$ | $z \rightarrow A$ | $a^{'} = z$ | $Z$ is interpreted as an integer and copied into $A$. |
| $-Z \rightarrow A$ | $-z \rightarrow A$ | $a^{'} = -z$ | $Z$ is interpreted as an integer, which is negated and copied into $A$ |
| $A + Z \rightarrow A$ | $a + z \rightarrow A$ | $a^{'} = a + z \\ g^{'} = \tau$ | $A$ is summed with $Z$ and the result is stored into $A$. |
| $A - Z \rightarrow A$ | $a - z \rightarrow A$ | $a^{'} = a - z \\ g = \tau$ | $A$ is subtracted with $Z$ and their result is stored into $A$. | 

### Instructions from $A$ to $Z$
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $A \rightarrow Z$ | $a \rightarrow Z$ | $z^{'} = a$ | The content of $A$ is copied into $Z$. |
| $-A \rightarrow Z$ | $-a \rightarrow Z$ | $z^{'} = -a$ | The content of $A$ is negated and copied into $Z$.
| $Z + A \rightarrow Z$ | $z + a \rightarrow Z$ | $z^{'} = z + a$ | $Z$ and $A$ are summed and their result is stored into $Z$. |
| $Z - A \rightarrow Z$ | $z - a \rightarrow Z$ | $z^{'} = z - a$ | $Z$ and $A$ are subtracted and their result is stored into $Z$. |


### Instructions from $Z$ to $B$
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $Z \rightarrow B$ | $z \rightarrow B$ | $b^{'} = z$ | $Z$ is interpreted as an integer and copied into $B$. | 
| $-Z \rightarrow B$ | $-z \rightarrow B$ | $b^{'} = -z \\ g^{'} = \tau$ | $Z$ is interpreted as an integer, negated, and copied into $B$. | 
| $B + Z \rightarrow B$ | $b + z \rightarrow B$ | $b^{'} = b + z \\ g^{'} = \tau$ | $B$ is summed with $Z$ and the result is stored in $B$.
| $B - Z \rightarrow B$ | $b - z \rightarrow B$ | $b^{'} = b - z \\ g^{'} = \tau$ | $B$ is subtracted with $Z$ and the result is stored in $B$. | 

### Instructions from $B$ to $Z$
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $B \rightarrow Z$ | $b \rightarrow Z$ | $z^{'} = b \\ g^{'} = \tau$ | $B$ is copied into $Z$.

### Absolute arithmetic instructions
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $A + \|Z\| \rightarrow A$ | $a + \|z\| \rightarrow A$ | $a^{'} = a + \|z\| \\ g^{'} = \tau$ | $A$ is summed with the absolute value of $Z$, the result is stored in $A$. |
| $A - \|Z\| \rightarrow A$ | $a = \|z\| \rightarrow A$ | $a^{'} = a - \|z\| \\ g^{'} = \tau$ | $A$ is subtracted with the absolute value of $Z$, the result is stored in $A$. |

### Instructions from $C$ to $A$
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $C \rightarrow A$ | $c \rightarrow A$ | $a^{'} = c$ | $C$ is interpreted as an integer and copied into $A$. |
| $-C \rightarrow A$ | $-c \rightarrow A$ | $a^{'} = -c \\ g^{'} = \tau$ | $C$ is interpreted as an integer, negated, and copied into $A$. |
| $C + A \rightarrow A$ | $c + a \rightarrow A$ | $a^{'} = c + a \\ g^{'} = \tau$ | $C$ is summed with $A$ and the result stored in $A$. |
| $C - A \rightarrow A$ | $c - a \rightarrow A$ | $a^{'} = c - a \\ g^{'} = \tau$ | $C$ is subtracted with $A$ and the result stored in $A$. |

### Instructions from $C$ to $B$
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $C \rightarrow B$ | $c \rightarrow B$ | $b^{'} = c$ | $C$ is interpreted as an integer and copied into $B$. |
| $-C \rightarrow B$ | $-c \rightarrow B$ | $b^{'} = -c \\ g^{'} = \tau$ | $C$ is interpreted as an integer, negated, and stored in $B$. |
| $C + B \rightarrow B$ | $c + b \rightarrow B$ | $b^{'} = c + b \\ g^{'} = \tau$ | $C$ and $B$ are summed together and the result is stored in $B$. |
| $C - B \rightarrow B$ | $c - b \rightarrow B$ | $b^{'} = c - b \\ g^{'} = \tau$ | $C$ and $B$ are subtracted together and the results is stored in $B$. |

### Instructions from $O$ to $Z$
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $O \rightarrow Z$ | $o \rightarrow Z$ | $z^{'} = o$ | $O$ is copied into $Z$. |
| $Z \rightarrow A \rightarrow B$ | $a \rightarrow B \\ z \rightarrow A$ | $a^{'} = z \\ b^{'} = a$ | $A$ is copied into $B$ while simultaneously $Z$ is copied into $Z$.
| $Z \leftrightarrow A$ | $z \rightarrow A \\ a \rightarrow Z$ | $a^{'} = z \\ z^{'} = a$ | $A$ and $Z$ are swapped. | 
| $Z \leftrightarrow B$ | $z \rightarrow B \\ b \rightarrow Z$ | $b^{'} = z \\ z^{'} = b$ | $B$ and $Z$ are swapped. |

### Instructions from $C$ to $P$
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star C \rightarrow P$ | $c \rightarrow P$ | $p^{'} = c$ | $C$ is copied into parametric cell $P$. |
| $\star -C \rightarrow P$ | $-c \rightarrow P$ | $p^{'} = -c \\ g^{'} = \tau$ | $C$ is interpreted as an integer, negated, and copied into parametric $P$. | 
| $\star P + C \rightarrow P$ | $p + c \rightarrow P$ | $p^{'} = p + c$ | Parametric cell $P$ and $C$ are summed and stored in $P$. |
|$\star P - C \rightarrow P$ | $p - c \rightarrow P$ | $p^{'} = p - c \\ g^{'} = \tau$ | Parametric cell $P$ and $C$ are subtracted and stored in $P$. |

### Instructions from $Z$ to $P$
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star Z \rightarrow P$ | $z \rightarrow P$ | $p^{'} = z$ | $Z$ is copied into parametric cell $P$. |
| $\star -Z \rightarrow P$ | $-z \rightarrow P$ | $p^{'} = -z \\ g^{'} = \tau$ | $Z$ is interpreted as an integer, negated, and copied into parametric $P$. | 
| $\star P + Z \rightarrow P$ | $p + z \rightarrow P$ | $p^{'} = p + z$ | Parametric cell $P$ and $Z$ are summed and stored in $P$. |
|$\star P - Z \rightarrow P$ | $p - z \rightarrow P$ | $p^{'} = p - z \\ g^{'} = \tau$ | Parametric cell $P$ and $Z$ are subtracted and stored in $P$. |

### Instructions between parametric cells
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star P \leftrightarrow P$ | $q \rightarrow P \\ p \rightarrow Q$ | $p^{'} = q \\ q^{'} = p$ | Parametric cells $P$ and $Q$ are swapped. |

### Operations from $P$ to $Z$
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star P \rightarrow Z$ | $p \rightarrow Z$ | $z^{'} = p$ | The parametric cell $P$ is copied into $Z$. |

## Fixed point multiplications instructions

### Double precision integer
Recall that a double precision fixed point number in the CEP is a sequence $x$ of 71-bits, i.e. $x = x_0 x_1 x_2 \ldots x_{70}$.

In the CEP a double precision number is implemented as a sequence  distributed among two 36-bit words:

- **most significant portion $x^{*} = $** $x_0 x_1 \ldots x_{35}$. Sometimes it is also called the **first portion**.

- **least significant portion $x^{**}$** = $0 x_{36} x_{37} \ldots x_{70}$. Sometimes it is also called the **second portion**.

Then $x = x^{*} + 2^{-35} \times x^{**}$.

### Multiplication instructions

In all multiplications operations, register $A$ is always the multiplicand. In $A$ is always stored the most significant part of the result.

The double precision number stored by concatenating arithmetic registers  $A$ and $B$ is referred as $AB$.

### Overflow rule
In a multiplication an overflow is detected if $a = -1$ and $z = -1$.

| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----------- |
| $MOE$    | $a \times z \rightarrow AB$ | $a^{'} = (a \times z)^{*} \\ b^{'} = (a \times z)^{**} \\ g^{'} = \tau$ | Exact multiplication.
| $MOA$    | $a \times z + 2^{-36} \rightarrow AB$ | $a^{'} = (a \times z + 2^{-36})^{*} \\ b^{'} = (a \times z + 2^{-36})^{**} \\ g^{'} = \tau$ | Rounded multiplication. |
| $MOU$    | $a \times z + 2^{-35} \times b \rightarrow AB$ | $a^{'} = (a \times z + 2^{-35} \times b)^{*} \\ b^{'} = (a \times z + 2^{-35}\times b)^{**} \\ g^{'} = \tau$ | Cumulative multiplication between $A$ and $B$. |

## Fixed point division instructions

### Assumptions over $b_0$
All listed fixed point divisions assume that $b_0 = 0$, ignoring it in their computations. 

With $(a, 0)$ we refer to the double precision number having $a$ as the most significant part and $0$ as the least significant part.

The computed quotiend is always a single precision number stored in $A$.

### Division operations instructions
The divisor of all operations is always retrived from the memory address, i.e. is in $Z$.

### Overflow rules
An overflow **does not occur** if the quotient $q$ is $-1 \leq q < 1$.


An overflow **occurs** if $z = 0 \lor (z \neq 0 \land q < -1) \lor q \geq 1$. When the overflow is detected (i.e. $\tau  = 1$), then the most significant part of the division is in $B$, that is: $b^{'} = a$

| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star DCA$ | $(a, 0) \div z \rightarrow A$| $a^{'} = (a, 0) \div z \\ b^{'} \neq b  \\ g^{'} = \tau$ | Short rounded division. **The quotient is computed as a 37-bits number, then rounded to 36-bits.** |
| $\star DLA$ | $(a,b) \div z \rightarrow A$ | $a^{'} = (a, b) \div z \\ b^{'} \neq b \\ g^{'} = \tau$ | Full rounded division. **The quotient is computed as a 37-bits number, then rounded to 36-bits.** |
| $\star DLE$ | $(z \times ) \div z \rightarrow A$| $a^{'} = (a, b) \div z \\ 0 \leq b^{'} < \|z\| \\ g^{'} = \tau$ | Full division. Here the dividend $(a, b) = z\times a^{'} + 2^{-35} \times b^{'}.$|

## Arithmetic translations instructions

### Memory address $o$ rules
In both arithmetic and logic translations the final memory address $o$ must be in the range $0 \leq o \leq 127$. In the particular case that $o = 0$ the registers of the operation won't be modified.

In case $o$ is not in the range (i.e. $o < 0 \lor o > 127$), the traslation operation is done using $\bar{o} = o \mod 2^7$, which will satisfy the relation.

### Special instructions rule
All arithmetic translations instructions are special instruction, but with the exception of $\star NOR$ **they do not use the parametric cell $P$ in their computation**.

### Left arithmetic translation instructions
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star VS$ | $2^{o} \times a \rightarrow A$ | $a^{'} = (a_{o} a_{o+1} \ldots a_{35} 0 \ldots 0) \\ g^{'} = \tau$ | Short left translation. An overflow occurs if the first $o+1$ bits of $a$ are not equal to each other. | 
| $\star WS$ | $2^{o} \times (a,b) \rightarrow AB$ | $a^{'} = (a_o a_{o+1} \ldots a_{35} b1 \ldots b_o) \\ b^{'} = (0 b_2 \ldots b_{35} 0) \\ g^{'} = \tau$ | Full left translation. An overflow occurs if the bit at the head of $A$ does not match all the bits exited at the head of $A$. |
|$\star NOR$| | while ($a_0 \neq a_1) \rightarrow \{ a^{'} = (a_1 a_2 \ldots a_{35} b_1) \\ b^{'} = (0 b_2 \ldots b_{35} 0)\} \\ p^{'} = \#steps \ done$ | Normalization of (a,b). It applies left-shifts until the first two bits at the head of $A$ differs. The number of iterations (i.e. steps) done is stored in $P$. If $a_0 = a_1 \lor a_0 = a_1 = \ldots = a_{35} = b_1 = b_2 = \ldots = b_{35}$ then $a^{'}$ = a and $b^{'} = b$ and $p^{'} = 0$. |

### Right arithmetic translation instructions
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star VD$ | $2^{-o} \times a \rightarrow A$ | $a^{'} = (a_0 a_0 \ldots a_0 a_1 \ldots a_{35 - o})$ | Short right translation. |
| $\star VDA$ | $(2^{-o} \times a)_{rounded} \rightarrow A$ | $a^{'} = (2^{-o} \times a + 2^{-35} \times a_{35 - o})$ | Rounded short right translation. $VDA$ behaves like $VD$ when the last bit leaving $A$ is zero. Se instead the last parting bit is one, $VDA$ consists in executing $VD$ and then increment the result by $2^{-35}$. | 
$\star WD$| $2^{-o} \times (a,b) \rightarrow A,B$ | $for \ i = 0 \ to \ o \rightarrow \{a^{'} = (a_0 a_0 a_1 \ldots a_{34}) \\ b^{'} = (0 a_{35} b1 \ldots b_{34})\}$ | Full right translation. If $x$ is the double precision number obtained as $$x^{*} = (a\times a_1 \ldots a_{35}) \\ x^{**} = (0 b_1 \ldots b_{35})$$ then $a^{'}$ and b^{'} are the most significant and least significant portion of the number $2^{-1} \times x$ truncated by $70-o$ bits. |

## Double transfer instructions
Recall that with $Z^{1}$ we refer to the memory cell right after $Z$.

| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $Z \rightarrow AB$ | $(z, z^{1}) \rightarrow A,B$ | $a^{'} = z \\ b^{'} = z^{'}$ | The content of $Z,Z^{1}$ is copied into $A,B$. |
| $AB \rightarrow Z$ | $(a, b) \rightarrow Z, Z^{1}$ | $z^{'} = a \\ z^{1^{'}} = b$ | The content of $A,B$ is copied into $Z, Z^{1}$. |

## Double precision addition and subtraction instructions
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $AB + Z \rightarrow AB$| $(a,b) + (z, z^{1}) \rightarrow A,B$ | defined $y = b + z^{1}$: $$b^{'} = (0 y_1 y_2 \ldots y_{35}) \\ a^{'} = a + z + 2^{-35} \times y_0 \\ g^{'} = \tau$$ | Double precision addition. |

When $b_0 = z_0^{1} = 0$ we declare $u$ as a double precision number having $a$ and $b$ as its first and second portions; we assign $v$ to the number having $z$ and $z^{1}$ as its first and second portion. Then $a^{'}$ and $b^{'}$ are the most significant and least significant part of the number $w = u + v$.

### Overflow rule for addition
$\tau$ checks the sum $a + z + 2^{-35} \times y_0$


| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $AB - Z \rightarrow AB$ | $(a,b) - (z, z^{1}) \rightarrow A,B$ | defined $y = b - z^{1}$: $$b^{'} = (0 y_1 y_2 \ldots y_{35}) \\ a^{'} = (a - z - 2^{35} \times y_0) \\ g^{'} = \tau$$ |  Double precision subtraction. |

When $b_0 = z_0^{1} = 0$ then $a^{'}$ and $b^{'}$ are the most significant and least significant part of the number $w = u - v$.

### Overflow rule for subtraction
$\tau$ checks the sum $a - z - 2^{35} \times y_0$.

| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $$-Z \rightarrow AB$$ | $-(z, z^{1}) \rightarrow A,B$ | defined $y = -z$: $$b^{'} = (0 y_1 y_2 \ldots y_{35}) \\ a^{'} = (-z - 2^{35} \times y_0) \\ g^{'} = \tau$$| Double precision negation.

If $b_0 = 0$ then $a^{'}$ and $b^{'}$ are the first and second portion of the double precision number $-x$ where $x$ has $a$ and $b$ as most significant parti

### Overflow rule for double precision negation
$\tau$ refers to the sum $-z - 2^{-35} \times y_0$.

## Comparison instructions

### Common notation
For comparisons operations the CEP employes two registers: $R_0$ and $R_1$ knows as **comparison flags** (from the italian *"indicatori di confronto"*). 

We define the greater or equal value $\Pi_0$ such that:

- $\Pi_0 (x,y) = 0$ if $x \geq y$
- $\Pi_0 (x,y) = 1$ if $x < y$

We define the equality value $\Pi_1$ such that:

- $\Pi_1 (x,y) = 0$ if $x = y$
- $\Pi_1 (x,y) = 1$ if $x \neq y$

| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star RAZ$ | | $r_0^{'} = \Pi_0(a, z) \\ r_1^{'} = \Pi_1 (a,z) \\ p^{'} = a - z$ | |
| $\star RAC$ | | $r_0^{'} = \Pi_0(a, o) \\ r_1^{'} = \Pi_1 (a,o) \\ p^{'} = a - o$ | |
| $\star RZP$ | | $r_0^{'} = \Pi_0 (z,p) \\ r_1^{'} = \Pi_1 (z, p)$ | |

When in instruction $RZP$ the parametric cell $P$ has its relative address set to zero, the comparision is done between $z$ and zero.

| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star RCP$ | | $r_0^{'} = \Pi_0 (o,p) \\ r_1^{'} = \Pi_1 (o, p) \\ q^{'} = o$

Instruction $RCP$ is used cyclic iterations, more specifically for incrementing the current index (in $Q$) and comparing it with a number (in $P$).

## Halt instruction
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star ALT$ | | $n^{'} = o \\ p^{'} = n + 1$ | The CEP halts. |

## Unconditional jump instruction
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star S$ | | $n^{'} = o \\ p^{'} = n + 1$ | |

## Conditional jumps instructions

Note that a conditional jump is done **only** when the condition specified by the instruction is valid, otherwise the CEP moves on with the next instruction.


### Conditional jumps over $A$
| Mnemonic | Condition | Action when valid | Action when false |
| -------- | ------ | --------------- | ----- |
| $\star SA \geq$ | $a \geq 0$ (i.e. $a_0 = 0$) | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ |
| $\star SA >$ | $a < 0$ (i.e. $a_0 = 1$) | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ |
| $\star SA =$ | $a = 0$ | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ |
| $\star SA \neq$ | $a \neq 0$ | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ | 

### Conditional jumps over $B$
| Mnemonic | Condition | Action when valid | Action when false |
| -------- | ------ | --------------- | ----- |
| $\star SB \geq$ | $b \geq 0$ (i.e. $b_0 = 0$) | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ |
| $\star SB <$ | $b < 0$ (i.e. $b_0 = 1$) | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ |
| $\star SB =$ | $b = 0$ | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ |
| $\star SB \neq$ | $b \neq 0$ | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ |

## Overflow jumps instructions
| Mnemonic | Condition | Action when valid | Action when false |
| -------- | ------ | --------------- | ----- |
| $\star SGI$ | $g \neq 1$ | $n^{'} = o \\ p^{'} = n + 1 \\ g^{'} = 0$ | $n^{'} = n + 1 $ |
| $\star SGO$ | $g = 1$ | $n^{'} = o \\ p^{'} = n + 1 \\ g^{'} = 0$ | $n^{'} = n + 1 $ |

## Comparison jumps instructions

### Notation
Given two generic registers $X$ and $Y$, we refer with $RXY$ to the most recent comparison instruction (e.g. $RAZ$ or $RAC$) done during the execution. Then if a conditional jump instruction is executed, the jump occurs depending on the values of $x$ and $y$.

The jump condition will be checked using the comparison flags $R_0$ and $R_1$, which will be zeroed after the jump instruction regardless of its success or not.

| Mnemonic | Condition | Action when valid | Action when false | Action always done |
| -------- | ------ | --------------- | ----- | --- |
| $\star SR \geq$ | $x \geq y$ | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ | $r_0^{'} = 0 \\ r_1^{'} = 0$ |
| $\star SR <$ | $x < y$ | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ | $r_0^{'} = 0 \\ r_1^{'} = 0$ |
| $ \star SR = $ | $x = y$ | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ | $r_0^{'} = 0 \\ r_1^{'} = 0$ |
| $ \star SR \neq$ | $x \neq y$ | $n^{'} = o \\ p^{'} = n + 1$ | $n^{'} = n + 1$ | $r_0^{'} = 0 \\ r_1^{'} = 0$ |

## Unitary increment & jump instruction
| Mnemonic | Condition | Action when valid | Action when false | Action always done |
| -------- | ------ | --------------- | ----- | --- |
| $\star IPS$ | $p^{'} < 0$ | $n^{'} = o$ | $n^{'} = n + 1$ | $p^{'} = p + 1$ |

## Unitary decrement & jump instruction
| Mnemonic | Condition | Action when valid | Action when false | Action always done |
| -------- | ------ | --------------- | ----- | --- |
| $\star DPS$ | $p^{'} \geq 0$ | $n^{'} = o$ | $n^{'} = n + 1$ | $p^{'} = p - 1$

## Changing parametric group instructions
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $ \star CHO$ | $o \rightarrow H^{0}$ | $h^{0^{'}} = o \\ p^{'} = h^{0} $| Changes parametric group $\{H^{0}\}$.
| $\star CHU$ | $o \rightarrow H^{1}$ | $h^{1^{'}} = o \\ p^{'} = h^{1}$ | Changes parametric group $\{H^{1}\}$.

## Boolean operations

### Notation

- With $\bar{x}$ we refers to the word obtained by logically negating each bit in $x$.

- $x \land y$ is the logical product.

- $x \lor y$ is the logical sum.

- $x \neq y$ is the logic inequality.

| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $Z \land A \rightarrow A$ |  $z \land a \rightarrow A$ | $a^{'} = z \land a$ | |
| $Z \lor A \rightarrow A$ | $z \lor a \rightarrow A$ | $a^{'} = z \lor a$ |
| $Z \neq A \rightarrow A$ | $z \neq a \rightarrow A$ | $a^{'} = z \neq a$ |
| $\bar{Z} = A$ | $\bar{Z} \rightarrow A$ | $a^{'} = \bar{z}$ | |
| $Z \land B \rightarrow B$ | $z \land b \rightarrow B$ | $b^{'} = z \land b$ | |
| $A \land Z \bar{Z} \rightarrow AB$ | $a \land z \rightarrow A \\ a \land \bar{z} \rightarrow B $ | $a^{'} = a \land z \\ b^{'} = a \land \bar{z}$

## Bit counter instruction
| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $BIT$ | | defined $m = \# bits  \ of \ A \ set \ to \ 1$ $$a^{'} = 0 \\ b^{'} = b + m$$ | Counts the number of bits in $A$ set to $1$ and sums the result with $B$. | 

## Logic translations

### Memory address $o$ rules
In both arithmetic and logic translations the final memory address $o$ must be in the range $0 \leq o \leq 127$. In the particular case that $o = 0$ the registers of the operation won't be modified.

In case $o$ is not in the range (i.e. $o < 0 \lor o > 127$), the traslation operation is done using $\bar{o} = o \mod 2^7$, which will satisfy the relation.

### Special instructions rule
All arithmetic translations instructions are special instruction, but with the exception of $\star NOR$ **they do not use the parametric cell $P$ in their computation**.

⚠︎ are the same rules of arithmetic translations.

| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $\star WLS$ | | repeat $o$ times: $$a^{'} = (a_1 a_2 \ldots a_{35} b_0) \\ b^{'} = (b_1 b_2 \ldots b_{35} 0)$$| Logical left translation. |
| $\star WLD$ | | repeat $o$ times: $$a^{'} = (0 a_0 a_1 \ldots a_{34}) \\ b^{'} = (a_{35} b_0 b_1 \ldots b_{34})$$ | Logical right translation. | 
| $\star WLO$ | | repeat $o$ times: $$a^{'} = (a_1 a_2 \ldots a_{35} b_0) \\ b^{'} = (b_1 b_2 \ldots b_{35} a_0)$$ | Logical circular translation. |
| $\star WLN$ | | repeat $o$ times $$a^{'} = (0 a_0 a_1 \ldots a_{34}) \\ b^{'} = (b_1 b_2 \ldots b_{35} a_{35})$$ | Logical translation with inversion of bits order. |

## Address substitution instructions

Given the word $x = (x_0 x_1 \ldots x_{35})$, we define $x_{I}$ as the subsequence of $x$ containing the last 15-bits, i.e. $x_I = (x_{21} x_{22} \ldots x_{35})$.

| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $Z_{I} \rightarrow B_{I}$ | $z_{I} \rightarrow B_{I}$ | $b^{'} = (b_0 b_1 \ldots b_{20} z_{21} \ldots z_{35})$ | |
| $B_{I} \rightarrow Z_{I}$ | $b_{I} \rightarrow Z_{I}$ | $z^{'} = (z_0 z_1 \ldots z_{20} b_{21} \ldots b_{25})$ | |
| $Z_{I} \rightarrow P_I$ | $z_I \rightarrow P_I$ | $p^{'} = (p_0 p_1 \ldots p_{20} z_{21} \ldots z_{35})$ | |
| $P_I \rightarrow Z_I$ | $p_I \rightarrow Z_I$ | $z^{'} = (z_0 z_1 \ldots z_{20} p_{21} \ldots p_{35})$ | |

## Floating point arithmetic instructions

### Notation
We assume $a$ and $z$ contain floating point numbers. The results of floating point operations are always normalized and rounded, that is the result is a normalized number with 29-bits of rounded fractional part and then 28-bits.

With $\nu$ we define the number of translations used for normalizing the result.

### Overflow rules
An overflow is detected when:

- the middle or final result has the exponent over $128$. 

- the divisor is zero.

| Mnemonic | Action | Updated content | Notes |
| -------- | ------ | --------------- | ----- |
| $F + Z \rightarrow F$ | $a + z \rightarrow A$ | $a^{'} = a + z \\ b^{'} = \nu \\ g^{'} = \tau$ | |
| $F - Z \rightarrow F$ | $a - z \rightarrow A$ | $a^{'} = a - z \\ b^{'} = \nu \\ g^{'} = \tau$ | |
| $-Z \rightarrow F$ | $-z \rightarrow A$ | $a^{'} = -z \\ b^{'} = \nu \\ g^{'} = \tau$ | |
| $-F \rightarrow F$ | $-a \rightarrow A$ | $a^{'} = -a \\ b^{'} = \nu \\ g^{'} = \tau$ | |
| $F + \|Z\| \rightarrow F$ | $a + z \rightarrow A$ | $a^{'} = a + z \\ b^{'} = \nu \\ g^{'} = \tau$ | |
| $F - \|Z\| \rightarrow F$ | $a - z \rightarrow A$ | $a^{'} = a - z \\ b^{'} = \nu \\ g^{'} = \tau$ | |
| $MOF$ | $a\times z \rightarrow A$ | $a^{'} = a \times z \\ b^{'} = \nu \\ g^{'} = \tau$ | |
| $DIF$ | $a \div z \rightarrow A$ | $a^{'} = a \div z \\ b^{'} = \nu \\ g^{'} = \tau$
