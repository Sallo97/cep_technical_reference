# Error Checking [[7](../0_Additional_resources/0_reference.md)]
The CEP offers an assortment of techniques for verifying the correctess of a program.

Checking techniques are divided in two classes: **static** techniques and **dynamic** techniques.
 
## Static checking techniques
Also called *post-mortem* techniques, these will examine the registers and the main memory of the CEP after the computer stopped.


## Dynamic checking techniques

These techniques will check the program during execution, providing users useful information along the way. 
We will list some considered techniques, for which control programs have been implemented.

### Notation
Throughout the section we will use the following terms:

- **main program:** the program we are executing and want to guarantee correctness.

- **"critical" instruction:** a point in the main program where the user think an error could occur, so he wants to check the machine's state.

**NOTE:** *further in the document, the writer specifies that an user should provide **when** to print the debug information. I presume it is reffering if the user wants the print before instruction's execution, after, or in both cases.*

- **debug subprogram:** code for handling the debug print of a "critical" instruction. The data (e.g. a register, cella parametrica, ...) to print must be specified by the user.

### Manual insertion of debug prints
Consists in manually inserting at each "critical" instruction a jump to the associated debug subprogram. The downside of this approach is that, after the program has been confirmed to work correctly, the user needs to manually remove all the debug jumps.

### Delegation to an "helper" program
This solution automizes the previous attempt, by delegating the execution of debug jumps to another program, which will call the "helper" program. 

This requires that the "helper" program knows from the start for each "critical" instruction: its absolute address, what information to print, and **when** to print them (*I think the user needs to state if calling it before instruction execution, after or both*).

The "helper" program will then apply the substitution of the "critical" instructions in the main program with jumps to the associated debug subprogram. 

**NOTE:** *I think the "helper" program, assuming it already have all needed data, receives in input the main program and executes it, checking at each step if the current instruction is a "critical" one. If so it jumps to the associated debug subprogram.*

This procedure allow users to update their debug procedure, by for example changing in the "helper" program what debug informations we want to print. When we are sure that the main program is correct, then we only discord the "helper" program.

### Complete debugging
This final technique consists in defining an "helper" program delegated to check all instructions of the main program, executing each of them and printing information regarding execution state. 

**NOTE:** *I are assuming the "helper" program here receives in input the main program and then at each step executes an instructiona and prints machine state.*

# The CEP's Control Programs
To facilitate the implementation of the above **dynamic** techniques, the CEP has been enriched with three control programs: PCC, PCT, PCZP.

## The PCC Control Program
Supports the checking of a main program at the computation level, by printing the requested intermediate results, and at the control flow level, by printing "appropriate" information. 

**NOTE:** *as of now the document does not specify what "appropriate" information means*.

To work, PCC needs to know the absolute addresses of the "critical" instructions and for each of them what to print and when to print it. The requested arguments must be provided in a punched tape, called the **"nastro specificatore"**. 

### Format of the Nastro Specificatore
The "nastro specificatore" needs that the arguments must appear, for each "critical" instruction, in the following order:

1. The **absolute** address of the "critical" instruction we want to check.

1. A parameter which specifies for non-jump "critical" instructions or conditional jump "critical" instructions which **will fail**, when PCC needs to print the requested data.

1. A parameter which specifies for jump "critical" instructions or conditional jump "critical" instructions which **will execute**, when the PCC needs to print the requested data.

1. The requested data, which can be: registers, celle parametriche, instruction format, memory areas, and others. Additionally, the user must specify the format in which the info should be printed.

### PCC Implementation
The PCC is composed of two interdependent chunks:

- The first one reads the "nastro specificatore" for debug requests, substituiting each "critical" instruction in the main program with a jump instruction to an associated point in the second chunk.

- The second chunk will be called by the main program each time it arrives at a "critical" instruction, now being a jump to it. The reached point in the chunk will execute the "critical" instruction and print the requested info.

## The PCT Control Program
Handles the checking of a contiguous block of code in the main program. 
For each instruction to check it prints: the instruction, its position in memory, and the returned result.

The arguments for PCT are passed through the command panel ("quadro di comando"), and are:

- the output device where to print the debug info (telewriter, puncher, or printer).

- the initial address and final address of the block of code to debug.

- the class of instructions to check. This means that an instruction is checked only if it belong to an "activated" class. Each class is mapped to a particular key in the command panel.

**NOTE:** *the document does not specify how many classes there are, which instruction belongs to, and the mapping between classes and keys in the command panel.*

- How to handle pseudoinstruction. They can be interpreted as normal instruction, i.e. by considering them as a single computation, or by checking instruction-per-instruction the associated macroprogram.

If during debugging the main program refers to an area of memory from PCT, the control program will print an error and stop execution.

If the checked instruction is a stop instruction, PCT will always check it and then stop. Still it is always ready for restarting.

**NOTE:** *the document does not tell what "ready for restarting" means. Maybe it will loop back and re-debug the main program.*

## The PCZP Control Program
It is a specialization of PCT, handling only memory instructions. It is possible to specify from the command panel which areas of memory are "protected", i.e. the control program will check only instructions working on the requested regions.

Other that this difference, it behaves in the same way as PCT.