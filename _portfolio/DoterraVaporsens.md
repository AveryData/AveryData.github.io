---
title: "Smart Manufacturing of Essential Oils Through Artificial Intelligence"
excerpt: "Vaporsens R&D to separate essential oils"
header:
  #image: /Images/Family History Hacking/NameCloudTreeSmith.png
  teaser: /Images/DoterraVaporsens/ROCHAveryVersion.jpg

---
{% include figure image_path="/Images/DoterraVaporsens/ROCHAveryVersion.jpg" alt="this is a placeholder image"  url ="/Images/DoterraVaporsens/ROCHAveryVersion.jpg" %}

In 2017, I had pleasure of working with [DoTerra](https://www.doterra.com/US/en). They're *the* world leader in essential oils. They're science department was evaluting my employer's [Vaporsens' GCMS-like technology](vaporsens.com). Funly enough, the project was a few years old and I had just transitioned in the data science roll at Vaporsens, so not only did I do the data analysis on their testing, but I had actually done their testing in the lab months prior. And because I was so familiar with their data, and eventually their people, I became the contact point for the company and acted almost in a account executive roll as well.

During this R&D work we were able to prove the Vaporsens Pilot4.2 had great capabilities to separate and characterize different flavors of essential oils. My testing and work ended up getting published by Nicole Stevens in her *Investigating a Novel Nanofiber Sensor Device for Objective Aromatic Analysis* in the [Roseman Research Symposium](https://8eni411zmtt3hm1752a0zoxt-wpengine.netdna-ssl.com/wp-content/uploads/2019/02/Roseman-University-2019-Abstract-Book.pdf).

I also presented the work on [Research on Capital Hill](https://our.utah.edu/events/roch/), Utah's premiere undergrad research symposium. It's an opportunity to present cutting-edge and important research to Utah's legislature. It's great to talk to the local senate and house about the great work the university is doing, and in my case, how we were working with a budding, local business, DoTerra. It's also fun because high schoolers come on field trips to the capital that day and look at your poster. I loved teaching those kids. They were so fascinated by "artificial intelligence". I sold Vaporsens technolgy as teaching computers to smell, which isn't far off. They seemed to interested and many said they wanted to study chemistry or computer science.

{% include figure image_path="/Images/DoterraVaporsens/AveryPresenting.jpg" alt="this is a placeholder image"  url ="/Images/DoterraVaporsens/AveryPresenting.jpg" %}

You can check out [Vaporsens technology](https://www.vaporsens.com/nanofiber-sensor-technology), but the Pilot 4.2 captured the smell of 11 different oils on 16 different chemical sensors over time. The multivariate sensor curves then were preprocessed and placed into a PCA model. The PCA model lowered the dimensions to two and then a k-Nearest Neighbor algorithm created a classification model that separated the different flavors. From the structure of the PCA scores, we learned a lot about the different flavors.

{% include figure image_path="/Images/DoterraVaporsens/DoTERRApcaBoxCitrusFamily.jpg" alt="this is a placeholder image"  url ="/Images/DoterraVaporsens/DoTERRApcaBoxCitrusFamily.jpg" %}

From this image, we can see all the citrus flavors clustered really well together. Orange and grapefruit were really close, as well as lemon and lime. The woods were higher on the PC2 scale (at the top of the page) and include cedarwood, peppermint, and vetiver. Interestingly enough, you see oregano on the left hand side, decently low on the PC2, almost acting as a citrus. Turns out oregano has citrus roots (can't find the article right now).

It was a fun day and a great project.

Here's the group that presented at that symposium:
{% include figure image_path="/Images/DoterraVaporsens/EntireGroup.jpg" alt="this is a placeholder image"  url ="/Images/DoterraVaporsens/EntireGroup.jpg" %}

Here is a figure comparing different classification models on this data:
{% include figure image_path="/Images/DoterraVaporsens/doTerraPCAwAccuracy.jpg" alt="this is a placeholder image"  url ="/Images/DoterraVaporsens/doTerraPCAwAccuracy.jpg" %}

An earlier version of the classification map:
{% include figure image_path="/Images/DoterraVaporsens/DoTerraClassSurfaceNicewNames.jpg" alt="this is a placeholder image"  url ="/Images/DoterraVaporsens/DoTerraClassSurfaceNicewNames.jpg" %}

One more of me doing what I love best. Teaching youngins about the wonders of data science:
{% include figure image_path="/Images/DoterraVaporsens/AveryTeaching.jpg" alt="this is a placeholder image"  url ="/Images/DoterraVaporsens/AveryTeaching.jpg" %}
