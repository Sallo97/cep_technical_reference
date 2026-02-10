# The Ferrite Rod Store Technique
The Ferrite Rod Store Technique, also called Fixed Store or Slug Store was a Read-Only Memory (ROM) technology developed to solve the gap in speed between the CPU's logic gates and the much slower memory systems.

This technology was first introduced by the Manchester Atlas computer in 1962. It was used to hold the **Extracodes** (the equivalent of the CEP's pseudoinstructions) and the supervisor code.

The technique relies on **inductive coupling** (i.e. to let electrical energy jump from one wire to another without the need for them to touch) between two sets of wires: *drive wires* and *sense wires*. The wires are arranged in a perpendicular grid, referred to as a *mesh*. 

The mesh distributes the wires into a horizontal set of rows and a vertical set of columns. Perpendicular wires have very little magnetic coupling between them.

## Sending Information
To store a binary $"1"$ a small ferrite rod, about $1 mm$ in diameter and $5 mm$ long, is inserted into the hole where the wire cross. When a current pulse is sent through a drive wire, the ferrite rod concentrates the magnetic field and couples the energy into the sense wire, creating a detectable voltage.

If no rod is present at the intersection, no energy is transferred, and the sense wire remains silent.

## Previous Approches
Before the ferrite rod store, micro-programs were implemented using Diode Matrices, but they became impractical as the number of micro-instructions grew, the size of the matrix became large and larger, causing problem in avoiding electical noises.