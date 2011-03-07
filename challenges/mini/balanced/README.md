# balanced

Parse a sequence of balanced tags:

    balanced = *(wsp tag balanced end-tag) wsp

where

    tag      = "<" 1*ALPHA ">"
    end-tag  = "</" 1*ALPHA ">"
    wsp      = *(SP|HTAB|CR|LF)
    ALPHA    = %d65-90|%d97-122
    SP       = %d32
    CR       = %d13
    HTAB     = %d9
    LF       = %d10

Output OK if the input parses correctly, and FAIL otherwise.

## Notes

This challenge is specified with [ABNF notation](http://www.rfc-editor.org/rfc/rfc4234.txt).
