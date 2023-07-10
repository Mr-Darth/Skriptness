# Welcome, welcome, to The _NEW_ Mysterious Script! :ghost:
This is an updated version of the [old mysterious script](https://github.com/Mr-Darth/Skriptness/blob/master/learning/mysterious-script.md). It covers various little pitfalls Skript has, plus a few less known features.

> **Note** \
This script was created for version `2.7.0-beta3` of [SkriptLang's Skript](https://github.com/SkriptLang/Skript). Because it relies on some very funky and less-known behaviour of Skript, future updates can easily affect it, leading to different results.

The question is: Can you figure out what the script outputs?
> **Warning** \
No cheating or I will know 😡😡😡

```vbs
function helper(arg: object = {_none}) :: timespan:
    suppress starting with expressions warnings # Booooring warning
    if {_arg} is not set:
        set {_hello} to 1 second
        set {_there} to 2 seconds
        set {_anchovies} to 3 seconds
        set {_choice} to first element of "_hello", "_there" and "_anchovies"
        return {%{_choice}%}
    else:
        if {_arg} is a direction: # Fact: Vectors are convertible to directions.
            return 1 minute
        return 1 mc second

function mystery(value: integer = 0):
    set {_duration} to helper()
    set {_very::much::mystery} to "indeed"
    set {_very::weird::skript} to "always"
    set {_very::weird} to sum(length of ({_very::much::mystery} and {_very::weird::skript}))
    set {_size} to recursive size of {_very::weird::*}
    delete {_very::weird::skript}
    if {_value} is 0:
        if ({_size} - 2) is 10^-11:
            set {_result} to join (indices of {_very::*}) by " "
            wait {_duration}
            broadcast "%{_result}%"
        else:
            broadcast helper(vector 0, 1, 2)
    set {_time} to 10:00
    if {_time} is between 07:00 and 06:00:
        broadcast "yay!!!"
    if {_time} is "bananas":
        broadcast "omg no way!"
    set {_time} to 05:00:00
    if {_time} is 5 am or 5 pm:
        broadcast 200
    loop 9 hours, 9 mc hours and 9 pm:
        if loop-value ? {_hmm...} is 0 pm - 9 pm:
            if loop-value is a timespan:
                broadcast "timespan"
            else:
                broadcast "not timespan"

on load:
    mystery()
    mystery(3405688843) # Bonus points if you know what that number means :)
```

# Hints
<details>
<summary>Vectors and directions: two sides of the same coin</summary>
Vectors are, when needed and when applicable, converted to directions, but not vice versa. It's not actually the case in the commented line. I'm so evil :smiling_imp:

</details>

<details>
<summary>Ninja variables 🥷</summary>

The local variable token (`_`) has to be part of the literal variable name, for the variable to be local.
</details>

<details>
<summary>Null keys</summary>

Think of list variables as trees. When you have stuff like `{_a::hello::bob}` and `{_a::hello}`, the latter uses a `null` key (basically, `a -> hello -> null`).
</details>

<details>
<summary>Wait!</summary>
There are two calls to the function, so be careful with the order. \
Hold on! I left the stove on! I'll be with you in *no time*!

`wait ...`

...?
</details>

<details>
<summary>Epsilon</summary>

`69` is the same as `68.99999999991`!
> ~~r/unexpectedfactorial~~
</details>

<details>
<summary>Funny time</summary>

`[###:]##:##[.####]` is valid syntax for timespans 🤯
</details>

<details>
<summary>Comparing times</summary>

Currently, quite broken. Don't get your hopes up.
</details>

<details>
<summary>Times can be subtracted!</summary>

I lied. They can't. 🤣🤣🤣 I am the funniest 🤣🤣🤣

Still, `0 pm - 9 pm` is valid, but means something else. I wonder what... 🤔
<details>
<summary>Hint in a hint?!?</summary>

Think also of how `0-9` can actually mean something else besides -9 🤣🤣🤣
</details>
</details>

# Answer
<details>
<summary>Click to reveal!</summary>

This mysterious script outputs:
```
much weird
timespan
```
Did you get it right?
<details>
<summary>Yes</summary>

You are lying! You cheated! Disqualified.
</details>
<details>
<summary>No</summary>

Because I lied 🤣

It actually outputs:
```
not timespan
```

(I know, you are laughing so hard right now. My humour is so humorous 🤣)
</details>
</details>

# Explanation
<details>
<summary>Detailed explanation</summary>

Let's look into the `helper` function first.
> Totally unrelated, did you notice the argument defaults to a non-literal? 😱

As stated in the hints, the local variable token has to be part of the literal variable name for the variable to actually be local:
```vb
{_var}     # Local
{%"_var"%} # Cursed global
```
So, looks like the line `return {%{_choice}%}` doesn't give us anything.

Now into the juicy part... the `mystery` function! \
We found out that `set {_duration} to helper()` is a LIE!!! The variable will, in fact, not be set.

The next interesting, but trivial line is: `set {_very::weird} to sum(length of ({_very::much::mystery} and {_very::weird::skript}))`. The variable is going to be `12`.

Now, `set {_size} to recursive size of {_very::weird::*}`. Let's inspect how the variable `{_very::*}` actually looks internally:
```
# JSON-esque representation :o
{
    "much": {
        "mystery": "indeed"
    },
    "weird": {
        null: 12.0, # Why null?!? Because this is how variables work :)
        "skript": "always"
    }
}
```
So the recursive size (because it checks the tree of the variable) is `2`.

Next, `delete {_very::weird::skript}`. Let's see what happens:
```
{
    "much": {
        "mystery": "indeed"
    },
    "weird": {
        null: 12.0
    }
}
```
> There is another closely related issue: Skript does not clear parent branches even if they start leading to nothing.

Since the first call uses `0` as the argument, we enter the first conditional block. We then see: `if ({_size} - 2) is 10^-11`. Essentially, Skript has a margin of error when comparing numbers (because of floating point craziness). This margin is `1E-10`. Since `{_size} - 2` is `0` and `0` is close enough to `10^-11`, they are considered equal.

Then we have `set {_result} to join (indices of {_very::*}) by " "`. If you look just above, you can see that the indices of the list are `"much"` and `"weird"`. When joined, we get `"much weird"`.

A tricky one... `wait {_duration}`. We know the duration is not set. And if we try to wait a null amount of time, well... Skript just stops :grimacing:
> Ah, we love memory leaks!

The first call gives us absolutely nothing! It all depends on `mystery(3405688843)`. (Bonus points if you know what that number means :sunglasses:)

We will ignore the stuff that we already went over and jump straight to `set {_time} to 10:00`. \
`10:00` is unfortunately not between 07:00 and 06:00, according to Skript. It's an [old bug](https://github.com/SkriptLang/Skript/issues/1354).

Next, `{_time}` is definitely NOT `"bananas"`.

Now, timespans have a less-known, albeit documented pattern: `[###:]##:##[.####] ([hours:]minutes:seconds[.milliseconds])` :scream: \
So, `05:00:00` is `5 hours` - definitely not `5 am or 5 pm`.

The loop then just checks if any of the looped values is `0 pm - 9 pm` and broadcast whether the matching values are a timespan or not.
> `loop-value ? {_hmm...}` is just a little trick that you can safely ignore (bad type handling) :)

The last piece of the puzzle is `0 pm - 9 pm`. This is not the difference between `0 pm` and `9 pm`, but rather a [timeperiod](https://docs.skriptlang.org/classes.html?search=#timeperiod). When comparing `time` to `timeperiod`, it checks whether the time is included in the given period (and this works, unlike `is between`).
> This is actually a viable workaround for the broken `is between` condition. But, oh, the irony... ![image](https://i.imgur.com/ijMsHm3.png)

And, there we go! We get `not timespan` :tada:
</details>
