## Sending and receiving informations [[2](./0_reference.md)]
The Micro-programming control unit manages the communication with registers through asynchronous logic networks. These registers then send and receive informations across the machine through other aynchronous logic networks.

This is different from registers, whose content is updated synchronously through *impulsi di tensione*.

## Writing safely into registers [[2](./0_reference.md)]
To be sure that the requested information to be stored in a register is written safely, the CEP must guarantee that the input content is safely mantained during the whole writing process.
