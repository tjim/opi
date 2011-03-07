# utf8-sequence

Parse

    length ":" UTF8-octets

where

    length      = 1*DIGIT
    DIGIT       = "0" | "1" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9"
    UTF8-octets = *UTF8-char
    UTF8-char   = UTF8-1 | UTF8-2 | UTF8-3 | UTF8-4
    UTF8-1      = %x00-7F
    UTF8-2      = %xC2-DF UTF8-tail
    UTF8-3      = %xE0 %xA0-BF UTF8-tail | %xE1-EC 2( UTF8-tail ) |
                  %xED %x80-9F UTF8-tail | %xEE-EF 2( UTF8-tail )
    UTF8-4      = %xF0 %x90-BF 2( UTF8-tail ) | %xF1-F3 3( UTF8-tail ) |
                  %xF4 %x80-8F 2( UTF8-tail )
    UTF8-tail   = %x80-BF
    
and length, when interpreted as a number in decimal, gives the number of UTF8-chars in the UTF8-octets sequence.

Output OK if the input parses correctly, and FAIL otherwise.

## Notes

This challenge is specified with [ABNF notation](http://www.rfc-editor.org/rfc/rfc4234.txt).

The grammar for UTF8 characters is taken from [RFC 3629](http://www.rfc-editor.org/rfc/rfc3629.txt), and it permits "overlong sequences."

We do not specify an upper limit on the length.  Of course, solutions will have to impose some limit.
