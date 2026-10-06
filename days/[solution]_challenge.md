# Solution: Student Portal Detective

> [!NOTE]
> There is more than one correct answer. If yours gives the same output, it's fine.

---

#### TOC

- [Task 1 - First student ID](#task-1---first-student-id)
- [Task 2 - Validate student IDs](#task-2---validate-student-ids)
- [Task 3 - Find all emails](#task-3---find-all-emails)
- [Task 4 - Fix extra spaces](#task-4---fix-extra-spaces)
- [Task 5 - Count the errors](#task-5---count-the-errors)
- [Task 6 - Payment amounts](#task-6---payment-amounts)
- [Task 7 - QUT usernames only](#task-7---qut-usernames-only)
- [Bonus - Password checker](#bonus---password-checker)
- [Full solution](#full-solution)

---

## Task 1 - First student ID

```python
print(re.search(r"n\d{8}", log).group())  # -> n12345678
```

`n` followed by exactly 8 digits. `search` stops at the first match.

## Task 2 - Validate student IDs

```python
print(bool(re.fullmatch(r"n\d{8}", "n12345678")))  # -> True
print(bool(re.fullmatch(r"n\d{8}", "n8765432")))   # -> False
print(bool(re.fullmatch(r"n\d{8}", "N11223344")))  # -> False
print(bool(re.fullmatch(r"n\d{8}", "n1122334a")))  # -> False
```

`fullmatch` makes sure the **whole** text is `n` + 8 digits. `N` fails because regex is case-sensitive.

## Task 3 - Find all emails

```python
print(re.findall(r"[\w.]+@[\w.]+\.\w+", log))  # -> ['alice@qut.edu.au', 'bob@gmail.com', 'chen@qut.edu.au', 'dana@qut.edu.au', 'eve@outlook.com']
```

Letters/dots, an `@`, more letters/dots, then a dot and an ending like `au` or `com`.

## Task 4 - Fix extra spaces

```python
print(re.sub(r" {2,}", " ", log))
```

` {2,}` means "a space, 2 or more times". Single spaces are left alone.

## Task 5 - Count the errors

```python
print(len(re.findall(r"^\S+ \S+ ERROR", log, re.M)))  # -> 2
```

`re.M` makes `^` match at the start of **every** line. `\S+ \S+` skips the date and the time, so `ERROR` must be the level, not just a word somewhere in the line.

## Task 6 - Payment amounts

```python
print(re.findall(r"(?<=\$)\d+(?:\.\d{2})?", log))  # -> ['45.50', '120', '9']
```

The lookbehind checks for `$` but leaves it out. `(?:\.\d{2})?` is an optional cents part, non-capturing so `findall` returns the whole amount.

## Task 7 - QUT usernames only

```python
print(re.findall(r"\w+(?=@qut\.edu\.au)", log))  # -> ['alice', 'chen', 'dana']
```

The lookahead checks for `@qut.edu.au` but leaves it out. `bob` and `eve` are skipped.

## Bonus - Password checker

```python
pattern = r"(?=.*[A-Z])(?=.*\d)\S{8,}"
print(bool(re.fullmatch(pattern, "Qut2026!")))   # -> True
print(bool(re.fullmatch(pattern, "qut2026!")))   # -> False
print(bool(re.fullmatch(pattern, "Qut 2026!")))  # -> False
print(bool(re.fullmatch(pattern, "Short1")))     # -> False
```

Two lookaheads check "has uppercase" and "has digit". `\S{8,}` means 8+ characters with **no** whitespace.

## Full solution

```python
import re

log = """2026-10-01 09:15 INFO  n12345678 alice@qut.edu.au paid $45.50
2026-10-01 09:17 ERROR n8765432 bob@gmail.com payment failed
2026-10-01 09:20 WARN  n11223344 chen@qut.edu.au paid $120
2026-10-02 14:03 ERROR n22334455 dana@qut.edu.au timeout
2026-10-02 14:05 INFO  n33445566 eve@outlook.com paid $9"""

# Task 1
print(re.search(r"n\d{8}", log).group())

# Task 2
ids = ["n12345678", "n8765432", "N11223344", "n1122334a"]
for student_id in ids:
    print(bool(re.fullmatch(r"n\d{8}", student_id)))

# Task 3
print(re.findall(r"[\w.]+@[\w.]+\.\w+", log))

# Task 4
print(re.sub(r" {2,}", " ", log))

# Task 5
print(len(re.findall(r"^\S+ \S+ ERROR", log, re.M)))

# Task 6
print(re.findall(r"(?<=\$)\d+(?:\.\d{2})?", log))

# Task 7
print(re.findall(r"\w+(?=@qut\.edu\.au)", log))

# Bonus
for p in ["Qut2026!", "qut2026!", "Qut 2026!", "Short1"]:
    print(bool(re.fullmatch(r"(?=.*[A-Z])(?=.*\d)\S{8,}", p)))
```
