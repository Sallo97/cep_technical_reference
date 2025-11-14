# Pseudoistruzioni [[3](./0_reference.md)]
Pseudoinstruction extends the instructions offer of the CEP through the use of subprograms. They can be seen as custom-added instruction which in practice are implemented as subprograms.

They share the same 36-bit structure as normal instructions. The main difference is **how** the machine execute them: 

- a normal instructions is executed by running is associated microprogram, available in the microprogramming control unit.

- the pseudoinstruction uses a **macro-program**, a combination of normal instructions and lower-level pseudoinstructions, loaded in main memory.

The instruction decoder is able to distinguish instructions from pseudoinstructions by reading the first bit of the operation symbol: if the bit is '$0$' then it is considered a normal instruction, otherwise if setted to '$1$' it is a pseudoinstruction.

## Executing the macro-program
When the decoder detects that the first bit of the instruction is set to '$1$', after moving the *bimodificato* address in a specific *cella parametrica* of $HU$, sets the CEP in a special mode which does not update the instruction cycle, and then jumps to the starting address of the associated macro-program, stored in the macro-program table.

The jump is determined by the remaining 8-bits in the operation symbol and is done by a normal unconditional jump instruction, which stored the instruction counter in a *cella parametrica* of $HU$ for returning to the next instruction at the end of the macro-program.

Thus the pseudoinstruction is handed as a subprogram, whose pararameter its *biomodificato* address and the address of the next instruction in the program.

## Special pseudoistruzioni
Most pseudoinstruction behaves as normal instructions, still it is possible to definal special pseudoinstructions, which like special instructions have a second parametric address.

## Limitations
The main limitation is in the limited number of pseudoinstructions declarable within a program: it depends both on the space left in the main memory and on the maximum number of elements in the jump table.

Since from the symbol table, 8-bit are used to determine the jump, then we can have theoretically at most $256$ pseudoinstructions in a program. In actuality, since the jump table starts at the beginning of the group $HU$, since the first 32 registers are reserved as *celle parametriche*, there are at most 224 possible entries.