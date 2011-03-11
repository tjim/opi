# imap-command

Parse

    command             = tag SP (command-any | command-auth | command-nonauth | command-select) CRLF

where

    command-any         = "CAPABILITY" | "LOGOUT" | "NOOP" | x-command
    command-auth        = append | create | delete | examine | list | lsub | rename | select | status | subscribe | unsubscribe
    append              = "APPEND" SP mailbox [SP flag-list] [SP date-time] SP literal
    command-nonauth     = login | authenticate | "STARTTLS"
    authenticate        = "AUTHENTICATE" SP auth-type *(CRLF base64)
    base64              = *4base64-char [base64-terminal]
    base64-terminal     = 2base64-char "==" | 3base64-char "="
    base64-char         = %d43 | %d47-57 | %d65-90 | %d97-122
    auth-type           = atom
    command-select      = "CHECK" | "CLOSE" | "EXPUNGE" | copy | fetch | store | uid | search
    create              = "CREATE" SP mailbox
    date-time           = DQUOTE date-day-fixed "-" date-month "-" date-year SP time SP zone DQUOTE
    date-day-fixed      = SP DIGIT | 2DIGIT
    delete              = "DELETE" SP mailbox
    examine             = "EXAMINE" SP mailbox
    list                = "LIST" SP mailbox SP list-mailbox
    login               = "LOGIN" SP userid SP password
    lsub                = "LSUB" SP mailbox SP list-mailbox
    list-mailbox        = 1*list-char | string
    list-char           = %d33 | %d35-39 | %d42-91 | %d93-122 | %d124-126
    password            = astring
    rename              = "RENAME" SP mailbox SP mailbox
    select              = "SELECT" SP mailbox
    status              = "STATUS" SP mailbox SP "(" status-att *(SP status-att) ")"
    status-att          = "MESSAGES" | "RECENT" | "UIDNEXT" | "UIDVALIDITY" | "UNSEEN"
    subscribe           = "SUBSCRIBE" SP mailbox
    tag                 = 1*(%d33 | %d35-36 | %d38-39 | %d44-91 | %d93-122 | %d124-126)
    time                = 2DIGIT ":" 2DIGIT ":" 2DIGIT
    uid                 = "UID" SP (copy | fetch | search | store)
    store               = "STORE" SP sequence-set SP store-att-flags
    store-att-flags     = ["+" | "-"] "FLAGS" [".SILENT"] SP (flag-list | flag *(SP flag))
    flag-list           = "(" [flag *(SP flag)] ")"
    flag                = "\Answered" | "\Flagged" | "\Deleted" | "\Seen" | "\Draft" | flag-keyword | flag-extension
    flag-extension      = %d92 atom
    search              = "SEARCH" [SP "CHARSET" SP astring] 1*(SP search-key)
    search-key          = "ALL" | "ANSWERED" | "BCC" SP astring | "BEFORE" SP date | "BODY" SP astring |
                          "CC" SP astring | "DELETED" | "FLAGGED" | "FROM" SP astring | "KEYWORD" SP flag-keyword |
                          "NEW" | "OLD" | "ON" SP date | "RECENT" | "SEEN" | "SINCE" SP date | "SUBJECT" SP astring |
                          "TEXT" SP astring | "TO" SP astring | "UNANSWERED" | "UNDELETED" | "UNFLAGGED" |
                          "UNKEYWORD" SP flag-keyword | "UNSEEN" | "DRAFT" | "HEADER" SP header-fld-name SP astring |
                          "LARGER" SP number | "NOT" SP search-key | "OR" SP search-key SP search-key |
                          "SENTBEFORE" SP date | "SENTON" SP date | "SENTSINCE" SP date | "SMALLER" SP number |
                          "UID" SP sequence-set | "UNDRAFT" | sequence-set | "(" search-key *(SP search-key) ")"
    flag-keyword        = atom
    date                = date-text | DQUOTE date-text DQUOTE
    date-text           = date-day "-" date-month "-" date-year
    date-year           = 4DIGIT
    date-month          = "Jan" | "Feb" | "Mar" | "Apr" | "May" | "Jun" | "Jul" | "Aug" | "Sep" | "Oct" | "Nov" | "Dec"
    date-day            = 1*2DIGIT
    fetch               = "FETCH" SP sequence-set SP ("ALL" | "FULL" | "FAST" | fetch-att |
                                                      "(" fetch-att *(SP fetch-att) ")") [fetch-modifiers]
    fetch-modifiers     = SP "(" fetch-modifier *(SP fetch-modifier) ")"
    fetch-modifier      = fetch-modifier-name [SP fetch-modif-params]
    fetch-modifier-name = tagged-ext-label
    tagged-ext-label    = tagged-label-fchar *tagged-label-char
    tagged-label-fchar  = %d45-46 | %d65-90 | %d95 | %d97-122
    tagged-label-char   = %d45-46 | %d48-58 | %d65-90 | %d95 | %d97-122
    fetch-modif-params  = tagged-ext-val
    tagged-ext-val      = tagged-ext-simple | "(" [tagged-ext-comp] ")"
    tagged-ext-simple   = sequence-set | number
    tagged-ext-comp     = astring | tagged-ext-comp *(SP tagged-ext-comp) | "(" tagged-ext-comp ")"
    fetch-att           = "ENVELOPE" | "FLAGS" | "INTERNALDATE" | "RFC822" [".HEADER" | ".SIZE" | ".TEXT"] |
                          "BODY" ["STRUCTURE"] | "UID" | "BODY" section ["<" number "." nz-number ">"] |
                          "BODY.PEEK" section ["<" number "." nz-number ">"]
    section             = "[" [section-spec] "]"
    section-spec        = section-msgtext | section-part ["." section-text]
    section-text        = section-msgtext | "MIME"
    section-part        = nz-number *("." nz-number)
    section-msgtext     = "HEADER" | "HEADER.FIELDS" [".NOT"] SP header-list | "TEXT"
    header-list         = "(" header-fld-name *(SP header-fld-name) ")"
    header-fld-name     = astring
    copy                = "COPY" SP sequence-set SP mailbox
    sequence-set        = (seq-number | seq-range) *("," sequence-set)
    seq-range           = seq-number ":" seq-number
    seq-number          = nz-number | "*"
    nz-number           = digit-nz *DIGIT
    digit-nz            = %d49-57
    unsubscribe         = "UNSUBSCRIBE" SP mailbox
    mailbox             = "INBOX" | astring
    SP                  = %d32
    userid              = astring
    astring             = 1*ASTRING-CHAR | string
    string              = quoted | literal
    quoted              = DQUOTE *QUOTED-CHAR DQUOTE
    QUOTED-CHAR         = %d1-9 | %d11-12 | %d14-33 | %d35-91 | %d93-127 | %d92 quoted-specials
    quoted-specials     = %d34 | %d92
    DQUOTE              = %d34
    literal             = "{" number optional-plus "}" CRLF *CHAR8
    optional-plus       = [PLUS]
    PLUS                = %d43
    number              = 1*DIGIT
    CRLF                = CR LF
    LF                  = %d10
    CR                  = %d13
    CHAR8               = %d1-255
    ASTRING-CHAR        = %d33 | %d35-36 | %d38-39 | %d43-91 | %d93-122 | %d124-126
    x-command           = "X" atom
    atom                = 1*ATOM-CHAR
    ATOM-CHAR           = %d33 | %d35-36 | %d38-39 | %d43-91 | %d94-122 | %d124-126
    zone                = ("+" | "-") 4DIGIT
    DIGIT               = %d48-57

Output OK if the input parses correctly, and FAIL otherwise.

## Notes

A command is a command message in the IMAP protocol as specified by [RFC 3501](http://www.rfc-editor.org/rfc/rfc3501.txt) and related RFCs.

Literals need to be parsed specially.  Their specification is

    literal             = "{" number optional-plus "}" CRLF *CHAR8

with the side condition that the number indicates the length of the sequence of CHAR8s.

This specification is in [ABNF notation](http://www.rfc-editor.org/rfc/rfc4234.txt).
