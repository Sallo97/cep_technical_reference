# Instructions[[11](./0_reference.md)]
CEP's instructions all are long a memory word and follow a 1-address scheme, i.e. they can refer to one memory address used as ope

The sequence of bits of an instruction $(s_0 s_1 s_2 \ldots s_8 s_9 \ldots s_{14} s_{15} \ldots s_{20} s_{21} \ldots s_{35})$ is divided into six groups:

- **the pseudoinstruction flag (1-bits):** $s_0$ determines if the current instruction is a pseudoinstruction or not[[3](./0_reference.md)].
    
- **the automatic check flag (1-bits):** $s_1$ determines if we want execution to automatically check correctness of the instruction [[3](./0_reference.md)].

- **the operation code (7-bits):** $(s_2 \ldots s_{14})$ defines the command to execute.

- **first parametric address (6-bits):** $(s_p = s_9 \ldots s_{14})$ refers to the parametric cell $P$. Recall that the first bit is the group index and the last five bits are the relative address. We designate $p$ as the content of $P$.

- **second parametric address (6-bits):** $(s_q = s_{15} \ldots s_{20})$ refers to the parametric cell $Q$. Recall that the first bit is the group index and the last five bits are the relative address. We designate $1$ as the content of $Q$.

- **memory address (15-bits):** $(s = s_{21} \ldots s_{35})$ is the integer number representing the address of the instruction.

![image](../../resources/cep_instruction.svg)

The total number of normal instruction, i.e. those implemented through micro-programs of the Micro-program Control Unit, is $128$[[4](./0_reference.md)]. From now on we will call normal instructions simply instructions.

## The bit $s_0$ 
The first bit of an instruction, indicated as $s_0$, specifies if the instruction is a pseudoinstruction or not:

- $s_0 = 0 \rightarrow$ is a normal instruction.

- $s_0 = 1 \rightarrow$ is a special instruction.

Pseudoinstructions at their core are subprograms which can be called like normal instructions. The main differenze is that normal instructions are executed through microprograms statically stored in the Control Unit of the computer, while pseudoinstructions refer to subprograms in Memory.

### How the CEP behaves when a pseudoinstruction is detected
When the CEP detects a pseudoinstruction, a particular jump is done, in which $o$ and $n$ (more precisely $n+1$) are kept in some memory lication. The address of the jump is obtained by the first bits of the pseudoinstruction.

### How the jump is done
When $s_0 = 1$ then we identify as $s^{*}$ the sequences of subsequent eight bits ($s^{*} = s_1 s_2 \ldots s_8$ s.t. $0 \leq s^{*} \leq 255$). 

The current memory address $o$ (i.e. the address of the operand) is copied in the cell of index $1$ in the global parametric group $\{H^{1}\}$; $n$ (i.e. the instruction counter) is incremented and copied in $N$. Then a jump, **which does not modify N**, is done at location $h^{1} + s^{*}$. The location **always points** to an instruction which calls a subprogram and stores the content of registers $N$ in a parametric cell. In this way the called subprogram can easily retrieve $o$, for using it as its operand, and $n+1$, to call the next instruction.

### Properties of $s^{*}$
The address of the subprogram is computed as $h^{1} + s^{*}$. Recall that $h^{1}$ is the start address of parametric group $\{H^{1}\}$, which has reserved 32 cells. These cells are reserved, so $s^{*}$ must not refer to them. Additionally the three cells after $\{H^{1}\}$ are also reserved for the bit $s_1$ and thus cannot be touched by $s^{*}$.

This means that $35 \leq s^{*} \leq 255$. 

### The distinction between $H^{0}$ and $H^{1}$
$\{H^{1}\}$ is reserved to subprograms; while $\{H^{0}\}$ is shared among programs.


## The bit $s_1$
It is used for determining if the instruction should be automatically checked or not.

Let's consider for example the set of arithmetic instructions, for each of them it is convenient to automatically check for a possible overflow. 
Then in the CEP if:

1. we set for the arithmetic instruction $s_1 = 1$.

1. during the execution of the instruction an overflow occurs (i.e. $\tau = 1$).

1. in the control panel a specific button has been pressed.

then, the CEP instead of moving to the next instruction (i.e. $n+1$), it instead jumps to address $h^{1} + 32$. This instruction behaves like the one of pseudoinstructions, jumping to an associated program (in this case handling the overflow), ensuring that $n+1$ is kept somewhere safe.

Note that the special address $h^{1} + 32$ is associated for automatic checking of arithmetic instruction; other kind of instructions points to a different offset.

Instructions are divided into five classes, each identifying the type of automatic checking available among the members. 

We define the following notation:

- $\gamma_i$ identifies the key associated to a class in the control panel s.t.:

    - $y_i = 1 \rightarrow$ the button is pressed.
    - $y_i = 0 \rightarrow$ the button is raised.

- $\delta_i$ refers to the class of the instruction.

- $\tau_{\phi}$ is the overflow flag for fixed point arithmetics; $\tau_{\mu}$ is the overflow flag of the exponent in a floating point operation; $\tau_{\pi} is the parity flag$.

