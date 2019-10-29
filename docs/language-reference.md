# Language reference

- [Language reference](#language-reference)
  - [Keywords](#keywords)
  - [Operators](#operators)

## Keywords

+ `let`
+ `let!`
+ `if`
+ `is`
+ `is!`
+ `or`
+ `and`
+ `for`
+ `in`
+ `while`
+ `fn`
+ `return`

## Operators

The following table shows displays all of the operators in Spearmint along with their precedence, and an example of their usage. Items that are higher up in the table have a higher precedence.

Precedence | Operator name | Associativity | Example
--- | --- | --- | ---
15 | Parenthesised expression | n/a | `( … )`
14 | Member access | left-to-right | `… . …`
13 | Array access | left-to-right | `… [ … ]`
12 | Function call | left-to-right | `… ( … )`
11 | Unary NOT | right-to-left | `! …`
11 | Unary negation | right-to-left | `- …`
10 | Range construction | n/a | `… .. …`<br>`… ... …`
9 | Multiplication | left-to-right | `… * …`
9 | Division | left-to-right | `… / …`
9 | Remainder | left-to-right | `… % …`
8 | Addition | left-to-right | `… + …`
8 | Subtraction | left-to-right | `… - …`
7 | Bitwise left shift | left-to-right | `… << …`
7 | Bitwise right shift | left-to-right | `… >> …`
6 | Less than | left-to-right | `… < …`
6 | Less than or equal to | left-to-right | `… <= …`
6 | Greater than | left-to-right | `… > …`
6 | Greater than or equal to | left-to-right | `… >= …`
5 | Equality | left-to-right | `… is …`
5 | Inequality | left-to-right | `… is! …`
4 | Bitwise AND | left-to-right | `… & …`
4 | Bitwise XOR | left-to-right | `… ^ …`
4 | Bitwise OR | left-to-right | `… | …`
3 | Logical AND | left-to-right | `… and …`
2 | Logical OR | left-to-right | `… or …`
1 | Assignment | left-to-right | `… = …`<br>`… += …`<br>`… -= …`<br>`… *= …`<br>`… /= …`<br>`… &= …`<br>`… ^= …`<br>`… |= …`
