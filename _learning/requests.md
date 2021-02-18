---
title: "Requests"
excerpt: "HTTP Requests"
---


# The Basics of Requests
*Requests* is the library for making HTTP requests. It allows you to interact to consume data in an application.

You can follow [this article.](https://realpython.com/python-requests/)

## GET

*GET* is when you are trying to get data from a source.

Simply use *response = requests.get('https://api.github.com')*.

You can also pass parameters: *response = requests.get('https://api.github.com/search/repositories',params={'q': 'requests+language:python'})*.

You can further limit the GET by adding a parameters section where you specify the headers via dictionary.

## POST

*POST* is when you are going to send data to a source.

## Status Codes
A status code informs you of the status of a request. *200* means successful. *404 NOT FOUND* means it did not work. *204* means succesful but no content

## Content

Once you've "GOT" the request, you can say *response.content* which gives the raw output. You'll often want to convert into a string via UTF-8 which can be done via *response.text*.

If the content is JSON, it can easily be translated via *response.json()* which become a dictionary and values are accessible via the appropriate keys.

You might want to check out the headers, the column names, via *response.headers*.
