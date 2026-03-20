# Controllo Dinamico
The CEP's *Controllo Dinamico* unit is the component of the architecture delegated to produce the pulses (synchronous signals) at the right time. 

It is composed of:

1. A circuit which sends the temporal cycles to all registers of the CEP and external devices.

1. A circuits which distributes the starting pulses (*impulsi di avviamento* in italian) $x_1$ coming from the *tavolo di comando*.

    The $F_2$ flip-flop blocks signals coming from outside during the succession of cycles and sets conditions for the control unit depending on the operation setted in the *tavolo di comando*.

1. A circuit which depending on how the two keys of the *tavolo di comando* are set, set the CEP's mode to either:

    - *per automatico*
    - *per istruzione*
    - *per micro-istruzione*

    In both *per istruzione* and *per micro-istruzione* mode the inputs $x_{14}$ or $\pi o_9$ and $x_4$ are set, the pulse cycle moves to gate $A_5$ from gate $A_4$, resetting $F_2$ which shuts down the CEP.

    Concretely the CEP is shut down by moving downward the instruction key.

1. A circuit handled by the flip-flops $F_2$ and $F_3$, and by command $x_{10}$ which enables the possibilty to break a micro-instruction to be executed in two cycles instead of one.

    This feature is used for debugging the statically and separately the registers and logical networks

![img](../resources/cep_controllo_dinamico.png)