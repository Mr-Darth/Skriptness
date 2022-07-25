# Welcome, welcome, to The Mysteryous Script! :ghost:
This is a little puzzling script I came up with that covers various little pitfalls Skript currently has, plus a few less known features.

The question is: Can you figure out what the script outputs? No cheating!

```vbs
variables:
    {_foo::%number%} = 1
    {_bar::%object%} = 0

function foo(bar: integer) :: integer:
    return {_bar}

function mystery():
    set {_a} to rounded up (2^70 + 0.5)
    set {_b} to {_a}
    set {_a} to {_a} * ({_foo::%{_a}%} ? {_bar::%{_b}%})
    if {_a} is {_b}:
        set {_a} to 2
    else:
        set {_a} to 7
    if {_a} is 2, 3, 4, 5, 6, or 7:
        set {_c} to 3
    add {_c} to {_a}
    set {_a} to {_foo::%{_c}%} + {_a} + foo({_c})
    set {_c} to {_foo::2}
    set {_d} to {_a} / {_a}
    set {_e} to {_a} + {_c} + foo({_d})
    broadcast {_e}, twice

on load:
    mystery()
```

### Hints
<details>
<summary>The not-so-useless variables structure</summary>

The variable structure can take in variables whose indices contain types. Let's take `{_foo::%number%} = 1` from the provided script.
> This variable is internally stored as `{_foo::<number>} = 1`.

When the variable is now used, if the object in the index is has that type as *first* supertype and the variable is not set, it defaults to `1`.

*Note:* Don't be fooled by the fact that these variables accept types! Indices will still always end up as strings.
</details>

<details>
<summary>The serial killer comma</summary>

The serial comma (also known as the Oxford comma) is not properly supported by the Skript parser, so adding it to any conjunction makes it behave like `and`. This means that the `, or` in `1, 2, or 3` will be considered as `and`.
</details>

<details>
<summary>Types are not just stubborn illusion</summary>

Skript really likes its types, especially in functions. While Skript generally doesn't care about your feelings, you should care about its feelings. If you give it the wrong thing, it will become sad *silently*. And you don't want Skript to do that.
</details>

<details>
<summary>Times and times again</summary>

I believe most of you are familiar with the following constructs:
```vbs
loop 100 times:
loop {thing::*}:
```
But have you ever wondered why we can only use `%number% times` in loops, while we can use `{thing::*}` pretty much everywhere? \
Well, the truth is... We can actually use `%number% times` anywhere! But what does it *do*? The answer is trivial and left as an exercise to the reader.
</details>

# Answer
<details>
<summary>Click to reveal!</summary>

This mysterious script outputs:
```
7
1
2
```
Did you get it right?
<details>
<summary>Yes</summary>
You probably feel proud. If you didn't solve it in less than five minutes, you shouldn't be proud. It was very easy. You are a disappointment.

If you did actually solve it in less than five minutes, OK. You are not a total disappointment.
</details>
<details>
<summary>No</summary>
You are a disappointment.
</details>
</details>