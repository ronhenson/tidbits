---
title: "Django Create Secret Key"
created: 2021-08-03 11:57:01
tags: #coding, #python
keywords: django, coding, python, security
---

# Django Create Secret Key

## One liner

```python
python manage.py shell -c 'from django.core.management import utils; print(utils.get_random_secret_key())'
```

## script

```python
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
# generate_secret.py
from django.core.management import utils
print(utils.get_random_secret_key())
```
