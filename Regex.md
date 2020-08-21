# Regex

## 1. Basic REgex

- Obsolete

```man
.  # Any Character
^  # Line Beginning
$  # Line Ending
\<  # Word Beginning
\>  # Word Ending
\b  # Word Boundary
\B  # Not Word Boundary
\w  # Identifier Character
\W  # Non-Identifier Character
\NUM  # Catch Group Ref
\META_CHAR  # Literal Character
\(\)  # Catch Group
[]  # Char Set; - for Range; [.STRING.] for Collating Symbol as a Single Char; [=ELEMENT=] for Equivalence Class; [:NAME:] for Character Class: alnum, digit, punct, alpha, graph, space, blank, lower, upper, cntrl, print, xdigit
[^]  # Reverse Char Set; - for Range; [.STRING.] for Collating Symbol; [=ELEMENT=] for Equivalence Class; [:NAME:] for Character Class: alnum, digit, punct, alpha, graph, space, blank, lower, upper, cntrl, print, xdigit
*  # Previous Char [0, + \infty) Times
\+  # Previous Char [1, + \infty) Times
\?  # Previous Char [0, 1] Times
\{[NUM][,[NUM]]\}  # Previous Char [NUM, NUM] Times
\|  # Or
```

## 2. Extended REgex

- Modern

```man
.  # Any Character
^  # Line Beginning
$  # Line Ending
\<  # Word Beginning
\>  # Word Ending
\b  # Word Boundary
\B  # Not Word Boundary
\w  # Identifier Character
\W  # Non-Identifier Character
\NUM  # Catch Group Ref
\META_CHAR  # Literal Character
()  # Catch Group
[]  # Char Set; - for Range; [.STRING.] for Collating Symbol; [=ELEMENT=] for Equivalence Class; [:NAME:] for Character Class: alnum, digit, punct, alpha, graph, space, blank, lower, upper, cntrl, print, xdigit
[^]  # Reverse Char Set; - for Range; [.STRING.] for Collating Symbol; [=ELEMENT=] for Equivalence Class; [:NAME:] for Character Class: alnum, digit, punct, alpha, graph, space, blank, lower, upper, cntrl, print, xdigit
*  # Previous Char [0, + \infty) Times
+  # Previous Char [1, + \infty) Times
?  # Previous Char [0, 1] Times
{[NUM][,[NUM]]}  # Previous Char [NUM, NUM] Times
|  # Or
```

## 3. Perl-Compatible REgex

