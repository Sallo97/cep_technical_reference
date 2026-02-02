# FINAC
The FINAC was a computer manufactured by the British firm Ferranti Ltd. and acquired by the CNR (National Research Council) in Rome in 1955. It was the fourth produce model of the Ferranti "Mark I*" series, one of the first completely electronical computers.

# CEP Emulator for the FINAC [[7](../0_Additional_resources/0_reference.md)]
During CEP's development, an emulator was created for the FINAC, in order to test its base programs before the computer was fully operational.

The emulator was able to execute $7$ instructions per second. In general the FINAC was a lot slower compared to the CEP. Still, the emulator was used even after the CEP's completion, for testing simple program which had long execution time.

Despite both being binary systems, the architectures of the FINAC and the CEP were fundamentally different in terms of word length, memory capacity, and supported I/O peripherals.

## Arithmetic instructions

- **Single precision:** the CEP single precision arithmetic 
instructions dedicate $36$-bits for both the instruction and for numbers. Instead, the FINAC dedicates $20$-bits for the instruction and $40$-bits for numbers.

    In the FINAC each instruction is divided in four groups of $5$ bits each; each single precision number is stored in either $40$-bits or $60$-bits, depending if they are fixed point of floating point.

    It is important to note that single-precision floating-point arithmetic is not implemented at the hardware level. Instead, the FINAC relies on a specialized suite of subprograms to manage both the internal representation of numbers and the execution of all related operations.

    Each CEP word was mapped to a single FINAC number, having the first bit duplicated in the second position and the last three bits set to $0$. The instruction is decompose in groups of respectively $10, 6, 6, 18$ bits.

    CEP's single precision number are **always** mapped to $40$-bits, regardless being fixed or floating point.

- **Double precision:** an equivalent approach to the single precision numbers was used for double precision numbers.
    
    The FINAC stores double precision fixed point numbers into $80$-bits. Double precision floating point numbers **are not implemented natively in the FINAC**.
    
    The CEP's 72-bit double precision values are mapped to the FINAC's 80-bit double precision format, having always the last $8$ numbers set to $0$. 

## Main Memory
The FINAC possessed a significantly smaller memory capacity compared to the CEP. To compensate, the virtual CEP memory was distributed across the FINAC’s auxiliary storage and its main memory. The latter was constrained to a maximum capacity of 128 CEP words, which were strictly reserved for the program’s executable code and its data.

## Registers
CEP's registers are directly mapped to specific addresses in main memory. The parametric cells are distributed across main memory and auxiliary memory

**NOTE:** *The document says: anche le celle parametriche si trovano, oltre che nella memoria ausiliaria, all'indirizzo voluto, nella memoria rapida ad indirizzo determinato.*

## Emulator structure
The emulator is composed of two parts:

- The interpretation of CEP instructions is handled by a dedicated suite of subprograms. To optimize the FINAC’s limited primary memory, these subprograms are stored in auxiliary memory, with a subset of the *"most frequent"* operations cached in main memory.

    This solution permits to easily reload subprogram in main memory, if its allocated memory space was overwritten to accommodate a different routine.

- Three management programs stored in main memory:

    - **Indec:** checks if the current instruction to execute is already loaded in the FINAC's main memory, otherwise it calls it from auxiliary memory. 

        Applies the same reasoning to retrieve the operand.

    - **Modec:** retrieves from main memory the instruction to execute and it breaks it down to its single components, applying the requested modifications (if present) and updating the numerator.

        **NOTE:** *I assume the numerator is the program counter, but I am not sure. Another possible interpratation is that the instruction belongs to a cycle and in the next iteration we need to increment the operator.*

    - **Fundec:** decodes the operation simbol of the instruction to execute, calling from auxilary memory the delegated subprogram to its interpretation.

To ease its development, two programs have been added to the emulator: 

- **Programma Entrata:** developed to initialize the CEP's virtual memory (distributed among the auxiliary and main memory) and loading the CEP's program to execute, translating it into a suitable decimal code. 

    During loading it can modify to original program, by for example changing the addresses of CEP's registers and celle parametriche in order to guarantee that the program can be executed in the desired FINAC's address.

    The design of the "Programma Entrata" was necessary because it was not possible for the FINAC to read CEP's programs easily. CEP's programs are stored in $7$-bits binary tapes, while FINAC's programs are stored in $5$-bits tapes. Additionally, emulate the CEP's loading program would have been too much expensive, and using the FINAC's loading program impractical, since their instruction words' structure differs completely.

    Implementation-wise, the "Programma Entrata" constructs a particular alphabet containing digits and symbols associated to the decimal format used. For conversion of numbers it uses a set of helper subprograms.

- **Programma Correttore:** work in conjunction with the emulator by testing the CEP's program, without interfering with its execution. 

    To start it needs the user to provide a **jump table** which specifies in each entry:

    - The address of the instruction to check.
    
    - What the user wants to print.
    
    - The format in which we want to print the partial result (i.e. its precision and if its fixed or floating point).

    During the emulator execution, the "Programma Correttore" checks if the current instruction is in the jump table and, if so,  it prints both the partial result and the instruction itself.