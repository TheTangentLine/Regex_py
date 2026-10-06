# Day 3 - Going Further

Last day: name your groups, change how patterns behave with flags, look **forward** and **backward**, and avoid the most common mistakes.

_Estimated time: 25 minutes_

---

#### TOC

- [Named groups](#named-groups)
- [Flags](#flags)
- [Lookahead - look forward](#lookahead---look-forward)
- [Lookbehind - look backward](#lookbehind---look-backward)
- [Combining lookaheads](#combining-lookaheads)
- [Common mistakes](#common-mistakes)
- [Practice](#practice)

---

## Named groups

- Write `(?P<name>...)` to give a group a **name**.
- Get it back with `.group("name")`.

_Example:_

```python
import re

m = re.search(r"(?P<day>\d{2})/(?P<month>\d{2})/(?P<year>\d{4})", "Due: 15/10/2026")
print(m.group("year"))  # -> 2026
print(m.groupdict())    # -> {'day': '15', 'month': '10', 'year': '2026'}
```

In the above example, `m.group("year")` is much easier to read than `m.group(3)`.

## Flags

Flags change **how** a pattern behaves. Pass them as the last argument:

| Flag | Short | Does |
|---|---|---|
| `re.IGNORECASE` | `re.I` | uppercase = lowercase |
| `re.MULTILINE` | `re.M` | `^` and `$` work on **each line**, not just the whole text |

_Example:_

```python
import re

print(re.findall(r"python", "Python, PYTHON, python", re.I))  # -> ['Python', 'PYTHON', 'python']

text = "ERROR disk full\nINFO ok\nERROR no memory"
print(re.findall(r"^ERROR.*", text))        # -> ['ERROR disk full']
print(re.findall(r"^ERROR.*", text, re.M))  # -> ['ERROR disk full', 'ERROR no memory']
```

In the above example, `text` has 3 lines. Without `re.M`, `^` only matches at the start of line 1. Combine flags with `|`: `re.I | re.M`.

## Lookahead - look forward

- `X(?=Y)`: match X only if it is **followed by** Y.
- `X(?!Y)`: match X only if it is **NOT followed by** Y.
- Y is checked, but **not** part of the result.

_Example:_

```python
import re

print(re.findall(r"\w+(?=@)", "alice@qut.edu.au, bob@gmail.com"))  # -> ['alice', 'bob']
print(re.findall(r"\b\d+\b(?!%)", "50% off, only 20 left"))        # -> ['20']
```

In the above example, we get the words right before `@` (without the `@`), and the numbers **not** followed by `%`.

## Lookbehind - look backward

- `(?<=Y)X`: match X only if it is **preceded by** Y.
- `(?<!Y)X`: match X only if it is **NOT preceded by** Y.

_Example:_

```python
import re

text = "Coffee $5, Cake $12, 3 cookies"
print(re.findall(r"(?<=\$)\d+", text))     # -> ['5', '12']
print(re.findall(r"(?<!\$)\b\d+", text))   # -> ['3']
```

In the above example, regex looks **backward** for a `$` before each number. The `$` itself is left out of the result.

> [!NOTE]
> Easy way to remember: `<` points **backward**. `=` means "must be there", `!` means "must NOT be there". In Python, a lookbehind must be a **fixed length**: `(?<=\$)` works, `(?<=\$+)` doesn't.

## Combining lookaheads

- Lookaheads don't move forward, so you can **stack** them. Each one is a rule that must be true.

_Example:_

```python
import re

pattern = r"(?=.*[A-Z])(?=.*\d).{8,}"
print(bool(re.fullmatch(pattern, "Secret123")))  # -> True
print(bool(re.fullmatch(pattern, "secret123")))  # -> False
```

In the above example, the password must have an uppercase letter `(?=.*[A-Z])`, a digit `(?=.*\d)`, and at least 8 characters `.{8,}`.

## Common mistakes

```python
import re

# -------- Bad practice ---------
print(re.search("\bcat\b", "the cat sat"))             # -> None
print(bool(re.search(r"qut.edu.au", "qutXeduXau")))    # -> True
print(re.findall(r'".*"', 'say "hi" and "bye"'))       # -> ['"hi" and "bye"']
print(bool(re.search(r"\d{4}", "abc12345")))           # -> True
```

```python
import re

# -------- Good practice ---------
print(re.search(r"\bcat\b", "the cat sat").group())    # -> cat
print(bool(re.search(r"qut\.edu\.au", "qutXeduXau")))  # -> False
print(re.findall(r'".*?"', 'say "hi" and "bye"'))      # -> ['"hi"', '"bye"']
print(bool(re.fullmatch(r"\d{4}", "abc12345")))        # -> False
```

In the above example, the bad versions:

1. **Forget the `r`**, so Python turns `\b` into a "backspace" character before regex sees it.
2. **Don't escape `.`**, so it matches any character.
3. **Use greedy `.*`**, which runs to the last `"`.
4. **Use `search` to validate**, which accepts extra junk around the match.

> [!TIP]
> When a pattern doesn't work, paste it into [regex101.com](https://regex101.com) (choose the **Python** flavour). It explains every part of the pattern.

## Practice

1. From `"Alice Nguyen"`, use named groups to get `first` and `last`.
2. Get the numbers before `kg` in `"5kg apples, 2kg pears, 3 boxes"`.
3. Get the amount after `AUD ` in `"Price: AUD 49"`.

<details>
<summary>Answer</summary>

```python
import re

m = re.search(r"(?P<first>\w+) (?P<last>\w+)", "Alice Nguyen")
print(m.group("last"))  # -> Nguyen
print(re.findall(r"\d+(?=kg)", "5kg apples, 2kg pears, 3 boxes"))  # -> ['5', '2']
print(re.findall(r"(?<=AUD )\d+", "Price: AUD 49"))                # -> ['49']
```

</details>
