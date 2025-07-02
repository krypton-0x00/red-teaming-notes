## Memory Segments
- **Stack** - Holds non-static local variables. Discussed more in-depth soon.
- **Heap** - Contains _dynamically_ allocated data that can be uninitialized at first.
- **.data** - Contains global and static data initialized to a _non-zero value_.
- **.bss** - Contains global and static data that is _uninitialized or initialized to zero_.
- **.text** - Contains the code of the program (don't blame me for the name, I didn't make it).

## Overview of Memory Sections
![[Pasted image 20250626085608.png]]

## Stack Frame Layout
![[Pasted image 20250626090017.png]]