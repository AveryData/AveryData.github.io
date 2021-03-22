---
title: "Ads To Analysis"
excerpt: "How to Get from FB, Insta, And YouTube Data to Analysis"
---

## Things you should know
- Python in general  [Kaggle Introduction](https://www.kaggle.com/learn/python)
- Python *requests* library [requests library documentation](https://requests.readthedocs.io/en/master/)
- How API request are structured (and what is a JSON object) [Thorough Explanation](https://www.smashingmagazine.com/2018/01/understanding-using-rest-api/)
- How to load and manipulated data in Python (with Pandas) [Pands Intro](https://www.kaggle.com/learn/pandas)
- API to Pandas dataframe [Good Answer on StackOverFlow](https://stackoverflow.com/questions/41100303/convert-api-to-pandas-dataframe)
- Marketing analysis:
  - Prediction - I'm assuming it is mostly time series based? Predict new sign ups? If so, [here's a good overview of time series predictions in python](https://www.analyticsvidhya.com/blog/2016/02/time-series-forecasting-codes-python/).
  - Market Segmentation - [good intro to customer segmentation](https://towardsdatascience.com/customer-segmentation-in-python-9c15acf6f945)


## Getting Data

I think Facebook is easier to start with...

I've unfortunately never ran ads on either platform yet, so I've never done this. But I'm sure I'll be doing it in a few months when I'm running ads for my next course.


## Facebook

*facebookads* is a python package that is an interface between Python and Facebook's Marketing API.

[Documentation on Python SDK (module)](https://github.com/facebook/facebook-python-business-sdk?fbclid=IwAR3A9OFVCDz_8Mvk-zvU_hju9SmSmFmtetZwEfORoIeTgxr5YktP6hOCinY)

[Facebook Ad API Documentation](https://developers.facebook.com/docs/marketing-api/reference/sdks/python/v9.0)


[A Tutorial on Use](https://towardsdatascience.com/python-tutorial-visualize-the-reach-of-facebook-ads-faa8adba7892)


## YouTube

Google does things a bit different...they have you use their entire google api which is probably a bit harder...

[Set Up](https://developers.google.com/youtube/v3/quickstart/python)

[An Example](https://www.cdata.com/kb/tech/youtubeanalytics-python-pandas.rst). Looks like they did set up a database...don't think that is necessary unless there is TONS of data, which could be the case. If you're under 2GB of data, and Excel sheet should work fine.
