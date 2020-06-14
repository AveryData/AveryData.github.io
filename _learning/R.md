---
title: "R"
excerpt: "Coding for Statistical Pirates"
---

To use R, you'll have to download R. I prefer to do all my R work in the IDE, [RStudio](https://rstudio.com/).

RStudio is an IDE for R, sort of like Spyder for Python. Within RStudio, I prefer to use [RMarkdown](https://rmarkdown.rstudio.com/) which is pretty similar to something like a Jupyter Notebook. Markdown text, with R code snippits that run and have results.


## RMarkdown

### Shortcuts
- [To make a new R-chunk (Ctrl+option+i)](https://rmarkdown.rstudio.com/lesson-3.html)




## Some Coding Tips + Tricks

### Reading in a csv
```{r}
airbnb <- read.csv('airbnb_data.csv')
```

### What type of variable?
'''r
typeof(var)
'''

### To masking or subsetting of rows by conditions, try filter function:
```{r}
t1_ctrl = PlantGrowth
t1_ctrl = t1_ctrl %>% filter(group == "trt1" |  group == "ctrl")

t2_ctrl = PlantGrowth
t2_ctrl = t2_ctrl %>% filter(group == "trt2" |  group == "ctrl")
```
See [this site](https://www.datanovia.com/en/lessons/subset-data-frame-rows-in-r/)

### To create a dummy variable
'''{r}
Salaries_Dataset<- Salaries_Dataset %>%
  mutate(AssocProf = ifelse(rank=="AssocProf",1,0)) %>%
  mutate(Prof = ifelse(rank=="Prof",1,0))

'''


### To find the average of a vector
'''{r}
mean(treatment2)
'''

### To combine strings in R
'''{r}
paste('Control : ' , toString(mean(control)))
'''
