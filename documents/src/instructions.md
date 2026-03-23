# Instructions
CEP's instructions follow a 1-address scheme and share all the same length of one memory word (i.e. 36-bits). [[11](../helper_resources/references.md)]

Given the sequence of bits representing an instruction $(s_0 s_1 s_2 \ldots s_8 s_9 \ldots s_{14} s_{15} \ldots s_{20} s_{21} \ldots s_{35})$, it can be divided into six subsequences.

- **Pseudoinstruction flag (1-bits):** ($s_0$) determines if the current instruction is a pseudoinstruction or not. [[11](../helper_resources/references.md)]
    
- **Automatic check flag (1-bits):** ($s_1$) determines if the automatically checking of the instruction is activated. [[11](../helper_resources/references.md)]

- **Operation code (7-bits):** $(s_2 \ldots s_{14})$ identifies the command to execute. [[11](../helper_resources/references.md)]

- **First parametric address (6-bits):** $(s_p = s_9 \ldots s_{14})$ the address of parametric cell $P$. Recall that the first bit is the group index and the last five bits are the relative address. We designate $p$ as its content. [[11](../helper_resources/references.md)]

- **Second parametric address (6-bits):** $(s_q = s_{15} \ldots s_{20})$ the address of parametric cell $Q$. Recall that the first bit is the group index and the last five bits are the relative address. We designate $q$ as the content of $Q$. [[11](../helper_resources/references.md)]

- **Operand address (15-bits):** $(s = s_{21} \ldots s_{35})$ the address of the entry in Main Memory used as operand. [[11](../helper_resources/references.md)]

![image](../../resources/cep_instruction.svg)

The total number of microprogrammed instruction. From now on within this document we will refer to them as instructions.

## The bit $s_0$ 
The first bit of an instruction $s_0$ specifies if the instruction is a pseudoinstruction or not:

- $s_0 = 0 \rightarrow$ is a normal instruction.

- $s_0 = 1 \rightarrow$ is a special instruction.

At their core, pseudoinstructions are subprograms which can be called in the form of an instruction. [[9](../helper_resources/references.md)]

## The bit $s_1$
It is used for determining if the instruction should be automatically checked or not. Let's consider for example the set of arithmetic instructions; for each of them could be convenient to let the computer test if an overflow occurred. This can be done by setting $s_1 = 1$ to the instruction and by pressing the key in the control panel of its instructions' group. Then, the CEP instead of moving to the next instruction, jumps to address $h^{1} + 32$. Which will store the address of the debug program for checking the overflow, stored in Main Memory. Note how the behaviour its similar to pseudoinstructions. [[11](../helper_resources/references.md)]

Note that the special address $h^{1} + 32$ is associated for automatic checking of arithmetic instruction, other kind of instructions points to a different offset. [[11](../helper_resources/references.md)]


### Instructions debug  group
Instructions are divided into five classes, each identifying the type of automatic checking available among the members. [[11](../helper_resources/references.md)]

We define the following notation:

- $\gamma_i$ identifies the key associated to a class in the control panel s.t.:

    - $y_i = 1 \rightarrow$ the button is pressed.
    - $y_i = 0 \rightarrow$ the button is raised.

- $\delta_i$ refers to the class of the instruction.

- $\tau_{\phi}$ is the overflow flag for fixed point arithmetics; $\tau_{\mu}$ is the overflow flag of the exponent in a floating point operation; $\tau_{\pi} is the parity flag$.

Then we can define the conditional jump for each class $X_i$:

- **$TVF$ class:** $X_1 = s_1 \times \gamma_1 \times \delta_1 \times \tau_{\phi} \rightarrow h_1 + 32$

- **$TVM$ class:** $X_2 = s_1 \times \gamma_2 \times \delta_2 \times \tau_{\mu} \rightarrow h_1 + 33$

- **$ICP$ class:** $X_3 = s_1 \times \gamma_3 \times \delta_3 \rightarrow h_1 + 34$

- **$SAL$ class:** $X_4 = s_1 \times \gamma_4 \times \delta_4 \rightarrow h^{1} + 35$

- **$CBP$ class:** $X_5 = s_1 \times \gamma_5 \times \delta_5 \times \tau_{\pi} \rightarrow h_1 + 36$

