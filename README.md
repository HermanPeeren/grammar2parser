# grammar2parser
Goal: parser generator, to generate a (Parsica-)parser from a grammar. The grammar-input is in EBNF. The grammar defines the parser, which can be *automatically*  built from it.

Parsica is a toolbox to build parsers, written in PHP, using principles from Functional Programming, like referential transparency, immutability and above all: composability. In this project we want to keep that same style of programming.

Grammar is the language of languages; a grammar defines the well-formed strings in a language. A grammar is a set of production-rules. In Extended Backus-Naur form (EBNF) we can express a Context Free Grammar (CFG). Symbols produce expressions, containing other symbols and/or terminal expressions. Here is an incomplete part of a grammar that  describes a JSON-array, with terminals in single quotes (and the symbol "value" not resolved):
```
array = '[' whitespace ']' | '[' elements ']'
elements = element ',' elements | element
element = whitespace value whitespace
whitespace = ' ' | '\n' | | '\r' | '\t' | ''
```
| indicates OR.

We use a PEG (Parsing Expression Grammar) = a grammar where the choices are ordered. So when we have a rule:
```
protocol = 'https' | 'http'
```
We first try to match 'https' and only if that would not succeed, we try 'http'.  
See: https://parsica.verraes.net/docs/tutorial/order_matters

### Parts:
* Grammar in PHP: an array of rules, each rule consisting of a symbol and an array of expressions
* Parser for EBNF, so we can read a grammar into our PHP-format 
* The generator, that can build the parser source-code from the grammar. 

### Investigate
* can we (automatically) convert any CFG into a Parsica-parser? Are there limitations to such a LL-parser?
* how to define indentation, used to indicate begin and end of blocks, in a grammar? 
* can we do anything with context sensitivity? For instance; if we want to check uniqueness of keys in JSON-objects?

### References
* https://parsica.verraes.net/
* https://en.wikipedia.org/wiki/Context-free_grammar
* https://en.wikipedia.org/wiki/Parsing_expression_grammar
* https://en.wikipedia.org/wiki/Extended_Backus–Naur_form
* https://two-wrongs.com/parser-combinators-parsing-for-haskell-beginners.html