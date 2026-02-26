# CEP Overview [[9](../0_Additional_resources/0_reference.md)]

The CEP was a digit-based parallel binary computer mainly designed for scientific problems. It's design has strong similarity to the [Atlas computer developed at the University of Manchester](https://curation.cs.manchester.ac.uk/atlas/).

It had a parallel binary architecture with fixed 36-bit word length, and supported both fixed point and floating point arithmetics.

## General Structure [[9](../0_Additional_resources/0_reference.md)]

### Arithmetic Unit
The unit responsible for all arithmetic computations. It supports both fixed point and floating point arithmetics.

Its main components are:

- Registers $A$ and $B$, which work as **accumulators**.

- Register $C$, which could stores the **memory address of the current operand**, or generic **auxiliary data**.

- The **adder** $AD$.

- Switching circuits $KU, KA, KB, KC, KV$.

#### ⚠︎⚠︎⚠︎ Extra components ⚠︎⚠︎⚠︎
Although they are not part of the arithmetic unit logic, physically are also placed:

- Register $Z$, used for **retrieving data from memory**
- The associated switching circuit $KZ$. 

### Memory Address Unit
The memory address unit has the responsability of both understanding the component referred by the address of instructions (e.g. register, main memory, auxiliary memory, etc...) and to retrieve the data from it.

The addresses are 15 bit wide.

It has the following components:

- Register $H^{0}$ stores the start address of the **local parametric cell group**. This group of parametric cells is reserved to the current program in execution. It can be though as the memory area where the local variables of a block of code is stored.

- Register $H^{1}$ (also referred in some papers as $H_U$) stores **the start address of the global parametric cells groups**. This group of parametric cells is common to all programs. it can be though as the global heap of the execution.

- Register $R$ stored the **relative address** of the two parametric cells in reserved memory.

- Register $N$ stored the **address of the instruction being called**.

- The **adder** $AJ$.

- Switching cirucits $KN, KR, KL, KK, KI$.

### Memory
The CEP's memory is divided into a *faster* **main memory** (constructed as a **magnetic core memory**) storing up to $8192$ words and access time of $5,5 \mu s$; and a *slower* **auxiliary memory** (in the form of a **magnetic drum**) of $16384$ words and access time of $10ms$.

#### Main Memory Unit
The main memory is divided into **two magnetic core groups**. Each group contains $4096$ cores, distributed among $36$ planes. The access time is of $10 \mu sec$ [[11](../0_Additional_resources/0_reference.md)]. 

To allow memory accesses operations it employs a **Coincident-Current Driving System** using linear transformer matrices and reading circuits, inhibit circuits, and pre-drive circuits. Also to synchronize the access with the rest of the architecture, it used delay lines for determining timing pulses for register $Z$ and its switching circuit $DZ$.

#### Auxiliary Memory Unit
The auxiliary memory is a magnetic drum, which has control circuit $TM$ for both reading and writing using characters of 6-bit (+1 parity bit). It was possible to transfer from or to it both single memory word or contiguous blocks of memory words of dynamic length.

### Control Unit
The control unit is responsible for orchestrating the execution. It is based upon Wilkes' micro-program design. Physically it is constructed as a **Manually Programmable Read Only Memory** (MPROM). 

The Micro-program Control Unit (MCU) employed the following components:

- Register $O$, which **stores the current operation code** of the micro-instruction to execute.

- $SC$ is the **driver sub-matrix**.

- $DC$ the **micro-instruction decoder**. It transforms the current micro-instruction code into the series of control signals which instruct the machine to execute the associated micro-order. 

    It has $256$ columns (i.e. the inputs) and $240$ rows (i.e. to outputs).

- A **timing system** responsible to synchronize the computer's various unit during execution.

- A circuit for determing the next micro-instruction.

###  I/O devices
The CEP had a series of both input and output devices.

To handle communication with them, register $E$ worked as a buffer both for reading from or writing to an I/O device. The size was of one memory word (36-bit).

⚠︎ Note that the magnetic drum (the auxiliary memory) is an I/O device, still I have preferred to write about it in the Memory section.

#### List of devices

- **Magnetic Tapes** used both for input and output. The control circuit $NM$ responsible for their reading and writing worked using characters of 6-bit (+1 parity bit). It could've transfer blocks of words of dynamic length.

    The magnetic tapes where used both as auxiliary memory and as "quick entry". Inside of them the data is divided into blocks, whose length is fixed during the writing operation. The first cell of each block specifies its length.

- **Two photoelectric sensors** used mainly for reading punched tapes at $300$ characters per second. Their control circuit $LF$ readed characters of 6-bit (+1 parity bit). It could've transfer blocks of dynamic length.

- A **parallel printer** used mainly for output. It could write $150$ rows per minute with each row having $120$ characters. Each character occupied 11-bits. It had itsown control circuit, named $SP$.

- A **tape punches** used mainly for output. It supported up to$60$ characters per second. The punches could punch using both 7-bits or 5-bits. Its control circuit is in common with the teletype.

- A **teletype** used mainly for output. It supported up to $7$ characters per second.

- The **external timing system** indicated as $TE$.

- The **manual command panel**.

- Two switching circuits $KS$ and $KE$

### Speed metrics 

<!-- Copy table 3.1 at page 99 in CEP's book. -->


The control unit resided in a Manually Programmable Read Only Memory (MPROM). The idea behind its development came thanks to prototype donated by professor T. Kilburn from Manchester University.

### Power Consumption data
Supply voltage from $-150V$ to $+250V$ in $50V$ steps.

The whole CEP required a stedy 80KWatt.

The main magnetic core memory has $8192$ words; the auxiliary memory is composed of a magnetic drum with $15.384$ words and $8$ magnetic tape units. [[8](../0_Additional_resources/0_reference.md)]

Each instructions occupies a full word, with a single address doubly modifiable by means of memory cells, called **parametric cells**. [[8](../0_Additional_resources/0_reference.md)]

There are $128$ instructions and $219$ **pseudo-instructions** which refer to arbitrary closed subroutines. Input programs are loaded as paper table, while the output can be generated through a typewriter or paper tape or a printer. [[8](../0_Additional_resources/0_reference.md)]
