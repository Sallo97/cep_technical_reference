<!-- 
Style guide: 
    the glossary is a list of terms used throught the book in alphabetical order. 
    
    For each term first provide a brief description about it. 
    
    At the end always put, separated by a new line from the description, the modern day equivalent term for that concept. If there not exists a modern day equivalent, specify it explicitly. 
    
-->

# Glossary

- **0 Register**: In the Main Control Unit (MCU) of the CEP the 0 register is responsible for keeping the address of the next micro-instruction to be executed.

    In Wilkes' Architecture is called the **Control Register**. In modern day terms it corresponds to the **Program Counter** (PC).

---

- **1 + 1 Address Control Unit**: the Order Code of the machine offers only instructions composed of one *explicit* operator and one *implicit* operator.

    There is no exact modern day equivalent, most modern Instruction Set Architecture do not follow this scheme. Instead, interconnected logic elements communicate whenever new data becomes available, using communication protocols like the **handshaking protocol**.

    This concept is still used today.

---

- **Asynchronous Computers**: machine which do not rely on a clock component to guarantee stability in the output reading. Instead, 

---

- **Control Register**: in the context of Wilkes' Architecture, it is a special purpose register present in the **Control Unit** which content is the address of the next micro-instruction to be executed. 

    In the CEP computer it is called the **0 Register**. 

    In modern day terms it corresponds to the **Program Counter** (PC).

---

- **Degree of parallelism (registers)**: in the context of registers in older computer architectures, the degree of parallelism $p$ of register $R$ denotes the number of bits in register $R$ which can be stored and/or processed simultaneously.

    There is no exact modern day equivalent, since in contemporary architectures registers all share the same bit width, referred to as a **memory word**.

---

- **Diode Matrix**: an array of horizontal and vertical wires, where the horizontal are the inputs of the circuit and the vertical are the outputs of the circuit.

    They are mainly used in a micro-program control unit to encode the current micro-instruction.

    There is no exact modern day equivalent.

---

- **Electrical Pulses**: refers to electrical signals used to define a synchronous network (i.e. a clock signal).

    This term is used across the CEP's papers to distinguish synchronous logical networks in the architecture to asynchronous components.

    Today they are simply referred to as electrical signals.

---

- **Handshaking protocol**: protocol to guarantee a safe communication between two logic elements asynchronously. The two components communicate with **request signals** and **acknowlegment** to tell the other that the signal has been sent/read.

    This concept is still used today.

---

- **Impulsi di tensione**: used across CEP's italian paper for referring to electrical signals used across **synchronous** logical networks. 

---

- **Livelli di tensione stabile**: used across CEP's italian paper for referring to electrical signals used across **asynchronous** logical networks.

---

- **Logical Network**: a circuit implementing a complex logical function, obtained by the composition of interconnected *AND*, *OR* or *NOT* gates.

    This name and concept is still valid today.


---

- **Macro-Order**: in the context of micro-programming, a macro-order is a high-level instruction which is executed as an ordered series of more elementary lower-level micro-instructions. Macro-orders are the operations available to programmers (e.g. a binary addition between integers). 

    During program's execution, each macro-order will be decoded by the Main Control Unit (MCU), which issues the execution of its corresponding micro-instructions.

    There is no exact modern day equivalent, it can roughly correspond to the machine-level instructions of an Instruction Set Architecture (ISA).

---

- **Micro-Order**: in the context of micro-programming, it is the combination of control signals and computer's internal state which executes the associated micro-instruction. 

    The micro-instruction is the abstract behavior, the micro-order is its physical embodiment.

    There is no exact modern day equivalent.

---

- **Micro-Program**: the *ordered* sequence of micro-instructions which implements a macro-order.

    Refer to the [Wilkes' Architecture](./1_wilkes_architecture.md) document for an example.

    There is no exact modern day equivalent.

---

- **Micro-Programming Control Unit**: a processor following the micro-programming principle. Following Wilkes' Architecture, it is implemented as a sequence of two diode matrix.

    There is no exact modern day equivalent.

---

- **Order Code**: the set of all possible instructions (considering also their *binary representation*) that the computer can execute. 

    In modern day terms it corresponds to the **Instruction Set** (also called sometimes the **Instruction Code**).

---

- **Order Interpreter**: is responsible for decoding a given macro-order into the associated micro-program.

    The modern day equivalent component is the **Instruction Decoder**.

---

- **Synchronous Computers**: architectures where an universal **clock** component provides space instants defining which signals are guaranteed to be stable and can be sampled reliably.

    This concept is still used today.

---

- **$X$-Address Order Code**: that all instructions in the architecture have exactly $X$ explicit operands.

    For example:
    - **Zero Address Code:** ADD 
    - **Single Address Code:** ADD X
    - **Two Address Code:** ADD A, B
    - **Three Address Code:** ADD A, B, C

    This concept is still used today.