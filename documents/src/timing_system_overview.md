# Timing system overview
The Timing System (in italian *"Temporizzatore Interno"*) governs the electrical pulses which dictate **when** data can be read from or written to the CEP. The device is non considered part of the Micro-programming Control Unit (MCU), but it is physically connected to it for coordinating its execution. [[2](../helper_resources/reference.md)]

Unlike modern-day clock systems which provide a single periodic signal, Timing Systems of early computers produced a sequence of **micro-timing phases**. Each phase was responsible for triggering specific, coordinated internal operations during a single micro-instruction cycle. [[1](../helper_resources/reference.md)]

## Conditions
Conditions can influence "in the meanwhile" or at the "end" of a micro-instruction:

- **conditions which influence in the meanwhile** of micro-instruction comes form conditional networks of the Control Unit and come back to it after modifying commands $\psi_i$.

- **conditions which influence at the end** of a micro-instruction come from the Control Unit. More precisely they come form the networks which determine the next micro-instruction of the micro-program.


## Temporal classes
Each micro-order in the MCU lasts for a time interval $t_s^{'} - t_s$, where:
- $t_s$  is the **triggering edge**, it specifies when the time pulse starts.
- $t_s^{'}$ specifies when the processing and transfer of information requested by the micro-operation is done, i.e. when the hardware reaches a **stable state**. [[2](../helper_resources/reference.md)]

A Temporal Class identifies a set of micro-orders sharing the same temporal cycle. [[2](../helper_resources/reference.md)]

## Temporal cycles
For a given time class, its Temporal Cycle is the **discrete amout of time time necessary for each member to let the hardware assert/sample/store its signals**. It must be at least as much as the time-interval of the micro-orders. [[2](../helper_resources/reference.md)]

## Selection time
The Selection Time is the delay required to activate a specific register, bus line, or memory element and for it to become ready. Its value is defined as the interval between issuing a control signal from the timing system and the moment the targeted component responds or becomes stable. Selection time is not identical to combinational propagation delay, but it constrains the minimum duration of a temporal cycle. [[2](../helper_resources/reference.md)]

## Timing System Structure
We introduce some nomenclature:

- $°^{(p)}$: describes that component $°$ has **degree of parallelism** set to $p$ (i.e. the amount of bits available in it).

- $x_{c}$ are **control signals**.

- $\alpha_i$ and $\beta_{i}$ are **conditions signals**.

- $R_j, R_k$ and $R_l$ are **registers**.

- $\forall k \in \{0, 1, 2, 3,4\}.D_k$ is a **delay circuit**. Its primary function is to propagate the input signal to the output without logical modification, introducing a precise, calibrated delay during its transfer.


![image](../../resources/cep_timing_system.svg)

### Time intervals considerations
As the Timing System shows, we have multiple delay circuits. During a micro-order execution, all micro-orders will pass through $D_0$, then the path diverges depending on its timing class.

We define $\Delta T_{D_{x}}$ as the time-interval of delay circuit $D_x$. 

From the schematics we can note that $D_1$ is not defined, rather there are sub-delays $D_1^{'}, D_1^{''}, D_1^{'''}$. During execution, when passing through $D_1^{'}$ two possible sequences are possible:

1. $D_1^{'} \rightarrow D_1^{''} \rightarrow D_1^{'''}$

1. $D_1^{'} \rightarrow D_1^{'''}$

both sharing the same time interval amount. Since from a timing point of view the two cases are equivalent, we can abstract the three sub-components in the single delay circuit $D_1$. 

All delay circuits **except $D_0$** share the same time interval, meaning $\Delta T_{D_{1}} = \Delta T_{D_{2}} = \Delta T_{D_{3}} = \Delta T_{D_{4}}$.

Finally, the time-interval for execution all micro-operations (regardless of their time class) is: $\forall i \in \{1,2,3,4\}\Delta T_{D_0} + \Delta T_{D_i}$.

## Control signals considerations
In the Timing System there are two sets of control signals:

- $x_{c_1}$ **selects the temporal class of the micro-operation**. 

- $x_{c_2}$ **select the registers that should be activated by the micro-operation**.

$D_0$ ensures that signals in $x_{c1}$ are stabilized.

