# Automated checks [[3](./0_reference.md)]
The CEP can be set to automatically check a specific group of instructions by for each of them executing right away a macro-program specifically designed to be sure there are no unexpected behaviours.

To do so it requires to set to '$1$' the condition bit of the operation code and by setting the associated keys in the *quadro di comando*.

## Controlled class groups
Instructions hare subdivided in five classes that identify different groups being automatically checked by the CEP:

1. Instructions implementing fixed point arithmetic operations (checks for overflow).

2. Instructions implementing floating point arithmetic operations (checks for overflow).

3. Instructions which manipulate *celle parametriche*.

4. Jump instructions.

5. I/O instructions between the main memory and external device (checks the parity bit, that the device is attached, etc ...).

When the instruction with the check bit set to '$1$' in a class for which the automatic check is set, the CEP stops execution and an associated subprograms do the checking. The machine behaves similarly to when its handling pseudoinstructions.

This functionality has been implemented to automize the checking process for complex programs (mainly regarding computing long arithmetic operations), without requiring to explicitly call the subprograms. It also reduces significantly the time and memory required to execute the program.