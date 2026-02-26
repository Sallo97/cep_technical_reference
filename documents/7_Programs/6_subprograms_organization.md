# Subprograms distribution in memory
The CEP guarantees that there is no fixed location for any entity in memory, including programs. This means that any subprogram can be stored in any memory location. 

The place in memory for a subprogram is dediced during the assembly phase of the main program. The assembler need to guarantee two things:

1. how can a program know the memory location of its own subprograms.

2. retrieving the absoluto addressing e internal reference of a routine.

## Determining subprograms locations [[5](../0_Additional_resources/0_reference.md)]
For calling a subprogram, the current program needs to jump to the first instructon of the subprogram. However there is no way a user can know a priori the memory location of the subprogram, it will be decided by the assembler.

The solution used will employ a lista di trasfermento: a mapping between name labels and memory locations.

The user simply declare a name label for each subprogram, referring to it each time it wants to call it.

The assembler then works in two phases:

1. For each subprogram, it constructs its lista di trasferimento by looking through the code for each name label. Labels in the list are stored in order of appearance. Finally each, instruction containing a label will be substitued by its numbered entry in the lista di trasferimento.

2. When the assembler has determined the memory location for a subprogram, it updates the entries having as key the subprogram's name, adding the discovered address.

## Find the absolute address and internal references of a routine [[5](../0_Additional_resources/0_reference.md)]

A first solution consists in using the special parametric cell $63$, which stores the instruction counter. This allows us to easily determine the absolute address of the current instruction by adding the content of $63$ to the starting address of the routine. 

The downside of this approach is that it requires to waste a parametric address of the routine call for storing its absolute starting address.

A better solution employes the special parametric cell $0$. When constructing the dedicated memory area for a subprogram, the support programs of the CEP store automatically the start instruction address of the routine at cell $0$.

This process is implement through a series of pseudoistruzioni:

1. the pseudoistruzione "Ricerca origine del sottoprogramma" will retrieve from the associated lista di trasferimento the position of the entry associated to the routine.

2. the pseudoistruzione "Carica Argomenti" will have as addresses the current values of the parameters of the routine.

3. the pseudoistruzione "Esecuzione" will determine the $H0$ and $HU$ groups of the current routine and jump to the first instruction of the subprogram. 
    
    Additionally this pseudoistruzione will store in some celle parametriche the origin of the caller's $H0$ group and the address of its memory location. This information will be used by the pseudoistruzione "Rietro" which handles the return to the caller.

This solution not only allows us to freely distribute subprograms in memory, but also their storing in auxiliary memory.

In the case of storing a subprogram in the magnetic drum, the assembler will specify in its entries in the lista di trasferimento both its main memory address and the drum address. 

Then the "Ricerca origine del sottoprogramma" pseudoistruzione, when requested to call a subprogram in drum memory, will preventively load it from the drum to main memory.

## Subprogram parameters [[7](.../0_Additional_resources/0_Additional_resources/0_reference.md)]
One of the main problems of supporting subprograms is guaranteeing distinct parameter values at each subprogram call during execution.

Dynamic parameters are supported through the use of celle parametriche: the user provides to the subprogram the address of the celle parametriche groups $H0$ and $HU$, in which local and global parameters are stored.

Local parameters are stored at the start of group $H0$; the rest of the cells of the group are exclusive to the subprogram and can be used by it freely.
