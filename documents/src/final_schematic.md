# General Architecture [[1](../0_Additional_resources/0_reference.md)]
In typical computer architectures of the era, the cardinality of the control store (i.e. the number of supported micro-orders) usually held between $n = 2^{8}$ and $n = 2^{9}$, with the latter being the preferred choice. The ratio $k$ of low-level micro-orders to higher-level macro-orders (i.e. machine instructions) was rather large, ranging between $4 \leq k \leq 8$, indicating that serveral micro-steps were necessary to complete a single machine command.

In comparison, the CEP was more efficient, utilizing a total of $n = 2^{8}$ micro-orders and a ratio of only $k = 2$. This makes the computer a highly dense micro-programming scheme were each micro-step performed multiple parallel operations.

# CEP Execution and Signaling [[1](../0_Additional_resources/0_reference.md)]
A defining feature of the CEP's execution cycle is that **the *fetch* and *execute* phases are runned simultaneously**. This is possible because the logic governing these phases is entirely decoupled form the Control Matrix ($MC$).

All internal signals within the CEP are implemented as levels (i.e. steady state voltages), simplifying the logic synchronization and ensuring that control signals remain stable.

# Technology used in the implementation [[1](../0_Additional_resources/0_reference.md)]s
Drawing inspiration from the Manchester Atlas computer, the CEP utilizes in its architecture the **ferrite rod storage technique** pioneered by Kilburn and Grimsdale. This technology is implemented within the read circuits and column drives, allowing for a high-density Control Matrix of $256 \times 240$.

## Matrix Driving and Sub-matrices
Rather than employing a direct-drive system, which would require a prohibitive number of active components, the CEP employs a **submatrix architecture with diodes and linear transformers**.  The design works because the electrical performance of the system remains largely unaffected by the total number of ferrite rods installed in any given row or column. The transformers provide the necessary current gain to drive the inductive sensing logic across the large matrix.

## Timing and Performance

Using the ferrite rod storage technique the CEP performance was:

- **Read-time:** $35 ns$ for sensing the state of a rod.

- **Cycle-time:** $300 ns$ for fetching and stabilizing a micro-instruction.

These values are **position-independent**, that is **the latency of the ferrite rod store is constant**, regardless of their position within the matrix.

# Final architecture schematic
The integration of magnetic components (i.e. the ferrite rod store) into the Control Matrix necessitates a shift from purely level-based signaling to pulse-driven control signals. To maintain stability and synchronization across the CPU, the schematics we have provided need some modifications.

Below a complete schematic of the final CEP architecture:

![image](../../resources/cep_overview_schematics.png)

## Signal Staticization
Because the Timing System and the control matrix operate on pulsed signals, the control signals $x_c, x_{c_1}, x_{c_2}$, controlling sequential circuits $A, B, C$ and AND-gates in the Timing System, **must be staticize**. This means that transient pulses are latched into a stable state (i.e. a level) so they remain active for the entire duration required by the logic gates, preventing synchronization errors.

## Decoder $DC$ and matrix $CM$
The micro-instruction decoder $DC$ and control matrix $CM$ have been divided into submatrix $SM$. The new component is pulse-driven and has two sub-decoders with level outputs.

The timing circuit cycles are closed through $SM$ and $CM$.

## Delay $D_0$ Decomposition
In the final architecture, the primary delay component $D_0$, responsible for the stabilization of control signal $x_{c_1}$, is partitioned into two distinct sub-components to manage the transition between levels and pulses:

- $D_0^{'}$ given as input to sub-matrix $SM$. This delay ensures that the level-based signals $x_{c_2}$, at AND-gates $A_2$ and the $Y$ inputs to registers $R_A$ and $R_C$, remain constant and stable while synchronized pulses are dispatched to the $R_A, R_C$ and $0^{'}$ registers.

- $D_0^{''}$ given as output to control matrix $CM$. This delay stabilizes the $x_{c_1}$ level and AND-gates $A_1$, ensuring a secure selection of the next operative cycle.

## Circuit K-0
The $K-0$ circuit consolidates the functions previously handled by the independent $K_0$ and $0$ circuits.s

During the execution of the $E_0$ micro-order (the sequence triggered at the conclusion of every micro-program), a pulse from AND-gates $A_2$ writes the contents of Main Memory into register $0^{'}$. This effectively initiates the fetch phase for the next machine-level instruction.

![image](../../resources/k-0_schematics.png)

## Handling the special micro-order
The recognition of an I/O interrupt is handled by the call to a special micro-order. In the previous attempt, this signal was instrumented through flip-flop $F_p$.

Now, this signal, labeled $f_0$, comes from flip-flop $F_0$ and enters AND-gates $A_3$. When $f_0$ is set to $"1"$, the cycle-pulse is diverted to drive the special micro-order directly. 

## Potential Simplifications
Some further simplifications were also considered, even though they were not implemented in the final CEP hardware, by removing the dependency of $x_{c_1}$ and $x_{c_2}$ on conditioning circuits like $K_3^{'}$:

1. **Eliminating $A_1$:** if in $x_{c_1}$ the conditioning is removed, the $A_1$ gates and $D_0^{'}$ delay are no longer necessary. The delays $D_1,D_2$ and $D_3$ would instead receive pulse control signals directly from the $CM$ outputs.

1. **Eliminating $A_2$:** if in $x_{c_2}$ conditioning is removed, the write pulses for registers $R_A$ and $R_C$ could be triggered directly by individual $CM$ outputs.
