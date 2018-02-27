# REGEX

## INTRODUCTION

### BASICS
- start : ^
- stop : $
- anychar : .
- number of characters {%youtube youtubeid %}
- JS REGEX methods - match, test, replace

#### what is regex
- special string describing search patterns
- in JS it is a type of object
    - using RegExp contructor
    - literal value

#### what it looks like
```javascript=
var re1 = new RegExp("abc");
var re2 = /abc/;
```

#### what are they made of
```
/pattern/modifiers;
```

###### pattern
- /abc/ : literal search of 'abc'
Brackets
- [abc] : match single characters between the brackets
- [0-9] : match any number
- [1-4] : match numbers in the range
- [^abc] : match any characters NOT between the brackets
- [^0-5] : match any number NOT between the brackets
- (x|y) : match alternatives contained in the brackets. alternatives can be any character

###### meta characters
- \w : find a word character. From a-z, A-Z, 0-9, including the _ (underscore) character.
- \d : find digit between 0-9.
- \W : find a non-word character.
- \D : find a non-digit character.


###### ancors
lets you choose where to start and where to end your match
- ^ : start of string
- $ : end of string
- \b : A word boundary
- \B : Non-word boundary

###### quantifiers
lets you control number of matched elements
- a? : zero or one of a
- a* : zero or more
- a+ : one or more
- a{n} : exactly n of a
- 

###### modifiers
aka flags
- g : Global
    - Don't stop after the first match
- m : Multiline
    - Makes ^ and $ anchors mach per line instead of whole string
- i : Case insensitive
    - captial and non-capital letters are both matched
- y : Sticky
    - If the next match is not located directly after the last, this match is discarded

- u : Unicode support
    - dotall and unicode escape sequences

##### examples
- /and/: founders and coders
- [aeiou]: founders and coders
- [0-9]: 02074792750
- [1-4]: 02074792750
- [^aeiou]: founders and coders
- [^02468]: 02074792750
    - how do we avoid matching strings
- how how would you match a london landline number?
    - Tip. starts with 0207



### ADVANCED

- Groups: 
- https://javascript.info/regexp-groups#non-capturing-groups-with

    Groups multiple tokens together to create a match. There are two types: 

    - **(subexpression) Capturing Groups**: They capture the text matched by the regex inside them and remembers tham as a numbered group that can be reused with a numbered backreference. 

        Expression: (hah)
        Text: **hah**aha haa **hah**!
        
        - The capturing groups are remembered as numbers, starting from 1. The matched substring can be recalled from the resulting array's elements `[1], ..., [n]` or from the predefined `RegExp`object's properties `$1, ..., $9`.
        - These capture goups can be recalled.
        - Capturing groups have a performance penalty. If you don't need the matched substring to be recalled, prefer non-capturing parentheses (see below).


        - Named Captured Groups - captures a matched subexpression and lets you access it by name (must not contain any punctuation characters and cannot begin with a number)
            - (?<name>subexpression)
            - (?'name'subexpression)
            - Aceess Named Capure Group By using the `${`_name_`}` replacement sequence in a [Regex.Replace](https://docs.microsoft.com/en-us/dotnet/api/system.text.regularexpressions.regex.replace) or [Match.Result](https://docs.microsoft.com/en-us/dotnet/api/system.text.regularexpressions.match.result) method call, where _name_ is the name of the captured subexpression.




- non-capturing

- **backreferences** - Backreferences match the same text as previously matched by a capturing group.

- 




## RESOURCES

**REGEX methods**
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/RegExp



https://www.regexbuddy.com/   - good explanations
https://regexr.com/ 
https://www.regexpal.com/
https://regex101.com/
http://eloquentjavascript.net/09_regexp.html # regex chapter of eloquent javascript
https://www.w3schools.com/jsref/jsref_obj_regexp.asp



## Presentation Link with Repl examples
https://docs.google.com/presentation/d/1ZQwmUWKE5sQj_z8yYSKQ4nGhLJvAxFDmy4NnSkX_S0o/edit?usp=sharing
