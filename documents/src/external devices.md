# External Devices[[2]](./0_reference.md)
The CEP's external components are:

- **Drum Memory(*tamburo* in italian)**: external data storage using punched tape (*nastri magnetici* in italian) for loading data and programs onto the CEP's main memory.

- **Two photoelectric sensor (*lettori fotoelettrici* in italian)**: A photoelectric sensor is a device used to determine the distance, absence, or presence of an object by using a light transmitter, often infrared, and a photoelectric receiver. 
<!-- I am still not clear on what the CEP could do with it -->

- **Parallel Printer (*stampante in parallelo* in italian)**: a device.
<!-- I am still not sure on what it is. -->

- **Two Perforatori veloci**: **NOT CONFIRMED, THIS IS MY THEORY**: used for writing punched tape for storing data or programs produced by the CEP. One has five channels, the other seven.

- **Teleprinter (*telescriventi* in italian)**: used to send and receive text messages over a a telegraphy network.

## Storing a subprogram in the *tamburo magnetico* [[5](./0_reference.md)]
This process is automatic and only requires the starting address of the subprogram in the main memory and the starting address of the subprogramm in the magnetic tape. These information will be included by the assembler in the *liste di trasferimento* for use by the pseudoinstruction which handles calls. In case a program is in the *tamburo magnetico* the pseudoinstruction *Ricerca origine del sottoprogramma* will copy it into main memory before starting the call execution.

This makes coding calls completely independent from where concretely their code is stored, since the actual loading is automated.s