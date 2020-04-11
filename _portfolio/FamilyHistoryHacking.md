---
title: "Family History Hacking"
excerpt: "Rethinking the family tree - just when you thought genealogy couldn't get more exciting."
header:
  image: /Images/Family History Hacking/NameCloudTreeSmith.png
  teaser: /Images/Family History Hacking/NameCloudTreeSmith.png
gallery:
  - url: /Images/Family History Hacking/WordCloudBirthLat.PNG
    image_path: /Images/Family History Hacking/WordCloudBirthLat.PNG
    alt: "Family Word Cloud colored by Latitude"
  - url: /Images/Family History Hacking/WordCloudBirthLat.PNG
    image_path: /Images/Family History Hacking/WordCloudYear.PNG
    alt: "Family Word Cloud colored by Year Born"
  - url: /Images/Family History Hacking/WordCloudBirthLat.PNG
    image_path: /Images/Family History Hacking/WordCloudRandoColo.PNG
    alt: "Family Word Cloud colored by Random Colors"
---

Genealogy often gets a boring rap, but knowing where you come from and who made you can be fun and exciting. Family history is big in my family, and in my culture so I thought, "This is a lot of data. What else can you do with it? How else can you display it?"

Genealogy is mainly just information. The most important informations are: 1) Name 2) Relation to you (and to other relatives) 3) Years Lived. Some other things you might be interested in are: what did they look like, where did they live, when were they buried or baptized or married (big life events). You might want to know where there grave is. You might want any journalism that talks about them. Basically, genealogy is anything that will tell you about your ancestors.

The fundamental map for understanding genealogy is the family tree. This is a tree that ties you, to your parents, and to their parents, and so forth until you can't go back any more.This is what a family tree usually looks like:

{% include figure image_path="/Images/Family History Hacking/Family Tree Landscape.png" alt="this is a placeholder image" caption="Typical family tree (taken from FamilySearch.org)" %}

There are other versions that look more like a fan, such as this:
{% include figure image_path="/Images/Family History Hacking/Family Tree Fan Chart.png" alt="this is a placeholder image" caption="Typical family tree fan chart (taken from FamilySearch.org)" %}


