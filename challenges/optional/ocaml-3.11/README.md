# ocaml-3.11

Parse the input according to the [Ocaml 3.11 specification](http://caml.inria.fr/pub/distrib/ocaml-3.11/ocaml-3.11-refman.txt).

Output OK if the input parses correctly, and FAIL otherwise.

## Notes

The Ocaml grammar is a context-free, LR(1) grammar over lexical tokens, and the lexical specification is straightforward (unlike Python).
