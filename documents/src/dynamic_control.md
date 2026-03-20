# Dynamic Control
The Dynamic Control is the circuit where all the pulses of the CEP are produced (taking consideration of the correct timing). [[10](../helper_resources/reference.md)]

It is composed of:

- A sub-circuit for producing temporal cycles with exits to all registers of the CEP and external devices.

- A sub-circuit for distributing the starting pulses $X_1$ coming from the control panel.

    The flip-flop $F_2$ manages the dispatching of entry pulses coming from the external devices. Their pulses are stopped while the computer is moving from a cycle to another. $F_2$ also conditions the Control Unit depending on the operations specified by the control panel.

- Circuit which through the use of two keys in the control panel, manages the execution mode of the CEP, deciding if it must be automatic, per instruction or per micro-instruction.

### Per micro-instruction or per-instruction mode selection

- for activate the per micro-instruction mode use the pulse $x_{14}$

- for activate the per instruction mode use both $\pi O_9$ and $x_{14}$

The cycle pulse is diverted through gate $A_5$ from gate $A_4$. $F_2$ is resettend and the machine stops.

Flip Flop $F_1$ synchronizes with the temporal cycle the *"stop machine and end instruction"* command, obtained by pressing the instruction key (i.e. button) in the control panel. [[10](../helper_resources/reference.md)]

### Debugging micro-instructions
A circuit managed by flip-flops $F_2$, $F_3$, and command $x_{10}$ gives the possibility to brake down from the control panel the execution of a micro-instructions in two cycles. This circuit is used for debugging the correct behaviour of registers and logical networks. [[10](../helper_resources/reference.md)]

#### First cycle
The first starting pulse instructs the Control Unit, sending the $\psi$ commands through the computer. If the Memory is used, then is is also read and written back. [[10](../helper_resources/reference.md)]

#### Second cycle
The second starting pulse writes in the registers of the main kernel of the CEP all the data of the micro-information in their entry point. [[10](../helper_resources/reference.md)]

## Technical Details
The delays use coaxial cable segments (characteristic impedance: $3,9 k\Omega$; delay: 3,3 $\mu sec/m$ ). The maximum delay of the pulses is set to $0,4 \mu sec$. [[10](../helper_resources/reference.md)]



