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

- **B-lines**: processor's register of the Manchester Mark I computer which stored operand addresses to implement iterations over contiguous data structures like arrays and vectors.

    Today they are reffered to as index registers.

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

- **Index-registers**: processor's registers pointing to operand addresses for implement iterative operations over contiguous data structures like arrays and vectors.

---
- **Kernel CEP**: indicates the main structure of the CEP, obtained as the combination of the main memory, the *Unità di Calcolo*, and the *Unità degli Indirizzi*.

    Today the concept of kernel is used completely differently and generally refers to the core of Operating Systems.

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

- **Manchester Mark I**: one of the first stored-program computer developed by the Victoria University of Manchester in 1949. 

    It is mainly known for having introduced the concept of **index-register**.

    Computers today implement index-registers thanks to this machine.

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

- **Mode**: explicitly specifies the type of the argument.

    It is the modern day equivalent of a type specifier in a statement.

---

- **Order Code**: the set of all possible instructions (considering also their *binary representation*) that the computer can execute. 

    In modern day terms it corresponds to the **Instruction Set** (also called sometimes the **Instruction Code**).

---

- **Order Interpreter**: is responsible for decoding a given macro-order into the associated micro-program.

    The modern day equivalent component is the **Instruction Decoder**.

---

- **Programmazione simbolica**: programming paradigm in which the user defines programs able to modify both its local data and own code.

    This paradigm was used in old computers to overcome memory limitations, by defining programs able to free their code in order to add new behaviour.

    Today it is still used mainly in artificial intelligence.

---

- **Selection Time**: the delay required to activate a specific register, bus line, or memory element and for it to become ready to perform its function.

    There is no modern day equivalent.

---

- **Synchronous Computers**: architectures where an universal **clock** component provides space instants defining which signals are guaranteed to be stable and can be sampled reliably.

    This concept is still used today.

---

- **Stored program computers**: machines which followed the Von Neumann's theoretical architecture by storing both program's code and local data onto the main memory.

    Still today modern computers follow this principle.

---

- **Subscript**: the dimension indices for accessing a certain element in a matrix.

---

- **Subscripted value**: a value contained inside a matrix.

---

- **Temporal Cycle**: fundamental time interval of the timing system during which a defined set of micro-operations may occur.

    It can be tought as the equivalent of the clock-cycle.

---

- **Temporal Classes**: group of micro-operations that are activated or valid during the same temporal cycle.

    There is no modern day equivalent.

---

- **Timing System**: component in the computer's architecture responsible for coordinating operations across the machine.

    In italian it is referred as the *"temporizzatore interno"* [[2](./0_reference.md)]

    It is the precursor of the modern day clock system.

---

- **Unità degli Indirizzi**: component of the CEP delegated for constructing the concrete memory addresses.

    There is no modern day equivalent.

---

- **Unità di Calcolo**: component of the CEP delegated to computations. Not to be confused with the micro-programming control unit, which has the responsability to manage the program's execution.

    It can be thought as a precursor of the modern day ALU.

---

- **$X$-Address Order Code**: that all instructions in the architecture have exactly $X$ explicit operands.

    For example:
    - **Zero Address Code:** ADD 
    - **Single Address Code:** ADD X
    - **Two Address Code:** ADD A, B
    - **Three Address Code:** ADD A, B, C

    This concept is still used today.