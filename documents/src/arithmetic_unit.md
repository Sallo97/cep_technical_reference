# Decimal arithmetic unit
The CEP provides in-hardware support for decimal addition, subtraction, and multiplication. Most decimal computers of the era for supporting arithmetics would employ:

- **Look-up tables:** a series of hard-coded tables to storing results for additions, subtraction, and multiplication. To add two digits, the computer would perform a memory fetch to the entry in the associated table containing the result.

- **Specialized Programs:** Programs stored in Main Memory that implemented decimal operations as long sequences of basic machine-instructions.

This approach limits operation speed up to the Main Memory Cycle Time, which typically ranges between $2 \mu sec$ to $10 \mu sec$. [[1](../helper_resources/reference.md)]

## Moving from main memory to the arithmetic unit
By comparison, the cycle time of the Micro-Programming Control Unit (MCU) is far smaller, being $0.3 \mu s$. For this reason, **the CEP implements decimal arithmetic look-up tables directly within the Control Matrix $CM$ and Sub-matrix $SM$**, limiting the use of Main Memory and registers only for data transfers. [[1](../helper_resources/reference.md)]

### The decimal look-up tables
To perform the decimal addition, subtraction, and multiplication, the CEP employs ten look-up tables. These tables are physically mapped onto $100$ **intersection points** within sub-matrix $SM$, with their logic defined by $28$ rows in the Control Matrix $CM$. [[1](../helper_resources/reference.md)]

The tables are grouped as follows:

- **Addition tables**: four tables are employed to implement the addition of two decimal digits $i$ and $j$, where:

    - A 4-bit table computes the results of a normal addition, $i + j$.

    - A 4-bit table computes the result of the addition in the presence of a carry-in, $( i + j ) + 1$.

    - A 1-bit table detects the carry-in in the first table, $i + j \geq 10$.

    - A 1-bit table detect the carry-in the second table, $( i + j ) + 1 \geq 10$.

- **Subtraction tables:** four tables are employed to implement the subtraction of two decimal digits $i$ and $j$, where:

    - A 4-bit table computes the results of the normal subtraction, $A - B$.

    - A 4-bit table computes the result of the presence in the presence of a borrow-in, $( i - j ) - 1$.

    - A 1-bit table detects the borrow-in in the first table, $i < j$.

    - A 1-bit table detect the borrow-in in the second table, $( i - j ) - 1 \leq 0$.


- **Multiplication tables:** two tables are used, each retrieving one of the two digits $i$ $j$ of a product simultaneously, where:

    - A 4-bit table retrieves the **right digit** of the multiplication $i \times j$ (e.g. for $4 \times 5$ it retrieves $0$).
    
    - A 4-bit table retrieves the **left digit** of the multiplication $i \times j$ (e.g. for $4 \times 5$ it retrieves $2$).

## Technical Implementation of the Decimal Arithmetic Unit
The Decimal Arithmetic Unit is integrated directly into the Control Unit's signaling path.

Below we provide a schematics of the decimal arithmetic unit, including overlapping portions of the control unit and timing system:

![image](../../resources/decimal_arithmetic_unit_schematics.png)

