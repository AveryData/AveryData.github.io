---
title: "R"
excerpt: "Coding for Statistical Pirates"
---

To use R, you'll have to download R, but I prefer doing all my work in RStudio

RStudio is an IDE for R, sort of like Spyder for Python. Within RStudio, I prefer to use RMarkdown's which is pretty similiar to something like a Jupyter Notebook. Markdown text, with R code snippits that run and have results.




## Reading in a csv
```{r}
airbnb <- read.csv('airbnb_data.csv')
```

## What type of variable?
'''r
typeof(var)
'''
