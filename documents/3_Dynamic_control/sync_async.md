# Synchronous and asynchronous computers[[10](../0_Additional_resources/0_reference.md)]
The architecture of a computer is divided into three levels:

1. **Lower-level:** design of combinatorial-logic circuits. They implement complex boolean operations through the combination of basic ones (i.e. $AND$, $OR$, $NOT$).

1. **Mid-level:** design of sequential circuits (i.e. memory components). They are constructed as complex networks closed in themselves.

1. **High-level:** design of the whole machine.

Each design level can be classified as being synchronous or asynchronous. In general we define:

## Synchronous machines
computers where binary informations and commands are represented by **voltage pulses**. During the execution of micro-operations, several logical networks are employed to send/receive necessary information from different part of the computer, e.g. some network is employed to send information to Main Memory, while others commands to the Control Unit. Their coordination is crucial for the correct execution of operations and is governed **always** by voltage pulses. These pulses define a precise temporal relationship, definining the fundamental frequency of the machine, called a **machine cycle**. The machine cycle time should remain consistent during the whole execution, otherwise it leads to incorrect behaviours.

## Asynchronous machines
Binary informations and commands are represented by **tension levels**. To enable communication between two different components of the machine, which we call $A$ and $B$, element $A$ can send information to element $B$ only if $A$ is stabilized (i.e. the informations are reached A and stay within it consistently). $B$ then accepts to receive $A$'s info only if it is stabilized. Since components handles their communication independently, execution time of an operation is reduced to the minimum time necessary. However since there is no consistent time relation in the system, errors are difficult to debug. For highly parallel networks, employing an asynchronous design can reduce significantly execution time (e.g. a parallel adder is asynchronous).

