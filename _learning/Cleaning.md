---
title: "Data Cleaning"
excerpt: "Glorified data plumbers"
---
>>> Garbage In, Garbage Out

Data scientists spend a lot of their time cleaning data. Unfortunately, cleaning data takes a long time and is seen as the least exciting task data scientists do. Check out this [Forbes article](https://www.forbes.com/sites/gilpress/2016/03/23/data-preparation-most-time-consuming-least-enjoyable-data-science-task-survey-says/#5f35b4066f63) to read more about it.



# Ways Data Can Be Dirty

## Dates
Dates are some of trickiest parts of data. There are all sorts of formats that you have to deal with. Some look like:
- Jan 18, 2020
- January 18, 20
- 1/18/20
- 18/1/2020
- Many many more...


## Manual Entry
A lot of data is entered by humans and you can get misspellings, similar words, and blanks. A question could be something like, "How did you feel after watching the movie?" People could answer "Happy", "Hapy", "Content", "Joyful" could all be representative of "Happy".

## Units of measure
You could have things like temperature unit differences.


## Strings
Capitals and lowercase sometimes make a difference as "Salt Lake City" would need special programming to be matched with "salt lake city" and maybe even harder, "Salt lake city".


## Others
- Missing data
- Leading zeros
- Missing data
- Duplicates
- Inconsistant
- Wrong data type



# Ways to Clean Data


## Open Refine
A free software from Google to help clean data sets. Mostly deals with string/text data with typos and similarities. Very similar to JMP's recode platform. Find and fix inconsistencies.

Check out this video to understand what OpenRefine can do with you:
<iframe width="560" height="315" src="https://www.youtube.com/embed/B70J_H_zAWM" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

If you want to learn every more, look at the [Open Refine](https://openrefine.org/) website.


## Wrangler
Wrangler is from Stanford's data visualization team. It's an interactive interface that intuitively understands what we are doing. It can take dirty data and make it clean.

Watch a video demonstrating the power:
<iframe src="https://player.vimeo.com/video/19185801" width="640" height="480" frameborder="0" allow="autoplay; fullscreen" allowfullscreen></iframe>
<p><a href="https://vimeo.com/19185801">Wrangler Demo Video</a> from <a href="https://vimeo.com/stanfordvis">Stanford Visualization Group</a> on <a href="https://vimeo.com">Vimeo</a>.</p>

If you want to learn more about Wrangler, their [intro page](http://vis.stanford.edu/wrangler/) has more information.
