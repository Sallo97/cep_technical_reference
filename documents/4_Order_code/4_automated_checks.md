# Automated checks [[3](./0_reference.md)]
The CEP is capable of automatically checking the correct behavior of specific groups of instructions. To enable the feature the user must:

1. enable the checking of a particular group by setting the associated physical key in the CEP's control panel.

1. for an instruction of said group which we want to check, we must set to $1$ the last bit in the operation symbol.

We refer to instructions for which we can enable automatic checking as controlled instructions.

## Controlled class groups [[3](./0_reference.md)]
Controlled instructions are subdivided into five classes:

1. **Fixed** point arithmetic operations for which we check a possible overflow.

2. **Floating** point arithmetic operations for which we check a possible overflow.

3. Instructions which manipulate celle parametriche.

4. Jump instructions.

5. I/O instructions between the main memory and an external device (checks the parity bit, that the device is attached, etc ...). 

    The automatic checking program can both signal the user an occured malfunction and try again the I/O operation.

When the check is enable for one of these instructions, the CEP stops execution and an associated subprograms do the checking. The machine behaves similarly to when its handling pseudoinstructions.

Note that the checking programs are not fixed, rather an user can define new checking programs depending on the need. [[7](./0_reference.md)] 

# History
This functionality has been implemented for automize the checking process of complex programs, mainly those computing long arithmetic operations. 

As an additional benefit, it reduces significantly the time and memory required to execute the program when we need to check possible overflows continuously.