### Shared components
The components $SM$, $CM$, $d$, $K_1^{'}$, $K_2^{'}$, $K-0$ and $O^{'}$ from the control system are used by the arithmetic logic. [[1](../helper_resources/reference.md)]

### Registers
$R^{'}, R, A$ are registers.

Register $R$ provide the carry/borrow signals $r$ and $\bar{r}$.

Registers $B$ and $C$ act as **selectors for the $K_c^{'}$ and $K_c^{''}$ switching circuits**, ensuring that the contents of the operand registers are routed into the matrix. [[1](../helper_resources/reference.md)]

The register set $H_1, H_2, H_3, H_4$ **define the arithmetic task in execution**, dictated by **only** one currently active:

- $H_1$: addition.

- $H_2$: subtraction.

- $H_3$: right-digit multiplication.

- $H_4$: left-digit multiplication.


Four 4-bit registers, $S, Y, T, X$, are designed for **coincidence selection**, using them as coordinates to "point" to a specific decimal digits stored within the arithmetic tables of $SM$. Register $O_1^{'}$ is used as an indicator during the cycle of a micro-program. Its output is $\alpha_1$. [[1](../helper_resources/reference.md)]


#### Logic types
Register $T$ shares the logic diagram of $K-0$ (interfaced with memory/sequencing), while $S, Y, $ and $X$ follow the logic of $O^{O^{'}}$ (interfaced with opcode/parameter bits). [[1](../helper_resources/reference.md)]

#### Addition and subtraction
$T$ and $S$ are used by the addition and subtraction microprograms to hold the operands. [[1](../helper_resources/reference.md)]

#### Multiplication
All four registers ($X, T, Y, S$) are used in the multiplication microprogram. For the micro-operations implementing right-digit multiplication and left-digit multiplication, only $X$ and $Y$ are used. [[1](../helper_resources/reference.md)]

#### Main memory communication
Registers $X$ and $T$ receive from main memory the requested arithmetic data. The $S$ output transfers to main memory the partial results of the operations, and to $T$ the intermediate results during the multiplication microprogram. [[1](../helper_resources/reference.md)]

#### Fetch phase
During the fetch phase, registers $X$ and $T$ hold the micro-order code stored in $O$ and partly in $O^{'}$. [[1](../helper_resources/reference.md)]

### Control pulse sequences
The synchronization of the arithmetic process is managed by a specific sequence of pulses $p$:

| Pulse           | Action                | Significance                           |
| -----           | --------------------- | ------------------------------------------|
| $p_4$           | Zeros $R$ and $R^{'}$ | Clears the results buffers for a new operation.           |
| $p_6, p_7, p_8$ | Zeros $T, S, O_1^{'}$ | Prepares the operand registers and the $O^{'}$ status flag. |
| $p_9$           | Writes $0001$ to $S$  | Loads the constant $1$ into $S$.                              | 
| $p_{10}$          | Sets $O_1^{'} to $1$$ | Asserting the status flag for the $O^{'}$ register.         |

### Switching circuits $K_c^{'}$ and $K_c^{'}$
Switching circuits $K_c^{'}$ and $K_c^{''}$ implements the **multiplexing logic** of the CEP, deciding whether to interpret the matrix output as a **jump address** or **numeric data**.

The behavior of these circuits is governed by the control signals $a, b, c$. These signals operate under **mutual exclusion**, that is at any operative cycle, only one signal may be asserted (i.e. set to $1$), while the others remain at zero. 

#### Mapping
The routing is determined according to the following truth table:

| Signals value      | Action                                 |
| -----------------  | -------------------------------------- |
| $a = 1, b = c = 0$ | Selects the micro-order address.       |
| $b = 1, a = c = 0$ | Selects the addition/subtraction data. | 
| $c = 1, a = b = 0$ | Selects the multiplication data.       |

## Arithmetics logic embedded in $SM$ and $CM$
The sub-matrix $SM$ acts as the primary execution engine of the CEP, containing $256$ columns. Each column represents a micro-operation. $100$ of them are reserved for micro-operations.

The relationship between $SM$ columsn and the $CM$ rows define the arithmetic capabilities of the CEP:

- **Arithmetic data rows:** $28$ rows of the $CM$ are specifically configured to store the "values" of the decimal look-up tables. 
When intersected by the arithmetic columns, the matrix "memorizes" the digits required for addition, subtraction, and multiplication. In the architectural schematics they are represed by **dashed lines**, indicating that they provide data and not control signals.

- **Control signal rows:** $4$ rows are dedicated to standard control signals. These are represented by **solid lines** in the architectural schematics.

### Arithmetic micro-order execution
The execution of an arithmetic micro-order differs from a standard control micro-order. While a normal micro-order simply performs a transfer and concludes, as the next address is always prefetched, an arithmetic micro-order need to do manage the routing of numeric data through the look-up tables before it can finalize the next address.

#### Micro-operation 1: operand selection and table addressing
Register $A$ is zeroed, while one register between $B$ or $C$, and one in the set $H_1, H_2, H_3, H_4$ are asserted (i.e. set to $1$).

$B$ and $C$ are updated to ensure that the contents of the operand registers are routed into the matrix.

The outputs of registers $H_1, H_2, H_3, H_4$ ($h_1, h_2, h_3, h_4$) work in tandem with the state of register R ( $r$ and $\bar{r}$), together selecting the correct output from one of the six 4-bit tables at circuit $K_t$.

$h_1, h_2$ and $r, \bar{r}$ selects at $K_r$ output the carry/borrow-bit of one out of the 1-bit tables.

#### Micro-operation 2: result transmission and epilogue
The calculated decimal digit is written into register $S$, and the resulting carry/borrow-bbit is latched into Register $R$ for use in the next cycle.

Registers $B$ and $C$ are zeroed.

Finally, register $A$ is set to $1$, allowing the next micro-order address to be selected at $K_1^{'}$ and $K_2^{'}$ outputs.

## Arithmetic microprograms

### Operational notations

- $\oplus$, $\times$, - : The Boolean operators (Sum, Product and Complement/NOT).

- $MM$ is the Main memory

- $0000, 0001$ are the binary coded decimal digits for $0$ and $1$.

- $C()$: Denotes the contents of a memory location, or a register, or an arithmetic table entry.

- $\rightarrow$: Indicates the transfer of data/address/constant to a register.

- **Table Entries:** we refer to entries in the look-up tables selected by $SM$ and coded by $CM$ as:

    - $T_a$: for Addition.

    - $T_{ac}$: for Addition with Carry.

    - $T_{rm}$: for Right-Digit Multiplication.

    - $T_{lm}$: for Left-Digit Multiplication.

- $\rho$: the carry-bit from $K_r$ output.

- For any micro-operation, if a signal ($r, \bar{r}, a, b, c, h_1, \ldots, h_4$) is not esplicitly listed, it is implied to be zeroed.


### Sequencing and memory
- $M_x$: represents the $x$-th micro-instruction in a sequence.

- $\mu{_1}^{(i)}, \ldots, \mu{_4}^{(i)}$ (with $i = 2, \ldots, 13$): the four possible branch addresses for the successor of micro-operation $M_i$, stored in $CM$.

- **Initialization:** the **fetch phase** is hard-wired to zero out registers $R, R^{'}$, and all loop counters.

### Loop control signals $c_1, c_2, c_3$
The arithmetic algorithms are governed by the three counters $c_1, c_2, c_3$, pulsed by $CC$:

| Signal | Instrumentaiton | Purpose |
| ------ | --------------- | ------- |
| $c_1$  | Transition to $1$ during $M_11$ (last multiplicand digit) or $M_2$ (last addend digit). | Signals the end of a numeric word (Most Significant Digit reached). |
| $c_2$  | Transitions to $1$ in $M_6$ (except for the first iteration); resets in $M_8$. | Manages the intermediate shifts during the multiplication loop. |
| $c_3$  | Transitions to $1$ during $M_{11}$ of the final multiplier digit. | Signlas the completion of the entire micro-operation. | 

### Arithmetic micro-operations
- $(M_a)$: addition micro-operation.

- $(M_{rm})$: right_digit multiplication micro-operation.

- $(M_{lm})$: left_digit multiplication micro-operation.

### Boolean expressions
The transfer of data, addresses, and constants is governed by a set of dynamic conditions. 

To evaluate these Boolean expressions:

- The status signals $r, \bar{r}$ (carry/borrow-bit) and $c_1, c_2, c_3$ (loop counters) act as variables.

- A value of $1$ is substituted when the condition is true (asserted), $0$ when false.

### Addition microprogram
| $M_x$ | Transfer | $R^{'}$ | $R$ | New Address | $h_j$ | $a/b/c$ | $p_k$ |
| ----- | -------- | ------- | --- | ------- | ----- | ------- | ----- |
| $M_1$ | $C(MM) \rightarrow S$ | / | / | $\mu_{1}^{(2)} \rightarrow 0$| / | $a_1$ | $p_1 = 1$
| $M_2$ | $C(MM) \rightarrow T$ | / | / | $\mu_{1}^{(3)} \rightarrow 0$ | $h_1 = 1$ | $b_1 = 1$ | $p_1 = 1$ |
| $(M_a)$ | $\bar{r} \times C(T_a) + r \times C(T_{ac}) \rightarrow S$ | $\rho \rightarrow R^{'}$ | $r^{'} \rightarrow R$ | / | / | $a = 1$ | $p_2 = 1$ |
| $M_3$ | $C(S) \rightarrow MM$ | $0 \rightarrow R^{'}$ | $r^{'} \rightarrow R$ | $\bar{c_1} \times \mu_1^{(1)} + (c_1 \times \bar{r^{'}}) \times \mu_2^{(5)} + (c_1 \times r^{'}) \times \mu_4^{(4)} \rightarrow 0$ | / | $a = 1$ | $p_1 = 1$ |
| $M_4$ | $0001 \rightarrow S$ | / | / | $\mu_1^{(3)} \rightarrow 0$ | / | $a = 1$ | $p_2 = 1$ |
| $M_5 \equiv E_0$ | / | / | / | / | / | / | / | 

### Multiplication microprogram
The multiplication micro-program follows a digit-by-digit approach, employing $X$ and $Y$ as the primary operand buffers:

- The multiplier is loaded into $X$, while the multiplicand is loaded into $Y$.

- As each digit of the multiplier is processed against the multiplicand, a partial result is formed. Instead of storing the entire growing product in internal registers, the CEP transfers these intermediate digits back into Main Memory through micro-operations $M_10$ and $M_13$.

- To calculate the final result, these sotred partial products are retrieved from memory during $M_7$ and $M_11$ and added to the next partial product being formed in the matrix.

| $M_x$ | Transfer | $R^{'}$ | $R$ | New Address | $\alpha_l$ | $h_j$ | $a/b/c$ | $p_k$ |
| ----- | -------- | ------- | --- | ------- | ---------- | ----- | ------- | ----- |
| $M_6$ | $C(MM) \rightarrow X$ | / | / | $\mu_1^{(7)} \rightarrow 0$ | / | / | $a = 1$ | $p_1 = 1$ |
| $M_7$ | $\bar{c_2} \times (0000) + c_2 \times C(MM) \rightarrow T$ | / | / | $\mu_1^{(8)} \rightarrow 0$ | / | / | $a=1$ | $p_1 = 1$ |
| $M_8$ | $C(MM) \rightarrow Y$ | / | / | $\mu_1^{(9)} \rightarrow 0$ | / | $h_3 = 1$ | $c = 1$ | $p_1 = 1$ |
| $(M_{rm})$ | $C(T_{rm}) \rightarrow S$ | / | / | / | / | / | $a = 1 $ | $p_2 = 1$ |
| $M_9$ | / | / | / | $\bar{\alpha_1} \times \mu_1^{(10)} + \alpha_1 \times \mu_2^{(12)} \rightarrow 0$ | / | $h_1 = 1$ | $b = 1$ | $p_2 = 1$ |
| $(M_a)$ | $\bar{r} \times C(T_a) + r \times C(T_{ac}) \rightarrow S$ | $\rho \rightarrow R^{'}$ | $r^{'} \rightarrow R$ | / | / | $a = 1$ | $p_2 = 1$ |
| $M_{10}$  | $C(S) \rightarrow MM$ | / | / | $(\bar{c_1} \times \bar{c_3}) \times \mu_1^{11} + (c_1 \times \bar{c_3}) \times \mu_2^{(6)} + (c_1 \times c_3) \times \mu_4^{(5)} \rightarrow 0$ | / | / | $a = 1$ | $p_1 = 1$ |
| $M_{11}$ | $\bar{c_2} \times (0000) + c_2 \times C(MM) \rightarrow T$ | / | / | $\mu_1^{(9)} \rightarrow 0$ | $\alpha_1 = 1$ | $h_4 = 1$ | $c = 1$ | $p_1 = 1$ |
| $(M_{lm})$ | $C(T_{lm}) \rightarrow S$ | / | / | / | / | / | $a = 1$ | $p_2 = 1$ |
| $M_{12}$ | $C(S) \rightarrow T$ | / | / | $\bar{c_1} \times \mu_1^{(8)} + c_1 \times \mu_2^{(13)} \rightarrow 0$ | $\alpha_1 = 0$ | / | $a = 1$ | $p_1 = 1$ |
| $M_{13}$ | $0000 \rightarrow S$ | / | / | $\mu_1^{(9)} \rightarrow 0$ | / | / | $a = 1$ | $p_2 = 1$ |
| $M_5 \equiv E_0$ | / | / | / | / | / | / | / | / |

#### Flow Diagram
Below a flow diagram of the two micro-programs:

![image](../../resources/flow_diagram_microprograms.png)