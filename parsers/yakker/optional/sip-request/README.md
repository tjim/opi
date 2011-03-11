# sip-request in Yakker

This is a complicated data format, with over 280 nonterminal symbols.

The grammar is ambiguous, in particular, it uses a nonterminal, extension-header, as a catch-all for headers not specifically mentioned in the spec.  This production matches all headers, so, for example, both Content-Length and extension-header match Content-Length headers.

Clearly the intention is that extension-header should only match if nothing else does.  For the moment I have simply disabled extension-header in our parser.
