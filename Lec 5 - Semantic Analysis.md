# Lec 5 -  Semantic Analysis
09/05/2024

#### Road to Semantic Analysis
- [[Lec 4 - Lexical Analysis Overview|Lexical analysis]]
	- Detects inputs with illegal tokens
- [[Lec 2 - Syntax Analysis Overview|Parsing]]
	- Detects inputs with ill-formed parse trees
- Semantic analysis
	- Last "front end" phase
	- Catches all remaining errors

#### Theory of Practice and Frontend
- Lexical analysis
- Parsing
	- Context free grammars
- Semantic analysis (type/scope rule checking)

#### For Parsing, can we just use RE of Scanning?
- Regex can describe tokens for scanning
	- Easy to implement with DFAs
- So just use them to describe the syntax of a programming language?
	- **NO** - They don't have enough power to express non-trivial syntax
	- E.g., nested constructs
#### For Semantic Analysis, can we just use CFG of Parsing?
- CFG can expressively describe syntax of program for parsing
	- Not all strings of tokens are valid programs
	- Context-free language for describing valid strings of tokens
- So just use them to describe the semantic of a programming language
	- **NO** - Parsing cannot catch some errors
	- Some language constructs are not context free (e.g., undefined variable error)

#### Categories of Semantic Analysis
- Examples of semantic rules
	- Variables must be defined *before* being used
	- A variable should *not* be defined *multiple* times
	- In an assignment stmt, the var and expr must have the same *type*
	- The test expr of an if stmt must have a *boolean type*
- Two categories
	- Rules regarding *types*
	- Rules regarding *scopes*
- Aka *non-context free syntactic analysis*

#### Type Information/Checking
- **Type Info** - Describes what kind of values correspond to different constructs: variables, statements, expressions, functions, etc.
- **Type Checking Rules** - Set of rules, used by type judgment, which ensure the type consistency of different constructs in the program

#### Type Judgment
- Static semantics = formal notation which describes type judgments
	- E : T means "*E is a well-typed expression for type T*"
	- In the type checking sense, E is typable if there is some type T such that E : T
- Type judgement examples
	- 2 : int
	- true : bool
	- 2 * (3 + 4) : int
	- "Hello" : string
	- false : bool
	- (true ? 2 : 3) : int

#### Deriving a Type Judgment 
- Consider the judgment - (b ? 2 : 3) : int
- What do we need to decide so that this is a well-typed expression of type int?
	- b must be a bool (b : bool)
	- 2 must be an int (2 : int)
	- 3 must be an int (3 : int)
- Figuring out these things will help the compiler derive what the type of the expression is

#### Type Judgment (Cont.)
- Can see if INT == unaryE.getType(A) for a type judgment A |-- unaryE : INT
	- A is a type context (aka environment), i.e., *a symbol table*
	- UnaryE is term to be type-checked
	- INT is its type in the given type context A

#### Type Inference Rule
- **Basis** for judging type of a higher level of construct
- E.g., inference rule of addition expression (+) says:
	- If antecedent judgements (i.e., premises) are true with some substitutions for A, E1, E2, then the consequent judgement (i.e. conclusion) is true (Derivable) with a consistent substitution
	- If E1 : int and E2 : int, then E1 + E2 : int
- Type checking of program can be done by navigating the AST in a recursive fashion

#### Proof Tree
- Expr is well-typed *if there exists a type derivation* for a type judgment of the expr
- Type derivation* is shown by a *proof tree* where all the leaves are axioms

#### Scope Information
- Characterizes the *declaration of identifiers* and the portions of the program where it is allowed to use each identifier
	- Example identifiers: vars, functions, objects, labels
- Lexical scope (*LS*) of identifier declaration determines the portion, i.e., the scope of identifier
- LS: textual region of the program
	- Examples: Statement block, formal arg list, object body, function or method body, source file, whole program

#### Symbol Tables
- Semantic checks refer to properties of identifiers in the program (i.e. their scope or type) and ensure their correct usage
- Need an environment to store the info about identifiers and this is implemented as a symbol table
- There is a hierarchy of scopes in the program
	- Use similar idea with a *hierarchy of symbol tables*
	- One symbol table for each scope
	- Each symbol table contains the symbols declared in that lexical scope

#### Symbol Table Operations
- Initially, we need to create a new empty symbol table for each scope
- Then, we need to insert new identifiers in the scope to the table
- In the subsequent stages of the compiler we need to access the info from the table - *use lookup function*

#### Array implementation
- *Simple implementation* = array
	- One entry per symbol
	- Scan array for lookup, compare name at each entry
- *Disadvantage*
	- Table has fixed size
	- Need to know in advance the # of entries; only known in syntax analysis, decoupling it from semantic analysis

#### List Implementation
- *Dynamic structure* = list
	- One cell per entry in table
	- Can grow dynamically
- *Disadvantage*
	- Inefficient for large symbol tables
	- Need to scan half the list on average -> **slow** search

#### Hash Table Implementation
- *Efficient implementation* = hash table
	- It is an array of lists (buckets)
	- Uses a hashing function to map symbol name to corresponding bucket
		- `hashfunc: string -> int`
	- Good hash func = even distribution in the buckets

#### Sheaf of Tables
- Described in 5.5.3 in *Engineering a Compiler*
- Create a new table for each scope 
	- `class ScopeTable` in USC
- Chain them together for lookup
		- `ScopeTable* mParent`
		- `std::list<ScopeTable*> mChildren`
		- Again, each table is a hash table

#### Summary
- **Semantic checks** ensure the correct usage of variables, objects, expressions, statements, functions, and labels in the program
	- *Scope semantic checks* ensure that identifiers are correctly used within the scope of their declaration
	- *Type semantic checks* ensure the type consistency of various constructs in the program
- **Symbol Tables** - a data structure for storing info about symbols in the program
	- Used in semantic analysis and subsequent compiler stages