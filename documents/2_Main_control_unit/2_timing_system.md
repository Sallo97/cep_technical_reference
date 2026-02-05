# Timing System [[1](../0_Additional_resources/0_reference.md)]
The Timing System (called in italian *"Temporizzatore Interno"*) govern the electrical time pulses which dictate **when** data can be consistently read from or written to in the CEP.

The device is non considered part of the Micro-programming Control Unit (MCU), but it is connected to it for coordinating its execution.

Unlike modern-day clock systems which provide a single periodic signal, Timing Systems of early computers produced a sequence of **micro-timing phases**. Each phase was responsible for triggering specific, coordinated internal operations during a single micro-instruction cycle.

## Temporal Classes
Each micro-order in the MCU lasts for a time interval $t_s^{'} - t_s$ where:
- $t_s$  is the **triggering edge**, it specifies when the time pulse starts.
- $t_s^{'}$ specifies when the processing and transfer of information requested by the micro-operation is done, i.e. when the hardware reaches a **stable state**.

**The time interval defines the amount of time necessary for the micro-order to be executed.**

A Temporal Class identifies a set of micro-orders sharing the same temporal cycle.

## Temporal Cycles
For a given time class, its Temporal Cycle is the **discrete amout of time time necessary for each member to let the hardware assert/sample/store its signals**. It must be at least as much as the time-interval of the micro-orders.

## Selection Time
The Selection Time is the **delay required to activate a specific register, bus line, or memory element and for it to become ready** so that it can perform its function. Its value is defined as the interval between issuing a control signal from the timing system and the moment the targeted component responds or becomes stable.


Selection time is not identical to combinational propagation delay, but it constrains the minimum duration of a temporal cycle.

## Timing System Structure
Before diving into it, we introduce some nomenclature:

- $°^{(p)}$: describes that component $°$ has **degree of parallelism** (i.e. the amount of bits available in it) set to $p$.

- $x_{c}$ are **control signals**.

- $\alpha_i$ and $\beta_{i}$ are **conditions signals**.

- $R_j, R_k$ and $R_l$ are **registers**.

- $\forall k \in \{0, 1, 2, 3,4\}.D_k$ is a **delay circuit**. Its primary function is to propagate the input signal to the output without logical modification, introducing a precise, calibrated delay during its transfer.


![image](../../resources/cep_timing_system.svg)

### Time intervals considerations
As the Timing System shows, we have multiple delay circuits. During a micro-order execution, all micro-orders will pass through $D_0$, then **the path changes depending on its timing class**.

We define $\Delta T_{D_{x}}$ as the time-interval of delay circuit $D_x$. 

In the timing system $D_1$ is not defined, rather there are sub-delays $D_1^{'}, D_1^{''}, D_1^{'''}$. During execution, when passing through $D_1^{'}$ two possible sequences are possible:

1. $D_1^{'} \rightarrow D_1^{''} \rightarrow D_1^{'''}$

1. $D_1^{'} \rightarrow D_1^{'''}$

both sharing the same time interval amount. Since from a timing point of view the two cases are equivalent, we can abstract the three sub-components in the single delay circuit $D_1$. 

All delay circuits **except $D_0$** share the same time interval, meaning $\Delta T_{D_{1}} = \Delta T_{D_{2}} = \Delta T_{D_{3}} = \Delta T_{D_{4}}$.

Finally, **the time-interval for execution all micro-operations, regardless of their time class**, is: $\forall i \in \{1,2,3,4\}\Delta T_{D_0} + \Delta T_{D_i}$.

## Control Signals considerations
In the Timing System there are two sets of control signals:

- $x_{c_1}$ **selects the temporal class of the micro-operation**. 

- $x_{c_2}$ **select the registers that should be activated by the micro-operation**.

$D_0$ ensures that signals in $x_{c1}$ are stabilized.

A conditional control signal is a **signal whose activation depends on the machine's current state or some status flag**. $x_{c_1}$ and $x_{c_2}$ are conditional. By conditioning both signals at the same time it is possible, as in the case of repetitive micro-orders, to block the pulses at register $O$ at the end of the first micro-operation and to decrease (because the control unit is not operating at the moment) the cycle time in the successive micro-operation.


## Temporal classes of the CEP
In the Timing System, each temporal class is associated with the delay circuit its members will be propagated to.

### $D_1$ - Memory operations
All operations regarding memory will propagate over the abstract delay circuit $D_1$. As said previously, two different paths are available after passing over $D_1^{'}$, each tacklinf memory differently:

- The path $D_1^{'} \rightarrow D_1^{'''}$ identifies two kinds of micro-operations:
    
    1. Transfer memory content. 

    1. Add the contents of an address in Main Memory to the content in register $R_i^{(p)}$ and store it in $R_i^{(p)}$ (i.e. an `ADD` operation using an **adder** and storing the result in it).

        Formally: $C(MM) + C(R_i^{(p)}) \rightarrow C(R_i^{(p)})$

        For this cycle the time-interval is $\Delta T_{D_1^{'}} - \Delta T_r + \Delta T_{D_1^{'''}}$.

- The cycle $D_1^{'} \rightarrow D_1^{''} \rightarrow D_1^{'''}$ identifies micro-operations which add the content of an address in Main Memory to the content of $R_i^{(p)}$ and store it in the same address in Main Memory. 

    Formally: $C(MM) + C(R_i^{(p)}) \rightarrow C(MM)$

    For this cycle the time-interval is $\Delta T_{D_1^{'}} - \Delta T_r + \Delta T_{D_1^{''}}$.