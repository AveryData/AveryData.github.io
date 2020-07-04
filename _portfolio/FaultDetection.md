---
title: "Fault Detection on Big Data "
excerpt: "A Novel Algorithm for Clustering Big Data to Detect and Diagnose Faults"
header:
  #image: /Images/Family History Hacking/NameCloudTreeSmith.png
  teaser: /Images/FaultDetection/Group3Demo.jpg
---


In January 2018 - May 2018, I worked with Dr. Kody Powell on using machine learning to perform fault detection on big data in industrial systems. We created an algorithm that took traditional multivariate control approaches and made it a big more distributed and nuanced. The end paper ended up getting published in International [Federation of Automatic Control](https://www.ifac-control.org/) (IFAC). I was extremely proud to be an undergraduate, first-author. To this day, it remains on of my favorite achievements.

Here is the abstract:
> With computer technology improving exponentially, data will grow incomprehensibly in size, complexity, and noise. However, latent within the data, valuable signals are hidden that, if discovered, can offer abundant information, such as fault detection. Traditionally, principal component analysis has been used to perform fault detection in large, multivariate systems. However, these methods often struggle to find the true origin, as they are susceptible to contribution smearing. In this work, a chemical plant system was analyzed and a novel cluster and detect method for fault detection utilizing machine-learning clustering algorithms was created in aim to improve fault detection time and diagnosis. Plant data containing complex variables were simulated, clustered into groups through a unique algorithm based upon correlations, and analyzed through principal component analysis as individual groups. This approach often resulted in quicker identification and more accurate diagnosis than the traditional principal component analysis method.

{% include figure image_path="/Images/FaultDetection/PaperFrontPage.jpeg" alt="this is a placeholder image"  url ="/Images/FaultDetection/PaperFrontPage.jpeg" %}

To fully understand what we did, I would reference you to the paper. [You can read it for free on ScienceDirect.](https://www.sciencedirect.com/science/article/pii/S2405896319309073)

I also presented this research at the 2018 University of Utah Symposium on Environmental And Energy, as well as the 2018 AiChE Rocky Mountain Conference.

{% include figure image_path="/Images/FaultDetection/PowellSustainPoster.jpg" alt="this is a placeholder image"  url ="/Images/FaultDetection/PowellSustainPoster.jpg" %}


## I'll include some additional images / thoughts below

### Univariate analysis is difficult at scale
The thought is multivariate analysis is pretty standard in industry, but it is inadequate. Take this process for example. You'll see 35 sensors moving through time. At one point, one of them will have a fault and change dramatically, indicated by red x's instead of green dots. How long does it take you to find it, even when you know it is going to happen.

{% include figure image_path="/Images/FaultDetection/ShewhartChartAllSensors.gif" alt="this is a placeholder image"  url ="/Images/FaultDetection/ShewhartChartAllSensors.gif" %}

Okay, be honest. How long did it take you? I programmed the darn thing and it still takes me a while to find it.

Further more, what if we graphed all the sensors on one, normalized graph.

{% include figure image_path="/Images/FaultDetection/AllSensorsOver3Hours.jpg" alt="this is a placeholder image"  url ="/Images/FaultDetection/AllSensorsOver3Hours.jpg" %}

The paper basically uses correlations to organize the data into families. Taking it from chaos, to organization.

{% include figure image_path="/Images/FaultDetection/CorrelationsRandom.jpg" alt="this is a placeholder image"  url ="/Images/FaultDetection/CorrelationsRandom.jpg" %}

{% include figure image_path="/Images/FaultDetection/Group3Demo.jpg" alt="Organized Correlations"  url ="/Images/FaultDetection/Group3Demo.jpg" %}
