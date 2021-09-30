---
title: "BASH extracting substring"
created: 2015-01-24 20:36:00
tags: #shell, #example
keywords: bash, substring
---
# BASH extracting substring

## ${BASH_REMATCH\[1\]}

```bash
[["US/Central - 10:26 PM (CST)" =~ -[[:space:]]*([0-9]{2}:[0-9]{2})]] &&  echo ${BASH_REMATCH[1]}
```
