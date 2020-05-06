---
title: "Pandas Basics"
---

# Reading an excel sheet
Typical just use:
import pandas as pd

pd.read_excel("workbook_name.xlsx")

# Read specific sheet
pd.read_excel("workbook_name.xlsx", sheetname = "Sheet1)





# Combining two data frames 
 df4 = pd.DataFrame({'B': ['B2', 'B3', 'B6', 'B7'],
   ...:                     'D': ['D2', 'D3', 'D6', 'D7'],
   ...:                     'F': ['F2', 'F3', 'F6', 'F7']},
   ...:                    index=[2, 3, 6, 7])
   ...: 

In [9]: result = pd.concat([df1, df4], axis=1, sort=False)
see https://pandas.pydata.org/pandas-docs/stable/user_guide/merging.html

