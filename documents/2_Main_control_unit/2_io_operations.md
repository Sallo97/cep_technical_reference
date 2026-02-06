# Input-Output Operations
The communication between the Central Processing Unit (CPU) and external peripherals is typically asynchronous, requiring significant coordination challenges: the CPU needs to synchronize its internal operations with external components that request services at unpredictable intervals.

Each I/O device is assigned a **priority level**, i.e. a numerical rank, which determines an hierarchy of importance among the current operations and incoming incoming interruptions.

The priority level is determined generally by the buffer capacity, timing constraints, and data transfer rate:

1. **Essential Interruptions (High-Priority):** devices with limited buffer size that can only store a small portion of the data to transfer (e.g. a single word from a high-speed magnetic tape). When an operation of this type is suspended, **the buffer could lose its content within a strict real-time window**. If the CPU fails to return to the execution in time, the previous data is overwritten, resulting in a transfer error and data loss.

1. **Deferred Interruptions (Low-Priority):** devices whose buffers can hold an entire data record or transfer unit. While the CPU should execute their operations as fast as possible, to keep the channel available for other tasks, **there is no risk of data loss if the service is delayed**. 

Additionally, multiple interruption signals could come out simultaneously from different units, having the same priority. In this case the priority classification depends on the **length of time the transfer can be kept waiting and on the equipment rate of data flow**.

## Implementing I/O Organization

Managing priority levels coordinate interruption execution cannot rely only on hardware logic. On the software-side one or more **special programs** are defined, executed only when a valid interrupt request is recognized.

Each special program is divided in two parts:

1. The special micro-order sequence initializing the special program, **stored inside the architecture**. It cannot be part of the standard micro-order set for safety reason, **it must be called only by the hardware**.

1. The rest of the program in main memory.

Below a graphical rapresentation of the components components governing interruption=handling and communicating with the CPU:

![image](../../resources/io_circuit.png)

## The Special Micro-Order and the $F_p$ Register

To ensure that the interrupt entry point stays distinct from the computer's normal micro-orders, **a special column is appended to matrix $MC$, being completely isolated from the decoder $DC$**. 


The activation of this column is governed by the register $F_p$. 

When $F_p$ is set, the decoder $DC$ is zeroes, avoiding that a standard micro-order can be fired. Simultaneously $F_p$ activates the special column in the matrix, which will execute the special micro-order.

## Interrupt Priority Levels as Registers
The interrupt priority levels levels are assigned to three distinct flip-flops, $F_1, F_2, F_3$, ordered from lower priority to highest priority. When a device requires an interruption, it will set to $1$ the flip-flop associated to its priority level. As soon as one is set, **a priority interruption is requested**.

### Enforcing Priority Hierarchy
The signals are fed into register $P$, which, select among the active priority level requests the one with the highest rank (i.e. selects the address of the special program with highest priority). 

### Propagating the Interrupt request
The architecture should propagate the interrupt request to start execution of the special micro-order. The outputs of $F_1, F_2$,and $F_3$ are monitored by an OR-gate to detect any pending request. However the computer execution cannot be interrupted while a micro-order hasn't finished its execution. An AND-gate acts as a guard, requiring the control signal $x_c$ to be active. $x_c$ is set by each micro-order after it terminates. The signal is then propagated to register $F_0$. $F_0$ receives also as input a signal from the Timing System, telling him when the current cycle is ended

Finally, the interrupt routine can start at the beginning of the next cycle. A final AND-gate synchronizes the interrupt request with the timing system, propagating the signal to $F_p$ at the start of the new cycle.

Note that the special micro-order will zero the control signal $x_c$.

### Timing Considerations
The time interval from interruption request to access to the special program is called $\Delta T_{i}$ and depends on the inequality:

$\Delta T_{D_{1}} \leq \Delta T_{i} \leq \Delta T_{D_{0}} + 2 \times \Delta T_{D_{1}}$

Recall that $\Delta T_{D_{1}}$ and $\Delta T_{D_{0}}$ are the time interval of the delay circuits $D_0$ and $D_1$ of the Timing System.

### Special Interrupt 
Only the initial special micro-order is handled by hardware, avoiding completely decoder $DC$. After the routine begins, the next micro-orders are determined standardly through the decoder. 

The special micro-order branches into one of three micro-orders, each corresponding to a priority level. The chosen pick then branches to one of three possible micro-orders, again corresponding to a priority level.

This process repeats until all the portion of the special program inside the architecture has been completed. The CEP now needs to retrieve the rest of the procedure from Main Memory. The address of the remaining fragment is computed by register $P$.

### Picking the Higher Priority Address
Register $P$ is responsible for generating the starting address of the "second portion" of the interrupt program. When multiple interrupt requests from different priority levels ($F_1, F_2, F_3$) are triggered simultaneously, Register $P$ selects address of the highest ranking request, following the precedence rule:

$\mu_1 \prec \mu_2 \prec \mu_4$ 


<!-- The precedence is valid if the following signals are setted as requested: 

$m_h^{'} = m_l^{'} = m_l^{''} = 1 \land a_h = p_2 \land a_1 = p_3$ -->

### Saving Previous State
The CEP needs to guarantee that it can resume the interrupted program execution state after the interruption process is completed.

Within the MCU, the address of the next micro-instruction in the original sequence is held in register $0$. This value must be copied before the special micro-order take control of the architecture and generates its next address. 

The original address is transferred through control signals $x^{(p)}$, which can store it either in the sequential circuits $A, B, $ or $C$.

Beyond the return address, any other data from the original program should be preserved in order to avoid that the interrupt routine overwrited it.

After the interruption ended all this content must be restored.