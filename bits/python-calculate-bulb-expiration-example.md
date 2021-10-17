---
title: "Python Calculate Bulb Expiration Example"
created: 2021-09-20 06:50:14
---

tags: #python, #example
keywords: date, python, calculate expiration date

# Python Calculate Bulb Expiration Example

Approximate date bulb needs to be replaced by adding hours to date

Filename: `/home/ronh/calculate-expriation-by-hours.py`
Following is the python script as of 2021-09-20

```python
#! /usr/bin/env python

# Created date: 2021-09-20
# Author: ronh

"""
Simple and dumb, no error checking, aborts if invalid date or hours
Calculate an expiration date by adding hours to current date
Example: Using this to calculate approximante Bulb lifetime based on expected hours
"""

from datetime import datetime
from datetime import timedelta

def input_hours():
    return int(input("Enter number of hours: "))

def input_date():
    date_format_str = "%Y/%m/%d"
    start_date = input("Enter date YYYY/MM/DD: ")
    if start_date == "":
        start_date = datetime.now().strftime(date_format_str)
    return datetime.strptime(start_date, date_format_str)


def calculate_date( start_date, num_hours):
    date_format_str = "%Y/%m/%d"
    return start_date + timedelta(hours=num_hours)

def main():
    start_date = input_date()
    print(start_date)
    num_hours = input_hours()
    final_date = calculate_date(start_date, num_hours)
    start_date_str = f"{start_date.year}/{start_date.month:02d}/{start_date.day:02d}"
    final_date_str = f"{final_date.year}/{final_date.month:02d}/{final_date.day:02d}"
    print (f"Start date: {start_date_str}   number of hours: {num_hours:4d}   final date: {final_date_str}")

if __name__ == "__main__":
    main()
```
