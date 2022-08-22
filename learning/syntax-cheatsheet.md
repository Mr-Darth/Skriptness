# Skript Syntax Cheatsheet
A brief explanation of Skript's syntax format :)

## Pattern Elements
| Element | Description |
| ------- | ----------- |
| `stuff` | literal text |
| `(stuff)` | group (used to bundle together multiple elements) |
| `(stuff\|stuff)` | choice |
| `[stuff]` | optional |
| `%stuff%` | expression of certain type |
| `<stuff>` | regex pattern |
| `(s:stuff)` <br> `(d:stuff)`, `(d¦stuff)` | parse tag <br> parse mark|

## Expression Modifiers
Expression (type) elements support a few special modifiers.

| Modifier | Description |
| -------- | ----------- |
| `%*stuff%` | literal |
| `%~stuff%` | non-literal |
| `%-stuff%` | nullable |
| `%stuff@d%` | forced time state <br> this takes in `-1` for past and `1` for future |
| `%stuff/stuff%` | multiple allowed types |

Combinations like `%~-stuff@-1%` also work.

For multiple allowed types, the modifiers apply to *all* classes.

### Nullability
Nullability means that, if there is no value provided for the expression (e.g. the expression was in an omitted optional element), it shouldn't try finding a default one. \
For example, if we have the pattern `bananas [%player%]` and we use `bananas`, the first expression will be the event-player (if there is one, otherwise it will error). This is because the default expression of the `player` type is the player event value. \
If a type doesn't have a default expression defined and is not marked nullable, an error will be thrown. \
The default expression must also respect constraints imposed by other modifiers.

## Parse Tags & Parse Marks
Parse(r) tags and marks are used to keep track of what pattern was used. Tags are always strings, while marks are always integer numbers. \
For parse marks, there exist two variations:
* `(d:stuff)` - the "modern", preferred one; not requiring any special characters
* `(d¦stuff)` - the traditional one, used in legacy code, requiring the "broken pipe" special character

Multiple matched parse marks get [XOR](https://en.wikipedia.org/wiki/Bitwise_operation#XOR)'ed together. Parse tags are simply added to a list.

Parse tags can also "infer" content if they are empty. \
When the next element is a literal element, the tag becomes the content of the literal element: `(:foo|:BOO)` -> tag becomes the used variation in *lowercase*. \
When the next element is a (grouped) choice element, the tag becomes whatever variation was used (again, in lowercase).