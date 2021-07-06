---
title: "Regex Lookahead Lookbehind Examples"
created: 2021-07-03 10:54:59
tags: #regex
keywords: regex, lookahead, look-ahead, lookbehind, look-behind
---

# Regex Look-ahead Look-behind Exampless

## look-ahead

### positive

```regexp
^(?=.*\bjava\b)(?=.*\binterview\b).*$
```
Will find  in the example below java and interview in any order on a single line

## look-behind

### positive

```regexp
(?<y)x
```
Will match and `x immediately after a `y`

```regexp
(?<!y)x
```
will match `x` if `y` is not immediately in front of the 'x'

this yb ax
not ax yb
