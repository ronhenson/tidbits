---
title: "BASH extracting substring"
created: 20150124203600
tags: [ bash, example ]
---

links
: [[placeholder]]

# BASH extracting substring

## ${BASH_REMATCH\[1\]}

```bash
$ [[ "US/Central - 10:26 PM (CST)" =~ -[[:space:]]*([0-9]{2}:[0-9]{2}) ]] &&  echo ${BASH_REMATCH[1]}
```
