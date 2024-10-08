# Lec 6 - Intermediate Representation (IR)
09/10/2024

#### IR
- Represents program in a source/target *independent* way
- Is where analyses/transformations happen
- Enable retargetable compilers
- Part of the "*middle-end*"

#### What Makes a good IR?
- Captures high-level language constructs
	- Easy to translate from program
	- Supports high-level optimizations
- Captures low-level machine features
	- Easy to translate to ASM
	- Supports machine-level optimizations
- Language/machine independent though
	- Sounds like two conflicting goals

#### Multiple IRs
- Most compilers use 2-3 IRs:
	- **High-level IR (HIR)** - Language *independent* but closer to the language
	- **Medium-level IR (MIR)** - Machine *independent* but closer to the machine
	- A significant part of the compiler is both language and machine independent

#### HIR
- Essential the AST + new node types not generated by parser
	- Must be expressive for all input languages
- Preserves high-level language constructs
	- Structured control flow: `if`, `while`, `for`, `switch`
	- Variables, expressions, statements, functions
- Allows high-level optimizations based on properties of the source language
	- Function inlining, reuse of constant variables, loop transformations

#### MIR
- Between the AST and assembly, e.g., 3-address code, LLVM
- Convenient for translation to high-quality machine code
- A set of instructions which emulates an abstract machine in form of opcode and operands (still machine-independent)
- Have not-so-high-level constructs like unstructured jumps, virtual registers (temporaries), memory locations
- Types of instructions
	- Arithmetic/logic (a = b OP c), unary operations, data movement (move, load, store), function call/return, branches

#### Low-Level IR
- ASM + extra pseudo instructions
	- E.g. MIR (Machine IR - different from middle-level IR): LLVM's machine specific IR
- Machine dependent - kinds of instructions are determined and target registers are allocated
- Translation to ASM is trivial
- Allows for optimization of code for low level considerations - scheduling and memory layout

#### Alternatives for Compiler's Middle-End
- **Three-address code** - Simplifies complex expressions to a reduced set of instructions
	- *Advantage* - Makes compiler analysis/optimization much easier
- **Tree representation**
	- Was popular for CISC architectures
	- *Advantage* - Easier to generate complex machine code
- **Stack machine**
	- Like Java bytecode
	- *Advantage* - Easier to generate from AST

#### Three-Address Code (TAC) Instructions
- a = b OP c
	- Originally called TAC because at that time instruction had *at most 3 addresses* or operands
	- May have fewer operands
- Also called quadruples - a, b, c, OP
- This is not enforced today

#### TAC Operands
- The operands in TAC can be
	- Program variables
	- Constants or literals
	- Temporary variables (virtual registers in LLVM)
- Temporary variables = new locations
	- Used to store intermediate values
	- Because TAC is not as expressive as high-level languages

#### Lowering HIR (AST) to MIR (TAC)
- We have nested language constructs
	- E.g., while nested within an if statement
- Need an algorithmic way to translate
	- Different strategy for each HIR construct (i.e., each AST node)
	- HIR construct -> sequence of MIR instructions
		- For PA3, our medium level IR (MIR) is LLVM
- Solution  (called 'syntax-directed translation')
	- Start from the HIR (AST like) representation
	- Define translation for each node in MIR (TAC)
	- Recursively translate AST nodes by visiting them with depth-first order

#### IR Lowering (Syntax Directed Translation)
- Use temporary variables to convey the result between the IR instructions
- *Temporary* variables in the MIR store intermediate values corresponding to the nodes in the HIR

#### Notation for IR Lowering
- Use the following notation:
	- \\\[\[*e*\]\] = the MIR representation of HIR construct *e*
- \\\[\[*e*]] is a *sequence* of MIR (TAC) instructions
- If *e* is an expression (or statement expression), \[\[*e*]] represents a value
	- Denoted as: t = \[\[e]]
	- MIR representation of *e* whose result value is stored in *t*
- Temporary variables = new locations
	- Use temporary variables to store intermediate values during this syntax-directed translation

