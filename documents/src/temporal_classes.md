# Internal micro-instruction temporal classes
Each internal micro-instruction is associated to one of nine temporal cycles. The temporal cycles are then divided into temporal classes.

## Classes using Main Memory

### Class $I-I$
Micro-instructions which read from Main Memory. The read content can be used by the Compute Unit in two ways:

1. process the read data with or without the parallel adder in $X$, then store the result in one of the registers in $X^{'}$. 

1. process the read data withouth the parallel adder in $X$ and then store the result in the same memory cell.

### Class $I-III$ 
Micro-instructions similar to those in $I-I$. The main difference lies in the construction of the memory address, which is produced by the parallel adder in $Y$.

### Class $I-II$
Micro-instructions which read from Main Memory. THe Compute Unit process the data and store the result back into the same memory cell through the use of the parallel adder in $X$. Note that the parallel adder in $Y$ is **not** used.

### Class $I-IV$
Micro-instructions similar to class $I-II$, with the main differenze being that they use the parallel adder in $Y$ to compute the address of the memory cell.

## Classes using only the Compute Unit

### Class $II-II$
Micro-instructions which use either only or concurrently both the Compute Unit and the Address Unit. They could have or not condinutions during or at the end of the process, which **are not** computed through the parallel adder in $Y$.


### Class $II-III$
Micro-instructions which use either only or concurrently both the Compute Unit and the Address Unit. They could have or not condinutions during or at the end of the process, which **are** computed through the parallel adder in $Y$.

### Classes of cyclyc micro-instructions

### Class $III-I$
Cyclic micro-instructions which use the Compute Unit and produce information **without** the parallel adder in $X$.

### Class $III-II$
Cyclic micro-instructions which use the Compute Unit and produce data **using** the parallel adder in $X$.