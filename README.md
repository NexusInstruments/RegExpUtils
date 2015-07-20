# RegExpUtils
A Wildstar LUA Library for python-like Regular Expressions

##Usage
```lua
local RegExp = Apollo.GetPackage("RegExpUtils").tPackage
local regex = RegExp.compile("\\w+")
for match in regex:finditer("Hello, World!") do
  print(match:group(0))
end
```

##Reference

###RegExp.compile(regex, [flags])
Creates a new Regular Expresion object with the regex and optional flags.

#####Returns: _RegExp object_
**Example:**
```lua
  local regex = RegExp.compile("\\w+")
```
###RegExp.match(regex, str, [pos], [flags])
Returns all group matches a string contains of the regular expression

#####Parameters:
| name | type | required | description |
| ---- | -----| -------- | ----------- |
| regex | string or RegExp object | Yes | Regular Expression to match |
| str | string | Yes | String to test |
| pos | number | No | Starting position in string (default: 1) |
| flags | string | No | Regular Expression flags to use |

>Note: If position is negative, the the match starts that number of characters from the end of the string

#####Returns: _RegExpMatch object_
**Example:**
```lua
local regex = RegExp.compile("([0-9]+)")
local matches = RegExp.match(regex, "TestString 123124122 12312312")
print(matches:group(1))
```
**Results:**
```
123124122
```

###RegExp.search(regex, str, [pos], [flags])
Tests if a string contains a Regular Expression match

#####Parameters:
| name | type | required | description |
| ---- | -----| -------- | ----------- |
| regex | string or RegExp object | Yes | Regular Expression to match |
| str | string | Yes | String to test |
| pos | number | No | Starting position in string (default: 1) |
| flags | string | No | Regular Expression flags to use |

>Note: If position is negative, the the match starts that number of characters from the end of the string

#####Returns: _RegExpMatch object_ or _nil_
**Example:**
```lua
local regex = RegExp.compile("[0-9]+")
local test1 = RegExp.search(regex, "TestString")
local test2 = RegExp.search(regex, "123124122")
print(tostring(test1))
print(tostring(test2))
```
**Result:**
```
[RegExpMatch object]
nil
```

###RegExp.sub(regex, repl, str, [count], [flags])
Performs pattern substitution of a string using a regular expressions. This will also perform group replacement and insertion

#####Parameters:
| name | type | required | description |
| ---- | -----| -------- | ----------- |
| regex | string or RegExp object | Yes | Regular Expression to match |
| repl | string | Yes | String replacement pattern |
| str | string | Yes | String to perform replacements |
| count | number | No | The number of replacements to make (default: all) |
| flags | string | No | Regular Expression flags to use |

>Note: If position is negative, the the match starts that number of characters from the end of the string

###RegExp.findall(regex, str, [pos], [flags])
Finds all matches of a specific Regular Expression contains in a string.

#####Parameters:
| name | type | required | description |
| ---- | -----| -------- | ----------- |
| regex | string or RegExp object | Yes | Regular Expression to match |
| str | string | Yes | String to test |
| pos | number | No | Starting position in string (default: 1) |
| flags | string | No | Regular Expression flags to use |

>Note: If position is negative, the the match starts that number of characters from the end of the string

#####Returns: _table of strings_ or _nil_

###RegExp.finditer(regex, str, [pos], [flags])
Finds the first match of a specific Regular Expression contains in a string and returns an iterator object that can be iterated through all matches

#####Parameters:
| name | type | required | description |
| ---- | -----| -------- | ----------- |
| regex | string or RegExp object | Yes | Regular Expression to match |
| str | string | Yes | String to test |
| pos | number | No | Starting position in string (default: 1) |
| flags | string | No | Regular Expression flags to use |

>Note: If position is negative, the the match starts that number of characters from the end of the string

#####Returns: _RegExpMatch object_, _RegExp object_, match or _nil_

###RegExpMatch:group(...)
Returns a match or group of matches that matched a submatch of a regular expression.

#####Returns: _strings_ or _nil_

**Example:**
```/(a)(b)(c)/``` matching "abc", then
* ```group(0,1,2,3)``` returns "abc", "a", "b", "c"
* ```group()``` is equivalent to ```group(0)```

###RegExpMatch:span(groupId)
Returns the begining and end positions of a match within a string based on the "groupId".

#####Returns: _numbers(begining,end)_ or _nil_


##Regular Expression Syntax
**or-exp:**
* _pair-exp_
* _or-exp_ "|" _pair-exp_

**pair-exp:**
* _repeat-exp_opt_
* _pair-exp_ _repeat-exp_

**repeat-exp:**
* _primary-exp_
* _repeat-exp_ _repeater_
* _repeat-exp_ _repeater_ "?"

**primary-exp:**
* "(?:" _or-exp_ ")"
* "(?P<" _identifier_ ">" _or-exp_ ")"
* "(?P=" _name_ ")"
* "(?=" _or-exp_ ")"
* "(?!" _or-exp_ ")"
* "(?<=" _or-exp_ ")"
* "(?<!" _or-exp_ ")"
* "(?(" _name_ ")" _pair-exp_ "|" _pair-exp_ ")"
* "(?(" _name_ ")" _pair-exp_ ")"
* "(" _or-exp_ ")"
* _char-class_
* _non-terminal_
* _terminal-str_

**repeater:**
* "*"
* "+"
* "?"
* "{" _number_opt_ "," _number_opt_ "}"
* "{" _number_ "}"

**char-class:**
* "[^" _user-char-class_ "]"
* "[" _user-char-class_ "]"

**user-char-class:**
* _user-char-range_
* _user-char-class_ _user-char-range_

**user-char-range:**
* _user-char_ "-" _user-char_opt_
* _user-char_

**user-char:**
* _class-escape-sequence_
* CHARACTER OTHER THAN "\, ]"

**class-escape-sequence:**
* _term-escape-sequence_
* "\b"

**terminal-str:**
* _terminal_
* _terminal-str_ _terminal_

**terminal:**
* _term-escape-sequence_
* CHARACTER OTHER THAN "^, $, \, |, [, ], {, }, (, ), *, +, ?"

**term-escape-sequence:**
* "\a"
* "\f"
* "\n"
* "\r"
* "\t"
* "\v"
* "\\"
* "\" _ascii-puncuation-char_
* "\x" _hex-number_

**non-terminal:**
* "^"
* "$"
* "."
* "\d"
* "\D"
* "\s"
* "\S"
* "\w"
* "\W"
* "\A"
* "\b"
* "\B"
* "\Z"
* "\" _number_

**name:**
* _identifier_
* _number_

**number:**
* STRING THAT MATCHES REGEX ```/[0-9]+/```

**identifier:**
* STRING THAT MATCHES REGEX ```/[A-Za-z_][A-Za-z_0-9]*/```

**ascii-puncuation-char:**
* CHAR THAT MATCHES REGEX ```/[!-~]/``` and also ```/[^A-Za-z0-9]/```

**hex-number:**
* STRING THAT MATCHES REGEX ```/[0-9A-Fa-f]{1,2}/```
