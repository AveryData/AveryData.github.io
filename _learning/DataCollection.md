---
title: "Data Collection"
excerpt: "You can't spell 'data science' without 'data/!"
---

Of course the first step to any data science project is to get the data.

You can get the data in 3 ways:
1) Download (easy)
2) API (medium)
3) Scrape (Hard)

## Just Download
See Data sets page.


## Via API
- Google Maps,
- Twitter
- Flickr
- Data.gov
- data.nasa.gov
- Facebook (only your friends)



## Via Scraping (really hard)
- ESPN
- Amazon

Scraping can be done in Python via the Beautiful Soup package. Basically, it uses a web browser to open web pages, and inspects the HTML. You can use regex inside the HTML to find data within certain fields to return the data you are interested in.

Other viable packages to scrape include:
- Selenium
- Scrapy
- JSoup

Some webpages might look different depending on the browser so many of the packages include or need a "web driver". Some also might need to interact or click buttons. 
