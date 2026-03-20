# External Timing Circuit
The external timing system is responsible for managing the consistency for operations communicating with external devices (e.g. the magnetic tapes). Microinstructions employing input and output devices external to the kernel of the CEP are referred to as **external microinstructions**.
 
In these microinstructions the temporal cycle is broken down between the internal timing system and a pulse which is dispatched throught the External Control Unit to the requested I/O device. Then:

- **if the I/O device is ready:** it sends back to the internal timing system a new pulse which goes back into the cycle for terminating it and move to the next microinstruction.

- **if the I/O device is not ready:** the internal timing system waits that the device is ready. 

This communication between the device and the timing circuit and the CEP behave like an asynchronous machine. [[10](../helper_resources/reference.md)]

<!--Put here all diagram of external temporal cycles-->

## Minimization of temporal cycles
Registers in $X^{'}$ and $Y^{'}$ are closed in themself through the logical networks $X$ and $Y$. For mantaining constants the data at the entry of $X^{'}$ and $Y^{'}$ during the clear & write pulse, the entry and exit of registers needs to include the delay $R_1$.

For avoiding that the delay is summed to the delay for elaborating the data, its necessary to put it at the exit of the register. 

The delay can be minimized by design the register in a way that it is written only by a writing pules or two pulses which simultenaously clear and write.

For this reason we set $R_1 \geq $ time of the writing pulse. [[10](../helper_resources/reference.md)]

### Technical Details

The CEP has a controlled register which has a writing pules of $0.4 \mu sec$ and a delay of $0.4 \mu sec$. The delay is placed at the exit of the register.

All registers of $X^{'}$ and $Y^{'}$ (mainly those pulsed by the main memory: $N$, $C$, $R$, $H^{0}$, and $H^{1}$), are pulsed at the end of the cycle.

The delay $T_3$ (in cycle $II-III$) is reduced by $R_1$ if the delay $R_{C_{min}}$ of the Control Unit (int cycle $II-II$) satisfy the relation $R_{C_{min}} \geq R_1$. This is important only for microinstruction that do not use the Main Memory, for the one using only the registers is unimportant.

From these considerations we can derive that the best design is obtained by setting $R_C = R_1$ where $R_{C_{max}} \approx R_{C_{min}}$. [[10](../helper_resources/reference.md)]