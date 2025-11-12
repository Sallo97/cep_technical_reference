# General kernel information
With kernel we indentify the core of the CEP's architecture.
The main kernel of the CEP is divided in three main components (showed as enclosing rectangles in the image below): the memory, the *Unità di Calcolo*, and the *Unità degli Indirizzi*.

These blocks communicate between each other through registers. The information moves across registers through logical networks.

More precisely we indentify:

- For the *Unità di Calcolo*: $X$ as a group abstracting its logical network; $X^{'}$ as a group abstracting its registers.

- For the *Unità degli Indirizzi*: $Y$ as a group abstracting its logical network; $Y^{'}$ as a group abstracting its registers.

- For the *Memory*: we abstract it as the single register $M$.

The links (translated from the italian *"collegamenti"*) are names as `<number> <block's name>`, for example:

$2 x^{'} \rightarrow $ is the second exit of block $X^{'}$

![image](../resources/cep_kernel.png)

## Registers in $X^{'}$
Inside the $X$ group of registers of the Unità di Calcolo, there are the two arithmetic registers $A, B$ and the *memory register* $C$. 

More precisely, $C$ stores the address of the memory address passed explicitly as operand by the current instruction. $C$ sends its content through $Y$ by using link $2x^{'}$, passing by the main memory $M$.

## Registers in $Y^{'}$

- $N$: addresses memory instructions

- $R$: relative address for the two parametric cells in the reserved memory.

- $H_1$:origin address for the first parametric cell in the reserved memory.

- $H_0$: origin address for the second parametric cell in the reserved memory.

## Logical network $Y$
Within $Y$ it is located the **memory address decoder**.

## Decoding in $X$ and $Y$
Decoding (from the italian *"elaborare"*) information from the two logic network $X$ and $Y$ can be done with the help of parallel adders, one for each component. 

If the adders are used, the maximum time required for decoding the information depends of the time required for propagating the carry over.

For notation we refer to $X_D$ and $Y_D$ as the respective logical network when they **use** a parallel adders, $X$ and $Y$ instead when they **do not**.

