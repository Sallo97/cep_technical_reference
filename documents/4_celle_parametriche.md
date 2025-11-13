# Index Register
With index register we indentify processor's registers whose responsability is to point to an operand's address. In this way we can use the register to implement interations over contiguous data structures like arrays or vectors.

Generally the index register contains the base address of the data structure we want to iterate. At each iteration the pointer is added/subtracted by an offset (specified by the instruction or stored in another register) to obtain the complete address to the current memory location.

The first computer to implement the concept of index register was the Manchester Mark I developed by the Victoria University of Manchester in 1949. In this computer there were only two index registers,  to as a *B-lines*.

# CEP's celle parametriche
At their core, CEP's *celle parametriche* have the same roles as the *B-lines* of the Manchester Mark I, with a major extension: as their name implies they are also used to pass parameters for a sub-program.

During CEP's development one of the main proposal was to extend the concept of automatic instruction modification introduced by *B-lines* in order to support parameter's passing and calls in subprograms.

The machine has in total 64 *celle parametriche*, numbered from 0 to 63. The registers are divided into two groups of 31 consecute registers plus a special cell, called $HO$ and $HU$. 

$HO$ contains the *celle parametriche* used from the main program in execution, plus the special *cella parametrica* $0$, which always contains the constant $0$.

$HU$ contains *celle parametriche* used for the system's organization and some in common between the sub-programs. Additionaly it contains the special *cella parametrica* $63$ which contains the instruction counter. 