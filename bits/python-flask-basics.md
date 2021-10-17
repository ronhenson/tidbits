---
title: "Python Flask Basics"
created: 2021-07-26 11:50:32
---

tags: #web, #coding
keywords: flask, python, environment, web

# create and active venv

```bsh
python -m venv .venv
source .venv/bin/activate
```

# Python Flask Basics

## code to initiate flask

```python
from flask import flask

app = Flask(__name__)

@app.route('/'):
def index():
  return("Hello!")
```

## environment variables

```bsh
export FLASK_APP=application.py
export FLASK_ENV=development
flask run
```
