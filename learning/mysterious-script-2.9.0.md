# Wow!!! The Mysterious Script 🥗
Remember the old mysterious scripts[^1][^2]? Time for a ✨new✨ one! 🤩😱
[^1]: [2.6.4 script](https://github.com/Mr-Darth/Skriptness/blob/master/learning/mysterious-script.md)
[^2]: [2.7.0-beta3 script](https://github.com/Mr-Darth/Skriptness/blob/master/learning/mysterious-script-2.7.0-beta3.md)

> [!IMPORTANT]
> This script is designed for the `2.9.0` version of [SkriptLang's Skript](https://github.com/SkriptLang/Skript).
> Running it on a version different from the recommended one is not guaranteed to produce the same result.
> This is because the script relies on some very funky and lesser-known behaviour of Skript, thus future updates 
> can easily affect it. 

## The Goal: Find out what the script outputs!
Your goal is to figure out what the script outputs(\*). Be careful, it can be tricky :) \
The code will add numbers to an `{_output::*}` variable, so you have to figure out what the variable will contain at the very end. 
The script is structured in a way that lets you easily keep track of objects added, without requiring a lot of going back and forth. \
<sup>(*) There may be a small difference between what is actually sent back and what the variable contains, so you should keep track of both for a better experience 😁😊</sup>
> [!CAUTION]
> **Absolutely NO cheating or I will know!** 😡\
> <sup>pls</sup>

So now, without further ado, behold THE SCRIPT!!!
```vbs
variables:
    {magic::%integer%} = 1200.5
    {magic::%string%} = "banana!!!"

function integer(integer: integer) returns integer:
    return {_integer}

function forgetful(object: object) returns object:
    return {_object}

function does_nothing(objects: objects) returns objects:
    return {_objects::*}

on load:
    set {_first-list::*} to "0, 3.1, 4.1, 5.9" parsed as "%numbers%"
    set {_second-list::*} to does_nothing("0, 3.1, 4.1, 5.9" parsed as "%numbers%")

    if {_first-list::*} is {_second-list::*}:
        add 1 to {_output::*}

    if first element of {_first-list::*} is first element of {_second-list::*}:
        add 2 to {_output::*}

    loop {_second-list::*}:
        set {_third-list::%loop-iteration - 1%} to loop-value

    copy {_third-list::*} into {_second-list::*}

    loop {_second-list::*}:
        set {_second-list::-%loop-index%} to loop-value

    if mod(size of {_second-list::*}, 2) is 0:
        add 3 to {_output::*}

    set {_first-number} to first element of {_second-list::*}
    set {_second-number} to {magic::%{_first-number}%}

    if {_second-number} > 0:
        add 4 to {_output::*}

    set {_infinity} to infinity
    set {_nan} to nan value
    set {_four} to 2^2

    if {magic::%{_four}%} < {_infinity}:
        add 5 to {_output::*}

    if any:
        nan value is nan value
        {_nan} is {_nan}
        {_unset-variable} is {_unset-variable}
    then:
        add 6 to {_output::*}

    add integer(7.00) to {_output::*}

    if {magic::banana} is "banana!!!":
        add 8 to {_output::*}

    if {magic::<string>} is set:
        add 9 to {_output::*}

    if {magic::%forgetful("this is a string :)")%} exists:
        add 10 to {_output::*}

    set {_blob::a::b} to 1
    set {_blob::a::c} to 2
    delete {_blob::a::b}, {_blob::a::c}

    if size of indices of {_blob::*} > 0:
        add 11 to {_output::*}

    set {magic::<itemtype>} to "red"

    set {_colourful} to coloured "<%{magic::%random itemtype of all itemtypes%}%>bob"
    set {_super-colourful} to coloured "<%last element of {_unset::*}%>"
    set {_super-duper-colourful} to "<%last element of {_unset::*}%>"

    if all:
        uncoloured {_colourful} is "bob"
        {_colourful} is not "bob"
    then:
        add 12 to {_output::*}

    if {_super-colourful} is not empty:
        add 13 to {_output::*}

    if {_super-duper-colourful} is not empty:
        add 14 to {_output::*}

    set {_block} to block at (location at 0, 0, 0 in world "world") # Assume the world exists :)
    if {_block} is within location(0.9, 0.9, 0.9, "world") and location(1, 1, 1, "world"):
        add 15 to {_output::*}

    if "3" is "3", "4":
        add 16 to {_output::*}

    wait forgetful(01:00)

    broadcast "The Answer: %{_output::*}%"
```
> A *bit* longer than the other ones, I know.

## 💡 Need a hint?
> [!NOTE]
> These hints do not cover absolutely everything.
<details>
<summary>Parsing as a list can be a rather funny experience</summary>

I will just give you an [example from the SkriptLang documentation](https://docs.skriptlang.org/expressions.html?search=#ExprParse) for this one:
<img width="536" alt="silly Njol and his very broken system, luckily a certain turd fixed it after a long bug hunt" src="https://github.com/user-attachments/assets/7c5c328e-c0b2-4ff0-b868-03729ccaaec5">
</details>
<details>
<summary>Lists and their indices, indexes, indexs, indice, whatever...</summary>

Elements in a list are sorted by the 'natural' order of their indices. \
*"But... but... They are strings!!!"* Hehehe, Skript always finds a way to amaze us! \
**(Historical parenthesis)** You may be too young to remember that silly way of sorting before we had a good expression in Skript, but here it is:
```vbs
function sort(indices: strings, values: numbers, descending: boolean = true) :: strings:
 loop {_indices::*}:
  set {_sort::%{_values::%loop-index%}%.%loop-index%} to loop-value
  return (reversed {_sort::*}) if {_descending} is true, else {_sort::*}
```
Where the sorting happening?!? Truly silly right?
</details>
<details>
<summary>The variables structure</summary>

Most people have no idea how it works, but it's useful nonetheless! Simple variables defined there get a default value if they aren't already set.
The real forte of this structure is that it allows you to specify types. So if you have defined `{blob::%integer%} = 3` and you use `{blob::%something that gives you an integer%}`, you will get 3, assuming the variable didn't have another value.
</details>
<details>
<summary>Infinity</summary>

I'll let the docs do the talking...
<img width="494" alt="wow!!!" src="https://github.com/user-attachments/assets/d1714d55-cb26-463c-8da3-4486455967be">
</details>
<details>
<summary>Cheating with types</summary>

When you give an object of the wrong type to something, Skript *may* try to help and convert it. So kind!
</details>
<details>
<summary>The empty lists</summary>

(Or, what I like to call, '*phantom*' branches 👻!)\
When you delete all sub-values of a sub-list, its index will be kept by the Mother list, even if it leads nowhere.
</details>
<details>
<summary><<none>></summary>

Colouring nothing gives you nothing. Makes sense right?
</details>
<details>
<summary>The block went somewhere else!</summary>

It actually didn't. When you check for 'location within-ness', Skript actually checks the cuboid bounded by the blocks at those corners. \
Need an example? Checking if a location is within (3.7, 3.7, 3.7) and (5.3, 5.3, 5.3) checks if the location is within (3, 3, 3) and (5, 5, 5).
<sup>(I indirectly carry some of the blame for this 😬)</sup>
</details>

## Answer
<details>
<summary>Click to reveal!</summary>

```
The Answer: 3, 6, 7, 9, 10, 11, 12, 14 and 15
```
<details>
<summary>Wow! I got it right!</summary>

Plot twist - you didn't! It outputs nothing, because that `wait` stops the code! 🤣 \
But the `{_output::*}` list is, in fact, that!
</details>

<details>
<summary>Whaaat??? 😭</summary>

Plot twist - that's not the answer! It outputs nothing, because that `wait` stops the code! 🤣 \
But the `{_output::*}` list is, in fact, that!

<details>
<summary>Ha! I knew it outputs nothing!</summary>

Ok.
</details>
</details>
</details>

## Explanation
<details>
<summary>Click to reveal!</summary>

felt lazy... coming soon... hopefully
</detailsdetails>
