---
title: "Strings in Python"
excerpt: "Hello, World!"
---


## Remove spaces at front of a word in python
>>> '     hello world!'.lstrip()
'hello world!'

see [https://stackoverflow.com/questions/959215/how-do-i-remove-leading-whitespace-in-python]

## Splitting A String 
If it is space split. 
'''python
string.split(' ') 
'''
see [https://www.geeksforgeeks.org/python-program-convert-string-list/]


## String to date time
from datetime import datetime

datetime_object = datetime.strptime('Jun 1 2005  1:33PM', '%b %d %Y %I:%M%p')

see [https://stackoverflow.com/questions/466345/converting-string-into-datetime]

for a list of all the short cuts %  see [https://docs.python.org/3/library/datetime.html#strftime-and-strptime-behavior]


If it follows a certain format, this will be easier:
from dateutil import parser
datetime_obj = parser.parse('2018-02-06T13:12:18.1278015Z')

See [https://stackoverflow.com/questions/20327937/valueerror-unconverted-data-remains-0205]


# Get the day of week from date
pd.DatetimeIndex(df_clean['Time']).weekday
