# External Devices
The CEP's supports five distinct types of external components. [[2](../helper_resources/references.md)]

- **Drum Memory(from the italian *tamburo magnetico*)**: the main Auxiliary Memory used for loading data and programs onto Main Memory.

- **Two photoelectric sensor (from the italian *lettori fotoelettrici*)**: a device used to read from punched tapes. It interprets the content by determine the distance, absence, or presence of an object by using a light transmitter, often infrared, and a photoelectric receiver. 

- **Parallel Printer (from the italian *stampante in parallelo*)**

- **Two fast puncher (from the italian *perforatore veloce*):**: used for writing punched tape. One has five channels, the other seven.

- **Teleprinter (from the italian *telescriventi*)**: used to send and receive text messages over a a telegraphy network.

## Storing programs in Auxiliary Memory
This process is handled automatically. It only requires the start address of the program in Auxiliary Memory, and the start address fo the memory area where the program will be loaded into Main Memory. These information will be provided by the assembler in the **transfer lists**. [[5](../helper_resources/references.md)]

In the particular case of a program stored in the magnetic drum, the loading into main memory is handled by the pseudoinstruction  *Ricerca origine del sottoprogramma*  this makes coding independent from where the code is stored, since the actual loading is automated. [[5](../helper_resources/references.md)]