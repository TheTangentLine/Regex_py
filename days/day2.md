# Day 2 - Find, Check & Replace

Today: check the **whole** text, find **every** match, replace, split, and grab **parts** of a match with groups.

_Estimated time: 25 minutes_

---

#### TOC

- [Anchors](#anchors)
- [Validate with re.fullmatch](#validate-with-refullmatch)
- [Find all with re.findall](#find-all-with-refindall)
- [Replace with re.sub](#replace-with-resub)
- [Split with re.split](#split-with-resplit)
- [Groups](#groups)
- [Alternation](#alternation)
- [Practice](#practice)

---

## Anchors

Anchors match **positions**, not characters:

| Anchor | Means |
|---|---|
| `^` | start of the text |
| `$` | end of the text |
| `\b` | edge of a word |

_Example:_

```python
import re

print(bool(re.search(r"\.py$", "main.py")))        # -> True
print(bool(re.search(r"\.py$", "main.py.txt")))    # -> False
print(bool(re.search(r"\bcat\b", "concatenate")))  # -> False
print(bool(re.search(r"\bcat\b", "the cat sat")))  # -> True
```

In the above example, `\.py$` only matches `.py` at the very end. `\bcat\b` only matches `cat` as a **whole word**, not hidden inside `concatenate`.

> [!NOTE]
> `^` has two jobs: at the start of a pattern it means "start of text", inside `[^...]` it means "NOT".

## Validate with re.fullmatch

- **`re.fullmatch`** only succeeds if the **whole text** matches.
- Use it to **validate** input (IDs, postcodes, passwords,...).

_Example:_

```python
import re

print(bool(re.search(r"\d{4}", "40001234")))     # -> True
print(bool(re.fullmatch(r"\d{4}", "40001234")))  # -> False
print(bool(re.fullmatch(r"\d{4}", "4000")))      # -> True
```

In the above example, `search` is happy to find 4 digits **somewhere** in `40001234`. `fullmatch` rejects it because the whole text isn't exactly 4 digits.

> [!IMPORTANT]
> `search` = "is it **in** there?", `fullmatch` = "is it **exactly** this?".

## Find all with re.findall

- **`re.findall`** returns **every** match as a list.
- No match gives an empty list `[]`.

_Example:_

```python
import re

print(re.findall(r"\d+", "Marks: 85, 92, 78"))  # -> ['85', '92', '78']
print(re.findall(r"\d+", "No marks yet"))       # -> []
```

## Replace with re.sub

- **`re.sub(pattern, replacement, text)`** replaces **every** match and returns the new text.

_Example:_

```python
import re

print(re.sub(r"\d", "#", "Call 0412 345 678"))     # -> Call #### ### ###
print(re.sub(r"\s+", " ", "too    many   spaces"))  # -> too many spaces
```

In the above example, every digit becomes `#`, and every run of whitespace becomes one space.

## Split with re.split

- **`re.split`** cuts the text wherever the pattern matches.
- Unlike `"...".split(",")`, it can split on **many** separators at once.

_Example:_

```python
import re

print(re.split(r"[,;]\s*", "apple, banana;cherry"))  # -> ['apple', 'banana', 'cherry']
```

In the above example, the text is cut at a `,` or `;`, plus any spaces after it.

## Groups

Round brackets `( )` create a **group**:

- `.group()` is the **whole** match, `.group(1)`, `.group(2)`,... are the parts inside each `( )`.
- A quantifier after a group repeats the **whole group**: `(ha)+` matches `hahaha`.

_Example:_

```python
import re

m = re.search(r"(\d{2})/(\d{2})/(\d{4})", "Due: 15/10/2026")
print(m.group())   # -> 15/10/2026
print(m.group(1))  # -> 15
print(m.group(3))  # -> 2026
```

In the above example, one pattern finds the whole date **and** splits it into day, month and year.

> [!IMPORTANT]
> When a pattern has groups, `findall` returns **only the groups**. Use `(?:...)` for brackets that **don't capture**.

```python
import re

print(re.findall(r"(\w+)@qut\.edu\.au", "alice@qut.edu.au, bob@qut.edu.au"))  # -> ['alice', 'bob']
print(re.findall(r"(?:ha)+", "haha hahaha"))                                    # -> ['haha', 'hahaha']
```

## Alternation

- A pipe `|` means **OR**.
- Put it inside a group to limit how far the OR reaches.

_Example:_

```python
import re

print(re.search(r"cat|dog", "I love dogs").group())                # -> dog
print(re.search(r"(Mon|Tues|Wednes)day", "See you Tuesday").group())  # -> Tuesday
```

In the above example, `(Mon|Tues|Wednes)day` means "Mon, Tues or Wednes, followed by day". Without the group, `Mon|Tues|Wednesday` would mean "Mon" OR "Tues" OR "Wednesday".

## Practice

1. Check that `"4000"` is a valid 4-digit postcode and `"40001"` is not.
2. Find every word that starts with a capital letter in `"Alice and Bob met Chen"`.
3. From `"Time: 14:35"`, get the hour and the minute separately.

<details>
<summary>Answer</summary>

```python
import re

print(bool(re.fullmatch(r"\d{4}", "4000")))   # -> True
print(bool(re.fullmatch(r"\d{4}", "40001")))  # -> False
print(re.findall(r"\b[A-Z]\w*", "Alice and Bob met Chen"))  # -> ['Alice', 'Bob', 'Chen']

m = re.search(r"(\d{2}):(\d{2})", "Time: 14:35")
print(m.group(1))  # -> 14
print(m.group(2))  # -> 35
```

</details>
