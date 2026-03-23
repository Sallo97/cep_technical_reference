# Controlled instructions
The CEP offers special modes to activate automatic debug routines for groups of instructions. To enable the feature the user must set the key of the interested group of instruction in the control panel, and in the instruction set the second most-significant-bit to one (i.e. $s_1 = 1$). All instructions supported by debug routines are called **controlled instructions**. [[3](../helper_resources/references.md)]

## Controlled class groups
Controlled instructions are subdivided into five classes, each defining a group of operations which can be checked by the same debug routines. They are:

1. **Fixed point arithmetic operations** for which we check a possible overflow.

2. **Floating point arithmetic operations** for which we check a possible overflow.

3. **Parametric cells instructions**.

4. **Jump instructions**.

5. **I/O instructions** between the Main Memory and an External Device (e.g. checks the parity bit, that the device is attached, etc ...). 

    For this group the debug routine can both signal the user if a malfunction occured or try again the I/O operation.

When the CEP determines that an instruction must be checks, it behaves similarly as when it needs the execute a pseudoinstruction. Execution is stopped and subprogram implementing the debug routine is called. [[3](../helper_resources/references.md)]

Note that debug routines are not fixed, users can define new ones depending on their need. [[7](./0_reference.md)] 

# History
This functionality has been implemented for automizing the debug of complex programs, mainly those computing long arithmetic operations.  As an additional benefit, it reduces significantly the time and memory required to implement the debug operation requested.