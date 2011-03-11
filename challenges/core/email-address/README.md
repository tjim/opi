# email-address

Parse

    address = mailbox | group 

where

    group          = display-name ":" [mailbox-list | CFWS] ";"
    mailbox-list   = mailbox *("," mailbox)
    mailbox        = name-addr | addr-spec
    name-addr      = [display-name] angle-addr
    display-name   = phrase
    phrase         = 1*word
    angle-addr     = [CFWS] "<" addr-spec ">" [CFWS]
    addr-spec      = local-part "@" domain
    local-part     = dot-atom | quoted-string
    domain         = dot-atom | domain-literal
    dot-atom       = [CFWS] dot-atom-text [CFWS]
    dot-atom-text  = 1*atext *("." 1*atext)
    domain-literal = [CFWS] "[" *([FWS] dcontent) [FWS] "]" [CFWS]
    dcontent       = dtext | quoted-pair
    dtext          = NO-WS-CTL | %d33-90 | %d94-126
    word           = atom | quoted-string
    quoted-string  = [CFWS]
    atom           = [CFWS] 1*atext [CFWS]
    atext          = ALPHA | DIGIT | "!" | "#" | "$" | "%" | "&" | "'" |
                     "*" | "+" | "-" | "/" | "=" | "?" | "^" | "_" |
                     "`" | "{" | " | " | "}" | "~"
    DIGIT          = %d48-57
    ALPHA          = %d65-90 | %d97-122
    CFWS           = *([FWS] comment) ([FWS] comment | FWS)
    comment        = "(" *([FWS] ccontent) [FWS] ")"
    ccontent       = ctext | quoted-pair | comment
    quoted-pair    = "\" text
    text           = %d1-9 | %d11 | %d12 | %d14-127
    ctext          = NO-WS-CTL | %d33-39 | %d42-91 | %d93-126
    NO-WS-CTL      = %d1-8 | %d11 | %d12 | %d14-31 | %d127
    FWS            = [*WSP CRLF] 1*WSP
    WSP            = SP | HTAB
    SP             = %d32
    HTAB           = %d9
    CRLF           = CR LF
    LF             = %d10
    CR             = %d13

Output OK if the input parses correctly, and FAIL otherwise.

## Notes

An address is an email address as specified by [RFC 2822](http://www.rfc-editor.org/rfc/rfc2822.txt).

This specification is in [ABNF notation](http://www.rfc-editor.org/rfc/rfc4234.txt).

The language of addresses is not regular, due to the nested parentheses of comments.

See also [this page](http://www.dominicsayers.com/isemail/) for another take.
