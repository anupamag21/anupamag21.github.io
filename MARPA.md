# An introduction to how I learnt MARPA

## What this is about

MARPA is a parsing algorithm.  

At its simplest, it takes a **"grammar"** and an **"input stream"** as inputs.  It first parses the grammar and stores it in memory.  It then parses the input stream and attempts to match it against the grammar.

If the input stream "matches" or "conforms to" the grammar, then we can say that the input was _recognized_ by the parser, and is thus _**grammatically correct**_.

## Parsing in simple terms

**Parsing** in computer science is the process of taking text in some language, specified in the form of a _grammar_, as input, and making sense out of it.

As with many things in the world of CS, "making sense" means the input text is converted into a data structure - specifically, a **tree**, because it is hierarchical and expresses relationships well - and usually, an _**abstract syntax tree (AST)**_.  

The process of generating this tree checks the syntax of the input; i.e., a valid AST is generated if the input is valid.  Validity here means it conforms to the _rules_of the grammar it is based on.

A grammar is thus a set of rules.  It could define the set of _tokens_ that are acceptable and the _order_ in which they must appear.

Let's take the example of a simple calculator (not a scientific one).

The grammar for this application will have to define:

- The set of allowable tokens 
  - This would be the digits `0-9`, and the characters `+`, `-`, `*`, `/`.
- The order in which they are allowed to occur
  - For instance, `+` and `-` are legal at the start of a string.  `*` and `/` are not.
  - None of `+`, `-`, `*`, `/` are allowed at the end of a string.
  - ...
  
Such a grammar is usually expressed in a format called [Backus-Naur Form]https://en.wikipedia.org/wiki/Backus%E2%80%93Naur_form.

The grammar understood by Marpa is written in a format of BNF called _extended_ BNF (EBNF), named SLIF-DSL.  That scary acronym is actually two acronyms, standing for "Scanless Interface - Domain Specific Language".
In English, the former just means that there is no explicit "scanner" (commonly called "lexer") required to split the input up into tokens - the parsing algorithm takes care of that.  See the next section for more details.
The latter is pretty self-explanatory - it means that the Marpa grammar can contain tokens which are highly specific to its own domain, and may not be sensible or applicable in other domains (this is in contrast to a "general purpose" language).

A parser could well be hand-written.  Quite obviously, this approach will present difficulties if the language it is written for is of any measure of complexity.

Usually, parsers are _generated_.  MARPA is one such parser generator - it generates the parser for you based on the grammar defined.

## Steps in parsing

Parsing in basic terms is constituted of the following steps:

- Take the input stream and **tokenize** it.  Tokenization is often called **lexical analysis** and is sometimes the responsibility of a wholly different program, called a **lexer** (or scanner, as noted above), rather than being part of the parser itself.
  - This breaks up the input stream into allowable _tokens_.  For instance, the expression `x = a + b + (c*d)` could be split into the tokens `x`, `=`, `a`, `+`, `b`, `+`, `(`, `c`, `*`, `d`, and `)`.  Evidently, a good parser must not care about the number of whitespaces in such a context (though it well could, or should, in other contexts).  That is, `x  = a+b+ (c * d)` must produce the same tokens.
  - How is it decided that this is how tokenization should work?  For instance, why couldn't the list of output tokens include something like `(c` or `*d`?  Well, that depends on the **rules of tokenization** defined by the lexical grammar.  
- Next, determine if the tokens form a syntactically correct expression and are in the right order, as determined by the **rules of parsing**.  
  - The parser must ultimately generate a derivation of the input from the grammar.  It does so by generating, or attempting to generate, a tree (such as an AST) from the input.
  - The rules to generate this tree depends on the grammar.  For instance, for the calculator application, I might build stacks for expressions and operators; and generate a tree from the stacks.  The code for the AST could be produced by a traversal of the tree.  See this[https://softwareengineering.stackexchange.com/a/254075] for an excellent example of AST construction.
  
