# General Timing System concepts
The timing system is the part of the architecture responsible for generating all **timing pulses** (also called timing phases) used to coordinate operations throughout the machine.

It is the precursor to the modern **clock system**. Unlike modern clocks, which provide a single periodic signal, timing systems of early computers produced a sequence of **micro-timing phases**, each controlling specific internal operations.

## Temporal Cycles
A temporal cycle is the fundamental time interval of the timing system during which a defined set of micro-operations may occur. It represents the smallest scheduling unit in which signals may be asserted, sampled, or stored.

In spirit, it plays a role analogous to a clock cycle in modern synchronous computers.

## Temporal Classes
A temporal class groups together signals, operations, or micro-operations that are activated or valid during the same temporal cycle (or set of cycles). Temporal classes ensure that related control signals occur in the proper timing relationship.

## Selection Time
Selection time is the delay required to activate a specific register, bus line, or memory element and for it to become ready to perform its function.

It is the interval between issuing a control signal from the timing system and the moment the targeted component responds or becomes stable.
Selection time is not identical to combinational propagation delay, but it constrains the minimum duration of a temporal cycle.

# CEP Timing System (Temporizzatore Interno)
The CEP's timing system (also called in italian *temporizzatore interno*) is responsible for coordinating CEP's operation by appropriately setting timing pulses. 

![image](../resources/cep_timing_system.svg)

## Timing

Each micro-instruction in the CEP requires **nine** temporal cycles.

Looking at the schematics, each rectangle marked with *D* represents a delay component. Defined:
- $\Delta T_{D_{0}}$ the time interval for the common initial delay $D_{0}$.
-  $\Delta T_{D_{i}}$ with i = {1, $\ldots$, n} the time interval for the $i_{th}$ delay.

$\Delta T_{D_{0}} + \Delta T_{D_{i}}$ is the time interval between two successive pulses to register $O$, **the time assigned to each micro-operation**.

## Control Signals
Control signals $x_{c1}$ select the temporal classes. 

Control signals $x_{c2}$ select the registers to which pulses have to be sent during each micro-operation..

The delay $D_0$ has the role to ensure that the signals $x_{c1}$ are stabilized.

## Handling timing for micro-operations regarding memory
As can be seen from the schematics, there are multiple $D_1$ definitions concatenated one after the other. These are used for handling micro-operations accessing the main memory $MM$.

The pulse $P_{r}$ is responsible for starting the read operation, while pulse $P_{w}$ starts the write operation of the MM.

### Distinguishing memory accesses
As can be seen from the concrete schematic, there are two possible series of delays when doing a memory operation:

- $D_0 \rightarrow D^{'}_{1} \rightarrow D^{'''}_{1}$: refers to micro-orders that transfer memory contents and to contents of the type:

    $C(MM) + C(R_{i}^{(p)}) \rightarrow R_{i}^{(p)}$

    The formula is read as: add the contents of a location in MM to the contents of register $R_{i}^{(p)}$ and store in $R_{i}^{(p)}$. 
    
    The time required to make this micro-order is $\Delta T_{D_{1}^{'}} - \Delta T_{r} + \Delta T_{D_{1}^{'''}}$    

- $D_{0} \rightarrow D_{1}^{'} \rightarrow D_{1}^{''} \rightarrow D_{1}^{'''}$ : refers to micro-orders described of the formula:
    
    $C(MM) + C(R_{i}^{p}) \rightarrow MM$
    
    The formula is read as: add the contents of a location MM to the contents of register $R_{i}^{(p)}$ and store the result in main memory.
    
    <!-- (I ASSUME TO THE SAME ADDRESS, BUT THE PAPER DO NOT SPECIFY IT) -->    

### Timing for conditional Control Signals
Recall that a conditional control signal is one whose activation depends on the machine's current state or some status flag.

In the CEP's timing system the control signals $x_{c_1}$ and $x_{c_2}$ are conditional. By conditioning both signals at the same time it is possible, as in the case of repetitive micro-orders, to block the pulses at register $O$ at the end of the first micro-operation and to decrease (because the control unit is not operating at the moment) the cycle time in the successive micro-operation.
