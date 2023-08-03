# Parsing Order
A quick overview on the order in which elements are tried when parsing a script.

> **Note** \
> This only includes elements for which there exist clear ordering rules when attempting them.
> Structures and sections are not covered.

## Statements
1. Function calls
2. Effect sections
3. Other effects & conditions \
   These do not follow any specific order (only the order they are registered in, but that's not entirely reliable).
   > See also [PR #5809](https://github.com/SkriptLang/Skript/pull/5809).

## Expressions
1. Variables
2. Function calls
3. Strings
4. 'Normal' expressions
   1. `SIMPLE` expressions
   2. `COMBINED` expressions
   3. `PROPERTY` expressions
   4. `PATTERN_MATCHES_EVERYTHING` expressions
   > There is another expression type called `NORMAL`, that sits between `SIMPLE` and `COMBINED`. It was marked as
   > deprecated by Njol, since he forgot 'what this was used for'.
   > <details>
   >
   > Some digging shows it was basically used as `COMBINED` is used today, while the old `COMBINED` expressions took
   > in slightly more complicated expressions. (Not always the case, however. Still unsure about this.)
   > </details>
5. ClassInfo literals
   > ClassInfo order can be changed relative to other registered types.

> **Warning** \
> Depending on the parse context and the flags of the parser, some of the above expressions will not be tried at all.
> The order remains, however, the same.

# Reordering
It might be the rare case a certain element should be tried before/after another and the rules above cannot guarantee this. \
One way is to access Skript's collections of syntax elements and do some simple rearrangement

A good example is [skript-reflect's `ParseOrderWorkarounds`](https://github.com/TPGamesNL/skript-reflect/blob/2.x/src/main/java/com/btk5h/skriptmirror/ParseOrderWorkarounds.java).