[FamilySearch.org](http://familysearch.org) is the website I use for family history. It is well organized and dependable. It allows you to search records such as church books or government census to find traces of your ancestors and document them. It also serves as a storage location for all your memories of ancestors, and your long lost 4th cousin's memories of their ancestors. It is open-sourced and people can add written stories, audio tapes, pictures, and memorabilia with their ancestors being remembered.

With that being said, I set off to see if I could scrape FamilySerach.org and pull together my family tree and do somethings with it.

After some Google-Fu and experimentation, I stubbled upon https://github.com/Linekio/getmyancestors. This Python package allows you to sign into FamilySearch and use their API to pull data. It exports everything as a GEDCOM file. If your'e scratching your head and wondering, 'What's a GEDCOM file?', you're not alone. It's a structured text file that has things in a certain format specifically for genealogy. Here's an example of how GEDCOM files look:

There are other versions that look more like a fan, such as this:
{% include figure image_path="/Images/Family History Hacking/GedComExample.png" alt="this is a placeholder image" caption="Example of Gedcom - the typical family history file type" %}

Once I had the GEDCOM file, I Google-d around more for a GEDCOM reader. I knew software had to be out there to parse this file for me. Played with some free software and some Python libraries, but didn't get very far and decided it would just be faster to write my own in Python. Plus, at work, I've been mainly using JMP and in school, I mostly used R this semester so I thought it appropriate to spend a little time with my lost friend.

Here's what the script ended up looking like:

#### Insert Python Script


From there, I had an organized CSV where I could perform analysis! Simple and organized.

I brought the table to JMP and did a little clean up and started to graph. What I was most interested in was a word cloud of names. I thought it would be cool to see all the names that make me. I like JMP because it has some pretty easy text analysis features, including a word cloud.

Viola! I made this.
{% include figure image_path="/Images/Family History Hacking/WordCloudYear.PNG" alt="this is a placeholder image"  %}

Let's see what we can see. First thing I notice is biggest word is 'Smith', this is probably not a surprise as my last name is Smith, but that's a good check to make sure everything is working as expected. Next thing I notice is 'Wilson', that's my mom's last name so that makes sense as well. Next thing to pop is Jane, John, Marry, and George; these are common Brittish names and I have a lot of Brittish routes. I see Elizabeth, Karren, and William (more Brittish names). Then I see Averett and Larsen (more of my mom family names). Then I see Damico. This name belongs to my dad's Italian line. I see Christensen and I know that's actually Danish. You can see more evidence of my Scandinavian routes if you dig in. On the left hand side, you'll notice 'Pedersen'; that's a Danish last name. Next to it, you'll see 'Hans'; the most generic of Scandinavian names. In the bottom left, around 7 o' clock you'll see Håkansdotter (very Swedish(. On the right hand side, you'll see Pearson and Peterson. At the top you'll see Kjerstina and Vilhelm. The evidence is everywhere. I see 'Beth' my grandma's first name. I found my name (can you find it, it is highlighted in blue to help you!).

This was all great. I could see the different names that make me. Who knew I had a relative named Luigi? (I literally do, can you find that name?)

But then I thought, well what if I can add some more information to this graph. Instead of arbitrary coloring, I added a color map corresponding to year born. Trying to highlight the older and newer names.

{% include figure image_path="/Images/Family History Hacking/WordCloudYear.PNG" alt="this is a placeholder image" caption="Word Cloud colored by year born" %}


If you analyze the graph, you'll notice the more orange and yellow names are sometimes the less 'American' sounding: Andreas, Jens, Pedersen, Mathaniel, Erichdatter, Mogensen, Luigi. We're covering a lot of those Scandinavia and Italian lines there. Then you get to names like John and William and Elizabeth and George (the more Brittish). Then finally, you end with my grandparents names, my last name, my mom's last name, ect.

Well, what else could I color map it with? The last one was year history, but I focused on geographic area....so maybe I'll just focus on latitude on where they were born. Because of American history, this doesn't end up so different. We see a lot of the foreign names (higher lat (further east)) names as the older ones. But this picks out the people born in the USA and makes them blue for the most part. Curtis for example, American. Jewel, American. Frederik, Yaughn, Johan, not American.

{% include figure image_path="/Images/Family History Hacking/WordCloudBirthLat.PNG" alt="this is a placeholder image" caption="Word Cloud colored by latitude of born location" %}


These figures really inspired me. Here are my ancestors. The Damico's, The Jagger's, Hans, Sarah, Juliana, the Palmer that made me into the guy I am today. Here is the origin of (A)very Smith. I thought, "Wow, I want to remember these people more often." So I set out to make a sticker so I could put it on my laptop (remind me, we need a blog post on my laptop stickers). I ordered it, but when the sent me the digital proof for review, I thought, if a random person sees this, are they going to know what it is? Or just see a bunch of clutter? I thought there had to be a better way to show the data. I ended up finding this website: https://wordart.com/create. They allowed me to put the word cloud into a shape that was representative of the data, so instead of just a cloud, I could make...yes, a TREE!


{% include figure image_path="/Images/Family History Hacking/NameCloudTreeSmith.PNG" alt="this is a placeholder image" caption="New definition of Family Tree" %}

Now that's a way to look at your family tree! A literal, family, tree! Howe beautiful. I love the leaves and different shades of green. I love the trunk and brown of the root of the tree. It just looked great. Loved it so much, I made one for my wife's family as well and decided it was part of their Christmas present this year! I ordered the stickers and they just arrived today. Check them out!

{% include figure image_path="/Images/Family History Hacking/FamilyTreeStickers_with_computer.png" alt="this is a placeholder image" caption="Family tree stickers to give to family" %}


I didn't only look at the names, I tried a few other things as well. This included trying to make a family Gannt Chart. If you're unfamiliar with a Gannt Chart, it's basically a way of seeing how events overlap. I wanted to see what ancestors lived with who. It's hard to display because I'm not sure who all these people are but it's interesting to see a lot of them were alive during the same time. 1895 had a lot of my ancestors alive. Of course, I only have 2 parents and hence 4 grandparents, and hence 8 great grandparents and thus each generation has more people. It's interesting to see the anomalies of short lives. There are a few in there that where quite short, although a lot were relatively long.

{% include figure image_path="/Images/Family History Hacking/Family Gantt Chart.png" alt="this is a placeholder image" caption="Which family members lived during the same time?" %}


I also did a graphic on where they were born and in what year.
{% include figure image_path="/Images/Family History Hacking/FamilyMapByYear.png" alt="this is a placeholder image" caption="Where are my ancestors from?" %}

I wasn't lying! Italy, UK, Scandinavia !!!

{% include gallery caption="See some alternatives on the family word cloud." %}
