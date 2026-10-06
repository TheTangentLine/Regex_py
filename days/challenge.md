# Challenge: Student Portal Detective

The student portal has been acting up. Dig through its log and pull out the important bits.

_Estimated time: 30–40 minutes_

> [!NOTE]
> Each task says which day it uses. Do them in order: they get harder.

---

#### TOC

- [The data](#the-data)
- [Task 1 - First student ID](#task-1---first-student-id)
- [Task 2 - Validate student IDs](#task-2---validate-student-ids)
- [Task 3 - Find all emails](#task-3---find-all-emails)
- [Task 4 - Fix extra spaces](#task-4---fix-extra-spaces)
- [Task 5 - Count the errors](#task-5---count-the-errors)
- [Task 6 - Payment amounts](#task-6---payment-amounts)
- [Task 7 - QUT usernames only](#task-7---qut-usernames-only)
- [Bonus - Password checker](#bonus---password-checker)

---

## The data

Copy this to the top of your file:

```python
import re

log = """2026-10-01 09:15 INFO  n12345678 alice@qut.edu.au paid $45.50
2026-10-01 09:17 ERROR n8765432 bob@gmail.com payment failed
2026-10-01 09:20 WARN  n11223344 chen@qut.edu.au paid $120
2026-10-02 14:03 ERROR n22334455 dana@qut.edu.au timeout
2026-10-02 14:05 INFO  n33445566 eve@outlook.com paid $9"""
```

## Task 1 - First student ID

_Day 1._ Find the **first** student ID (`n` + 8 digits) in `log`.

_Expected output:_

```
n12345678
```

## Task 2 - Validate student IDs

_Day 2._ Print `True` or `False` for each ID. A valid ID is a lowercase `n` + **exactly** 8 digits, nothing else.

```python
ids = ["n12345678", "n8765432", "N11223344", "n1122334a"]
```

_Expected output:_

```
True
False
False
False
```

## Task 3 - Find all emails

_Day 2._

_Expected output:_

```
['alice@qut.edu.au', 'bob@gmail.com', 'chen@qut.edu.au', 'dana@qut.edu.au', 'eve@outlook.com']
```

## Task 4 - Fix extra spaces

_Day 2._ Replace every run of **2 or more spaces** with a single space, then print the log.

_Expected output:_

```
2026-10-01 09:15 INFO n12345678 alice@qut.edu.au paid $45.50
2026-10-01 09:17 ERROR n8765432 bob@gmail.com payment failed
2026-10-01 09:20 WARN n11223344 chen@qut.edu.au paid $120
2026-10-02 14:03 ERROR n22334455 dana@qut.edu.au timeout
2026-10-02 14:05 INFO n33445566 eve@outlook.com paid $9
```

## Task 5 - Count the errors

_Day 3._ Count how many lines have the level `ERROR`. Hint: `len(...)`.

_Expected output:_

```
2
```

## Task 6 - Payment amounts

_Day 3._ Get every amount paid, **without** the `$`.

_Expected output:_

```
['45.50', '120', '9']
```

## Task 7 - QUT usernames only

_Day 3._ Get the usernames of **QUT emails only**, without the `@qut.edu.au` part.

_Expected output:_

```
['alice', 'chen', 'dana']
```

## Bonus - Password checker

Write **one** pattern that checks a password has at least 8 characters, an uppercase letter, a digit and **no spaces**.

```python
passwords = ["Qut2026!", "qut2026!", "Qut 2026!", "Short1"]
```

_Expected output:_

```
True
False
False
False
```

---

Stuck? Check [the solution](./%5Bsolution%5D_challenge.md), but try first!