Note that, with the exception of the jump instructions of class $SAL$, the jump to the debug program is done **after** the associated instruction has been executed. In the case of jump instruction (i.e. condition $X_4$) the check is done **before** the requested operation is executed. This inversion is necessary to check both the address before and after a jump. [[11](../helper_resources/references.md)]


#### The $TVF$ class (fixed point overflow)
The class $TVF$ (from the italian *"Traboccamento Virgola Fissa"*, i.e. "fixed point overflow") identifies all fixed point arithmetic operations which can lead to overflow. 

This set contains the following instructions:

- $COR$, $VS$, $WS$, $BIT$
- $MOA$, $MOU$, $MOE$, $DCA$, $DLA$, $DLE$

#### The $TVM$ class (floating point overflow)
The class $TVM$ (from the italian *"Traboccamento Virgola Mobile"*, i.e. "floating point overflow") identifies all floating point operations which can lead to overflow.

This set contains the following instructions:

- $-Z \rightarrow F, F + Z \rightarrow F, F - Z \rightarrow Z, -F \rightarrow F$
- $F + \|Z\| \rightarrow F, F - \|Z\| \rightarrow F, MF, DF$

#### The $ICP$ class (parametric cell instructions)
The class $ICP$ (from the italian *"Istruzioni Celle Parametrice"*, i.e. "parametric cells instructions") identifies all instructions which operate over parametric cells, considering comparison instruction and instructions for changing the origin of parametric groups. 

This set contains the following instructions:

- $C \rightarrow P, -C \rightarrow P, P + C \rightarrow P, P - C \rightarrow P$
- $Z \rightarrow P, -Z \rightarrow P, P + Z \rightarrow P, P - Z \rightarrow P$
- $P \leftrightarrow Q, P \rightarrow Z, A + C \rightarrow P, B + C \rightarrow P$
- $VDA, RAZ, RAC, RZP, RCP$
- $CHO, CHU, ZI \rightarrow PI, PI \rightarrow ZI$

#### The $SAL$ <!-- IO! --> class (jumps)
The class $SAL$ (from the italian *"SALti"*, i.e. "jumps") identifies all jump operations. 

This set contains the following instructions:

- $SR \geq, SR <, SR =, SR \neq $
- $SA \geq, SA <, SA =, SA \neq $
- $SB \geq, SB <, SB =, SB \neq $
- $IP \geq, IP <, DP \geq, DP < $
- $SGI, SGO, S, ALT$
- SAB =, SAB \neq

#### The $CBP$ class (parity bit check)
The $CBP$ class (from the italian *"Controllo bit di parità"*, i.e. "parity bit check") identifies all operations communicating with external devices in input, for which a parity bit exists.

This set contains the following instructions:

- $EAP, ELZ, ETZ, EKZ, ENZ$

## Instruction types
Before execution, the operand addres $s$ of the instruction is modified. There are two ways to modifies an address, identifying two types of instructions.

### Ordinary instructions
In **ordinary instructions** the modification is computed by applying both parametric cells $P$ and $Q$. The final address of the operation is $c = s + q + p$. Most arithmetic instructions belong to this class. [[11](../helper_resources/references.md)]

⚠︎ Note that it is not important the order in which $c$ is evaluated (i.e. if we sum first by $q$ or $p$). [[11](../helper_resources/references.md)]

### Special instructions
In **special instructions** only parametric cell $Q$ is used, with $P$ being considered as a second address. The final address of the first operand is $c = s + q$. The parametric cell $P$ is used in different ways depending on the instruction.  [[11](../helper_resources/references.md)]

Special instructions are divided into:

- Operations over parametric cells.

- Jump instructions. 

- Transfer operations between Main Memory and Auxiliary Memory.

- Query instruction over a table.


## Using the final address [[11](../helper_resources/references.md)]
We identify as $c$ the final computed operand address. The instruction identifies the memory unit pointed by $c$.

- **If $c$ points to Auxiliary Memory** then all of its 15-bits determine the cell.

- **If $c$ points to Main Memory** then only 12-bits are used to determine the cell.

## Timing considerations
Computing the final address $c$ usually requires a memory access, which takes $10 \mu sec$. However if one of the parametric addresses refers to the first cell of its parametric group, then the CEP assumes its value is zero, and avoids to retrieve the content, not wasting any time. [[11](../helper_resources/references.md)]