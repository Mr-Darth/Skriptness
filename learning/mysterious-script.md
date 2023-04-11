# Welcome, welcome, to The Mysteryous Script! :ghost:
This is a little puzzling script I came up with that covers various little pitfalls Skript currently has, plus a few less known features. \
It was created and tested on version 2.6.4 of [SkriptLang's Skript](https://github.com/SkriptLang/Skript). Because it relies on some very funky and less-known behaviour of Skript, future updates can easily affect it, leading to different results.

The question is: Can you figure out what the script outputs? \
No cheating or I will know 😡

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

The variable structure can take in variables whose names contain types. Let's take `{_foo::%number%} = 1` from the provided script.
> This variable is internally stored as `{_foo::<number>} = 1`.

When the variable is now used, if the return type of *the expression* in the variable name has that type (`number`) as its *closest registered* type, it defaults to `1` when the variable is not set.
> The closest registered type here means either the type itself if available or the closest registered supertype. \
> For example, the type `cow` is not actually registered as a classinfo, so the closest type to `cow` is `livingentity`.

*Note:* Don't be fooled by the fact that these variables' names accept types! Indices will still always end up as strings.
</details>

<details>
<summary>The serial killer comma</summary>

The serial comma (also known as the Oxford comma) is not properly supported by the Skript parser, so adding it to any conjunction makes it behave like `and`. For example, this means that the `, or` in `1, 2, or 3` will be considered as `and`.
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

# Explanation
<details>
<summary>Detailed explanation</summary>

Let's start with the function `mystery()` because this is actually where the magic happens.

We begin with setting `{_a}` to be `rounded up (2^70 + 0.5)`. Rounding returns integers evidently, so `{_a}` is now an integer (doesn't matter its value). \
We then proceed to set `{_b}` to `{_a}`. \
Then we multiply `{_a}` by `({_foo::%{_a}%} ? {_bar::%{_b}%})`. Let's see... Is `{_foo::%{_a}%}` set? Well, `{_a}` is an integer, integers are numbers, and we used `%number%` in the default variable. So you might be tempted to say that `{_foo::%{_a}%}` is `1`. But you would be wrong. Skript uses the return type of *the expression* in the index. For variables, it's *always* `object`. So we default to `{_bar::%{_b}%}` which is `0` because `{_b}` is indeed an object.
> Skript also has a *type hint* system which is able to keep track of the types of local variables, but it is currently disabled.

Because we multiplied `{_a}` by `0`, `{_a}` is now `0` which is not equal to `{_b}`. The condition fails and `{_a}` becomes `7`. \
Then, we check whether `{_a} is 2, 3, 4, 5, 6, or 7`. Notice the serial comma before `or`. This makes the conjunction behave like `and`. And we can safely say that seven is not equal to all the numbers from two through seven. \
The variable `{_c}` remains unset; adding it to `{_a}` won't make a difference. \
Okay, the next line has more expressions: `set {_a} to {_foo::%{_c}%} + {_a} + foo({_c})`. Let's evaluate them one by one:
* `{_foo::%{_c}%}` - as I explained above, this will be unset;
* `{_a}` - we established it's `7`;
* `foo({_c})` - `{_c}` is unset, we don't get anything.

Thus, `{_a}` remains `7`. \
Now, a tricky one `set {_c} to {_foo::2}`. `2` is indeed a number so you would be inclined to say the default variable actually applies here. But keep in mind the `2` is not actually an expression in this case, it's part of the literal name. Because of this, the variables section is never involved, therefore `{_c}` remains unset. \
We next set the variable `{_d}` to `{_a} / {_a}`. This is evidently `1`. But that `1` is a *number*, not an integer! Division and exponentiation *always* return numbers. \
Again, we evaluate expressions one by one:
* `{_a}` - this guy is 7;
* `{_c}` - remain unset;
* `foo({_d})` - this would've returned `1` if `{_d}` was actually an integer, but it's not; we get nothing.

We are close! `{_e}` becomes `7`. \
And, finally, `broadcast {_e}, twice`. Does this broadcast two sevens? No! It's the 'times' expression which, here, returns the integers from 1 to 2. \
We get the answer:
```
7
1
2
```
And we're done!
</details>
