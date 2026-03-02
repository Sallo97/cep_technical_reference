# Control panel[[11](../0_Additional_resources/0_reference.md)]
The control panel is made of a display, a 36-bits keyboard, a 15-bits keyboard, a series of control keys, switching circuit ($KIM$), set of light bulbs.

## The display
The display is made of a series of neon bulbs, used for seeing, when the computer is stopped, the content of:

- all the main registers ($E, C, A, B, R, N, H_0, H_1$).

- the last memory cell that has been read $LZ$.

## The $QZ$ keyboard
A 36-bits keyboard connected with the Main Memory ($Z$) for transfering a word to the machine. The transfer is done either through the instruction $EQZ$, **or the manual instruction $QZI$**. 

## The $QI$ keyboard
A 15-bit keyboard used for addresses. This keyboard is used for sending the address to one of the manual instructions $QTZ, QLZ, QSI, QZI, QIV$.

## Control keys
A series of control keys with two positions:
    - $TVF$ for the fixed point overflow class.
    - $TVM$ for the floating point overflow class.
    - $ICP$ for the parametric cell class.
    - $SAL$ for the jump class.
    - $CBP$ for the parity bit class.

## Switching circuit $KIM$
A 8-bit keyboard having:
- $INT$
- $Q TZ$ for having called the magnetic drum (from the italian "chiamato tamburo magnetico").
- $Q LZ$ for having loaded the photoelectic reader (from the italian "caricamente lettore fotoelettrico").
- $Q SI$ for jumping to the address written in keyboard $QI$.
- $Q ZI$ for transfering the word written in $QZ$ into memory.
- $Q IV$ for reading a memory cell.
- $IBS$ external instruction setted over $QZ$.

## The starting button $PA$
Responsible for starting execution of the CEP.

## The single key $AI$
It can be setted either to $AUT$ or $IST$. This key is also used for terminating execution.

## Set of light bulbs
A series of light bulbs s.t.:

- $R$ (from the italian *"Rosso"*, i.e. "red"): a red gear indicator light.
- $V$ (from the italian *"Verde"*, i.e. "green"): a green light for starting execution over internal instructions.
- $B$ (from the italian *"Blu"*, i.e. "blue")" a blue light for starting execution over manual instructions.

## Additional keys
The control panel has other keys not listed here, used for debugging operations.

# Starting conditions
The starting conditions (from the italian "condizioni di avviamento") are determined by the switching circuit $KIM$ and the $AI$ key.

The $AI$ key can be at several positions:

1. If $AI$ is at position $AUT$ (from the italian *"AUTomatico"*, i.e. "automatic") and $KIM$ is at position $INT$ (from the italian *"INTerno"*, i.e. "inside"), and the CEP is stopped (i.e. the green light bulb is active), then by pressing the starting button $PA$, execution starts. The first instruction will be the one at the memory cell pointed by register $N$. During execution the red light bulb is active.

1. If $AI$ is at position $IST$ and $KIM$ is at $INT$ (i.e. the green light bulb is active), the computer executes the instruction in register $N$ and then it stops.

In any other position of switching circuit $KIM$ the CEP stays idle, **having active the blue light bulb**.

1. If $AI$ is at position $IST$, by pressing the starting button then the CEP executes the manual instruction associated to the position of $KIM$. Then it stops showing the blue light bulb.

To move the machine into automatic mode, the user needs to move $KIM$'s at position $INT$ and then press the start button $PA$.

1. If $AI$ is at position $AUT$ e the machine is idle, by pressing the starting button $PA$, the machine executes the instruction associated to the position of $KIM$, then it continues (lighting up the red light bulb) executing instruction whose address it at register $N$. This is connected to the address setted in keyboard $QI$ for the instructions $QTZ, QLZ, QSI$, and it is equal to the content of $N$ for al others. This mode is used for executing a manual instruction and then move to automatic mode seamlessly.

⚠︎ Note that during execution the position of switching circuit $KIM$ is ignored, the CEP assumed that it is always set to $INT$.


