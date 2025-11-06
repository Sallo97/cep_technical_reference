<!-- 
Style guide: 
    the glossary is a list of terms used throught the book in alphabetical order. 
    
    For each term first provide a brief description about it. 
    
    At the end always put, separated by a new line from the description, the modern day equivalent term for that concept. If there not exists a modern day equivalent, specify it explicitly. 
    
-->

# Glossary

- **0 Register**: In the Main Control Unit (MCU) of the CEP the 0 register is responsible for keeping the address of the next micro-instruction to be executed.

    In Wilkes' Architecture is called the **Control Register**. In modern day terms it correponds to the **Program Counter** (PC).

- **Order Code**: the set of all possible instructions (considering also their *binary representation*) that the computer can execute. 

    In modern day terms it corresponds to the **Instruction Set** (also called sometimes the **Instruction Code**).

- **Control Register**: in the context of Wilkes' Architecture, it is a special purpose register present in the **Control Unit** which content is the address of the next micro-instruction to be executed. 

    In the CEP computer it is called the **0 Register**. 

    In modern day terms it corresponds to the **Program Counter** (PC).

- **Micro-Order**: in the context of micro-programming, it is the combination of control signals and computer's internal state which executes the associated micro-instruction. 

    The micro-instruction is the abstract behavior, the micro-order is its physical embodiment.

    There is no exact modern day equivalent.

- **Macro-Order**: in the context of micro-programming, a macro-order is an high-level instruction which is executed as a order series of more elementary lower-level micro-instructions. Macro-orders are the operations available to programmers (e.g. a binary addition between integers). 

    During program's execution, each macro-order will be decoded by the Main Control Unit (MCU), which issues the execution of its corresponding micro-instructions.

    There is no exact modern day equivalent, it can roughly correspond to the machine-level instructions of an Instruction Set Architecture (ISA).

- **Degree of parallelism (registers)**: in the context of registers in older computer architectures, the degree of parallelism $p$ of register $R$ denotes the number of bits in register $R$ which can be stored and/or processed simultaneosly.

    There is no exact modern day equivalent, since in contemporary architectures registers all share the same bit width, referred to as a **memory word**.

- **Micro-Program**: the *ordered* sequence of micro-instructions which implements a macro-order.

    Refer to the [Wilkes' Architecture](./1_wilkes_architecture.md) document for an example.

    There is no exact modern day equivalent.

- **Order Interpreter**: is responsible for decoding a given macro-order into the associated micro-program.

    The modern day equivalent component is the Instruction decoder

- **Micro-Programming Control Unit**: a processor following the micro-programming principle. Following Wilkes' Architecture, it is implemented as a sequence of two diode matrix.

    There is no exact modern day equivalent.


- **Diode Matrix**: an array of horizontal and vertical wires, where the horizontal are the inputs of the circuit and the vertical are the outputs of the circuit.

    They are mainly used in a micro-program control unit to encode the current micro-instruction.

    There is no exact modern day equivalent.
