# Lec 3 - Ambiguous Grammar and Recursive Descent Parsing
08/29/2024

## Ambiguity
#### Ambiguity
- A grammar is *ambiguous* if it has more than one parse tree for some string
	- Equivalently, there is more than one right-most or left-most derivation for some string
- Ambiguity is **BAD**
	- Leaves the meaning of programs ill-defined
![[ambiguous_grammar.png]]

#### Handling Ambiguity
- Most direct method is to rewrite grammar unambiguously
	- Example:
		- E -> E' + E | E'
		- E' -> id * E' | id | (E) * E' | (E)
	- The stratified grammar enforces the precedence of * over +
	- * is nested more deeply than + in the parse tree

#### Automatic Grammar Rewriting
- *Impossible* to convert automatically an ambiguous grammar to an unambiguous one
- Used with care, ambiguity can simplify the grammar
	- Sometimes allows for more natural definitions
	- We need disambiguation mechanisms

#### Disambiguating Declaration w/ Associativity
- Consider: E -> E + E | int
	- Ambiguous
	- Left associativity declaration: "%left +" rules out the right parse tree
- Consider: E * E + E | E * E | int
	- Precedence declarations: "%left +" < "%left \*"

## Recursive Descent Parsing (Top-Down Parser Implementation)
#### Recursive Descent Parsing
- The parse tree is constructed
	- **From top to bottom**: Call recursively a parse function for a non-terminal
	- **From left to right**: When function returns, call its siblings left to right still recursively
- Terminals are seen during the parse tree building in the order of appearance in token stream, i.e., input string of tokens
- **Lexeme**: A sequence of chars in a program that match a certain pattern
- **Token**: a pair of lexeme and its type; they are often used interchangeably
- Consider the grammar
	- E -> T | T + E
	- T -> int | int * T | ( E )
- Token stream is: (int_5)
	- 5 is lexeme; its type is integer
- Start w/ non terminal E
	- Try production rules for E *in order*

#### Recursive Descent Parsing Algorithm
- Let TOKEN be the type of token
	- Special tokens: INT, OPEN, CLOSE, PLUS, TIMES
- Let the global *next* point to the next input token
- Define boolean function that check for a match of:
	- A given token terminal
		- `bool term(TOKEN tok) { return *next++ == tok; }`
	- The $n_{th}$ production of S:
		- `bool S_n() {...}`
		- Is one particular production of S
	- Try all productions of S:
			- `bool S() {...}`
			- Combines all productions
			- True if any of them match
- For production E -> T
	- `bool E_1() { return T(); }`
- For production E -> T + E
	- `bool E_2 () { return T() && term(PLUS) && E();`
- For all productions of E (with backtracking)
```
bool E() {
	TOKEN *save = next;
	return (next = save, E_1()) || (next = save, E_2());
}
```
- To start the parser 
	- Initialize *next* to point to the first token
	- Invoke `E()`

#### Recursive Descent Parsing
- Simple and general parsing strategy
- Left recursion must be eliminated first (mostly easy by hand)
	- But that can be done automatically
- Used in many real compilers
	- E.g. GCC

## Caveats of Recursive Descent Parsing
#### Problem 1: Left Recursion
- Consider production S- > Sa
	- `bool S_1() { return S() && term(a); }`
	- `bool S() { return S_1(); }`
- A left-recursive grammar has a non-terminal S
	- S -> S$\alpha$ for some $\alpha$
- Recursive descent does not work in such cases

#### Left Recursion
- Consider the left recursive grammar
	- S -> S $\alpha$ | $\beta$
	- S generates all strings starting with a $\beta$ and followed by any number of $\alpha$'s
	- Can rewrite using right-recursion
		- S -> $\beta$ S'
		- S' -> $\alpha$S' | $\epsilon$

#### Problem 2: Backtracking of a Production that Succeeds
- Not the same type of backtracking as we talked about before
- Example: int * int
	- First int gets taken, then we're left with just *"\* int"*
	- Results in parsing error

#### Toward Backtracking Free Parsing?
- Picked the wrong production, a top-down parser may end up backtracking
	- Alternative is to look ahead and use context to pick the right production
	- This is the idea of *predictive parsing*
- How much lookahead is needed?
	- Determines K in LL(K)
		- **L**eft to right scanning, **L**eft most derivation, **K** tokens of look-ahead
		- *In theory* an arbitrary large amount of K is possible
		- *In practice*, K is 1

#### Left Factoring
- What if my grammar doesn't have LL(1) property?
	- Sometimes we can transform the grammar
- 