```man
# Special Character
\a  # Alarm Character
\A  # String Beginning
\b  # Word Boundary
\B  # Not Word Boundary
\cX  # Ctrl-X
\C  # Data Unit
\d  # Decimal Digit
\D  # Not Decimal Digit
\e  # Escape Character
\f  # Form Feed Character
\G  # First Matching Position
\h  # Horizontal White Space Character
\H  # Not Horizontal White Space Character
\K  # Reset Start of Match
\n  # New Line Character (Line Feed)
\N  # Not New Line Character
\p{PROPERTY}, \pX  # Character With Property
\P{PROPERTY}, \PX  # Character Without Property
Property:
  L Letter:
    Ll Lower Case
    Lm Modifier
    Lo Other
    Lt Title Case
    Lu Upper Case
    L& Lower Case, Upper Case or Title Case
  M Mark:
    Mc Spacing
    Me Encloding
    Mn Non-Spacing
  N Number:
    Nd Decimal
    Nl Letter
    No Other
  P Punctuation:
    Pc Connector
    Pd Dash
    Pe Close
    Pf Final
    Pi Initial
    Po Other
    Ps Open
  S Symbol:
    Sc Currency
    Sk Modifier
    Sm Mathematical
    So Other
  Z Separator:
    Zl Line
    Zp Paragraph
    Zs Space
  C Other:
    Cc Control
    Cf Format
    Cn Unassigned
    Co Private
    Cs Surrogate
  X PCRE Special:
    Xan Alphanumeric
    Xps POSIX Space
    Xsp Perl Space
    Xuc Univerally-Named Character
    Xwd Perl Identifier Character
\Q...\E  # Literal String
\r  # Carriage Return Character
\R  # New Line String
\s  # White Space Character
\S  # Not White Space Character
\t  # Tab Character
\v  # Vertical White Space Character
\V  # Not Vertical White Space Character
\w  # Identifier Character
\W  # Not Identifier Character
\X  # Unicode Extended Grapheme Cluster
\z  # String Ending
\Z  # String Ending Before \n
\0XX, \o{X...}  # Octal Character XX/X...
\xXX, \x{X...}, \uXXXX  # Hex Character XX/X.../XXXX

.  # Any Character
^  # Line Beginning
$  # Line Ending
\NUM, \gNUM, \g{NUM}  # Catch Group Ref by Number
\g{-NUM}  # Catch Group Ref by Relative Number
\k(<|')NAME(>|'), \(g|k){NAME}, (?P=NAME)  # Catch Group Ref by Name
\META_CHAR  # Literal Character; \ , \# for Literial Space and # in Extended Mode
[]  # Character Set; -: Range; [:NAME:]: Character Class; [:^NAME:]: Reverse Character Class: alnum, digit, punct, alpha, graph, space, blank, lower, upper, cntrl, print, xdigit, ascii, cntrl, word
[^]  # Reverse Character Set; -: Range; [:NAME:]: Character Class; [:^NAME:]: Reverse Character Class: alnum, digit, punct, alpha, graph, space, blank, lower, upper, cntrl, print, xdigit, ascii, cntrl, word

()  # Catch Group, Subpattern; Count by Left Parenthese
(?[P](<|')NAME(>|'))  # Named Catch Group
(?OPTION*:)  # Consume Group; No Catch
(?>)  # Atomic Consume Group: No Backtrack Re-Evaluate; No Catch
(?|)  # Consume Group Resetting Group Numbers; No Catch
(?=)  # Positive Look Ahead
(?!)  # Negative Look Ahead
(?<=)  # Positive Look Behind
(?<!)  # Negetive Look Behind
(?(DEFINE))  # Define Subpattern ONLY; No Match Test, No Consume, No Catch
(?[(?C[NUM])](CONDITION)YES_PATTERN[|NO_PATTERN])  # Conditional Pattern
# Condition:
#   Whether Subpattern Is Already Matched: [-|+]NUM, <NAME>, 'NAME'
#   Whether Current In (Specified) Pattern Recursion: R[NUM|&NAME]
#   Zero-Width Position Assertion: ?=, ?!, ?<=, ?<!

(?R)  # Whole Pattern Ref Recursion; Differs from (?(R)), Which is a Conditional Pattern
(?[-|+]NUM), \g(<|')[-|+]NUM(>|')  # Subpattern Ref Recursion by [Relative] Number
(?&NAME), (?P>NAME), \g(<|')NAME(>|')  # Subpattern Ref Recursion by Name

(?(R))  # Overall Recursion Condition
(?(RNUM))  # Group by Number Recursion Condition
(?(R&NAME))  # Group by Name Recursion Condition

(*ACTION)  # Immediately Act: ACCEPT, F[AIL], [MARK]:NAME: Set Returning Name; Backtrack Act After Forced Matched Failure: COMMIT: No Advance of Starting Point; PRUNE[:NAME]: [Set Returning Name and) Advance to Next Starting Character; SKIP: Advance to Current Matching Position; SKIP:NAME: Advance to (*MARK:NAME); THEN[:NAME]: [Set Returning Name and) Backtrack to Next Alternation
(?C[NUM])  # Callout With First Argument as 0/NUM
(?[-]OPTION+)  # [Un]Set Option; i: caseless; J: Allow Duplicate Names; m: Multiline; s: Single Line; U: Default to Lazy; x: Ignore White Space
(*OPTION[=ARG])  # Set Option; LIMIT_MATCH=COUNT; LIMIT_RECURSION=COUNT; NO_AUTO_POSSESS; NO_START_OPT: No Start-Match Optimization; UTF[8|16|32]; UCP: Unicode Properties; [ANY][CR][LF]; BSR_(ANYCRLF|UNICODE): \R Matching

(?#)  # Comment
 , # COMMENT  # Comment in Extended Mode

*[+|?]  # Previous Char [0, + \infty) Times; Default to Greedy, + for Possessive, ? for Lazy
+[+|?]  # Previous Char [1, + \infty) Times; Default to Greedy, + for Possessive, ? for Lazy
?[+|?]  # Previous Char [0, 1] Times; Default to Greedy, + for Possessive, ? for Lazy
{[NUM][,[NUM]]}[+|?]  # Previous Char [NUM, NUM] Times; Default to Greedy, + for Possessive, ? for Lazy

|  # Or
```

## 4. Glob: Unix Style Shell/Path Pattern

```man
*  # Any String (Except /)
**  # Any String Including /
?  # Any Character (Except /)
[]  # Character Set; START-END for Range
[!]  # Negative Character Set; START-END for Range
\META_CHAR  # Literal Character
```
