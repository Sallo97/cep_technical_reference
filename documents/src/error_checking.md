# Error Checking
The CEP offers an assortment of techniques for verifying the correctess of a program Checking techniques are divided in two classes: **static techniques** and **dynamic techniques**. [[7](../helper_resources/references.md)]
 
## Static checking techniques
Also called *post-mortem techniques*, these will examine the state of the registers and the Main Memory after the CEP stopped.


## Dynamic checking techniques
These techniques will check the program during its execution, providing users useful information along the way.  We will list the control programs that have been implemented. [[7](../helper_resources/references.md)]

### Notation
Throughout the section we will use the following terms:

- **Main program:** the base program we are executing.

- **Critical instruction:** a breakpoint in the main program specified by the user because he thinks an error could occur.

- **Debug program:** the subroutine used for printing debugging information. The data  to print must be specified by the user (e.g. a register, parametric cell, ...).

### First debug technique - manual insertion of debug prints
The first debug technique used consisted in manually inserting at each critical instruction a jump to the associated debug subprogram. The downside of this approach is that, after the program has been confirmed to work correctly, the user needs to manually remove all the debug jumps. [[7](../helper_resources/references.md)]

### Second debug technique - delegation to an helper program
This solution automizes the previous attempt, by delegating the execution of the jumps to the debug programs to another program, which will call the helper program. This requires that the helper program knows from the start of each critical instruction its absolute address, what information to print, and **when** to print them. The "helper" program will then add after or before each critical instruction the jumps. [[7](../helper_resources/references.md)]

This procedure allow users to update their debug procedure more easily. When we are sure that the main program is correct, we only need to discard the "helper" programs. [[7](../helper_resources/references.md)]

### Complete debugging
This final technique consists in defining an helper program delegated to check all instructions of the main program, executing each of them and printing information regarding execution state. [[7](../helper_resources/references.md)]

# The CEP's control programs
To facilitate the implementation of the above **dynamic techniques**, the CEP has been enriched with three control programs: $PCC$, $PCT$, $PCZP$.

## The $PCC$ control program
Supports the checking of a main program at the computation level, by printing the requested intermediate results, and at the control flow level, by printing "appropriate" information. To work, $PCC$ needs to know the absolute addresses of the critical instructions and for each of them what to print and when to print it. The requested arguments must be provided in a punched tape, called the **specifier tape** (from the italian *"nastro specificatore"*). [[7](../helper_resources/references.md)]

### Format of the specifier tape
The specifier tape requires that the arguments must appear, for each critical instruction, in the following order:

1. The **absolute address** of the critical instruction.

1. A parameter which specifies for non-jump critical instructions or conditional jump critical instructions what to do when the fail.

1. A parameter which specifies for jump critical instructions or conditional jump critical instructions what to do when they will execute.

1. The requested data, which can be: registers, parametric cells, instruction format, memory areas, and others. Additionally, the user must specify the representation (e.g. symbolical, base-10, base-8, ...) in which the info should be printed.

### $PCC$ implementation
The $PCC$ control program is composed of two interdependent chunks. The first one reads the specifier tape for debug requests, substituiting each critical instruction in the main program with a jump instruction to an associated point in the second chunk. The second chunk will be called by the main program each time it reaches a critical instruction, now being a jump to it. The reached point in the chunk will execute the critical instruction and print the requested info.

## The $PCT$ control program
Handles the checking of a contiguous block of code in the main program. 
For each instruction to check it prints: the instruction, its position in memory, and the returned result. [[7](../helper_resources/references.md)]

The arguments for $PCT$ are passed through the control panel, and are:

- The output device where to print the debug information (i.e. either the telewriter, the puncher, or the printer).

- The initial address and final address of the block of code to debug.

- The class of instructions to check. This means that an instruction is checked only if it belong to an activated class. Recall thet each class is mapped to a particular key in the control panel.

- How to handle pseudoinstructions. They can be interpreted as normal instruction, i.e. by considering them as a single computation, or by checking step-by-step the associated macroprogram.

If during debugging the main program tries to acces the memory area reserved to $PCT$, the execution is immediately stopped and an error is printed. [[7](../helper_resources/references.md)]

If the instruction to check is a stop instruction, $PCT$ will always check it and then stop. Execution can be restarted from the control panel. [[7](../helper_resources/references.md)]

## The $PCZP$ control program
At its core is an extension of $PCT$, specialized in handling only memory instructions. It is possible to specify from the control panel which areas of memory are "protected", i.e. the control program will check only instructions working on the requested regions.
It behaves in the same way as PCT. [[7](../helper_resources/references.md)]