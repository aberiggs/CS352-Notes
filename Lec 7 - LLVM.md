#### Static Single Assignment (SSA)
- Assume unlimited register machine
	- Number of registers in LLVM is infinite; they're called *virtual registers*
- Variable (register) may be assigned to *only once*
	- Makes compiler optimization easier
- Now registers are created to hold the results of every instruction
- Register allocation determines the mapping from virtual registers to physical registers

#### Inefficient?
- Lots of redundant memory operations
- Stores followed immediately by loads
- The mem2reg pass cleans it up
	
#### Basic Block
- A *sequence* of instructions that execute in order forms a **basic block**
- Basic blocks end with control flow change, e.g., branch (where flow *diverges*) and it's target (where flow *merges*)

#### Phi Node for SSA is a way to go
- Phi functions are special instructions used in SSA construction
	- Phi functions are just a notation fiction for SSA transformation, thus being an artificial definition
- Their value is determined by the preceding *basic block*
- PHI nodes must come before any non-PHI instructions in a basic block