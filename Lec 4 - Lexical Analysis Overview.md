# Lec 4 - Lexical Analysis (Scanning)
09/03/2024
#### Frontend Structure
- *Source Code -> Preprocessor -> Lexical Analysis, Syntactic Analysis, Semantic Analysis -> Abstract Syntax Tree*
- Language Preprocessor
	- Catches Trivial Errors

#### Lexical Analysis Process
- Lexical analysis
	- Transforms multi-character input stream to token stream
	- Reduces length of program representation
		- Removes spaces

#### How to Describe Tokens
- Use Regular Expressions to describe programming language tokens
- A regular expression (RE) is defined inductively
	- a
		- Ordinary character stands for itself
	- $\epsilon$
		- Empty string
	- R|S
		- Either R or S (alternation), where R and S are REs
	- RS
		- R followed by S (concatenation)
	- R*
		- Concatenation of R zero or more times
	- (R|S)*
		- Concatenation of either R or S zero or more times; parentheses are notational symbols for grouping
- Each token can be defined by using a regular expression
	- A regular language *R* describes a set of string of characters denoted *L(R)*

#### How to Break up Text
- REs alone are not enough, we need a rule for choosing when we get multiple matches
	- **Longest matching** token wins
	- Ties in length resolved by **priorities**
		- Token specification often decides priorities
	- RE's + priorities + longest matching token rule = *definition of a scanner*
		- aka lexer, tokenizer, or lexical analyzer

#### Automatic Generation of Lexers
- 2 programs developed at Bell Labs in mid 70's for use with Unix
	- **Lex** - transducer, transforms an input stream into the alphabet of the grammar
	- **Yacc** - Another compiler compiler
		- *Bison* is the GNU counterpart

#### How Does Lex Work?
- Formal basis for lexical analysis is the finite state automaton (FSA)
	- REs generate regular sets
	- FSAs recognize regular sets
- FSA - informal defn
	- Finite set of states
	- Transitions between states
	- An initial state
	- *A set of final (accepting) states*

#### Two Kinds of FSA
- Non-deterministic finite automata (NFA)
	- There may be multiple possible transitions for one input in a given state
	- There may be some transitions that don't consume an input (i.e., $\epsilon$-transition)
- Deterministic finite automata (DFA)
	- The transition from each state is uniquely determined by the current input character, i.e., state transition is completely determined by input
	- No $\epsilon$-transitions



