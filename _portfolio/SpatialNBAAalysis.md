---
title: "NBA Player Tracking"
excerpt: "Where do NBA players spend their time on the court?"
header:
  #image: /Images/Family History Hacking/NameCloudTreeSmith.png
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
Basketball has become my favorite sport. I grew up playing and loving soccer, and don't get me wrong, I still do. With soccer, it is hard to find a adequate competition and teammates, and even a league to play in. Hence, I've landed with basketball. I also love the community and atmosphere associated with basketball. March madness, NBA twitter, and of course, all the analytics involved in basketball just make it very entertaining for me.

Analytics in basketball is huge and it has changed the way the game is played. Probably the biggest instance of this with the 3-point revolution. Spacing, shooting efficiency, and 3-pointers have been huge focus points over the last five years. I'll talk about these in another post coming up. Today, I looked at something that I haven't seen that many people look at and that is, player movement.

If you took a player, dipped his feet in ink, and let them play a game, what would the court end up looking like?

Getting this data was not easy, and it makes sense, it could definitely be proprietary and give teams insight. Luckily for us, the NBA used to be more forthcoming with some of this data in the early days when the technology was first installed. They posted the entire 14-15 season; every game, every player, every second. Eventually the NBA wasn't so sure they wanted to give this away, so to my understanding, they took if off their site. But lucky for us, some brilliant minds foresaw this and made a copy and made it publicly available. Check out this Git with all the info on how to get the data: https://github.com/sealneaward/nba-movement-data.

As you can imagine, this data set is enormous, like after it is compressed, it's over 50 GB so I only grabbed a portion of it for the time being. It's a really cool opportunity to use all of the data but that is a little more than a side project. Maybe 'Big Side Project' would be more accurate for that. In the mean time, I'll just look at one game. For better or worse, that game ended up being a Knicks vs Heat game. If you want to check out the game, here's the ESPN recap: https://www.espn.com/nba/recap?gameId=400828117.



I started off by just trying to understand the data. This data set didn't come with any instructions really on what it was or how it was organized so it took some time to comprehend it all. Also, it is huge; 2.6 million rows huge.

We can find the team and player ID's in another sheet so I added those in. Then I had to reorganize the time to make more sense and not be seconds remaining in quarter. Then I had to figure out the x and y locations, but after all that, I was able to do some interesting things.

First, dip players feet in ink. Second, relish in NBA player movement.

Carmelo Anthony

It looks like abstract art, does it not?!? The almost random, almost patterns really speaks artistically. You'll notice the shape of the court. Not much happening in the middle x and middle y (middle court) because not much happens there. It's the furthest point away from both hoops. What's interesting about Carmelo is you see a lot of movement on the baselines. He's moving a lot in the 2-point jumper range around the baseline post areas. This makes sense, this was his game in NY.

Let's look at another superstar's. How about the unicorn himself, Kristaps Prozingis.

Kristaps Prozingis

Ah! More abstract art I can sell. This one definitely has a different shape. We see a much more streamlined center court with a lot of action going on close the basket. Kristaps is around 7'2" and hence players center which typically is close to the basket so this makes sense.

Now you might want to say, let's look at Kristaps while also looking at the Heat's center, how similar do they look? If you think about it, they were probably guarding each other and hence look pretty similar.

Battle of Centers

Pretty close to the same shape!! They probably weren't guarding each other the entire game due to substitutions and switches on defense, but we can see they definitely occupied a similar zone.

Now, I mentioned spacing earlier, what does that mean? Essentially it means you want to use as much space on the court as you can to make it harder for the defense to cover you. This would lead to players not playing right next to each other constantly. How does that look? If we take two players on the same team, how close are they? We'll continue to look at Whiteside and add his superstar teammate Dwyane Wade.

Whiteside and Wade

Not all that much overlap! Wade mostly plays outside of Whiteside's areas.

The other thing I did was look at this type of movement in 'live' time, actually watch them step through a game. Obviously, with the temporal and spatial precision of this data, it takes a long time to walk through a whole game but here's a snippet of what the start of Carmelo Anthony's game looked like.


I added a background image of a court for a little bit more context. The time in seconds is shown in the lower left hand corner. This could be a new way to watch a game!

Just for fun, I took the average of everyone's movements and plotted them. This is how they looked at the end of the game.

Kinda looks like Madusa or something. But very centered! This could mean everyone just played in the center...or it means the court was very evenly spaced.

How bout watching the game with the whole team? And the ball?


Pretty cool to see passes and how the team moves as a whole.

Finally, I made some distributions to show where players tend to be on the court in an x,y fashion.



There's a lot more I could do with this data.
- There's play by play commentary I'd love to incorporate somehow. That would allow for shots and such.
- There's a radius stat I'm not so sure what it is yet.
- There are more games I'd like to look at.
- I'd like to see movements by position a bit more.
- Wonder if there is clustering that I could do
- Wonder if I could create a team spacing stat
- Wonder if there is any predictive models I can make

But that's all for today!
