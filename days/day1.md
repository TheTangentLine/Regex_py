# Day 1 - Regex Basics

Regex (regular expression) is a **pattern** used to find text inside other text.

_Estimated time: 25 minutes_

---

#### TOC

- [What is regex](#what-is-regex)
- [Raw strings](#raw-strings)
- [Searching with re.search](#searching-with-research)
- [Character classes](#character-classes)
- [Shorthand classes](#shorthand-classes)
- [Escaping special characters](#escaping-special-characters)
- [Quantifiers](#quantifiers)
- [Practice](#practice)

---

## What is regex

Regex is:

- A small "search language" that describes **what text looks like**, not the exact text.
- In other words, instead of asking "is `0412 345 678` here?", we ask "is **any phone number** here?".

You'll use regex to **find** things (emails, dates,...), **check** things (is this a valid student ID?) and **replace** things (hide every phone number).

All of Python's regex tools live in the built-in **`re`** module.

## Raw strings

- A **raw string** has an `r` in front: `r"hello"`.
- In a raw string, a backslash `\` is just a normal character. Regex uses `\` a lot, so **always write patterns as raw strings**.

_Example:_

```python
print(len("\n"))   # -> 1
print(len(r"\n"))  # -> 2
```

In the above example, Python turns `"\n"` into **one** "new line" character. The raw string `r"\n"` keeps both characters, `\` and `n`, which is what regex wants to see.

## Searching with re.search

- **`re.search(pattern, text)`** looks for the pattern **anywhere** in the text.
- If found, it returns a **match object**. Use **`.group()`** to get the matched text.
- If not found, it returns **`None`**.

_Example:_

```python
import re

print(re.search(r"cat", "I have a cat").group())  # -> cat
print(re.search(r"dog", "I have a cat"))          # -> None
```

In the above example, `cat` is found, `dog` is not.

> [!WARNING]
> Calling `.group()` on `None` crashes. If a match might not exist, check with `if` first. Also, regex is **case-sensitive**: `r"Cat"` won't find `"cat"`.

## Character classes

| Pattern | Means | Example match |
|---|---|---|
| `.` | any one character (except new line) | `c.t` → `cat`, `c9t` |
| `[ae]` | one of `a` or `e` | `gr[ae]y` → `grey`, `gray` |
| `[0-9]` `[a-z]` `[A-Z]` | one character in the range | `[A-Z]` → `L` |
| `[^0-9]` | one character **NOT** in the list | `[^0-9]` → `a` |

_Example:_

```python
import re

print(re.search(r"gr[ae]y", "The sky is grey").group())  # -> grey
print(re.search(r"[A-Z]", "room b12 Level").group())     # -> L
print(re.search(r"[^0-9]", "123abc").group())            # -> a
```

In the above example, each pattern returns the **first** character (or word) that fits.

## Shorthand classes

| Shortcut | Means | Same as |
|---|---|---|
| `\d` | digit | `[0-9]` |
| `\w` | word character (letter, digit, `_`) | `[A-Za-z0-9_]` |
| `\s` | whitespace (space, tab, new line) | |
| `\D` `\W` `\S` | the **opposite** of the lowercase version | |

_Example:_

```python
import re

print(re.search(r"\d\d\d", "Room 404 is empty").group())  # -> 404
```

## Escaping special characters

- These characters have special meaning: `. ^ $ * + ? { } [ ] \ | ( )`
- To match them **literally**, put a `\` in front.

_Example:_

```python
import re

print(re.search(r"3.5", "345").group())  # -> 345
print(re.search(r"3\.5", "345"))         # -> None
```

In the above example, `.` matches **anything** (even `4`). `\.` only matches a real dot.

## Quantifiers

A quantifier goes **right after** the thing it repeats:

| Quantifier | Means |
|---|---|
| `+` | 1 or more |
| `*` | 0 or more |
| `?` | 0 or 1 (optional) |
| `{n}` | exactly n |
| `{n,m}` | between n and m |

_Example:_

```python
import re

print(re.search(r"\d+", "I am 19 years old").group())  # -> 19
print(re.search(r"colou?r", "What color?").group())    # -> color
print(re.search(r"\d{4}", "Brisbane QLD 4000").group())  # -> 4000
print(re.search(r"n\d{8}", "ID: n12345678").group())   # -> n12345678
```

In the above example, `\d+` grabs all digits in a row, `u?` makes the `u` optional, and `n\d{8}` is a QUT-style student ID: `n` + exactly 8 digits.

> [!NOTE]
> Quantifiers are **greedy**: they take as much as possible. `<.*>` on `<b>hi</b>` matches the whole thing. Add `?` to take as little as possible: `<.*?>` matches just `<b>`.

## Practice

1. Find the first uppercase letter in `"hello World"`.
2. Find the price `$5` in `"Coffee costs $5"`.
3. Match both `"file.txt"` and `"file.text"` with one pattern.

<details>
<summary>Answer</summary>

```python
import re

print(re.search(r"[A-Z]", "hello World").group())        # -> W
print(re.search(r"\$\d+", "Coffee costs $5").group())    # -> $5
print(re.search(r"file\.te?xt", "file.text").group())    # -> file.text
```

`$` and `.` are special, so they need a `\` in front.

</details>