Then we can define the jump operation for a class as $X_i$:

- **$TVF$ class:** $X_1 = s_1 \times \gamma_1 \times \delta_1 \times \tau_{\phi} \rightarrow h_1 + 32$

- **$TVM$ class:** $X_2 = s_1 \times \gamma_2 \times \delta_2 \times \tau_{\mu} \rightarrow h_1 + 33$

- **$ICP$ class:** $X_3 = s_1 \times \gamma_3 \times \delta_3 \rightarrow h_1 + 34$

- **$SAL$ class:** $X_4 = s_1 \times \gamma_4 \times \delta_4 \rightarrow h^{1} + 35$

- **$CBP$ class:** $X_5 = s_1 \times \gamma_5 \times \delta_5 \times \tau_{\pi} \rightarrow h_1 + 36$

Note that, with the exception of the jump instructions of class $SAL$, all conditions deermine the jump to a checker program **after** the associated instruction has been executed. In the case of jump instruction (i.e. condition $X_4$) the check is done **before** the requested operation is executed. This inversion is done to being able to check both the address before and after a jump.

## Instruction classes
Instructions are divided intro classes, depending on the kinds of automatic checks can be done onto them.

### The $TVF$ class (fixed point overflow)
The class $TVF$ (from the italian *"Traboccamento Virgola Fissa"*, i.e. "fixed point overflow") identifies all fixed point arithmetic operations which can lead to overflow. In this set we have:

- $COR$, $VS$, $WS$, $BIT$
- $MOA$, $MOU$, $MOE$, $DCA$, $DLA$, $DLE$

### The $TVM$ class (floating point overflow)
The class $TVM$ (from the italian *"Traboccamento Virgola Mobile"*, i.e. "floating point overflow") identifies all floating point operations which can lead to overflow. In this set we have:

- $-Z \rightarrow F, F + Z \rightarrow F, F - Z \rightarrow Z, -F \rightarrow F$
- $F + \|Z\| \rightarrow F, F - \|Z\| \rightarrow F, MF, DF$

### The $ICP$ class (parametric cell instructions)
The class $ICP$ (from the italian *"Istruzioni Celle Parametrice"*, i.e. "parametric cells instructions") identifies all instructions which operate over parametric cells, considering comparison instruction and instructions for changing the origin of parametric groups. In this set we have:

- $C \rightarrow P, -C \rightarrow P, P + C \rightarrow P, P - C \rightarrow P$
- $Z \rightarrow P, -Z \rightarrow P, P + Z \rightarrow P, P - Z \rightarrow P$
- $P \leftrightarrow Q, P \rightarrow Z, A + C \rightarrow P, B + C \rightarrow P$
- $VDA, RAZ, RAC, RZP, RCP$
- $CHO, CHU, ZI \rightarrow PI, PI \rightarrow ZI$

### The $SAL$ <!-- IO! --> class (jumps)
The class $SAL$ (from the italian *"SALti"*, i.e. "jumps") identifies all jump operations. In this set belongs:

- $SR \geq, SR <, SR =, SR \neq $
- $SA \geq, SA <, SA =, SA \neq $
- $SB \geq, SB <, SB =, SB \neq $
- $IP \geq, IP <, DP \geq, DP < $
- $SGI, SGO, S, ALT$
- SAB =, SAB \neq

### The $CBP$ class (parity bit check)
The $CBP$ class (from the italian *"Controllo bit di parità"*, i.e. "parity bit check") identifies all operations for communication with external devices in input and for which exists a parity bit. In this set belongs:

- $EAP, ELZ, ETZ, EKZ, ENZ$

## Instruction types
Before execution, the addres $s$ of the instruction is modified. There are two ways to modifies an address, identifying two types of instructions.

### Ordinary instructions
In **ordinary instructions** the modification is computed by applying two modifications depending on both parametric cells $P$ and $Q$. The final address of the operation is $c = s + q + p$. Most arithmetic instructions belong to this class.

⚠︎ Note that it is not important the order in which $c$ is evaluated (i.e. if we sum first by $q$ or $p$).

### Special instructions
In **special instructions** the modification only uses the parametric cell $Q$; the final address of the operation is $c = s + q$. The parametric cell $P$ is used in different ways depending on the instruction. For this instructions the CEP works as a 2-address machine.

Special instructions are divided into:

- Operations over parametric cells.

- Jump instructions. 

- Transfer operations between Main Memory and Auxiliary Memory.

- Query instruction over a table.


## Using the final address
The instruction defines which type of memory is referred by the final address $c$:

- **If it refers to Auxiliary Memory** then all 15-bits of $s$ are used to determine the cell.

- **If it refers to Main Memory** then all 12-bits of $s$ are used to determine the cell.

## Timing considerations
Each operation done to compute the final address $c$ requires a memory access, which costs $10 \mu sec$. However if one of the parametric address refers to the first cell of its parametric group, then the CEP assumes its value is always zero, and thus does not sum its content and goes one without any time wasted.