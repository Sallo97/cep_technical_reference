# Control Signals
In a computer architecture a Control Signal is an electrical level (i.e. a steady voltage) that is sent to the **control input** of a logic gate or a registe. Its signal orchestrates in the component the movement and processing of data for:

- **When the signal is 0 (False):** the gate is closed, stopping data from moving even if it is waiting on the input wires. 

- **When the signal is 1 (True):** the gate is opened and data can flow from the source to the destination.

Abstractly a control signal can be thought as the concrete implementation of a command in the architecture.

Matrix $MC$ is the matrix generating control signals from the output of the decoder $DC$. Its control signals are responsible for implementing concretely the execution of micro-operations. They can be classified in four categories:

1. Signals intersected by matrices $I$ and $II$, representing the addresses $\mu_1$ and $\mu_2$ of the next micro-order entering the switching circuit $K_0$. 

1. Signals controlling the operations in the control unit and intervening at the end of the micro-operation. This group includes the signals entering circuits $K_1$ and $K_2$, and some other unconditional signals $c_1$.

1. Signals controlling all operations **outside** the control unit, and intervining during the micro-operation. This group includes two types of signals:

    - **conditional**, i.e. those entering circuits as $K_3$.
    
    - **unconditional**.

1. Signals controlling the **timing** of the computer. This group is divided in two sub-groups:
    - $x_{c_1}$ determining the **operative cycle**, i.e. the total time required to execute a single, complete micro-instruction..

    - $x_{c_2}$ selecting the register to which pulses are to be sent.

    Both sub-groups may include condition or unconditional signals.