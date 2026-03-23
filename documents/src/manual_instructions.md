# Manual instructions
Manual instructions are instructions sent to the CEP through the control panel. They  request debug operations. [[11](../helper_resources/references.md)]

## Notation
Some common symbols used for describing the manual instructions:

- The word specified by the key pressed of keyboard $QZ$ are referred as $q_Z$.
- The address specified by the key pressed of keyboard $QI$ are referred as $q_I$.
- $Z_i$ we refer to the conte of memory cells at address $q_I + 1$.
- $t_S$ is the content of cell $S$ in the magnetic drum.
- $f_S$ the first punched word in a punched tape.

## The $Q \ TZ$ instruction
Requests to write into Main Memory, starting from cell $q_I$, a program stored in the magnetic drum starting in its first cell. Its main use case is for calling from the magnetic drum some fundamentals service programs. The indication of which program we want is done through keyboard $QZ$.[[11](../helper_resources/references.md)]

This instruction also changes the origins of the $\{H^{0}\}$ parametric group. setting it at value $q_I$. It also stores in $q_{I}+1$ and $q_{I}+2$ the old value of $h^{0}$ and the numerator $n$. [[11](../helper_resources/references.md)]

The program called over the magnetic drum is transferred using blocks of words. There words write starting at position $q_{I} + 3$.[[11](../helper_resources/references.md)]

Defining $Z_i$ the content of memory cells at address $q_I +i$ and with $t_S$ the content of the cell $S$ in the magnetic drum, then we write:

1. $Z^{'}_{1} = h^{0}$

1. $Z^{'}_{2} = n$

1. $Z^{'}_{2 + s} = t_s$ with $(s = 1 \ldots t_0)$

1. $h^{'}_0 = q_I$

1. $n^{'} = q_I + 3$

## The $Q \ LZ$ instruction
Instruction $Q \ LZ$ is used for writing into Main Memory a block of binary information, reassembling them with six characters (each of 6-bits) per word. The blocks come from a punched tape, following a similar behaviour as instruction $Q \ TZ$. [[11](../helper_resources/references.md)]

Indicating $f_S$ as the the first punched word in the tape:

1. $Z^{'}_1 = h_0$: saving old origin of parametric group $\{H_0\}$.

1. $Z^{'}_2 = n$: saving old value of $N$.

1. $Z^{'}_{2 + s} = f_S$ withh $s = 1 \ldots f_o$: trasfering program from the magnetic drum to the main memory.

1. $h_0^{'} = q_I$: changes the origin of parametric group $\{H_0\}$

1. $n^{'} = q_I + 3$: jump to the first instruction of the loaded program.

## The $Q \ SI$ instruction
Instruction $Q \ SI$ does an unconditional jump to address $q_I$. [[11](../helper_resources/references.md)]

 $$n^{'} = q_I$$

## The $Q \ ZI$ instruction
Instruction $Q \ ZI$ writes in the memory cell at address $q_I$, identified as $Z_I$, the word $q_Z$. The instruction **does not** modify the numerator $N$. [[11](../helper_resources/references.md)]
$$Z^{'}_I = q_Z \\ n^{'} = n$$

## The $Q \ IV$ instruction
Instruction $Q \ IV$ is needed for reading the word at memory cell of address $q_I$, identified as $Z_I$, without changing the machine's state. The retrieved data is displayed in the control panel when the machine is idle. [[11](../helper_resources/references.md)]

## The $ZZZ$ instruction
Instruction $ZZZ$ zeros the Main Memory, having all entries contain the $ALT$ instruction (i.e. $0000\ldots000$). It does not modify any register. The machine stops right after. [[11](../helper_resources/references.md)]

## The $IES$ instruction
Instruction $IES$ allows the user to execute any instruction written in $QZ$, **without automatically increasing the program counter (i.e. numerator $N$).** [[11](../helper_resources/references.md)]