# sip-request

Parse

    Request              = Request-Line *message-header CRLF [message-body]

where

    Request-Line         = Method SP Request-URI SP SIP-Version CRLF
    SIP-Version          = "SIP" "/" 1*DIGIT "." 1*DIGIT
    message-body         = *OCTET
    OCTET                = %d0-255
    message-header       = (Accept | Accept-Encoding | Accept-Language |
                            Alert-Info | Allow | Authentication-Info |
                            Authorization | Call-ID | Call-Info |
                            Contact | Content-Disposition |
                            Content-Encoding | Content-Language |
                            Content-Length | Content-Type | CSeq | Date |
                            Error-Info | Expires | From | In-Reply-To |
                            Max-Forwards | MIME-Version | Min-Expires |
                            Organization | Priority |
                            Proxy-Authenticate | Proxy-Authorization |
                            Proxy-Require | Record-Route | Reply-To |
                            Require | Retry-After | Route | Server |
                            Subject | Supported | Timestamp | To |
                            Unsupported | User-Agent | Via | Warning |
                            WWW-Authenticate | extension-header) CRLF  
    extension-header     = header-name HCOLON header-value
    header-value         = *(TEXT-UTF8char | UTF8-CONT | LWS)
    header-name          = token
    Warning              = "Warning" HCOLON warning-value *(COMMA warning-value)
    WWW-Authenticate     = "WWW-Authenticate" HCOLON challenge
    Via                  = ("Via" | "v") HCOLON via-parm *(COMMA via-parm)
    User-Agent           = "User-Agent" HCOLON server-val *(LWS server-val)
    Unsupported          = "Unsupported" HCOLON option-tag *(COMMA option-tag)
    To                   = ("To" | "t") HCOLON (name-addr | addr-spec) *(SEMI to-param)
    Timestamp            = "Timestamp" HCOLON 1*DIGIT ["." *DIGIT] [LWS delay]
    delay                = *DIGIT ["." *DIGIT]
    Supported            = ("Supported" | "k") HCOLON [option-tag *(COMMA option-tag)]
    Subject              = ("Subject" | "s") HCOLON [TEXT-UTF8-TRIM]
    Server               = "Server" HCOLON server-val *(LWS server-val)
    Route                = "Route" HCOLON route-param *(COMMA route-param)
    Retry-After          = "Retry-After" HCOLON delta-seconds [comment] *(SEMI retry-param)
    Require              = "Require" HCOLON option-tag *(COMMA option-tag)
    Reply-To             = "Reply-To" HCOLON rplyto-spec
    Record-Route         = "Record-Route" HCOLON rec-route *(COMMA rec-route)
    Proxy-Require        = "Proxy-Require" HCOLON option-tag *(COMMA option-tag)
    Proxy-Authorization  = "Proxy-Authorization" HCOLON credentials
    Proxy-Authenticate   = "Proxy-Authenticate" HCOLON challenge
    challenge            = ("Digest" LWS digest-cln *(COMMA digest-cln)) | other-challenge
    digest-cln           = realm | domain | nonce | opaque | stale |
                           algorithm | qop-options | auth-param
    domain               = "domain" EQUAL LDQUOT URI *(1*SP URI) RDQUOT
    URI                  = absoluteURI | abs-path
    Priority             = "Priority" HCOLON priority-value
    Organization         = "Organization" HCOLON [TEXT-UTF8-TRIM]
    TEXT-UTF8-TRIM       = 1*TEXT-UTF8char *(*LWS TEXT-UTF8char)
    TEXT-UTF8char        = %d33-126 | UTF8-NONASCII
    Min-Expires          = "Min-Expires" HCOLON delta-seconds
    Max-Forwards         = "Max-Forwards" HCOLON 1*DIGIT
    MIME-Version         = "MIME-Version" HCOLON 1*DIGIT "." 1*DIGIT
    In-Reply-To          = "In-Reply-To" HCOLON callid *(COMMA callid)
    From                 = ("From" | "f") HCOLON from-spec
    from-spec            = (name-addr | addr-spec) *(SEMI from-param)
    from-param           = tag-param | generic-param
    Expires              = "Expires" HCOLON delta-seconds
    Error-Info           = "Error-Info" HCOLON error-uri *(COMMA error-uri)
    error-uri            = LAQUOT absoluteURI RAQUOT *(SEMI generic-param)
    Date                 = "Date" HCOLON SIP-date
    SIP-date             = rfc1123-date
    Content-Type         = ("Content-Type" | "c") HCOLON media-type
    media-type           = m-type SLASH m-subtype *(SEMI m-parameter)
    Content-Length       = ("Content-Length" | "l") HCOLON 1*DIGIT
    Content-Language     = "Content-Language" HCOLON language-tag *(COMMA language-tag)
    language-tag         = primary-tag *("-" subtag)
    Content-Encoding     = ("Content-Encoding" | "e") HCOLON content-coding *(COMMA content-coding)
    Content-Disposition  = "Content-Disposition" HCOLON disp-type *(SEMI disp-param)
    disp-type            = "render" | "session" | "icon" | "alert" | disp-extension-token
    disp-extension-token = token
    disp-param           = handling-param | generic-param
    handling-param       = "handling" EQUAL ("optional" | "required" | other-handling)
    Contact              = ("Contact" | "m") HCOLON (STAR | (contact-param *(COMMA contact-param)))
    contact-param        = (name-addr | addr-spec) *(SEMI contact-params)
    contact-params       = c-p-q | c-p-expires | contact-extension
    contact-extension    = generic-param
    c-p-q                = "q" EQUAL qvalue
    c-p-expires          = "expires" EQUAL delta-seconds
    STAR                 = SWS "*" SWS
    Call-Info            = "Call-Info" HCOLON info *(COMMA info)
    info                 = LAQUOT absoluteURI RAQUOT *(SEMI info-param)
    info-param           = ("purpose" EQUAL ("icon" | "info" | "card" | token)) | generic-param
    Call-ID              = ("Call-ID" | "i") HCOLON callid
    callid               = word ["@" word]
    CSeq                 = "CSeq" HCOLON 1*DIGIT LWS Method
    Authorization        = "Authorization" HCOLON credentials
    credentials          = ("Digest" LWS digest-response) | other-response
    digest-response      = dig-resp *(COMMA dig-resp)
    dig-resp             = username | realm | nonce | digest-uri |
                           dresponse | algorithm | cnonce | opaque |
                           message-qop | nonce-count | auth-param 
    dresponse            = "response" EQUAL request-digest
    digest-uri           = "uri" EQUAL LDQUOT digest-uri-value RDQUOT
    digest-uri-value     = Request-URI
    Request-URI          = SIP-URI | SIPS-URI | absoluteURI
    algorithm            = "algorithm" EQUAL ("MD5" | "MD5-sess" | token)
    Authentication-Info  = "Authentication-Info" HCOLON ainfo *(COMMA ainfo)
    ainfo                = nextnonce | message-qop | response-auth | cnonce | nonce-count
    cnonce               = "cnonce" EQUAL cnonce-value
    cnonce-value         = nonce-value
    Allow                = "Allow" HCOLON [Method *(COMMA Method)]
    Alert-Info           = "Alert-Info" HCOLON alert-param *(COMMA alert-param)
    alert-param          = LAQUOT absoluteURI RAQUOT *(SEMI generic-param)
    Accept-Language      = "Accept-Language" HCOLON [language *(COMMA language)]
    language             = language-range *(SEMI accept-param)
    language-range       = (1*8ALPHA *("-" 1*8ALPHA)) | "*"
    Accept-Encoding      = "Accept-Encoding" HCOLON [encoding *(COMMA encoding)]
    encoding             = codings *(SEMI accept-param)
    codings              = content-coding | "*"
    content-coding       = token
    Accept               = "Accept" HCOLON [accept-range *(COMMA accept-range)]
    accept-range         = media-range *(SEMI accept-param)
    media-range          = ("*/*" | (m-type SLASH "*") | (m-type SLASH m-subtype)) *(SEMI m-parameter)
    m-type               = discrete-type | composite-type
    discrete-type        = "text" | "image" | "audio" | "video" | "application" | extension-token
    composite-type       = "message" | "multipart" | extension-token
    m-subtype            = extension-token | iana-token
    iana-token           = token
    extension-token      = ietf-token | x-token
    ietf-token           = token
    m-parameter          = m-attribute EQUAL m-value
    m-value              = token | quoted-string
    m-attribute          = token
    accept-param         = ("q" EQUAL qvalue) | generic-param
    HCOLON               = *(SP | HTAB) ":" SWS
    HTAB                 = %d9
    message-qop          = "qop" EQUAL qop-value
    nextnonce            = "nextnonce" EQUAL nonce-value
    nonce                = "nonce" EQUAL nonce-value
    nonce-count          = "nc" EQUAL nc-value
    nc-value             = 8LHEX
    nonce-value          = quoted-string
    opaque               = "opaque" EQUAL quoted-string
    option-tag           = token
    other-challenge      = auth-scheme LWS auth-param *(COMMA auth-param)
    other-handling       = token
    other-response       = auth-scheme LWS auth-param *(COMMA auth-param)
    auth-scheme          = token
    auth-param           = auth-param-name EQUAL (token | quoted-string)
    auth-param-name      = token
    COMMA                = SWS "," SWS
    primary-tag          = 1*8ALPHA
    priority-value       = "emergency" | "urgent" | "normal" | "non-urgent" | other-priority
    other-priority       = token
    qop-options          = "qop" EQUAL LDQUOT qop-value *("," qop-value) RDQUOT
    qop-value            = "auth" | "auth-int" | token
    qvalue               = ("0" ["." *3DIGIT]) | ("1" ["." *3"0"])
    realm                = "realm" EQUAL realm-value
    realm-value          = quoted-string
    rec-route            = name-addr *(SEMI rr-param)
    request-digest       = LDQUOT 32LHEX RDQUOT
    response-auth        = "rspauth" EQUAL response-digest
    response-digest      = LDQUOT *LHEX RDQUOT
    RDQUOT               = DQUOTE SWS
    LHEX                 = %d48-57 | %d97-102
    LDQUOT               = SWS DQUOTE
    retry-param          = ("duration" EQUAL delta-seconds) | generic-param
    delta-seconds        = 1*DIGIT
    rfc1123-date         = wkday "," SP date1 SP time SP "GMT"
    date1                = 2DIGIT SP month SP 4DIGIT
    month                = "Jan" | "Feb" | "Mar" | "Apr" | "May" | "Jun" |
                           "Jul" | "Aug" | "Sep" | "Oct" | "Nov" | "Dec"
    route-param          = name-addr *(SEMI rr-param)
    rplyto-spec          = (name-addr | addr-spec) *(SEMI rplyto-param)
    rplyto-param         = generic-param
    name-addr            = [display-name] LAQUOT addr-spec RAQUOT
    display-name         = *(token LWS) | quoted-string
    RAQUOT               = ">" SWS
    LAQUOT               = SWS "<"
    addr-spec            = SIP-URI | SIPS-URI | absoluteURI
    absoluteURI          = scheme ":" (hier-part | opaque-part)
    opaque-part          = uric-no-slash *uric
    hier-part            = (net-path | abs-path) ["?" query]
    query                = *uric
    net-path             = "//" authority [abs-path]
    authority            = srvr | reg-name
    reg-name             = 1*(unreserved | escaped | "$" | "," | ";" | ":" | "@" | "&" | "=" | "+")
    abs-path             = "/" path-segments
    path-segments        = segment *("/" segment)
    SIPS-URI             = "sips:" [userinfo] hostport uri-parameters [headers]
    SIP-URI              = "sip:" [userinfo] hostport uri-parameters [headers]
    headers              = "?" header *("&" header)
    header               = hname "=" hvalue
    hvalue               = *(hnv-unreserved | unreserved | escaped)
    hname                = 1*(hnv-unreserved | unreserved | escaped)
    hnv-unreserved       = %d36 | %d43 | %d47 | %d58 | %d63 | %d91 | %d93
    rr-param             = generic-param
    scheme               = ALPHA *(ALPHA | DIGIT | "+" | "-" | ".")
    segment              = *pchar *(";" param)
    param                = *pchar
    pchar                = unreserved | escaped | ":" | "@" | "&" | "=" | "+" | "$" | ","
    server-val           = product | comment
    product              = token [SLASH product-version]
    product-version      = token
    comment              = LPAREN *(ctext | quoted-pair | comment) RPAREN
    ctext                = %d33-39 | %d42-91 | %d93-126 | UTF8-NONASCII | LWS
    RPAREN               = SWS ")" SWS
    LPAREN               = SWS "(" SWS
    srvr                 = [[userinfo "@"] hostport]
    stale                = "stale" EQUAL ("true" | "false")
    subtag               = 1*8ALPHA
    time                 = 2DIGIT ":" 2DIGIT ":" 2DIGIT
    to-param             = tag-param | generic-param
    tag-param            = "tag" EQUAL token
    uri-parameters       = *(";" uri-parameter)
    uri-parameter        = transport-param | user-param | method-param |
                           ttl-param | maddr-param | lr-param | other-param
    ttl-param            = "ttl=" ttl
    transport-param      = "transport=" ("udp" | "tcp" | "sctp" | "tls" | other-transport)
    other-param          = pname ["=" pvalue]
    pvalue               = 1*paramchar
    pname                = 1*paramchar
    paramchar            = param-unreserved | unreserved | escaped
    param-unreserved     = %d36 | %d38 | %d43 | %d47 | %d58 | %d91 | %d93
    method-param         = "method=" Method
    Method               = INVITEm | ACKm | OPTIONSm | BYEm | CANCELm | REGISTERm | extension-method
    extension-method     = token
    REGISTERm            = %d82 %d69 %d71 %d73 %d83 %d84 %d69 %d82
    OPTIONSm             = %d79 %d80 %d84 %d73 %d79 %d78 %d83
    INVITEm              = %d73 %d78 %d86 %d73 %d84 %d69
    CANCELm              = %d67 %d65 %d78 %d67 %d69 %d76
    BYEm                 = %d66 %d89 %d69
    ACKm                 = %d65 %d67 %d75
    maddr-param          = "maddr=" host
    lr-param             = "lr"
    uric                 = reserved | unreserved | escaped
    reserved             = %d36 | %d38 | %d43-44 | %d47 | %d58-59 | %d61 | %d63-64
    uric-no-slash        = unreserved | escaped | ";" | "?" | ":" | "@" | "&" | "=" | "+" | "$" | ","
    user-param           = "user=" ("phone" | "ip" | other-user)
    other-user           = token
    userinfo             = user [":" password] "@"
    user                 = 1*(unreserved | escaped | user-unreserved)
    user-unreserved      = %d36 | %d38 | %d43-44 | %d47 | %d59 | %d61 | %d63
    password             = *(unreserved | escaped | "&" | "=" | "+" | "$" | ",")
    unreserved           = %d33 | %d39-42 | %d45-46 | %d48-57 | %d65-90 | %d95 | %d97-122 | %d126
    escaped              = "%" HEXDIG HEXDIG
    username             = "username" EQUAL username-value
    username-value       = quoted-string
    via-parm             = sent-protocol LWS sent-by *(SEMI via-params)
    via-params           = via-ttl | via-maddr | via-received | via-branch | via-extension
    via-maddr            = "maddr" EQUAL host
    via-extension        = generic-param
    generic-param        = token [EQUAL gen-value]
    gen-value            = token | host | quoted-string
    via-branch           = "branch" EQUAL token
    sent-protocol        = protocol-name SLASH protocol-version SLASH transport
    transport            = "UDP" | "TCP" | "TLS" | "SCTP" | other-transport
    other-transport      = token
    protocol-version     = token
    protocol-name        = "SIP" | token
    SLASH                = SWS "/" SWS
    sent-by              = host [COLON port]
    COLON                = SWS ":" SWS
    SEMI                 = SWS ";" SWS
    via-received         = "received" EQUAL (IPv4address | IPv6address)
    via-ttl              = "ttl" EQUAL ttl
    ttl                  = 1*3DIGIT
    EQUAL                = SWS "=" SWS
    warning-value        = warn-code SP warn-agent SP warn-text
    warn-text            = quoted-string
    quoted-string        = SWS DQUOTE *(qdtext | quoted-pair) DQUOTE
    quoted-pair          = "\" (%d0-9 | %d11-12 | %d14-127)
    qdtext               = LWS | %d33 | %d35-91 | %d93-126 | UTF8-NONASCII
    UTF8-NONASCII        = (%d192-223 1UTF8-CONT) | (%d224-239 2UTF8-CONT) |
                           (%d240-247 3UTF8-CONT) | (%d248-251 4UTF8-CONT) | (%d252-253 5UTF8-CONT)
    UTF8-CONT            = %d128-191
    SWS                  = [LWS]
    LWS                  = [*WSP CRLF] 1*WSP
    WSP                  = %d9 | %d32
    CRLF                 = CR LF
    LF                   = %d10
    CR                   = %d13
    warn-code            = 3DIGIT
    warn-agent           = hostport | pseudonym
    pseudonym            = token
    hostport             = host [":" port]
    port                 = 1*DIGIT
    host                 = hostname | IPv4address | IPv6reference
    hostname             = *(domainlabel ".") toplabel ["."]
    toplabel             = ALPHA | (ALPHA *(alphanum | "-") alphanum)
    ALPHA                = %d65-90 | %d97-122
    domainlabel          = alphanum | (alphanum *(alphanum | "-") alphanum)
    IPv6reference        = "[" IPv6address "]"
    IPv6address          = hexpart [":" IPv4address]
    hexpart              = hexseq | (hexseq "::" [hexseq]) | ("::" [hexseq])
    hexseq               = hex4 *(":" hex4)
    hex4                 = 1*4HEXDIG
    HEXDIG               = %d48-57 | %d65-70 | %d97-102
    IPv4address          = 1*3DIGIT "." 1*3DIGIT "." 1*3DIGIT "." 1*3DIGIT
    DIGIT                = %d48-57
    SP                   = %d32
    wkday                = "Mon" | "Tue" | "Wed" | "Thu" | "Fri" | "Sat" | "Sun"
    word                 = 1*(alphanum | "-" | "." | "!" | "%" | "*" |
                              "_" | "+" | "`" | "'" | "~" | "(" | ")" |
                              "<" | ">" | ":" | "\" | DQUOTE | "/" |
                              "[" | "]" | "?" | "{" | "}")
    DQUOTE               = %d34
    x-token              = "x-" token
    token                = 1*(alphanum | "-" | "." | "!" | "%" | "*" |
                              "_" | "+" | "`" | "'" | "~")
    alphanum             = %d48-57 | %d65-90 | %d97-122

## Notes

A Request is a request message in the Session Initiation Protocol as specified by [RFC 3261](http://www.rfc-editor.org/rfc/rfc3261.txt).

This specification is in [ABNF notation](http://www.rfc-editor.org/rfc/rfc4234.txt).

The language of requests is not regular, due to the nested parentheses of comments.
