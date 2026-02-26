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

- We refer with the lower-case counterpart of a register/parametric cell name as its content **before** the current instruction has been executed. For example with $a$ we refer to the content of the arithmetic register $A$ before the instruction has been executed.

- We refer with the lower-case counterpart of a register/parametric cells as its content **after** the current instruction has been executed. For example with $a^{'}$ we refer to the content of the arithmetic register $A$ after the instruction has been executed.

- In the description of an instruction's actions, only the updated registers are specified. Any register/content not writen is assumed to have stayed unchanged.

- Ordinary instructions are recognized by their mnemonics being composed only by letters, whereas special instructions are always preprended by a $\star$.

## Fixed point transfers, additions, and subtractions

### Operations from $Z$ to $A$
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $Z\rightarrow A$ | $z \rightarrow A$ | $a^{'} = z$ | $Z$ is interpreted as an integer and copied into $A$. |
| $-Z \rightarrow A$ | $-z \rightarrow A$ | $a^{'} = -z$ | $Z$ is interpreted as an integer, which is negated and copied into $A$ |
| $A + Z \rightarrow A$ | $a + z \rightarrow A$ | $a^{'} = a + z \\ g^{'} = \tau$ | $A$ is summed with $Z$ and the result is stored into $A$. |
| $A - Z \rightarrow A$ | $a - z \rightarrow A$ | $a^{'} = a - z \\ g = \tau$ | $A$ is subtracted with $Z$ and their result is stored into $A$. | 

### Operations from $A$ to $Z$
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $A \rightarrow Z$ | $a \rightarrow Z$ | $z^{'} = a$ | The content of $A$ is copied into $Z$. |
| $-A \rightarrow Z$ | $-a \rightarrow Z$ | $z^{'} = -a$ | The content of $A$ is negated and copied into $Z$.
| $Z + A \rightarrow Z$ | $z + a \rightarrow Z$ | $z^{'} = z + a$ | $Z$ and $A$ are summed and their result is stored into $Z$. |
| $Z - A \rightarrow Z$ | $z - a \rightarrow Z$ | $z^{'} = z - a$ | $Z$ and $A$ are subtracted and their result is stored into $Z$. |


### Operations from $Z$ to $B$
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $Z \rightarrow B$ | $z \rightarrow B$ | $b^{'} = z$ | $Z$ is interpreted as an integer and copied into $B$. | 
| $-Z \rightarrow B$ | $-z \rightarrow B$ | $b^{'} = -z \\ g^{'} = \tau$ | $Z$ is interpreted as an integer, negated, and copied into $B$. | 
| $B + Z \rightarrow B$ | $b + z \rightarrow B$ | $b^{'} = b + z \\ g^{'} = \tau$ | $B$ is summed with $Z$ and the result is stored in $B$.
| $B - Z \rightarrow B$ | $b - z \rightarrow B$ | $b^{'} = b - z \\ g^{'} = \tau$ | $B$ is subtracted with $Z$ and the result is stored in $B$. | 

### Operations from $B$ to $Z$
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $B \rightarrow Z$ | $b \rightarrow Z$ | $z^{'} = b \\ g^{'} = \tau$ | $B$ is copied into $Z$.

### Absolute arithmetic operations
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $A + \|Z\| \rightarrow A$ | $a + \|z\| \rightarrow A$ | $a^{'} = a + \|z\| \\ g^{'} = \tau$ | $A$ is summed with the absolute value of $Z$, the result is stored in $A$. |
| $A - \|Z\| \rightarrow A$ | $a = \|z\| \rightarrow A$ | $a^{'} = a - \|z\| \\ g^{'} = \tau$ | $A$ is subtracted with the absolute value of $Z$, the result is stored in $A$. |

### Operations from $C$ to $A$
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $C \rightarrow A$ | $c \rightarrow A$ | $a^{'} = c$ | $C$ is interpreted as an integer and copied into $A$. |
| $-C \rightarrow A$ | $-c \rightarrow A$ | $a^{'} = -c \\ g^{'} = \tau$ | $C$ is interpreted as an integer, negated, and copied into $A$. |
| $C + A \rightarrow A$ | $c + a \rightarrow A$ | $a^{'} = c + a \\ g^{'} = \tau$ | $C$ is summed with $A$ and the result stored in $A$. |
| $C - A \rightarrow A$ | $c - a \rightarrow A$ | $a^{'} = c - a \\ g^{'} = \tau$ | $C$ is subtracted with $A$ and the result stored in $A$. |

### Operations from $C$ to $B$
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $C \rightarrow B$ | $c \rightarrow B$ | $b^{'} = c$ | $C$ is interpreted as an integer and copied into $B$. |
| $-C \rightarrow B$ | $-c \rightarrow B$ | $b^{'} = -c \\ g^{'} = \tau$ | $C$ is interpreted as an integer, negated, and stored in $B$. |
| $C + B \rightarrow B$ | $c + b \rightarrow B$ | $b^{'} = c + b \\ g^{'} = \tau$ | $C$ and $B$ are summed together and the result is stored in $B$. |
| $C - B \rightarrow B$ | $c - b \rightarrow B$ | $b^{'} = c - b \\ g^{'} = \tau$ | $C$ and $B$ are subtracted together and the results is stored in $B$. |

### Operations from $O$ to $Z$
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $O \rightarrow Z$ | $o \rightarrow Z$ | $z^{'} = o$ | $O$ is copied into $Z$. |
| $Z \rightarrow A \rightarrow B$ | $a \rightarrow B \\ z \rightarrow A$ | $a^{'} = z \\ b^{'} = a$ | $A$ is copied into $B$ while simultaneously $Z$ is copied into $Z$.
| $Z \leftrightarrow A$ | $z \rightarrow A \\ a \rightarrow Z$ | $a^{'} = z \\ z^{'} = a$ | $A$ and $Z$ are swapped. | 
| $Z \leftrightarrow B$ | $z \rightarrow B \\ b \rightarrow Z$ | $b^{'} = z \\ z^{'} = b$ | $B$ and $Z$ are swapped. |

### Operations from $C$ to $P$
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $\star C \rightarrow P$ | $c \rightarrow P$ | $p^{'} = c$ | $C$ is copied into parametric cell $P$. |
| $\star -C \rightarrow P$ | $-c \rightarrow P$ | $p^{'} = -c \\ g^{'} = \tau$ | $C$ is interpreted as an integer, negated, and copied into parametric $P$. | 
| $\star P + C \rightarrow P$ | $p + c \rightarrow P$ | $p^{'} = p + c$ | Parametric cell $P$ and $C$ are summed and stored in $P$. |
|$\star P - C \rightarrow P$ | $p - c \rightarrow P$ | $p^{'} = p - c \\ g^{'} = \tau$ | Parametric cell $P$ and $C$ are subtracted and stored in $P$. |

### Operations from $Z$ to $P$
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $\star Z \rightarrow P$ | $z \rightarrow P$ | $p^{'} = z$ | $Z$ is copied into parametric cell $P$. |
| $\star -Z \rightarrow P$ | $-z \rightarrow P$ | $p^{'} = -z \\ g^{'} = \tau$ | $Z$ is interpreted as an integer, negated, and copied into parametric $P$. | 
| $\star P + Z \rightarrow P$ | $p + z \rightarrow P$ | $p^{'} = p + z$ | Parametric cell $P$ and $Z$ are summed and stored in $P$. |
|$\star P - Z \rightarrow P$ | $p - z \rightarrow P$ | $p^{'} = p - z \\ g^{'} = \tau$ | Parametric cell $P$ and $Z$ are subtracted and stored in $P$. |

### Operations between parametric cells
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $\star P \leftrightarrow P$ | $q \rightarrow P \\ p \rightarrow Q$ | $p^{'} = q \\ q^{'} = p$ | Parametric cells $P$ and $Q$ are swapped. |

### Operations from $P$ to $Z$
| Mnemonic | Action | Updated content | Description |
| -------- | ------ | --------------- | ----------- |
| $\star P \rightarrow Z$ | $p \rightarrow Z$ | $z^{'} = p$ | The parametric cell $P$ is copied into $Z$. |