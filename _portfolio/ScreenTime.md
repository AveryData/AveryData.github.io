---
title: "Screen Time Dashboard"
excerpt: "How much time do I spend on my phone?"
header:
  teaser: /Images/ScreenTimeDashboard/ScreentimeDashboardPreview.png
---

## Let's be honest...phones are addicting!

{% include figure image_path="/Images/ScreenTimeDashboard/PhoneAddictionBathroom.jpg" alt="this is a placeholder image"  caption = "photo by https://www.instagram.com/romaodintsow/" url ="/Images/ScreenTimeDashboard/PhoneAddictionBathroom.jpg" %}


Phones are an incredible, modern technology that bless us in many ways, but they're also time suckers. They give us content we don't want, they make use worry, they keep our focus. It's a two-edged sword.

I love my phone for a lot of reasons, but there are certain times I wish I spent less time on my phone.

#### I made this dashboard to keep track of how much I'm on my phone and what apps I use.

It's actually quite a manual process. Unfortunately, [Apple doesn't allow an export of the Screen Time data](https://developer.apple.com/forums/thread/103745). Hence, every week I try to go manually input my screen time data into [Google Sheets](https://www.google.com/sheets/about/). I don't always remember, and it is tedious. But I am data obsessed so I do it!

I then use the *free* [Google Data Studio](https://support.google.com/datastudio/answer/6283323?hl=en) to make this sweet dashboard.


<iframe width="800" height="600" src="https://datastudio.google.com/embed/reporting/6aa3c00e-c01f-4e27-81c8-ef63ed4b28e6/page/7xhHB" frameborder="0" style="border:0" allowfullscreen></iframe>


#### Well, I'm Embarrassed
You'll see I spend an embarrassing amount of time on my phone. In that upper-right hand line chart, you'll see I sometimes spend upwards of 40 hours on my phone; almost a full-time job! Think about that...I could have a second job apparently? Makes me sad. A lot of that is Instagram. This seems disappointing but a lot of my Instagram time is actually posting to [my data visualization account](https://www.instagram.com/verydata_365/). At least that's what I tell myself...

Similarly, you'll see a decent amount of time spent on YouTube. A good chunk of my time on YouTube is watching "how to" videos about business, social media, data science, and even fixing household items. Netflix on the other hand...yeah I'm not doing any thing productive while watching Netflix.

With that, it's hard to say "spend less time on this app" just because I don't feel like I'm wasting my time on them all the time. I do good, productive things on YouTube and Instagram...I don't want to stifle my time there. Maybe that's just an excuse and self-justification. Thoughts?

##### Important Parts
The thing I care about most are actually probably the smallest (maybe poor design), but the upper-right hand corner is what I'm usually looking at. The total hours spent on phone is pretty glaring, maybe even more so if you divide by 24 to get how many days you spent on your phone. This large, all encompassing number is a BAN (Big Ass Number). I first heard about them from Steve Wexler of *The Big Book of Dashboards*. You can read about them [here](https://www.datarevelations.com/resources/bans/). But basically, they're large, key performance indicators (KPIs) that measure the purpose of the dashboard really. They're big, they stand out, they're beautiful, they're informative; it is all you really need!

##### TikTok
Another interesting thing is to look at the Social Media trends and look at TikTok. Man that came into my life like a wrecking ball. Have you been on there yet? It is A-DDICT-ING.

##### COVID and Quarantine
It's also interesting to think how this dashboard would look if COVID and quarantine would have never have happened. Week 12 would have been when quarantine happened. There seems to be a small increase in phone usage...but not as much as I thought.


##### Google Data Studio
I don't *love* Google Data Studio. I find it a bit constricting, but at the end of the day it is free. PowerBI [is difficult to use on Macs](https://www.holistics.io/blog/how-to-use-power-bi-on-mac-devices/). I haven't truly gotten into Tableau yet (as it really costs too much; I know there's Tableau Public, but meh--takes up 2GB on my computer). I love Flourish, but I don't think it really does this type of dashboarding.


#### What else can I do with this data?
- I'm wondering if looking at cumulative time spent might be interesting; just to see the slopes. With COVID19 cases, the confirmed cases are usually shown as a time series of cumulative (hence #FlattenTheCurve).
- Maybe looking at the correlations between apps? For instance, I find if I'm watching something on Netflix one week, I'm probably spending less time on Hulu, YouTube, ect. But maybe there are also positive correlations. More time on the Camera app lead to more time in the Photos app?
- Clustering? What would happen if I tried to group my apps? What number is a appropriate and what would the groups look like?
- If I was really ambitious, could I predict my screen time for next week using ARIMA or something...?
