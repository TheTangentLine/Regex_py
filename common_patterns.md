# Common Patterns

A cheat sheet of ready-to-use patterns.

> [!NOTE]
> These are **simple, "good enough"** patterns for everyday use, not perfect validators. For example, the email pattern accepts some odd addresses that are technically invalid.

---

#### TOC

- [Email](#email)
- [Australian mobile number](#australian-mobile-number)
- [URL](#url)
- [Date DD/MM/YYYY](#date-ddmmyyyy)
- [Time HH:MM](#time-hhmm)
- [Australian postcode](#australian-postcode)
- [QUT student ID](#qut-student-id)
- [Strong password](#strong-password)
- [IPv4 address](#ipv4-address)
- [Hex colour](#hex-colour)
- [Numbers](#numbers)
- [Hashtags and mentions](#hashtags-and-mentions)
- [Repeated words](#repeated-words)
- [Extra whitespace](#extra-whitespace)
- [Look forward - lookahead](#look-forward---lookahead)
- [Look backward - lookbehind](#look-backward---lookbehind)
- [Search from the end - last match](#search-from-the-end---last-match)

---

## Email

`[\w.+-]+@[\w-]+(?:\.[\w-]+)+`

```python
import re

text = "Contact alice@qut.edu.au or bob.smith+uni@gmail.com."
print(re.findall(r"[\w.+-]+@[\w-]+(?:\.[\w-]+)+", text))  # -> ['alice@qut.edu.au', 'bob.smith+uni@gmail.com']
```

Username (letters, digits, `.`, `+`, `-`), then `@`, then a domain with **one or more** `.something` parts.

## Australian mobile number

`(?:\+61\s?|0)4\d{2}\s?\d{3}\s?\d{3}`

```python
import re

text = "0412 345 678, +61 412 345 678, 0412345678"
print(re.findall(r"(?:\+61\s?|0)4\d{2}\s?\d{3}\s?\d{3}", text))  # -> ['0412 345 678', '+61 412 345 678', '0412345678']
```

Starts with `0` **or** `+61`, then `4`, then 8 more digits with optional spaces.

## URL

`https?://\S+`

```python
import re

text = "Visit https://www.qut.edu.au/study and http://example.com today"
print(re.findall(r"https?://\S+", text))  # -> ['https://www.qut.edu.au/study', 'http://example.com']
```

`http` or `https`, then `://`, then everything up to the next space. A full stop or comma right after the URL will be included too, so watch out.

## Date DD/MM/YYYY

`\b(?:0[1-9]|[12]\d|3[01])/(?:0[1-9]|1[0-2])/\d{4}\b`

```python
import re

text = "Due 15/10/2026, not 32/13/2026"
print(re.findall(r"\b(?:0[1-9]|[12]\d|3[01])/(?:0[1-9]|1[0-2])/\d{4}\b", text))  # -> ['15/10/2026']
```

Day is `01`–`31`, month is `01`–`12`. It doesn't know that February has no 30th, so check that in Python if it matters.

## Time HH:MM

`\b(?:[01]\d|2[0-3]):[0-5]\d\b`

```python
import re

print(re.findall(r"\b(?:[01]\d|2[0-3]):[0-5]\d\b", "At 09:30 and 23:59, not 25:00"))  # -> ['09:30', '23:59']
```

Hours `00`–`23`, minutes `00`–`59` (24-hour time).

## Australian postcode

`\b\d{4}\b`

```python
import re

print(re.findall(r"\b\d{4}\b", "Brisbane QLD 4000"))  # -> ['4000']
```

Exactly 4 digits as a whole word. It also matches years like `2026`, so use context (e.g. after `QLD`) when needed.

## QUT student ID

`\bn\d{8}\b`

```python
import re

print(bool(re.fullmatch(r"n\d{8}", "n12345678")))  # -> True
print(re.findall(r"\bn\d{8}\b", "n12345678, n1234"))  # -> ['n12345678']
```

## Strong password

`(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^\w\s])\S{8,}`

```python
import re

pattern = r"(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[^\w\s])\S{8,}"
print(bool(re.fullmatch(pattern, "Qut@2026")))  # -> True
print(bool(re.fullmatch(pattern, "qut2026")))   # -> False
```

Each lookahead is one rule: lowercase, uppercase, digit, symbol. Then 8+ characters with no spaces.

## IPv4 address

`\b(?:\d{1,3}\.){3}\d{1,3}\b`

```python
import re

print(re.findall(r"\b(?:\d{1,3}\.){3}\d{1,3}\b", "From 192.168.1.5 to 10.0.0.1"))  # -> ['192.168.1.5', '10.0.0.1']
```

Four numbers of 1–3 digits joined by dots. It also accepts `999.999.999.999`, so it's only good for **finding** IPs, not validating them.

## Hex colour

`#(?:[0-9a-fA-F]{3}){1,2}\b`

```python
import re

print(re.findall(r"#(?:[0-9a-fA-F]{3}){1,2}\b", "color: #fff; bg: #1A2b3C; bad: #12"))  # -> ['#fff', '#1A2b3C']
```

`#` followed by 3 or 6 hex characters.

## Numbers

`-?\d+(?:\.\d+)?`

```python
import re

print(re.findall(r"-?\d+(?:\.\d+)?", "Temp -3.5, rain 12mm, 0.25 chance"))  # -> ['-3.5', '12', '0.25']
```

Optional minus sign, digits, then an optional decimal part.

## Hashtags and mentions

`#\w+` and `@\w+`

```python
import re

text = "Loving #Python with @qut_uni #regex"
print(re.findall(r"#\w+", text))  # -> ['#Python', '#regex']
print(re.findall(r"@\w+", text))  # -> ['@qut_uni']
```

## Repeated words

`\b(\w+)\s+\1\b`

```python
import re

print(re.sub(r"\b(\w+)\s+\1\b", r"\1", "the the cat sat sat"))  # -> the cat sat
```

`\1` means "the same word again". Replacing the pair with `\1` keeps only one copy.

## Extra whitespace

`\s+`

```python
import re

print(re.sub(r"\s+", " ", "  too   many \n spaces  ").strip())  # -> too many spaces
```

Every run of spaces/tabs/new lines becomes one space. `.strip()` removes the spaces left at both ends.

## Look forward - lookahead

`X(?=Y)` is X **followed by** Y. `X(?!Y)` is X **not** followed by Y.

```python
import re

print(re.findall(r"\w+(?=@)", "alice@qut.edu.au, bob@gmail.com"))  # -> ['alice', 'bob']
print(re.findall(r"\b\w+\.(?!txt\b)\w+", "a.txt b.py c.csv"))     # -> ['b.py', 'c.csv']
```

First: the username before `@`. Second: every file name that is **not** a `.txt` file.

## Look backward - lookbehind

`(?<=Y)X` is X **preceded by** Y. `(?<!Y)X` is X **not** preceded by Y.

```python
import re

print(re.findall(r"(?<=\$)\d+(?:\.\d{2})?", "Coffee $4.50, Cake $12"))  # -> ['4.50', '12']
print(re.findall(r"(?<!#)\b\w+", "#python is fun"))                       # -> ['is', 'fun']
```

First: prices after `$`, without the `$`. Second: every word that isn't a hashtag.

> [!WARNING]
> In Python, a lookbehind must be a **fixed length**: `(?<=\$)` works, `(?<=\$+)` does not.

## Search from the end - last match

Regex reads left to right, but there are easy ways to get what's at the **end**:

```python
import re

print(re.findall(r"\d+", "Scores: 10, 25, 42")[-1])          # -> 42
print(re.search(r"\d+$", "Order 123 item 456").group())       # -> 456
print(re.search(r"[^/]+$", "home/user/notes.txt").group())    # -> notes.txt
print(re.search(r"\.(\w+)$", "archive.tar.gz").group(1))      # -> gz
```

- `[-1]` picks the **last** item of a list.
- `$` pins the pattern to the **end** of the text, so `\d+$` is the last number.
- `[^/]+$` is "everything after the last `/`", which is the file name.
- `\.(\w+)$` is the file extension.
