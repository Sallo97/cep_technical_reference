# Synchronous and asynchronous computers
The architecture of a computer is divided into three levels:

1. **Lower-level:** design of combinatorial-logic circuits. They implement complex boolean operations through the combination of basic ones (i.e. $AND$, $OR$, $NOT$).

1. **Mid-level:** design of sequential circuits (i.e. memory components). They are constructed as complex networks closed in themselves.

1. **High-level:** design of the whole machine.

Each design level can be classified as being synchronous or asynchronous. [[10](../helper_resources/reference.md)]

## Synchronous machines
Computers where binary informations and commands are represented by **voltage pulses**. During the execution of micro-operations, several logical networks are employed to send/receive necessary information from different part of the computer, e.g. some network is employed to send information to Main Memory, while others commands to the Control Unit. Their coordination is crucial for the correct execution of operations and is governed **always** by voltage pulses. [[10](../helper_resources/reference.md)]

These pulses define a precise temporal relationship, definining the fundamental frequency of the machine, called a **machine cycle**. The machine cycle time should remain consistent during the whole execution, otherwise it leads to incorrect behaviours.[[10](../helper_resources/reference.md)]

## Cycle Time
The synchronization type depends on the highest latency operation in the machine, which usually is the main memory access. Compared to operation working with registers, communicating with memory requires a lot more time. Additionally register are implemented using flip-flops, which can retain over time their content, discarding it only when it is explicitly requested. Instead, magnetic entries can lose their content over time, requiring to rewrite it constantly.[[10](../helper_resources/reference.md)]

### Maximum cycle time in the core kernel of the CEP
Excluding memory accesses, the major bottleneck in the core kernel of the CEP resides in the parallel adders. A parallel adder is made of an interconnected sequence of full adders and during execution each member needs the carry bit produced as output by its predecessor.

## Asynchronous machines
Binary informations and commands are represented by **tension levels**. To enable communication between two different components of the machine, which we call $A$ and $B$, element $A$ can send information to element $B$ only if $A$ is stabilized (i.e. the informations have reached $A$ and stay within it consistently). $B$ then accepts to receive $A$'s info only if it is stabilized. [[10](../helper_resources/reference.md)]


Components handles their communication independently, execution time of an operation is reduced to the minimum time necessary. However since there is no consistent time relation in the system, errors are difficult to debug. For highly parallel networks, employing an asynchronous design can reduce significantly execution time (e.g. a parallel adder is asynchronous).[[10](../helper_resources/reference.md)]

## CEP as both an asynchronous and synchronous machine
The CEP is at the combinatorial level an asynchronous machine, synchronous at the sequential level. 

At the level of micro-program, it is a sequential machine, with the exception of the handling of external devices for which it behaves like an asynchronous machine. [[10](../helper_resources/reference.md)]
