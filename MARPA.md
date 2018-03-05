# An introduction to how I learnt MARPA

## What this is about

MARPA is a parsing algorithm.  

At its simplest, it takes a **"grammar"** and an **"input stream"** as inputs.  It first parses the grammar and stores it in memory.  It then parses the input stream, token by token, and attempts to match it against the grammar.

If the input stream "matches" or "conforms to" the grammar, then we can say that the input was _recognized_ by the parser, and is thus _**grammatically correct**_.

## Parsing in simple terms

**Parsing** in computer science is the process of taking text in some language, specified as a _grammar_ as input, and making sense out of it.

As with many things in the world of CS, "making sense" means the input text is converted into a data structure - specifically, a **tree**, because it is hierarchical and expresses relationships well - and usually, an _**abstract syntax tree (AST)**_.  

The process of generating this tree checks the syntax of the input; i.e., a valid AST is generated if the input is valid.  Validity here means it conforms to the _rules_of the grammar it is based on.

