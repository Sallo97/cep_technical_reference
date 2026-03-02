# Manual instructions
Manual instructions are instructions sent to the CEP through the control panel. They mainly correspond to debug operations.

## The $Q \ TZ$ instruction
It is needed for writing into memory, starting from memory cell $q_I$ (setted in keyboard $QI$), a program written in the magnetic drum (starting in its first cell). This program is needed for calling from the magnetic drum or magnetic tables some fundamentals service programs. The indication of which program we want is done through keyboard $QZ$.

This instruction also changes the origins of the $\{H^{0}\}$ parametric group. setting it at value $q_I$. It also stores in $q_{I}+1$ and $q_{I}+2$ the old value of $h^{0}$ and the numerator $n$. 

The program called over the magnetic drum is transferred using blocks of words. There words write starting at position $q_{I} + 3$.

Defining $Z_i$ the content of memory cells at address $q_I +i$ and with $t_S$ the content of the cell $S$ in the magnetic drum, then we write:

1. $Z^{'}_{1} = h^{0}$

1. $Z^{'}_{2} = n$

1. $Z^{'}_{2 + s} = t_s$ with $(s = 1 \ldots t_0)$

1. $h^{'}_0 = q_I$

1. $n^{'} = q_I + 3$

## The $Q \ LZ$ instruction
Instruction $Q \ LZ$ is used for writing into Main memory a block of binary information reassembling them with six characters (each of six bits) per word. The blocks come from a punched tape, following a similar behaviour as instruction $Q \ TZ$. 

Indicating $f_S$ as the the first punched word in the tape:

1. $Z^{'}_1 = h_0$: saving old origin of parametric group $\{H_0\}$.

1. $Z^{'}_2 = n$: saving old value of $N$.

1. $Z^{'}_{2 + s} = f_S$ withh $s = 1 \ldots f_o$: trasfering program from the magnetic drum to the main memory.

1. $h_0^{'} = q_I$: changes the origin of parametric group $\{H_0\}$

1. $n^{'} = q_I + 3$: jump to the first instruction of the loaded program.

## The $Q \ SI$ instruction
Instruction $Q \ SI$ is needed for doing a unconditional jump to the instruction whose address is written in $QI$.

1. $n^{'} = q_I$

## The $Q \ ZI$ instruction
Instruction $Q \ ZI$ writes in cell $Z_I$ of address $q_I$ the word $q_Z$ setted in $QZ$. The instruction **does not** modify the numerator $N$.
$$Z^{'}_I = q_Z \\ n^{'} = n$$

## The $Q \ IV$ instruction
Instruction $Q \ IV$ is nedded for reading the word at cell $Z_I$ of address $q_I$, withouth changing the machine's state. This word value is displayed in the control panel when the machine is idle.

$$n^{'} = n$$

## The $ZZZ$ instruction
Instruction $ZZZ$ zeros all memoy cells. Each memory cell contains the $ALT$ instruction. It does not modify any reigster. The machine stops right after.

## The $IES$ instruction
Instruction $IES$ allows the user to execution any instruction written in $QZ$, **without automatically increasing the program counter (i.e. numerator $N$).**