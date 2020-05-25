---
title: "Pandas Basics"
excerpt: "The backbone of DS in Python"
---

## Reading an excel sheet
Typical just use:
import pandas as pd

pd.read_excel("workbook_name.xlsx")

## Read specific sheet
pd.read_excel("workbook_name.xlsx", sheetname = "Sheet1)





## Combining two data frames
 df4 = pd.DataFrame({'B': ['B2', 'B3', 'B6', 'B7'],
   ...:                     'D': ['D2', 'D3', 'D6', 'D7'],
   ...:                     'F': ['F2', 'F3', 'F6', 'F7']},
   ...:                    index=[2, 3, 6, 7])
   ...:

In [9]: result = pd.concat([df1, df4], axis=1, sort=False)
see [https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html]


## Slicing a dataframe by row value in specific column




## Readin in a json


import pandas as pd
patients_df = pd.read_json('E:/datasets/patients.json')

see [https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_json.html] or [https://stackabuse.com/reading-and-writing-json-files-in-python-with-pandas/]


## Read in a text file as a table
''' pd.read_csv('data/nodes.txt',sep="\t",header=None) '''

see [https://stackoverflow.com/questions/25013792/how-to-read-a-dataset-from-a-txt-file-in-python]


## Change entire series to a dictionary
use the map function
[https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.map.html]

# Two series to dictionary
pd.Series(df.A.values,index=df.B).to_dict()

see https://stackoverflow.com/questions/17426292/what-is-the-most-efficient-way-to-create-a-dictionary-of-two-pandas-dataframe-co

# Mask dataframe if value is in list
use .isin()
df[df['A'].isin([3, 6])]
see https://stackoverflow.com/questions/12096252/use-a-list-of-values-to-select-rows-from-a-pandas-dataframe
