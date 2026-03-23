# Control panel
The control panel is made of a display, the 36-bits keyboard $QZ$, the 15-bits keyboard $QI$, a series of control keys, the 8-bits switching circuit keyboard $KIM$ , and a set of light bulbs. [[11](../helper_resources/references.md)]

## The display
The display is made of a series of neon bulbs, showing when the computer is stopped the content of all the main registers ($E, C, A, B, R, N, H_0, H_1$) and of the the last memory cell (stored as $LZ$). [[11](../helper_resources/references.md)]

## The $QZ$ keyboard
A 36-bits keyboard connected with the Main Memory through register $Z$. It is used for transfering a word to the computer. The transfer is done either through the instruction $EQZ$, or the manual instruction $QZI$. [[11](../helper_resources/references.md)]

## The $QI$ keyboard
A 15-bit keyboard used for addresses. This keyboard is used for sending an address using one of the manual instructions $QTZ, QLZ, QSI, QZI, QIV$. [[11](../helper_resources/references.md)]

## Control keys for debug classes
When pressed each keys activates the automatic debug for an instruction class:
- $TVF$ for the fixed point overflow class.
- $TVM$ for the floating point overflow class.
- $ICP$ for the parametric cell class.
- $SAL$ for the jump class.
- $CBP$ for the parity bit class.
[[11](../helper_resources/references.md)]

## Switching circuit $KIM$
A 8-bit keyboard having the following keys:
- $INT$
- $Q \ TZ$: for calling the magnetic drum.
- $Q \ LZ$ for loading data from the photoelectic reader (from the italian "caricamente lettore fotoelettrico").
- $Q \ SI$ for jumping to the address written in keyboard $QI$.
- $Q \ ZI$ for transfering the word written in $QZ$ into memory.
- $Q \ IV$ for reading a memory cell.
- $IBS$ external instruction setted over $QZ$.
[[11](../helper_resources/references.md)]
<!-- KIM is 8-bits, but has only 7 keys -->

## The starting button $PA$
Responsible for starting execution of the CEP. [[11](../helper_resources/references.md)]

## The single key $AI$
It can be setted either to $AUT$ or $IST$. This key is also used for terminating execution. [[11](../helper_resources/references.md)]

## Set of light bulbs
A series of light bulbs s.t.:

- $R$ (from the italian *"Rosso"*, i.e. "red"): a red gear indicator light.
- $V$ (from the italian *"Verde"*, i.e. "green"): a green light for starting execution over internal instructions.
- $B$ (from the italian *"Blu"*, i.e. "blue")" a blue light for starting execution over manual instructions.

## Additional keys
The control panel has other keys not listed here, used for debugging operations.

# Starting conditions
The starting conditions (from the italian "condizioni di avviamento") are determined by the switching circuit $KIM$ and the $AI$ key. [[11](../helper_resources/references.md)]

The $AI$ key can be at several positions:

1. If $AI$ is at position $AUT$ (from the italian *"AUTomatico"*, i.e. "automatic") and $KIM$ is at position $INT$ (from the italian *"INTerno"*, i.e. "inside"), and the CEP is stopped (i.e. the green light bulb is active), then by pressing the starting button $PA$, execution starts. The first instruction will be the one at the memory cell pointed by register $N$. During execution the red light bulb is active.

1. If $AI$ is at position $IST$ and $KIM$ is at $INT$ (i.e. the green light bulb is active), the computer executes the instruction in register $N$ and then it stops.

In any other position of switching circuit $KIM$ the CEP stays idle, **having active the blue light bulb**.

1. If $AI$ is at position $IST$, by pressing the starting button then the CEP executes the manual instruction associated to the position of $KIM$. Then it stops showing the blue light bulb.

To move the machine into automatic mode, the user needs to move $KIM$'s at position $INT$ and then press the start button $PA$.

1. If $AI$ is at position $AUT$ and the machine is idle, by pressing the starting button $PA$, the machine executes the instruction associated to the position of $KIM$, then it continues (lighting up the red light bulb) executing instruction whose address it at register $N$. This is connected to the address setted in keyboard $QI$ for the instructions $QTZ, QLZ, QSI$, and it is equal to the content of $N$ for all others. This mode is used for executing a manual instruction and then move to automatic mode seamlessly.

⚠︎ Note that during execution the position of switching circuit $KIM$ is ignored, the CEP assumed that it is always set to $INT$.


