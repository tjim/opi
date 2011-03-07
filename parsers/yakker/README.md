# Yakker

These are parsers written in [Yakker](https://github.com/attresearch/yakker) to solve the [OPI](https://github.com/tjim/opi) challenges.

Parsers are stored in .bnf files, e.g., email-address.bnf, and can be run as follows:

    cat INPUTFILE | yakker exec email-address.bnf
