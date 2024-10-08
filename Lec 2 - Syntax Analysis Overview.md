# Lec 2 - Syntax Analysis (Parsing) Overview
08/27/2024
#### Parsing Analogy
- Syntax analysis for natural languages
	- Identify the function of each word
	- Recognize if a sentence is grammatically correct

#### Parsing Overview
- **Goal:*** determine if the input token stream satisfies the syntax of program
	- Not all strings of tokens are programs
	- Parser *must* distinguish between valid and invalid strings of tokens
- What do we need for the syntax analysis?
	- **Theory:** An expressive way to describe syntax
		- E.g. a language for describing valid strings of tokens
	- **Implementation:** A mechanism that determines if the input token stream satisfies the syntax description
		- E.g. a method for distinguishing valid from invalid strings of tokens and reporting error messages

#### Context-Free Grammars (CFG)
- Theoretical foundation
- Programming languages have *recursive* structure
- For example, an `EXPR` can be
	- `if EXPR then EXPR else EXPR endif`
	- `while EXPR loop EXPR endwhile`
- *Context-free grammars* are a natural notation for this recursive structures
- Consist of 4 components:
	1. **Terminals**: token or $\epsilon$ (lowercase)
	2. **Non-terminals:** syntactic variables (uppercase)
	3. **Start symbol S:** special non-terminal
	4. **Set of recursive rewriting rules (productions):** in form of LHS -> RHS
		- LHS = single non-terminal
		- RHS = concatenation of *non-terminals*, *terminals*, and *$\epsilon$ 
		- Specify how the LHS non-terminal may be expanded
- *Note:* $\epsilon$ is "empty"
- Shorthand notation - *vertical bar* for multiple productions
	- S -> a S a | T
	- T -> b T b | $\epsilon$

#### Derivation (High-Level)
- Sequence of applications of productions that result in a valid string of tokens
- For a program to be valid, there must be a valid derivation for S* -> {$t_1, t_2, ..., t_n$}

#### CFG - Example
- Grammar definition
	- S -> \[S\]S
	- S -> $\epsilon$
- Does this grammar accept the string "\[\[]]"
	- Yes

#### Parsers
- Exist as CFG acceptors
	- Or rejectors :)

#### Derivation Algorithm
1. Begin with a string with only the start symbol S
2. Replace any non-terminal X (something that needs to be broken down into more pieces) in the string by the right-hand side of some production
3. Repeat (2) until there are no non-terminals

#### Grammar for Sum Expression
- Grammar
	- S -> E + S | E
	- E -> number | (S)
- *Derive:* (1 + 2 + (3 + 4)) + 5
	- We will do left-most non-terminals first
	- S -> E + S -> (S) + S -> (E + S) + S -> (1 + S) + S -> (1 + E + S) + S -> (1 + 2 + S) + S -> (1 + 2 + E) + S -> (1 + 2 + (S)) + S -> (1 + 2 + (E + S)) + S -> (1 + 2 + (3 + S)) + S -> (1 + 2 + (3 + E)) + S -> (1 + 2 + (3 + 4)) + S -> (1 + 2 + (3 + 4)) + E -> (1 + 2 + (3 + 4)) + 5

# Parse Tree (Graphical Representation of Derivation)
#### Showing Derivation with a Parse Tree
- Derivation is a sequence of productions beginning with start symbol S -> ... -> ... -> ...
- Can be represented as a tree
	- Start symbol as the root
		- For a production $X$ -> $Y_1...Y_2$ add children $Y_1...Y_n$ to node X
	- An in-order traversal of the leaves is the original input comprised of only terminals
	- The parse tree shows the association of operations, input string does not

Original Input: (1 + 2 + (3 + 4)) + 5

![[parse_tree.png| 200]]

#### Abstract Syntax Tree
- Abbreviated as *AST*
- Discards (abstract) unneeded information

#### Ambiguous Grammar
- You can end up with 2 different parse trees