# Synchronous Computers
Computers where all information is represented in the form of **impulsi di tensione** (i.e. synchronous electrical signals) which are interpreted as a binary values (either $0$ or $1$).

Functions are represented as Logical Networks: circuits composed of interconnected *AND*, *OR* or *NOT* logic gates.

The output of a logical function is determined by combining signals coming from different sources (e.g., gates, registers, and other circuit elements) within the network.

However, these signals may change at different times due to *propagation delays*, so the architecture must define **when** it is safe to read the values produced by these sources (i.e. when their outputs have stabilized to the correct levels).

A **clock** provides regularly spaced instants at which signals are guaranteed to be stable and can be sampled reliably. The clock period cannot be changed arbitrarily, because modifying it may break synchronization among the circuit elements.

# Asynchronous Computers
Unlike synchronous computers, asynchronous computers do not rely on a central clock to coordinate the timing of operations across the entire architecture.

Across an asynchronous circuit, interconnected logic elements communicate whenever new data becomes available, independent from one another. interconnected logic elements communicate whenever new data becomes available

Consider two logic elements $E_1$ and $E_2$, where the output of $E_1$ is connected to the input of $E_2$.  Since there is no clock identifying where signals are stable, $E_1$ needs another way to assure $E_2$ that its output is stable.

To achieve this asychronous systems use **handshaking protocols**:

1. $E_1$ sends a **request signal** to $E_2$, in the form of a second output, indicating that is main output is stable. 

2. After $E_2$ has read and processed the value, it sends an **acknowlegment** back to $E_1$.

3. Received, $E_1$ can safely update its output with the next data value.

The biggest downside of this approach is that if the machine malfunction, since every component is independent, it is very difficult to spot where the problem is.

Thus it is only convenient when the architecture is mainly composed of parallel components, like for example a parallel adder unit.

### Italian Legenda
Across CEP's paper in italian, to distinguish across synchronous and asynchronous electrical signals, they are referred in two different ways:
- *impulsi di tensione* $\rightarrow$ synchronous electrical signals.
- *livelli di tensione stabile* $\rightarrow$ asynchronous electrical signals. 