# Lec 1 - Compilers Overview
08/22/2024
#### Why Compilers?
- Ease of programming
	- Nobody wants to program in assembly
	- Instead, we write a program at a high level language and let compilers take care of the complexity
- Compilers can also optimize our programs
	- Occupy less resources
	- Increased reliability & security
- Without compilers, you can't effectively leverage new HW features

#### Compiler Structure
1. **Scanning**: Lexical Analysis
2. **[[Lec 2 - Syntax Analysis Overview|Parsing]]**: Syntax Analysis
3. **Type checking/variable binding**: Semantic Analysis
4. **Optimization**
5. **Code Generation**

- Steps 1-3 are the *"front-end"*
- Steps 4-5 are the *"back-end"*
- Some people also consider the *"middle-end"*  to be optimization
- The first 3 can be understood by the human language analogy

#### Identifying Words (Lexical Analysis)
- Your program is just a series of characters in a sense; the same with an English sentence
- The first compilation step is to recognize words
	- In English, we know that the separators between words are spaces
- The lexical analyzer divides program text into units of "words" called "tokens" in compiler speak
	- Tokenizer
	- Scanner
##### Example
- Input
	- `if x == y then z = 1; else z = 2;`
- Units
	- *if*, *x*, *\==*, *then* , *z*, *=*, *1*, *;*, *else*, *z*, *=*, *2*, *;*, *BLANK*

#### Parsing: Understanding Structure (Syntax Analysis)
- Once words are recognized, the next step is to understand sentence structure
- Parsing = Diagramming Sentences
	- The diagram is a tree
##### Diagramming a Sentence

![[diagraming_a_sentence.png]]

##### Parsing Programs
- Consider `if x == y then z = 1; else z = 2;`

![[parsing_programs.png]]

#### Semantic Analysis (Understanding Meaning)
- We don't know yet exactly how humans understand the meaning of a sentence
- Compilers do a limited form of semantic analysis
	- They don't know what your program is doing 
	- So just try to catch some inconsistency

##### Semantic Analysis Example (Variable Binding)
- Programming languages define strict rules to avoid ambiguities

```cpp
{
	int Jack = 3
	{
		int Jack = 4;
		std::cout << Jack << std::endl;
	}
}
```

- Prints 4 because of cpp variable binding rules

#### Code optimization
- Optimization has no strong counterpart in English
	- But it's sort of like editing
- Automatically modify programs so that they
	- Run faster
	- Use less memory
	- E.g. `X = Y * 0` is the same as `X = 0` (for *integer*)
		- 60 cycles vs. 0 cycles
		- For floating point operations, `NaN * 0 = NaN`
			- Thus we don't always want this

#### Code Generation
- A translation into another language
	- Analogous to human translation (English -> French)
- Compilers for C, C++
	- Produce assembly (usually)
	- Optimal program -> x86 ASM
- Other code generators
	- `Javac` generates Java Bytecode

#### How Do Compilers Work?

![[how_compilers_work.png]]

#### The Big Picture of "Scanning & Parsing"
- **Scanning:** Converting input text into stream of known objects called tokens. Simplifies the *parsing* process
	- Lexical rules of language dictate how a legal word is formed by concatenating alphabet
- **Parsing:** Validating program structure by using rules of grammar & building intermediate representation (IR) of code
	- Grammar dictates syntactical rules of language (i.e., how a legal sentence could be formed)
	- We want a *context free grammar*

#### Overall Operation
##### Parsing
- Parser is in control of the overall operation
	- Demands scanner to produce tokens
- Scanner reads input file into token buffer and recognizes token based on the lexical rule
	- Token is returned to parser
- Parser tries to match the token based on the grammar rule
	- Failure: Syntax Error!
	- Success:
		- Do nothing and return to get the next token
		- Takes *semantic* action in the meantime
##### Semantic Analysis
- Semantic action, e.g., for the declaration of a variable, put it into symbol table
- Semantic check, e.g., for each use of the variable use, check the symbol table catching undefined variables
- If the checks succeed, continue to get the next token
	- Otherwise, report errors (non-declared variable usage)
- Continue until there are no more tokens

#### Parsing (Matching)
- Start matching using a grammar rule
- When match takes place at a certain position, move further
	- Get next token from scanner & repeat
- If expansion needs to be done, choose the appropriate rule
	- Things enclosed with `< >` are to be expanded
##### Examples
- `<DECL-STMT> -> <TYPE>VAR<VAR-LIST>`
- `<TYPE> -> INT | FLOAT | ...`

#### Traditional Three-pass Complier

![[three_pass_compiler.png]]


