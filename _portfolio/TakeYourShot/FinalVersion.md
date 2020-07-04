# Take Your Shot
## Final Submission
### Jacob Brown, Avery Smith, and Kyle Salisbury
#### Video Presentation Link: https://www.youtube.com/watch?v=HDrmcKn1qhI

Members | email | uid
--------|-------|-----------
Jacob Brown | u0729080@utah.edu | u0729080
Avery Smith | averyjs@gmail.com | u0838931
Kyle Salisbury | Kcsals@gmail.com |  u0711328

### Primary Questions:

What are the natural groupings/clusters of shots on a basketball court? 

Which combinations of player and shooting location have the highest expected value (shooting pct * points)? 

Are the differences in shooting percentage statistically significant? 

How does shooting pct vary at Home vs. Away?

Given only the location and shooters for a game not in our dataset, can we predict the final score of the Jazz, the amount of points each player scored, and whether or not they won?



## Accomplished:
 - Web scraped all data from sources and created "final" csv
 
 - Obtained key data points using Regex
 
 - Cleaned data and created various dataframes
 
 - Unsupervised clustering (k-means) to divide court into 6 clusters (futher divided by 2 pointer and 3 pointer)
 
 - Calculated expected value for each player in each court position and reported them on shot charts
 
 - Calculated significance for shooting percentages by player and location
 
 - Explored expected value difference for Home VS Away games
 
 - Predicted the score of Jazz game, along with individual player totals. 





### Methods Used:
 - Web scraping
 
 - Regex
 
 - Dataframes (including masking)
 
 - Unsupervised clustering (k-means)
 
 - Loops and logic
 
 - Hypothesis Testing
 
 - Visualizations (Scatter plots, heat maps)
 
 - Predictions via pseduo-model 

## Programming and Methods:


```python
# Import All Library Packages

from bs4 import BeautifulSoup 
import requests
import urllib.request
import re
import time
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from matplotlib.colors import ListedColormap
from sklearn.cluster import KMeans, AgglomerativeClustering
from sklearn import metrics
from sklearn.metrics import silhouette_samples, silhouette_score
import math
import scipy as sc
from scipy.stats import norm

# Develop some color maps
seven_colors = ListedColormap(["#e41a1c","#984ea3","#a65628","#377eb8","#ffff33","#4daf4a","#ff7f00"])
cmap_bold = ListedColormap(['#FF0000', '#00FF00'])

# Load in the court picture
img = plt.imread('JazzCortHalf.png')
```

### Data Aquisition Process
#### (This may take quite a while to run. It also saves local htmls so it is suggested, if running the code, to start later at Exploratory Analysis section)

We scraped shot charts for the Utah Jazz from http://www.espn.com .  We used the hyperlinks on the Jazz schedule page to find all the Jazz games for the entire season.  We saved each page as a .html file so we could interact with them without having to scrape them over and over again.


```python
# Function to get soups for a given URL
def getWebsiteAsSoup(url):
    """ 
    Retrieve a website and return it as a BeautifulSoup object.   
    """
 
    req = urllib.request.Request(url)
    with urllib.request.urlopen(req) as response:
        classlist_html = response.read()
    
    class_soup = BeautifulSoup(classlist_html, 'html.parser')
    with open('class_list.html', 'w') as new_file:
        new_file.write(str(class_soup))
        
    return class_soup
```


```python
# url for the jazz schedule
schedule_url = "http://www.espn.com/nba/team/schedule/_/name/utah/utah-jazz"
schedule_soup = getWebsiteAsSoup(schedule_url)
base_url = "http://www.espn.com/nba/game?gameId="  # append url_endings to this
url_endings = []
regex = '//www.espn.com/nba/recap/_/id/(\d+)"'
for a_element in schedule_soup.find_all('a'):  # find all elements of type 'a'
    ending = re.findall(regex, str(a_element))
    if ending != []:  # many of these elements won't contain the regular expression--skip them
        url_endings.append(ending[0])

print('Number of Jazz Games:')
print(len(url_endings))  # shows how many games the Jazz have played so far
```


```python
# Function to save html for a given URL
def saveWebsiteToLocal(url, number):
    """ 
    Retrieve a website and save it locally as an html.  
    """
 
    req = urllib.request.Request(url)
    with urllib.request.urlopen(req) as response:
        classlist_html = response.read()
    
    # print(classlist_html)
    
    class_soup = BeautifulSoup(classlist_html, 'html.parser')
    with open('html/game_' + str(number) + '.html', 'w') as new_file:
        new_file.write(str(class_soup))
        
    return
```


```python
# download all the games to a local copy
i = 1
for game in url_endings:
    saveWebsiteToLocal(base_url+url_endings[i-1], i)
    i+=1
    time.sleep(10)
```

### Data Processing

The following image was screenshotted from the url http://www.espn.com/nba/game?gameId=400975701 .  Each of the dots is an element of type 'li' which can be scraped.

![title](ShotChart.png)

We used beautiful soup to identify the html element for each shot, and used regular expressions to extract the interesting data from each shot. This is what the HTML looks like. We were mostly interested in data-text, data-homeaway, data-shooter, and left and top positions. 

![title](Inspection of HTML.jpeg)


```python
# regular expressions to obtain key data
utah_regex = 'utah.png'
made_missed_regex = r'class="(\w+)"'
period_regex = r'data-period="(\d)"'
shooter_regex = r'data-shooter="(\d+)"'
blocks_regex = r'blocks'
blocks_shooter_name_regex = r"blocks (\w+ \w+)"
shooter_name_regex = r'data-text="(\w+ \w+)'
distance_regex = r' (\d+-foot)'
type_regex = r'foot ([\w ]+)[ "]'
alt_type_regex = r'e*s ([\w ]+)["\(]'
assist_regex = r'\((\w+ \w+) assists\)'
left_regex = r'left:(\d+.\d+)%'
top_regex = r'top:(\d+.\d+)%'
three_regex = r'three'
```


```python
# Obtaining key words from scraping
start = time.clock()
array = []
tot_games = 80
for i in range(1, tot_games+1):
    GameWebsite = BeautifulSoup(open("html/game_" + str(i) + ".html"), "html.parser")
    court_symbol = GameWebsite.select('.shot-chart > .team-logo')
    home_team = re.findall(utah_regex, str(court_symbol))
    if home_team:        
        AllJazzShots = GameWebsite.find_all(class_="shots home-team")[0]
        homeaway = 1
    else:
        AllJazzShots = GameWebsite.find_all(class_="shots away-team")[0]
        homeaway = 0
    for j in range(0, 300):
        Shot = AllJazzShots.find(id="shot" + str(j))
        if Shot == None:
            continue
        game = i
        shot = j
        made_missed = re.findall(made_missed_regex, str(Shot))[0]
        if made_missed == "made":
            made_missed = 1
        else:
            made_missed = 0
        period = re.findall(period_regex, str(Shot))[0]
        shooter =  re.findall(shooter_regex, str(Shot))[0]
        block = re.findall(blocks_regex, str(Shot))
        shooter_name = re.findall(shooter_name_regex, str(Shot))
        if block:
            shooter_name = re.findall(blocks_shooter_name_regex, str(Shot))[0]
        elif shooter_name == []:
            shooter_name = None
        else:
            shooter_name = shooter_name[0]
        distance = re.findall(distance_regex, str(Shot))
        if distance == []:
            distance = None
        else:
            distance = distance[0]
        shot_type = re.findall(type_regex, str(Shot))
        if shot_type == []:
            shot_type = re.findall(alt_type_regex, str(Shot))
            #if shot_type == []:
            #    shot_type = "deviant"
        shot_type = shot_type[0]
        # clears out some problems associated with greedy regex        
        start = shot_type.find("makes ") + len("makes ")
        if start >= len("makes "):
            shot_type = shot_type[start:]
        start = shot_type.find("misses ") + len("misses ")
        if start >= len("misses "):
            shot_type = shot_type[start:]
        assist = re.findall(assist_regex, str(Shot))
        if assist == []:
            assist = None
        else:
            assist = assist[0]
        left = float(re.findall(left_regex, str(Shot))[0])
        # one axis needs to be flipped depending on if it is home or away
        if (homeaway == 0):
            left = 100-left
            #print('away')
        top = float(re.findall(top_regex, str(Shot))[0])
        if homeaway:
            top = 100-top
        three = re.findall(three_regex, shot_type)
        if three == []:
            three = 0
        else:
            three = 1
        game_array = [game, shot, homeaway, made_missed, period, shooter, shooter_name, distance, shot_type, 
                     assist, left, top, three]
        array.append(game_array)
end = time.clock()
print("This took " + str(end-start) + " seconds to run")
```

    This took 44.585532 seconds to run



```python
columns = ["game", "shot", "home/away", "made/missed", "period", "shooter", "shooter_name", 
           "distance", "shot_type", "assist", "left", "top", "ThreePt"]
print('Total Number of Shots: ' + str(len(array)))  # total number of shots
print('Average Shots Per Game: ' + str(len(array)/tot_games))  # avg shots per game
```

    Total Number of Shots: 6614
    Average Shots Per Game: 82.675



```python
panda_dataframe = pd.DataFrame(array, columns=columns)
panda_dataframe.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>game</th>
      <th>shot</th>
      <th>home/away</th>
      <th>made/missed</th>
      <th>period</th>
      <th>shooter</th>
      <th>shooter_name</th>
      <th>distance</th>
      <th>shot_type</th>
      <th>assist</th>
      <th>left</th>
      <th>top</th>
      <th>ThreePt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>4257</td>
      <td>Derrick Favors</td>
      <td>6-foot</td>
      <td>jumper</td>
      <td>Joe Ingles</td>
      <td>91.333333</td>
      <td>60.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>4257</td>
      <td>Derrick Favors</td>
      <td>12-foot</td>
      <td>jumper</td>
      <td>None</td>
      <td>86.888889</td>
      <td>70.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1</td>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>3032976</td>
      <td>Rudy Gobert</td>
      <td>3-foot</td>
      <td>dunk</td>
      <td>Joe Ingles</td>
      <td>90.222222</td>
      <td>48.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1</td>
      <td>5</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>3908809</td>
      <td>Donovan Mitchell</td>
      <td>8-foot</td>
      <td>pullup jump shot</td>
      <td>None</td>
      <td>84.666667</td>
      <td>58.0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>6</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
      <td>4011</td>
      <td>Ricky Rubio</td>
      <td>18-foot</td>
      <td>pullup jump shot</td>
      <td>None</td>
      <td>74.666667</td>
      <td>62.0</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>




```python
panda_dataframe.to_csv("shots_dataframe_final.csv")
```

# Exploratory Analysis
#### Data can be read from here without having to run the top half of the notebook - (which could take a while)


```python
# data can be read from here without having to run the top half of the notebook
# (which could take a while)
ShotsPD = pd.read_csv("shots_dataframe.csv")
```


```python
# Describe Data Set
ShotsPD.describe()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>game</th>
      <th>shot</th>
      <th>home/away</th>
      <th>made/missed</th>
      <th>period</th>
      <th>shooter</th>
      <th>left</th>
      <th>top</th>
      <th>ThreePt</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>6793.000000</td>
      <td>6793.000000</td>
      <td>6793.000000</td>
      <td>6793.000000</td>
      <td>6793.000000</td>
      <td>6793.000000</td>
      <td>6.793000e+03</td>
      <td>6793.000000</td>
      <td>6793.000000</td>
      <td>6793.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>3396.000000</td>
      <td>41.636096</td>
      <td>49.889445</td>
      <td>0.496688</td>
      <td>0.462093</td>
      <td>2.474901</td>
      <td>1.736174e+06</td>
      <td>83.788835</td>
      <td>50.669071</td>
      <td>0.342264</td>
    </tr>
    <tr>
      <th>std</th>
      <td>1961.114522</td>
      <td>23.667811</td>
      <td>30.681017</td>
      <td>0.500026</td>
      <td>0.498598</td>
      <td>1.129431</td>
      <td>1.664691e+06</td>
      <td>10.165391</td>
      <td>21.980889</td>
      <td>0.474502</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>1.007000e+03</td>
      <td>48.000000</td>
      <td>2.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1698.000000</td>
      <td>21.000000</td>
      <td>23.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>4.257000e+03</td>
      <td>74.666667</td>
      <td>44.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3396.000000</td>
      <td>42.000000</td>
      <td>49.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>2.581177e+06</td>
      <td>87.777778</td>
      <td>50.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>5094.000000</td>
      <td>62.000000</td>
      <td>75.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>3.032976e+06</td>
      <td>92.444444</td>
      <td>60.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>6792.000000</td>
      <td>82.000000</td>
      <td>133.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>5.000000</td>
      <td>4.065673e+06</td>
      <td>98.888889</td>
      <td>98.000000</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
print('Number of Shots Taken by Each Player:')
print('----------------------------------------')
print(ShotsPD['shooter_name'].value_counts(), '\n')
```

    Number of Shots Taken by Each Player:
    ----------------------------------------
    Donovan Mitchell    1361
    Ricky Rubio          827
    Joe Ingles           718
    Derrick Favors       702
    Rodney Hood          552
    Rudy Gobert          442
    Alec Burks           413
    Jonas Jerebko        341
    Jae Crowder          295
    Royce O              285
    Thabo Sefolosha      240
    Joe Johnson          226
    Raul Neto            151
    Ekpe Udoh            119
    Dante Exum            87
    Tony Bradley          11
    Georges Niang         11
    Nate Wolters           6
    David Stockton         3
    Erik McCree            2
    Naz Mitrou             1
    Name: shooter_name, dtype: int64 
    



```python
ShotsPD = ShotsPD.replace("Royce O", "Royce O'Neale")

# filter out any shooter with less than 100 shots for the season
ShotsPD = ShotsPD[ShotsPD["shooter_name"]!="Dante Exum"]
ShotsPD = ShotsPD[ShotsPD["shooter_name"]!="Tony Bradley"]
ShotsPD = ShotsPD[ShotsPD["shooter_name"]!="Nate Wolters"]
ShotsPD = ShotsPD[ShotsPD["shooter_name"]!="David Stockton"]
ShotsPD = ShotsPD[ShotsPD["shooter_name"]!="Georges Niang"]
ShotsPD = ShotsPD[ShotsPD["shooter_name"]!="Erik McCree"]
ShotsPD = ShotsPD[ShotsPD["shooter_name"]!="Naz Mitrou"]
```


```python
print('Types of Shots Taken:')
print('----------------------------------------')
print(ShotsPD['shot_type'].value_counts(), '\n')
```

    Types of Shots Taken:
    ----------------------------------------
    three point jumper              2004
    two point shot                   901
    driving layup                    590
    jumper                           533
    pullup jump shot                 490
    layup                            371
    dunk                             235
    step back jumpshot               190
    layup                            185
    driving floating jump shot       141
    three point pullup jump shot     135
    dunk                             131
    tip shot                         118
    driving layup                    103
    two point shot                    90
    three pointer                     84
    hook shot                         70
    three point jumper                55
    jump bank shot                    36
    driving dunk                      34
    alley oop dunk shot               28
    alley oop layup                   26
    alley oop dunk shot               26
    jumper                            21
    three point shot                  20
    alley oop layup                   10
    finger roll layup                 10
    driving dunk                       9
    running pullup jump shot           9
    finger roll layup                  4
    pullup jump shot                   4
    shot                               3
    hook shot                          3
    jump bank shot                     1
    step back jumpshot                 1
    driving floating jump shot         1
    Name: shot_type, dtype: int64 
    


### Unsupervised clustering via kmeans to find natural clusters of the shots

We ultimately wanted to group the shots into different clusters for further analysis as groups.  We wanted to try an unsupervised clustering algorithm to give us some insight into how the computer might see the court.  We used kmeans because it was easy to implement, and because we were interested only in x and y location as our variables, so kmeans seemed like it would naturally lend itself to our analysis.

We used kmeans to cluster the basketball shots based on their X and Y locations on the court.  We chose to use 6 different clusters, because that lead to results that were the most easily idenfifiable by humans.  We were quite happy with our results. One group was right by the rim in the area called the key/paint/post. There were 5 other regions spanning the court that included both 2 pointers and 3 pointers.  These zones correlated quite nicely with what we would naturally identify as the left and right corners, wings, and the middle of the court.  Further dividing the groups into two-pointers and three-pointers gives us 11 separate clusters for further analysis.


```python
# Show the Natural Clusterings on the court with colors
X = np.zeros( (len(ShotsPD), 2) )
X[:, 0] = ShotsPD['left']
X[:, 1] = ShotsPD['top']
y_pred = KMeans(n_clusters=6, n_init=10, init='random', max_iter=300).fit_predict(X)

# Saves these locations to the dataframe
ShotsPD['LocationCluster'] = y_pred
ShotsPD.to_csv("location_dataframe_final.csv")

# Redistribute the left data to be on scale of 0-100 (to plot on court pic)
xNorm = 100*(ShotsPD['left'] - min(ShotsPD['left'])) / (max(ShotsPD['left']) - min(ShotsPD['left']))

plt.scatter(xNorm[:], X[:, 1], c=ShotsPD['LocationCluster'],  marker="o", cmap=seven_colors);
plt.imshow(img, zorder=0, extent=[0, 100, 0, 100.0])
plt.grid(False)
plt.show()
```


![png](output_32_0.png)


These clusters are interesting.  They naturally appear to match what we would identify as the key, the left and right corners, the wings, and the middle of the court. We added these groupings to our dataset

### We created shot charts for the team as a whole, and for each individual player


```python
# unsupervised clustering can give different labels
# so read in already clustered data for consistency
ShotsPD = pd.read_csv("location_dataframe.csv")

# Redistribute the left data to be on scale of 0-100
xNorm = 100*(ShotsPD['left'] - min(ShotsPD['left'])) / (max(ShotsPD['left']) - min(ShotsPD['left']))
ShotsPD['left'] = xNorm
```


```python
# Entire shots by Jazz by location, makes and misses
# greens are makes, reds are misses
cmap_bold = ListedColormap(['#FF0000', '#00FF00'])
plt.scatter(ShotsPD['left'],ShotsPD['top'],c=ShotsPD['made/missed'],cmap=cmap_bold)
plt.colorbar()
plt.imshow(img, zorder=0, extent=[0, 100, 0, 100.0])
plt.title('2017-18 Season Shot Chart for Entire Jazz Team')
plt.grid(False)
plt.show()
```


![png](output_36_0.png)


##### Individual players


```python
# shot chart for every player on the Jazz
# greens are makes, reds are misses
for shooter_name in ShotsPD['shooter_name'].unique():
    shooter_shots = ShotsPD[ShotsPD['shooter_name'] == shooter_name]
    plt.scatter(shooter_shots['left'], shooter_shots['top'], c=shooter_shots['made/missed'], cmap=cmap_bold)
    plt.title(str(shooter_name))
    plt.imshow(img, zorder=0, extent=[0, 100, 0, 100.0])
    plt.grid(False)
    plt.show()
```


![png](output_38_0.png)



![png](output_38_1.png)



![png](output_38_2.png)



![png](output_38_3.png)



![png](output_38_4.png)



![png](output_38_5.png)



![png](output_38_6.png)



![png](output_38_7.png)



![png](output_38_8.png)



![png](output_38_9.png)



![png](output_38_10.png)



![png](output_38_11.png)



![png](output_38_12.png)



![png](output_38_13.png)


#### We can use simple data frame masking and math to compute some simple statistics for individual players


```python
mitchell_shots = ShotsPD[ShotsPD["shooter_name"]=="Donovan Mitchell"]
print('Number of Shots Mitchell has shot: ' + str(len(mitchell_shots)))
mitchell_makes = mitchell_shots[mitchell_shots["made/missed"]==1]
mitchell_misses = mitchell_shots[mitchell_shots["made/missed"]==0]
print('Number of Shots Mitchell has made: ' + str(len(mitchell_makes)))
print('Number of Shots Mitchell has missed: ' + str(len(mitchell_misses)))
print('Mitchell Field Goal Percentage: ' + str(len(mitchell_makes)/len(mitchell_shots)))  # Mitchell field goal shooting pct
```

    Number of Shots Mitchell has shot: 1361
    Number of Shots Mitchell has made: 595
    Number of Shots Mitchell has missed: 766
    Mitchell Field Goal Percentage: 0.4371785451873622



```python
mitchell_threes = mitchell_shots[mitchell_shots["ThreePt"]==1]
mitchell_twos = mitchell_shots[mitchell_shots["ThreePt"]==0]
two_pt_pct = len(mitchell_twos[mitchell_twos["made/missed"]==1])/len(mitchell_twos)
three_pt_pct = len(mitchell_threes[mitchell_threes["made/missed"]==1])/len(mitchell_threes)
print("Mitchell's two point percentage is: " + str(round(two_pt_pct*100, 2)) + " %")
print("Mitchell's three point percentage is: " + str(round(three_pt_pct*100, 2)) + " %")
```

    Mitchell's two point percentage is: 49.71 %
    Mitchell's three point percentage is: 33.79 %


#### We try to get an idea of which regions have the highest expected value (for the team as a whole). We'll plot them later as well. This shows the expected values for 3 pointers and then two pointers


```python
## Team Stats -- 3 Pointers
# 3 pointers
PercMadeDif3 = []
NumShots = []
NumMade = []
ExpectedValue3 =[]
AvLeft3 = []
AvTop3 =[]
PtVal = 3
for i in range(0,6):
    Location = ShotsPD[(ShotsPD['LocationCluster']==i) & (ShotsPD['ThreePt']==1)]
    NumShots.append(len(Location['made/missed']))
    NumMade.append(len(Location[Location['made/missed']==1]))
    if NumShots[i] > 1:
        PercMade = NumMade[i] / NumShots[i]
        PercMadeDif3.append(PercMade)
    else:
        PercMadeDif3.append(0)
    
    ExpectedValue3.append(PercMadeDif3[i]*PtVal)
    
    AvLeft3.append(np.mean(Location['left']))
    AvTop3.append(np.mean(Location['top']))
    
print('---- 3 Pointers ----')
print('Percentages')
print(PercMadeDif3)
print('---------------------')
print('Expected Values')
print(ExpectedValue3)
```

    ---- 3 Pointers ----
    Percentages
    [0, 0.3986636971046771, 0.4009111617312073, 0.35555555555555557, 0.31875, 0.3536231884057971]
    ---------------------
    Expected Values
    [0, 1.1959910913140313, 1.2027334851936218, 1.0666666666666667, 0.9562499999999999, 1.0608695652173914]



```python
## Team Stats -- 2 Pointers
# 2 pointers
PercMadeDif2 = []
NumShots = []
NumMade = []
ExpectedValue2 =[]
PtVal = 2
AvLeft2 = []
AvTop2 =[]

for i in range(0,6):
    Location = ShotsPD[(ShotsPD['LocationCluster']==i) & (ShotsPD['ThreePt']==0)]
    NumShots.append(len(Location['made/missed']))
    NumMade.append(len(Location[Location['made/missed']==1]))
    PercMade = NumMade[i] / NumShots[i]
    PercMadeDif2.append(PercMade)
    ExpectedValue2.append(PercMadeDif2[i]*PtVal)
    
    AvLeft2.append(np.mean(Location['left']))
    AvTop2.append(np.mean(Location['top']))
    
print('---- 2 Pointers ----')
print('Percentages')
print(PercMadeDif2)
print('---------------------')
print('Expected Values')
print(ExpectedValue2)
```

    ---- 2 Pointers ----
    Percentages
    [0.5643629217163446, 0.43023255813953487, 0.3561643835616438, 0.4018264840182648, 0.3465909090909091, 0.44086021505376344]
    ---------------------
    Expected Values
    [1.1287258434326892, 0.8604651162790697, 0.7123287671232876, 0.8036529680365296, 0.6931818181818182, 0.8817204301075269]



```python
## Delete some parts mostly because the paint (key) cluster won't have a 3, only a 2.
xx = np.isnan(AvLeft3)
for i in range(0, len(AvLeft3)):
    if  xx[i] == True:
        DeleteVar = i
DeleteVar
del AvLeft3[DeleteVar] 
del AvTop3[DeleteVar] 
```


```python
# Show where each cluster is located on the court (the means!)
import seaborn as sns

df = pd.DataFrame({
'x': AvLeft3 + AvLeft2,
'y': AvTop3 + AvTop2,
'group': ['0','1', '2','3','4','5','6','7','8','9','10']
})
 
p1=sns.regplot(data=df, x="x", y="y", fit_reg=False, marker="o", color="skyblue", scatter_kws={'s':400})
for line in range(0,df.shape[0]):
    p1.text(df.x[line]+0.2, df.y[line], df.group[line], horizontalalignment='left', size='medium', 
            color='black', weight='semibold')
 
plt.imshow(img, zorder=0, extent=[0, 100, 0, 100.0])
plt.grid(False)
plt.show()


```


![png](output_46_0.png)



```python
## Delete the 3's for the parents of cluster 5
for i in range(0,len(ExpectedValue3)):
    if ExpectedValue3[i] == 0:
        Extra = i
del ExpectedValue3[Extra]

# Create expected values and round it for simplicity
ExpectedValue = ExpectedValue3 + ExpectedValue2
ExpectedValueRound = np.round_(ExpectedValue, decimals=2) 
```

## Analysis

Plot the expected values for the team


```python
## Team chart with expected values
ExpectedValue = ExpectedValue3 + ExpectedValue2
df = pd.DataFrame({
'x': AvLeft3 + AvLeft2,
'y': AvTop3 + AvTop2,
'group': [str(ExpectedValueRound[0]),str(ExpectedValueRound[1]),str(ExpectedValueRound[2]),
          str(ExpectedValueRound[3]),str(ExpectedValueRound[4]),str(ExpectedValueRound[5]),
          str(ExpectedValueRound[6]),str(ExpectedValueRound[7]),str(ExpectedValueRound[8]),
          str(ExpectedValueRound[9]),str(ExpectedValueRound[10])]
})
 
p1=sns.regplot(data=df, x="x", y="y", fit_reg=False, marker="o", color="skyblue", scatter_kws={'s':400})
for line in range(0,df.shape[0]):
    p1.text(df.x[line]+0.2, df.y[line], df.group[line], horizontalalignment='left', 
            size='medium', color='black', weight='semibold')
 
plt.imshow(img, zorder=0, extent=[0, 100, 0, 100.0])
plt.grid(False)
sns.plt.show()
```


![png](output_50_0.png)


Heat map for the team with color scale


```python
x = AvLeft3 + AvLeft2
y = AvTop3 + AvTop2
B = ExpectedValueRound
low = np.min(B)
high = np.max(B)
cs = plt.scatter(x,y,c=B,cmap=plt.cm.bwr,vmin=low,vmax=high)

plt.colorbar(cs)
plt.imshow(img, zorder=0, extent=[0, 100, 0, 100.0])
plt.grid(False)
plt.show()
# red is hot--high expected value.  blue is cool--low expected value
```


![png](output_52_0.png)


#### As we can see, corner threes and twos in they key are the most efficient shots for the Jazz team overall. The longer jumper two's are the worst shot the Jazz can shoot as a team. Hence, from this plot we can take it that shooting a two pointer isn't really worth it, unless it is inside the paint. 

Using masking and loops with added logic, we are able to look at all the expected values on the court for each player. We will also print out the values and player name


```python
# 3 pointers
PlayerIDs = np.unique(ShotsPD['shooter'])
PlayerNames = np.unique(ShotsPD['shooter_name'])
NumOPlayers = len(PlayerNames)
for Name in range(0,NumOPlayers):
    PercMadeDif3 = []
    NumShots = []
    NumMade = []
    ExpectedValue3 =[]
    AvLeft3 = []
    AvTop3 =[]
    PtVal3 = 3
    PtVal2 = 2
    PercMadeDif2 = []
    NumShots3 = []
    NumShots2 = []
    NumMade3 = []
    NumMade2 = []
    ExpectedValue2 =[]
    ExpectedValueRound = []
 
    AvLeft2 = []
    AvTop2 =[]
    
    for i in range(0,6):
        Location = ShotsPD[(ShotsPD['LocationCluster']==i) & (ShotsPD['ThreePt']==1) 
                           & ( ShotsPD['shooter_name'] == PlayerNames[Name])]     
        NumShots3.append(len(Location['made/missed']))
        NumMade3.append(len(Location[Location['made/missed']==1]))
        PercMade3 = 0
        if NumShots3[i] > 1:
            PercMade3 = NumMade3[i] / NumShots3[i]
            PercMadeDif3.append(PercMade3)
        else:
            PercMadeDif3.append(0)
    
        ExpectedValue3.append(PercMadeDif3[i]*PtVal3)
    
        AvLeft3.append(np.mean(Location['left']))
        AvTop3.append(np.mean(Location['top']))
        
        
        
    
        
        Location = ShotsPD[(ShotsPD['LocationCluster']==i) & (ShotsPD['ThreePt']==0) 
                           & ( ShotsPD['shooter_name'] == PlayerNames[Name])]
        NumShots2.append(len(Location['made/missed']))
        NumMade2.append(len(Location[Location['made/missed']==1]))
        PercMade2 = 0
        if NumShots2[i] > 1:
                PercMade2 = NumMade2[i] / NumShots2[i]
                PercMadeDif2.append(PercMade2)
        else:
                PercMadeDif2.append(0)
        ExpectedValue2.append(PercMadeDif2[i]*PtVal2)
    
        AvLeft2.append(np.mean(Location['left']))
        AvTop2.append(np.mean(Location['top']))
        
    print('---------------------------------------------------------------')
    print(PlayerNames[Name])
    print('---------------------')
    print('---- 3 Pointers ----')
    print('Percentages')
    print(PercMadeDif3)
    print('---------------------')
    print('Expected Values')
    print(ExpectedValue3)
    

    
    print('---- 2 Pointers ----')
    print('Percentages')
    print(PercMadeDif2)
    print('---------------------')
    print('Expected Values')
    print(ExpectedValue2)
    
    for mm in range(0,len(ExpectedValue3)):
        if ExpectedValue3[mm] == 0:
            Extra = mm
    del ExpectedValue3[Extra]
    
    xx = np.isnan(AvLeft3)
    for mm in range(0, len(AvLeft3)):
        if  xx[mm] == True:
            DeleteVar = mm
    
    del AvLeft3[DeleteVar] 
    del AvTop3[DeleteVar]
    
    ExpectedValue = ExpectedValue3 + ExpectedValue2
    ExpectedValueRound = np.round_(ExpectedValue, decimals=2) 
    
    AvLefts = AvLeft3 + AvLeft2
    AvTops = AvTop3 + AvTop2
    
    for u in range(0,len(AvLefts)):
        if math.isnan(AvLefts[u]):
            AvLefts[u]=90
        if math.isnan(AvTops[u]):
            AvTops[u]= 0
        
    
    x = np.round(AvLefts,decimals=0)
    y = np.round(AvTops,decimals=0)
    valz = [str(ExpectedValueRound[0]),str(ExpectedValueRound[1]),str(ExpectedValueRound[2]),
            str(ExpectedValueRound[3]),str(ExpectedValueRound[4]),str(ExpectedValueRound[5]),
            str(ExpectedValueRound[6]),str(ExpectedValueRound[7]),str(ExpectedValueRound[8]),
            str(ExpectedValueRound[9]),str(ExpectedValueRound[10])]
    df = pd.DataFrame({
    'x': x,
    'y': y,
    'group': valz
    })
 
    p1=sns.regplot(data=df, x="x", y="y", fit_reg=False, marker="o", color="skyblue", scatter_kws={'s':400})
    for line in range(0,df.shape[0]):
        p1.text(df.x[line]+0.2, df.y[line], df.group[line], horizontalalignment='left', 
                size='medium', color='black', weight='semibold')
 
    plt.title(PlayerNames[Name])
    plt.imshow(img, zorder=0, extent=[0, 100, 0, 100.0])
    plt.grid(False)
    plt.show()
    
```

    ---------------------------------------------------------------
    Alec Burks
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.3888888888888889, 0.4, 0.37142857142857144, 0.2558139534883721, 0.30434782608695654]
    ---------------------
    Expected Values
    [0, 1.1666666666666667, 1.2000000000000002, 1.1142857142857143, 0.7674418604651163, 0.9130434782608696]
    ---- 2 Pointers ----
    Percentages
    [0.4816753926701571, 0, 0.0, 0.47619047619047616, 0.375, 0.36]
    ---------------------
    Expected Values
    [0.9633507853403142, 0, 0.0, 0.9523809523809523, 0.75, 0.72]



![png](output_55_1.png)


    ---------------------------------------------------------------
    Derrick Favors
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.26666666666666666, 0.19230769230769232, 0.0, 0, 0.3333333333333333]
    ---------------------
    Expected Values
    [0, 0.8, 0.576923076923077, 0.0, 0, 1.0]
    ---- 2 Pointers ----
    Percentages
    [0.6680584551148225, 0.3333333333333333, 0.3333333333333333, 0.2413793103448276, 0.3793103448275862, 0.44871794871794873]
    ---------------------
    Expected Values
    [1.336116910229645, 0.6666666666666666, 0.6666666666666666, 0.4827586206896552, 0.7586206896551724, 0.8974358974358975]



![png](output_55_3.png)


    ---------------------------------------------------------------
    Donovan Mitchell
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.36764705882352944, 0.5555555555555556, 0.271523178807947, 0.3287671232876712, 0.3333333333333333]
    ---------------------
    Expected Values
    [0, 1.1029411764705883, 1.6666666666666667, 0.814569536423841, 0.9863013698630136, 1.0]
    ---- 2 Pointers ----
    Percentages
    [0.5358931552587646, 0.7, 0.42857142857142855, 0.45454545454545453, 0.32558139534883723, 0.4]
    ---------------------
    Expected Values
    [1.0717863105175292, 1.4, 0.8571428571428571, 0.9090909090909091, 0.6511627906976745, 0.8]



![png](output_55_5.png)


    ---------------------------------------------------------------
    Ekpe Udoh
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0, 0, 0, 0, 0]
    ---------------------
    Expected Values
    [0, 0, 0, 0, 0, 0]
    ---- 2 Pointers ----
    Percentages
    [0.5357142857142857, 0, 0.0, 0, 0.0, 0]
    ---------------------
    Expected Values
    [1.0714285714285714, 0, 0.0, 0, 0.0, 0]



![png](output_55_7.png)


    ---------------------------------------------------------------
    Jae Crowder
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.5833333333333334, 0.22580645161290322, 0.2702702702702703, 0.2571428571428571, 0.3333333333333333]
    ---------------------
    Expected Values
    [0, 1.75, 0.6774193548387096, 0.8108108108108109, 0.7714285714285714, 1.0]
    ---- 2 Pointers ----
    Percentages
    [0.5617977528089888, 0.5, 0.2222222222222222, 0.3125, 0.2, 0.35294117647058826]
    ---------------------
    Expected Values
    [1.1235955056179776, 1.0, 0.4444444444444444, 0.625, 0.4, 0.7058823529411765]



![png](output_55_9.png)


    ---------------------------------------------------------------
    Joe Ingles
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.45871559633027525, 0.5, 0.43434343434343436, 0.3655913978494624, 0.4642857142857143]
    ---------------------
    Expected Values
    [0, 1.3761467889908259, 1.5, 1.3030303030303032, 1.096774193548387, 1.3928571428571428]
    ---- 2 Pointers ----
    Percentages
    [0.5621890547263682, 0.5, 0.25, 0.3333333333333333, 0.3, 0.45454545454545453]
    ---------------------
    Expected Values
    [1.1243781094527363, 1.0, 0.5, 0.6666666666666666, 0.6, 0.9090909090909091]



![png](output_55_11.png)


    ---------------------------------------------------------------
    Joe Johnson
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.3076923076923077, 0.21739130434782608, 0.14285714285714285, 0.35714285714285715, 0.4]
    ---------------------
    Expected Values
    [0, 0.9230769230769231, 0.6521739130434783, 0.42857142857142855, 1.0714285714285714, 1.2000000000000002]
    ---- 2 Pointers ----
    Percentages
    [0.56, 0.5454545454545454, 0.0, 0.4, 0.43478260869565216, 0.5833333333333334]
    ---------------------
    Expected Values
    [1.12, 1.0909090909090908, 0.0, 0.8, 0.8695652173913043, 1.1666666666666667]



![png](output_55_13.png)


    ---------------------------------------------------------------
    Jonas Jerebko
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.4883720930232558, 0.4772727272727273, 0.3548387096774194, 0.3333333333333333, 0.2857142857142857]
    ---------------------
    Expected Values
    [0, 1.4651162790697674, 1.4318181818181819, 1.064516129032258, 1.0, 0.8571428571428571]
    ---- 2 Pointers ----
    Percentages
    [0.5424836601307189, 0.14285714285714285, 0.36363636363636365, 0.2, 0.75, 0.4]
    ---------------------
    Expected Values
    [1.0849673202614378, 0.2857142857142857, 0.7272727272727273, 0.4, 1.5, 0.8]



![png](output_55_15.png)


    ---------------------------------------------------------------
    Raul Neto
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.5, 0.46153846153846156, 0.375, 0.38461538461538464, 0.2]
    ---------------------
    Expected Values
    [0, 1.5, 1.3846153846153846, 1.125, 1.153846153846154, 0.6000000000000001]
    ---- 2 Pointers ----
    Percentages
    [0.5232558139534884, 0.25, 0, 0.2857142857142857, 0.14285714285714285, 1.0]
    ---------------------
    Expected Values
    [1.0465116279069768, 0.5, 0, 0.5714285714285714, 0.2857142857142857, 2.0]



![png](output_55_17.png)


    ---------------------------------------------------------------
    Ricky Rubio
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.2558139534883721, 0.40625, 0.39080459770114945, 0.42857142857142855, 0.23255813953488372]
    ---------------------
    Expected Values
    [0, 0.7674418604651163, 1.21875, 1.1724137931034484, 1.2857142857142856, 0.6976744186046512]
    ---- 2 Pointers ----
    Percentages
    [0.47346938775510206, 0.45454545454545453, 0.5, 0.40425531914893614, 0.41935483870967744, 0.4772727272727273]
    ---------------------
    Expected Values
    [0.9469387755102041, 0.9090909090909091, 1.0, 0.8085106382978723, 0.8387096774193549, 0.9545454545454546]



![png](output_55_19.png)


    ---------------------------------------------------------------
    Rodney Hood
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.4074074074074074, 0.26666666666666666, 0.44086021505376344, 0.26666666666666666, 0.423728813559322]
    ---------------------
    Expected Values
    [0, 1.222222222222222, 0.8, 1.3225806451612903, 0.8, 1.271186440677966]
    ---- 2 Pointers ----
    Percentages
    [0.5220588235294118, 0.375, 0.5, 0.4897959183673469, 0.2653061224489796, 0.4583333333333333]
    ---------------------
    Expected Values
    [1.0441176470588236, 0.75, 1.0, 0.9795918367346939, 0.5306122448979592, 0.9166666666666666]



![png](output_55_21.png)


    ---------------------------------------------------------------
    Royce O
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.34285714285714286, 0.3684210526315789, 0.4166666666666667, 0.15789473684210525, 0.4]
    ---------------------
    Expected Values
    [0, 1.0285714285714285, 1.1052631578947367, 1.25, 0.47368421052631576, 1.2000000000000002]
    ---- 2 Pointers ----
    Percentages
    [0.4689655172413793, 0, 0.0, 0.5833333333333334, 0.5, 0.6666666666666666]
    ---------------------
    Expected Values
    [0.9379310344827586, 0, 0.0, 1.1666666666666667, 1.0, 1.3333333333333333]



![png](output_55_23.png)


    ---------------------------------------------------------------
    Rudy Gobert
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0, 0, 0, 0, 0]
    ---------------------
    Expected Values
    [0, 0, 0, 0, 0, 0]
    ---- 2 Pointers ----
    Percentages
    [0.636150234741784, 0, 0, 0.25, 0, 0.3333333333333333]
    ---------------------
    Expected Values
    [1.272300469483568, 0, 0, 0.5, 0, 0.6666666666666666]



![png](output_55_25.png)


    ---------------------------------------------------------------
    Thabo Sefolosha
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.45, 0.42857142857142855, 0.35714285714285715, 0.23076923076923078, 0.3333333333333333]
    ---------------------
    Expected Values
    [0, 1.35, 1.2857142857142856, 1.0714285714285714, 0.6923076923076923, 1.0]
    ---- 2 Pointers ----
    Percentages
    [0.6120689655172413, 0.25, 0.625, 0.3684210526315789, 0.16666666666666666, 0.3333333333333333]
    ---------------------
    Expected Values
    [1.2241379310344827, 0.5, 1.25, 0.7368421052631579, 0.3333333333333333, 0.6666666666666666]



![png](output_55_27.png)


## Statistical Significance

At low sampling rates, a given shot could have a high expected value purely by chance.  For example, 25% of the time, a 50% shooter will make two shots in a row.  If those two shots are the only sample we have, we might conclude that the shooter is a 100% shooter.  For this reason, it is important to determine if results are statistically significant, or if they most likely occured by chance.  Hypothesis testing is a good way to measure statistical significance.  It involves formulating a null hypothesis that you would like to disprove, calculating the probability of a given result occurring if you were to assume that the null hypothesis is true, and rejecting the null hypothesis if that probability is sufficiently low.

### Hypothesis Testing:

Player p-test:
- Take as the null hypothesis that the shooting percentage for a given shot is less than or equal to the average percentage for that player for threes or twos.

Team p-test:
- Take as the null hypothesis that the shooting percentage for a given shot is less than or equal to the average percentage for the whole team for threes or twos.

Location p-test:
- Take as the null hypothesis that the shooting percentage for a given shot is less than or equal to the average percentage for the whole team from that location.


```python
shots_array = np.array([["shooter", "three", "cluster", 'num_shots', "num_makes", "pct", "expectedval", 
                         "Player_p", "Team_p", "Location_p"]])
threemask = ShotsPD["ThreePt"] == 1
twomask = ShotsPD["ThreePt"] == 0
threePD = ShotsPD[threemask]
twoPD = ShotsPD[twomask]

for shooter in threePD["shooter_name"].unique():
    mask = threePD["shooter_name"] == shooter
    ShooterShots = threePD[mask]
    for cluster in ShooterShots["LocationCluster"].unique():
        mask2 = ShooterShots["LocationCluster"] == cluster
        ClusterShots = ShooterShots[mask2]
        num_shots =  len(ClusterShots)
        mask_made =ClusterShots["made/missed"] == 1
        num_makes = len(ClusterShots[mask_made])
        pct = num_makes/num_shots
        expectedval = pct*3
        # player p-value
        avg_pct = len(ShooterShots[ShooterShots["made/missed"]==1])/len(ShooterShots)  
        # total 3 pt avg for this player
        mu = num_shots*avg_pct  # mean number of makes for this cluster assuming average shooting
        sigma = sc.sqrt(mu*(1-avg_pct))  # standard deviation?
        player_p = 1-norm.cdf(num_makes, loc=mu, scale=sigma)
        # team p-value
        avg_pct = len(threePD[threePD["made/missed"]==1])/len(threePD)  # total 3 pt avg for the team
        mu = num_shots*avg_pct
        sigma = sc.sqrt(mu*(1-avg_pct))
        team_p = 1-norm.cdf(num_makes, loc=mu, scale=sigma)
        # location p-value
        teamClusterPD = threePD[threePD["LocationCluster"] == cluster]
        avg_pct = len(teamClusterPD[teamClusterPD["made/missed"]==1])/len(teamClusterPD)  
        # team 3 pt avg from this cluster
        mu = num_shots*avg_pct
        sigma = sc.sqrt(mu*(1-avg_pct))
        location_p = 1-norm.cdf(num_makes, loc=mu, scale=sigma)
        
        shots_array = np.append(shots_array, [[shooter, 1, cluster, num_shots, 
                                               num_makes, pct, expectedval, player_p, 
                                               team_p, location_p]], axis=0)
        
for shooter in twoPD["shooter_name"].unique():
    mask = twoPD["shooter_name"] == shooter
    ShooterShots = twoPD[mask]
    for cluster in ShooterShots["LocationCluster"].unique():
        mask2 = ShooterShots["LocationCluster"] == cluster
        ClusterShots = ShooterShots[mask2]
        num_shots =  len(ClusterShots)
        mask_made =ClusterShots["made/missed"] == 1
        num_makes = len(ClusterShots[mask_made])
        pct = num_makes/num_shots
        expectedval = pct*2
        
        avg_pct = len(ShooterShots[ShooterShots["made/missed"]==1])/len(ShooterShots)  
        #total 3 pt avg for this player
        mu = num_shots*avg_pct  # mean number of makes for this cluster
        sigma = sc.sqrt(mu*(1-avg_pct))  # standard deviation?
        player_p = 1-norm.cdf(num_makes, loc=mu, scale=sigma)
        # team p-value
        avg_pct = len(twoPD[twoPD["made/missed"]==1])/len(twoPD)  # total 3 pt avg for the team
        mu = num_shots*avg_pct
        sigma = sc.sqrt(mu*(1-avg_pct))
        team_p = 1-norm.cdf(num_makes, loc=mu, scale=sigma)
        # location p-value
        teamClusterPD = twoPD[twoPD["LocationCluster"] == cluster]
        avg_pct = len(teamClusterPD[teamClusterPD["made/missed"]==1])/len(teamClusterPD)  
        # team 3 pt avg from this cluster
        mu = num_shots*avg_pct
        sigma = sc.sqrt(mu*(1-avg_pct))
        location_p = 1-norm.cdf(num_makes, loc=mu, scale=sigma)
        
        shots_array = np.append(shots_array, [[shooter, 0, cluster, num_shots, 
                                               num_makes, pct, expectedval, player_p, 
                                               team_p, location_p]], axis=0)
```

    /Users/averysmith/anaconda/lib/python3.6/site-packages/scipy/stats/_distn_infrastructure.py:1732: RuntimeWarning: invalid value encountered in double_scalars
      x = np.asarray((x - loc)/scale, dtype=dtyp)



```python
NewLocationPD = pd.DataFrame(data=shots_array[1:],
                             columns=shots_array[0])
```


```python
NewLocationPD["three"] = NewLocationPD["three"].map(int)
NewLocationPD["cluster"] = NewLocationPD["cluster"].map(int)
NewLocationPD["num_shots"] = NewLocationPD["num_shots"].map(int)
NewLocationPD["num_makes"] = NewLocationPD["num_makes"].map(int)
NewLocationPD["pct"] = NewLocationPD["pct"].map(float)
NewLocationPD["expectedval"] = NewLocationPD["expectedval"].map(float)
NewLocationPD["Player_p"] = NewLocationPD["Player_p"].map(float)
NewLocationPD["Team_p"] = NewLocationPD["Team_p"].map(float)
NewLocationPD["Location_p"] = NewLocationPD["Location_p"].map(float)
print(NewLocationPD.dtypes, '\n')
```

    shooter         object
    three            int64
    cluster          int64
    num_shots        int64
    num_makes        int64
    pct            float64
    expectedval    float64
    Player_p       float64
    Team_p         float64
    Location_p     float64
    dtype: object 
    



```python
# print the 40 best expected value shots()
NewLocationPD.sort_values(by=["expectedval"], ascending=False).head(40)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>shooter</th>
      <th>three</th>
      <th>cluster</th>
      <th>num_shots</th>
      <th>num_makes</th>
      <th>pct</th>
      <th>expectedval</th>
      <th>Player_p</th>
      <th>Team_p</th>
      <th>Location_p</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>68</th>
      <td>Rudy Gobert</td>
      <td>0</td>
      <td>4</td>
      <td>1</td>
      <td>1</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>0.219013</td>
      <td>1.654043e-01</td>
      <td>0.084870</td>
    </tr>
    <tr>
      <th>120</th>
      <td>Raul Neto</td>
      <td>0</td>
      <td>5</td>
      <td>2</td>
      <td>2</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>0.070967</td>
      <td>8.451872e-02</td>
      <td>0.055618</td>
    </tr>
    <tr>
      <th>127</th>
      <td>Royce O</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1.000000</td>
      <td>2.000000</td>
      <td>0.148750</td>
      <td>1.654043e-01</td>
      <td>0.124909</td>
    </tr>
    <tr>
      <th>56</th>
      <td>Jae Crowder</td>
      <td>1</td>
      <td>1</td>
      <td>24</td>
      <td>14</td>
      <td>0.583333</td>
      <td>1.750000</td>
      <td>0.002547</td>
      <td>1.302116e-02</td>
      <td>0.032321</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Donovan Mitchell</td>
      <td>1</td>
      <td>2</td>
      <td>45</td>
      <td>25</td>
      <td>0.555556</td>
      <td>1.666667</td>
      <td>0.001011</td>
      <td>3.902713e-03</td>
      <td>0.017140</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Joe Ingles</td>
      <td>1</td>
      <td>2</td>
      <td>82</td>
      <td>41</td>
      <td>0.500000</td>
      <td>1.500000</td>
      <td>0.144763</td>
      <td>5.447301e-03</td>
      <td>0.033559</td>
    </tr>
    <tr>
      <th>43</th>
      <td>Raul Neto</td>
      <td>1</td>
      <td>1</td>
      <td>6</td>
      <td>3</td>
      <td>0.500000</td>
      <td>1.500000</td>
      <td>0.308538</td>
      <td>2.455023e-01</td>
      <td>0.306089</td>
    </tr>
    <tr>
      <th>133</th>
      <td>Jonas Jerebko</td>
      <td>0</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
      <td>0.750000</td>
      <td>1.500000</td>
      <td>0.166598</td>
      <td>1.724358e-01</td>
      <td>0.044999</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Jonas Jerebko</td>
      <td>1</td>
      <td>1</td>
      <td>43</td>
      <td>21</td>
      <td>0.488372</td>
      <td>1.465116</td>
      <td>0.170106</td>
      <td>4.596406e-02</td>
      <td>0.114789</td>
    </tr>
    <tr>
      <th>51</th>
      <td>Jonas Jerebko</td>
      <td>1</td>
      <td>2</td>
      <td>44</td>
      <td>21</td>
      <td>0.477273</td>
      <td>1.431818</td>
      <td>0.207412</td>
      <td>6.035049e-02</td>
      <td>0.150673</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Donovan Mitchell</td>
      <td>0</td>
      <td>1</td>
      <td>10</td>
      <td>7</td>
      <td>0.700000</td>
      <td>1.400000</td>
      <td>0.099649</td>
      <td>1.195645e-01</td>
      <td>0.042443</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joe Ingles</td>
      <td>1</td>
      <td>5</td>
      <td>56</td>
      <td>26</td>
      <td>0.464286</td>
      <td>1.392857</td>
      <td>0.368013</td>
      <td>6.071480e-02</td>
      <td>0.041625</td>
    </tr>
    <tr>
      <th>41</th>
      <td>Raul Neto</td>
      <td>1</td>
      <td>2</td>
      <td>13</td>
      <td>6</td>
      <td>0.461538</td>
      <td>1.384615</td>
      <td>0.325306</td>
      <td>2.340261e-01</td>
      <td>0.327786</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Joe Ingles</td>
      <td>1</td>
      <td>1</td>
      <td>109</td>
      <td>50</td>
      <td>0.458716</td>
      <td>1.376147</td>
      <td>0.361958</td>
      <td>2.067597e-02</td>
      <td>0.100186</td>
    </tr>
    <tr>
      <th>31</th>
      <td>Thabo Sefolosha</td>
      <td>1</td>
      <td>1</td>
      <td>20</td>
      <td>9</td>
      <td>0.450000</td>
      <td>1.350000</td>
      <td>0.267932</td>
      <td>2.139309e-01</td>
      <td>0.319572</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Derrick Favors</td>
      <td>0</td>
      <td>0</td>
      <td>479</td>
      <td>320</td>
      <td>0.668058</td>
      <td>1.336117</td>
      <td>0.000679</td>
      <td>7.471357e-12</td>
      <td>0.000002</td>
    </tr>
    <tr>
      <th>126</th>
      <td>Royce O</td>
      <td>0</td>
      <td>5</td>
      <td>3</td>
      <td>2</td>
      <td>0.666667</td>
      <td>1.333333</td>
      <td>0.258235</td>
      <td>2.983175e-01</td>
      <td>0.215423</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodney Hood</td>
      <td>1</td>
      <td>3</td>
      <td>93</td>
      <td>41</td>
      <td>0.440860</td>
      <td>1.322581</td>
      <td>0.120899</td>
      <td>6.343204e-02</td>
      <td>0.042846</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Joe Ingles</td>
      <td>1</td>
      <td>3</td>
      <td>99</td>
      <td>43</td>
      <td>0.434343</td>
      <td>1.303030</td>
      <td>0.560276</td>
      <td>7.488420e-02</td>
      <td>0.050744</td>
    </tr>
    <tr>
      <th>30</th>
      <td>Thabo Sefolosha</td>
      <td>1</td>
      <td>2</td>
      <td>28</td>
      <td>12</td>
      <td>0.428571</td>
      <td>1.285714</td>
      <td>0.308814</td>
      <td>2.411689e-01</td>
      <td>0.382603</td>
    </tr>
    <tr>
      <th>27</th>
      <td>Ricky Rubio</td>
      <td>1</td>
      <td>4</td>
      <td>35</td>
      <td>15</td>
      <td>0.428571</td>
      <td>1.285714</td>
      <td>0.174564</td>
      <td>2.160885e-01</td>
      <td>0.081620</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Rudy Gobert</td>
      <td>0</td>
      <td>0</td>
      <td>426</td>
      <td>271</td>
      <td>0.636150</td>
      <td>1.272300</td>
      <td>0.308772</td>
      <td>2.249937e-07</td>
      <td>0.001403</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Rodney Hood</td>
      <td>1</td>
      <td>5</td>
      <td>59</td>
      <td>25</td>
      <td>0.423729</td>
      <td>1.271186</td>
      <td>0.254158</td>
      <td>1.729582e-01</td>
      <td>0.130013</td>
    </tr>
    <tr>
      <th>48</th>
      <td>Royce O</td>
      <td>1</td>
      <td>3</td>
      <td>12</td>
      <td>5</td>
      <td>0.416667</td>
      <td>1.250000</td>
      <td>0.270146</td>
      <td>3.541097e-01</td>
      <td>0.329155</td>
    </tr>
    <tr>
      <th>104</th>
      <td>Thabo Sefolosha</td>
      <td>0</td>
      <td>2</td>
      <td>8</td>
      <td>5</td>
      <td>0.625000</td>
      <td>1.250000</td>
      <td>0.329155</td>
      <td>2.648511e-01</td>
      <td>0.056156</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Thabo Sefolosha</td>
      <td>0</td>
      <td>0</td>
      <td>116</td>
      <td>71</td>
      <td>0.612069</td>
      <td>1.224138</td>
      <td>0.080125</td>
      <td>1.723821e-02</td>
      <td>0.150045</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Rodney Hood</td>
      <td>1</td>
      <td>1</td>
      <td>27</td>
      <td>11</td>
      <td>0.407407</td>
      <td>1.222222</td>
      <td>0.392461</td>
      <td>3.222499e-01</td>
      <td>0.463034</td>
    </tr>
    <tr>
      <th>24</th>
      <td>Ricky Rubio</td>
      <td>1</td>
      <td>2</td>
      <td>64</td>
      <td>26</td>
      <td>0.406250</td>
      <td>1.218750</td>
      <td>0.186086</td>
      <td>2.447323e-01</td>
      <td>0.465276</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Joe Johnson</td>
      <td>1</td>
      <td>5</td>
      <td>5</td>
      <td>2</td>
      <td>0.400000</td>
      <td>1.200000</td>
      <td>0.253123</td>
      <td>4.348063e-01</td>
      <td>0.414141</td>
    </tr>
    <tr>
      <th>38</th>
      <td>Alec Burks</td>
      <td>1</td>
      <td>2</td>
      <td>15</td>
      <td>6</td>
      <td>0.400000</td>
      <td>1.200000</td>
      <td>0.277314</td>
      <td>3.880836e-01</td>
      <td>0.502873</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Royce O</td>
      <td>1</td>
      <td>5</td>
      <td>10</td>
      <td>4</td>
      <td>0.400000</td>
      <td>1.200000</td>
      <td>0.327360</td>
      <td>4.082131e-01</td>
      <td>0.379516</td>
    </tr>
    <tr>
      <th>26</th>
      <td>Ricky Rubio</td>
      <td>1</td>
      <td>3</td>
      <td>87</td>
      <td>34</td>
      <td>0.390805</td>
      <td>1.172414</td>
      <td>0.229947</td>
      <td>3.062398e-01</td>
      <td>0.246089</td>
    </tr>
    <tr>
      <th>85</th>
      <td>Joe Johnson</td>
      <td>0</td>
      <td>5</td>
      <td>12</td>
      <td>7</td>
      <td>0.583333</td>
      <td>1.166667</td>
      <td>0.298303</td>
      <td>3.152880e-01</td>
      <td>0.160097</td>
    </tr>
    <tr>
      <th>125</th>
      <td>Royce O</td>
      <td>0</td>
      <td>3</td>
      <td>12</td>
      <td>7</td>
      <td>0.583333</td>
      <td>1.166667</td>
      <td>0.235837</td>
      <td>3.152880e-01</td>
      <td>0.099837</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Alec Burks</td>
      <td>1</td>
      <td>1</td>
      <td>18</td>
      <td>7</td>
      <td>0.388889</td>
      <td>1.166667</td>
      <td>0.292241</td>
      <td>4.154618e-01</td>
      <td>0.533750</td>
    </tr>
    <tr>
      <th>42</th>
      <td>Raul Neto</td>
      <td>1</td>
      <td>4</td>
      <td>13</td>
      <td>5</td>
      <td>0.384615</td>
      <td>1.153846</td>
      <td>0.545075</td>
      <td>4.406020e-01</td>
      <td>0.305157</td>
    </tr>
    <tr>
      <th>40</th>
      <td>Raul Neto</td>
      <td>1</td>
      <td>3</td>
      <td>8</td>
      <td>3</td>
      <td>0.375000</td>
      <td>1.125000</td>
      <td>0.557383</td>
      <td>4.757867e-01</td>
      <td>0.454265</td>
    </tr>
    <tr>
      <th>106</th>
      <td>Joe Ingles</td>
      <td>0</td>
      <td>0</td>
      <td>201</td>
      <td>113</td>
      <td>0.562189</td>
      <td>1.124378</td>
      <td>0.053589</td>
      <td>8.558441e-02</td>
      <td>0.524781</td>
    </tr>
    <tr>
      <th>135</th>
      <td>Jae Crowder</td>
      <td>0</td>
      <td>0</td>
      <td>89</td>
      <td>50</td>
      <td>0.561798</td>
      <td>1.123596</td>
      <td>0.019917</td>
      <td>1.832057e-01</td>
      <td>0.519463</td>
    </tr>
    <tr>
      <th>83</th>
      <td>Joe Johnson</td>
      <td>0</td>
      <td>0</td>
      <td>75</td>
      <td>42</td>
      <td>0.560000</td>
      <td>1.120000</td>
      <td>0.179038</td>
      <td>2.124386e-01</td>
      <td>0.530371</td>
    </tr>
  </tbody>
</table>
</div>




```python
# identify the mean location for each of the six clusters identified through kMeans
# (ignoring two and three point differences)
left_coordinates = np.array([])
top_coordinates = np.array([])
clusters = np.arange(0, 6)
for cluster in clusters:
    cluster_mask = ShotsPD['LocationCluster'] == cluster
    cluster_df = ShotsPD[cluster_mask]
    left_coord = cluster_df["left"].mean()
    top_coord = cluster_df["top"].mean()
    left_coordinates = np.append(left_coordinates, left_coord)
    top_coordinates = np.append(top_coordinates, top_coord)
```


```python
# plot the mean location of each cluster on the court for reference purposes
import seaborn as sns

df = pd.DataFrame({
'x': left_coordinates,
'y': top_coordinates,
'group': clusters
})
 
p1=sns.regplot(data=df, x="x", y="y", fit_reg=False, marker="o", color="skyblue", scatter_kws={'s':400})
for line in range(0,df.shape[0]):
    p1.text(df.x[line]+0.2, df.y[line], df.group[line], horizontalalignment='left', 
            size='medium', color='black', weight='semibold')

plt.title('Clusters')
plt.imshow(img, zorder=0, extent=[0, 100, 0, 100.0])
plt.grid(False)
plt.show()
```


![png](output_64_0.png)


We choose a threshold for significance of p < .05 in order to reject the null hypothesis.

For each p-test we used, we filter the results for only the statistically significant shots.

The way to interpret these p-values is: if that location was not better than average (for the player or team as a whole), then the p-value represents the probability that the player would still, by coincidence, shoot as well from that location as they did.

#### Player p-test


```python
statistically_significant_mask = NewLocationPD["Player_p"] < .05
SignificantPD = NewLocationPD[statistically_significant_mask]
SignificantPD.sort_values(by=["expectedval"], ascending=False)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>shooter</th>
      <th>three</th>
      <th>cluster</th>
      <th>num_shots</th>
      <th>num_makes</th>
      <th>pct</th>
      <th>expectedval</th>
      <th>Player_p</th>
      <th>Team_p</th>
      <th>Location_p</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>56</th>
      <td>Jae Crowder</td>
      <td>1</td>
      <td>1</td>
      <td>24</td>
      <td>14</td>
      <td>0.583333</td>
      <td>1.750000</td>
      <td>0.002547</td>
      <td>1.302116e-02</td>
      <td>0.032321</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Donovan Mitchell</td>
      <td>1</td>
      <td>2</td>
      <td>45</td>
      <td>25</td>
      <td>0.555556</td>
      <td>1.666667</td>
      <td>0.001011</td>
      <td>3.902713e-03</td>
      <td>0.017140</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Derrick Favors</td>
      <td>0</td>
      <td>0</td>
      <td>479</td>
      <td>320</td>
      <td>0.668058</td>
      <td>1.336117</td>
      <td>0.000679</td>
      <td>7.471357e-12</td>
      <td>0.000002</td>
    </tr>
    <tr>
      <th>135</th>
      <td>Jae Crowder</td>
      <td>0</td>
      <td>0</td>
      <td>89</td>
      <td>50</td>
      <td>0.561798</td>
      <td>1.123596</td>
      <td>0.019917</td>
      <td>1.832057e-01</td>
      <td>0.519463</td>
    </tr>
    <tr>
      <th>71</th>
      <td>Donovan Mitchell</td>
      <td>0</td>
      <td>0</td>
      <td>599</td>
      <td>321</td>
      <td>0.535893</td>
      <td>1.071786</td>
      <td>0.028644</td>
      <td>1.412531e-01</td>
      <td>0.920028</td>
    </tr>
  </tbody>
</table>
</div>



This indicates that Jae Crowder shoots significantly better from the right corner than he does from any other three point location, and Donovan Mitchell shoots significantly better from the left corner than he does from any other three point location.  This information could help coaches modify the offense so that Crowder and Mitchell spend more time on the right and left side respectively.

Unsurprisingly, we found that Derrick Favors, Jae Crowder, and Donovan Mitchell all shoot better from the post than they do from any other two-point position.  That is unsurprising because the post is much closer to the basket than other locations, and is expected to have a better shooting percentage.


```python
statistically_significant_mask = NewLocationPD["Team_p"] < .05
TeamSignificantPD = NewLocationPD[statistically_significant_mask]
TeamSignificantPD.sort_values(by=["expectedval"], ascending=False)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>shooter</th>
      <th>three</th>
      <th>cluster</th>
      <th>num_shots</th>
      <th>num_makes</th>
      <th>pct</th>
      <th>expectedval</th>
      <th>Player_p</th>
      <th>Team_p</th>
      <th>Location_p</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>56</th>
      <td>Jae Crowder</td>
      <td>1</td>
      <td>1</td>
      <td>24</td>
      <td>14</td>
      <td>0.583333</td>
      <td>1.750000</td>
      <td>0.002547</td>
      <td>1.302116e-02</td>
      <td>0.032321</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Donovan Mitchell</td>
      <td>1</td>
      <td>2</td>
      <td>45</td>
      <td>25</td>
      <td>0.555556</td>
      <td>1.666667</td>
      <td>0.001011</td>
      <td>3.902713e-03</td>
      <td>0.017140</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Joe Ingles</td>
      <td>1</td>
      <td>2</td>
      <td>82</td>
      <td>41</td>
      <td>0.500000</td>
      <td>1.500000</td>
      <td>0.144763</td>
      <td>5.447301e-03</td>
      <td>0.033559</td>
    </tr>
    <tr>
      <th>53</th>
      <td>Jonas Jerebko</td>
      <td>1</td>
      <td>1</td>
      <td>43</td>
      <td>21</td>
      <td>0.488372</td>
      <td>1.465116</td>
      <td>0.170106</td>
      <td>4.596406e-02</td>
      <td>0.114789</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Joe Ingles</td>
      <td>1</td>
      <td>1</td>
      <td>109</td>
      <td>50</td>
      <td>0.458716</td>
      <td>1.376147</td>
      <td>0.361958</td>
      <td>2.067597e-02</td>
      <td>0.100186</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Derrick Favors</td>
      <td>0</td>
      <td>0</td>
      <td>479</td>
      <td>320</td>
      <td>0.668058</td>
      <td>1.336117</td>
      <td>0.000679</td>
      <td>7.471357e-12</td>
      <td>0.000002</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Rudy Gobert</td>
      <td>0</td>
      <td>0</td>
      <td>426</td>
      <td>271</td>
      <td>0.636150</td>
      <td>1.272300</td>
      <td>0.308772</td>
      <td>2.249937e-07</td>
      <td>0.001403</td>
    </tr>
    <tr>
      <th>100</th>
      <td>Thabo Sefolosha</td>
      <td>0</td>
      <td>0</td>
      <td>116</td>
      <td>71</td>
      <td>0.612069</td>
      <td>1.224138</td>
      <td>0.080125</td>
      <td>1.723821e-02</td>
      <td>0.150045</td>
    </tr>
  </tbody>
</table>
</div>



This indicates that the three point shots noted above for Jae Crowder and Donovan Mitchell are also significantly better than the team average for three point shots.  Joe Ingles (an excellent three point shooter) also shoots significantly better from both corners than the team three point average (although he does not shoot significantly better from the corners than he does from the other three point spots because his overall three point shooting percentage is so high).  Jonas Jerebko also shoots significantly better from the right corner than the team average for three pointers.  

For two pointers, we now find that Derrick Favors, Rudy Gobert, and Thabo Sefolosha all shoot significantly better from the post than the team average for two pointers.  The p-values for Derrick Favors and Rudy Gobert are extremely small, partially due to the large number of shots taken by both players from that location (>400).  Rudy Gobert likely didn't show up on the previous list because such a high percentage of his shots are taken from the post that two point percentage is effectively the same as his shooting percentage from the post.


```python
statistically_significant_mask = NewLocationPD["Location_p"] < .05
LocationSignificantPD = NewLocationPD[statistically_significant_mask]
LocationSignificantPD.sort_values(by=["expectedval"], ascending=False)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>shooter</th>
      <th>three</th>
      <th>cluster</th>
      <th>num_shots</th>
      <th>num_makes</th>
      <th>pct</th>
      <th>expectedval</th>
      <th>Player_p</th>
      <th>Team_p</th>
      <th>Location_p</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>56</th>
      <td>Jae Crowder</td>
      <td>1</td>
      <td>1</td>
      <td>24</td>
      <td>14</td>
      <td>0.583333</td>
      <td>1.750000</td>
      <td>0.002547</td>
      <td>1.302116e-02</td>
      <td>0.032321</td>
    </tr>
    <tr>
      <th>15</th>
      <td>Donovan Mitchell</td>
      <td>1</td>
      <td>2</td>
      <td>45</td>
      <td>25</td>
      <td>0.555556</td>
      <td>1.666667</td>
      <td>0.001011</td>
      <td>3.902713e-03</td>
      <td>0.017140</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Joe Ingles</td>
      <td>1</td>
      <td>2</td>
      <td>82</td>
      <td>41</td>
      <td>0.500000</td>
      <td>1.500000</td>
      <td>0.144763</td>
      <td>5.447301e-03</td>
      <td>0.033559</td>
    </tr>
    <tr>
      <th>133</th>
      <td>Jonas Jerebko</td>
      <td>0</td>
      <td>4</td>
      <td>4</td>
      <td>3</td>
      <td>0.750000</td>
      <td>1.500000</td>
      <td>0.166598</td>
      <td>1.724358e-01</td>
      <td>0.044999</td>
    </tr>
    <tr>
      <th>76</th>
      <td>Donovan Mitchell</td>
      <td>0</td>
      <td>1</td>
      <td>10</td>
      <td>7</td>
      <td>0.700000</td>
      <td>1.400000</td>
      <td>0.099649</td>
      <td>1.195645e-01</td>
      <td>0.042443</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Joe Ingles</td>
      <td>1</td>
      <td>5</td>
      <td>56</td>
      <td>26</td>
      <td>0.464286</td>
      <td>1.392857</td>
      <td>0.368013</td>
      <td>6.071480e-02</td>
      <td>0.041625</td>
    </tr>
    <tr>
      <th>60</th>
      <td>Derrick Favors</td>
      <td>0</td>
      <td>0</td>
      <td>479</td>
      <td>320</td>
      <td>0.668058</td>
      <td>1.336117</td>
      <td>0.000679</td>
      <td>7.471357e-12</td>
      <td>0.000002</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Rodney Hood</td>
      <td>1</td>
      <td>3</td>
      <td>93</td>
      <td>41</td>
      <td>0.440860</td>
      <td>1.322581</td>
      <td>0.120899</td>
      <td>6.343204e-02</td>
      <td>0.042846</td>
    </tr>
    <tr>
      <th>66</th>
      <td>Rudy Gobert</td>
      <td>0</td>
      <td>0</td>
      <td>426</td>
      <td>271</td>
      <td>0.636150</td>
      <td>1.272300</td>
      <td>0.308772</td>
      <td>2.249937e-07</td>
      <td>0.001403</td>
    </tr>
  </tbody>
</table>
</div>



The last hypothesis tested was whether certain players shot much better than the team average for that specific location.  Apart from identifying some of the same shots as above, this test would be expected to ffind some players who shoot exceptionally well from more difficult spots.  

Jonas Jerebko shooting two pointers from the right wing, Donovan Mitchell shooting two pointers from the right corner, Joe Ingles shooting from the top of the three-point arc, and Rodney Hood shooting three pointers from the left wing would all appear to fit in this category, although each falls just at the limits of statistical significance (.4 < p < .5).

**Conclusions**
This information can help coaches and decision-makers design offensive sets and plays, and can help players with shot-selection.  
For example, for the first p-test, we would advise Donovan Mitchell to take more three point shots from the left corner, and Jae Crowder to take more from the right corner.  Coaches could design their offense so both players spend more time on those sides.  We would also advise Donovan Mitchell to drive all the way to the basket when taking a two-point shot.  

For the second p-test, we would advise the coaches to develop their offense to maximize corner threes by Crowder, Mitchell, Ingles, and Jerebko, and post shots by Favors and Gobert.  

## Some additional Analysis

### Home and Away Differences
- We wanted to explore how the expected values for each player differ in home games vs away games. Once agian, we used simple masking to compare how players shoot home and away


```python
## HOME

# 3 pointers
PlayerIDs = np.unique(ShotsPD['shooter'])
PlayerNames = np.unique(ShotsPD['shooter_name'])
NumOPlayers = len(PlayerNames)
for Name in range(0,NumOPlayers):
    PercMadeDif3 = []
    NumShots = []
    NumMade = []
    ExpectedValue3 =[]
    AvLeft3 = []
    AvTop3 =[]
    PtVal3 = 3
    PtVal2 = 2
    PercMadeDif2 = []
    NumShots3 = []
    NumShots2 = []
    NumMade3 = []
    NumMade2 = []
    ExpectedValue2 =[]
    ExpectedValueRound = []
 
    AvLeft2 = []
    AvTop2 =[]
    
    for i in range(0,6):
        Location = ShotsPD[(ShotsPD['LocationCluster']==i) & (ShotsPD['ThreePt']==1) 
                           & ( ShotsPD['shooter_name'] == PlayerNames[Name])& ( ShotsPD['home/away'] == 0) ]     
        NumShots3.append(len(Location['made/missed']))
        NumMade3.append(len(Location[Location['made/missed']==1]))
        PercMade3 = 0
        if NumShots3[i] > 1:
            PercMade3 = NumMade3[i] / NumShots3[i]
            PercMadeDif3.append(PercMade3)
        else:
            PercMadeDif3.append(0)
    
        ExpectedValue3.append(PercMadeDif3[i]*PtVal3)
    
        AvLeft3.append(np.mean(Location['left']))
        AvTop3.append(np.mean(Location['top']))
        
        
        
    
        
        Location = ShotsPD[(ShotsPD['LocationCluster']==i) & (ShotsPD['ThreePt']==0) 
                           & ( ShotsPD['shooter_name'] == PlayerNames[Name]) & ( ShotsPD['home/away'] == 0) ]
        NumShots2.append(len(Location['made/missed']))
        NumMade2.append(len(Location[Location['made/missed']==1]))
        PercMade2 = 0
        if NumShots2[i] > 1:
                PercMade2 = NumMade2[i] / NumShots2[i]
                PercMadeDif2.append(PercMade2)
        else:
                PercMadeDif2.append(0)
        ExpectedValue2.append(PercMadeDif2[i]*PtVal2)
    
        AvLeft2.append(np.mean(Location['left']))
        AvTop2.append(np.mean(Location['top']))
        
    print('---------------------------------------------------------------')
    print(PlayerNames[Name])
    print('---------------------')
    print('---- 3 Pointers ----')
    print('Percentages')
    print(PercMadeDif3)
    print('---------------------')
    print('Expected Values')
    print(ExpectedValue3)
    

    
    print('---- 2 Pointers ----')
    print('Percentages')
    print(PercMadeDif2)
    print('---------------------')
    print('Expected Values')
    print(ExpectedValue2)
    
    for mm in range(0,len(ExpectedValue3)):
        if ExpectedValue3[mm] == 0:
            Extra = mm
    del ExpectedValue3[Extra]
    
    xx = np.isnan(AvLeft3)
    for mm in range(0, len(AvLeft3)):
        if  xx[mm] == True:
            DeleteVar = mm
    
    del AvLeft3[DeleteVar] 
    del AvTop3[DeleteVar]
    
    ExpectedValue = ExpectedValue3 + ExpectedValue2
    ExpectedValueRound = np.round_(ExpectedValue, decimals=2) 
    
    AvLefts = AvLeft3 + AvLeft2
    AvTops = AvTop3 + AvTop2
    
    for u in range(0,len(AvLefts)):
        if math.isnan(AvLefts[u]):
            AvLefts[u]=90
        if math.isnan(AvTops[u]):
            AvTops[u]= 0
        
    
    x = np.round(AvLefts,decimals=0)
    y = np.round(AvTops,decimals=0)
    valz = [str(ExpectedValueRound[0]),str(ExpectedValueRound[1]),str(ExpectedValueRound[2]),
            str(ExpectedValueRound[3]),str(ExpectedValueRound[4]),str(ExpectedValueRound[5]),
            str(ExpectedValueRound[6]),str(ExpectedValueRound[7]),str(ExpectedValueRound[8]),
            str(ExpectedValueRound[9]),str(ExpectedValueRound[10])]
    df = pd.DataFrame({
    'x': x,
    'y': y,
    'group': valz
    })
 
    p1=sns.regplot(data=df, x="x", y="y", fit_reg=False, marker="o", color="skyblue", scatter_kws={'s':400})
    for line in range(0,df.shape[0]):
        p1.text(df.x[line]+0.2, df.y[line], df.group[line], horizontalalignment='left', 
                size='medium', color='black', weight='semibold')
    plt.imshow(img, zorder=0, extent=[0, 100, 0, 100.0])
    plt.grid(False)

    plt.show()
```

    ---------------------------------------------------------------
    Alec Burks
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.2222222222222222, 0.2857142857142857, 0.2631578947368421, 0.17647058823529413, 0.3333333333333333]
    ---------------------
    Expected Values
    [0, 0.6666666666666666, 0.8571428571428571, 0.7894736842105263, 0.5294117647058824, 1.0]
    ---- 2 Pointers ----
    Percentages
    [0.40540540540540543, 0, 0, 0.2, 0.47368421052631576, 0.38461538461538464]
    ---------------------
    Expected Values
    [0.8108108108108109, 0, 0, 0.4, 0.9473684210526315, 0.7692307692307693]



![png](output_77_1.png)


    ---------------------------------------------------------------
    Derrick Favors
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.38461538461538464, 0.2, 0.0, 0, 0.5]
    ---------------------
    Expected Values
    [0, 1.153846153846154, 0.6000000000000001, 0.0, 0, 1.5]
    ---- 2 Pointers ----
    Percentages
    [0.6329113924050633, 0.2857142857142857, 0.14285714285714285, 0.2857142857142857, 0.46153846153846156, 0.42857142857142855]
    ---------------------
    Expected Values
    [1.2658227848101267, 0.5714285714285714, 0.2857142857142857, 0.5714285714285714, 0.9230769230769231, 0.8571428571428571]



![png](output_77_3.png)


    ---------------------------------------------------------------
    Donovan Mitchell
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.3793103448275862, 0.47368421052631576, 0.2835820895522388, 0.3717948717948718, 0.3584905660377358]
    ---------------------
    Expected Values
    [0, 1.1379310344827585, 1.4210526315789473, 0.8507462686567164, 1.1153846153846154, 1.0754716981132075]
    ---- 2 Pointers ----
    Percentages
    [0.5144694533762058, 0.5, 0.5, 0.3888888888888889, 0.36585365853658536, 0.4444444444444444]
    ---------------------
    Expected Values
    [1.0289389067524115, 1.0, 1.0, 0.7777777777777778, 0.7317073170731707, 0.8888888888888888]



![png](output_77_5.png)


    ---------------------------------------------------------------
    Ekpe Udoh
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0, 0, 0, 0, 0]
    ---------------------
    Expected Values
    [0, 0, 0, 0, 0, 0]
    ---- 2 Pointers ----
    Percentages
    [0.515625, 0, 0, 0, 0, 0]
    ---------------------
    Expected Values
    [1.03125, 0, 0, 0, 0, 0]



![png](output_77_7.png)


    ---------------------------------------------------------------
    Jae Crowder
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.8, 0.42857142857142855, 0.3, 0.23076923076923078, 0.2222222222222222]
    ---------------------
    Expected Values
    [0, 2.4000000000000004, 1.2857142857142856, 0.8999999999999999, 0.6923076923076923, 0.6666666666666666]
    ---- 2 Pointers ----
    Percentages
    [0.6129032258064516, 0.5, 0.4, 0.3333333333333333, 0.16666666666666666, 0.25]
    ---------------------
    Expected Values
    [1.2258064516129032, 1.0, 0.8, 0.6666666666666666, 0.3333333333333333, 0.5]



![png](output_77_9.png)


    ---------------------------------------------------------------
    Joe Ingles
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.425531914893617, 0.5135135135135135, 0.5098039215686274, 0.3902439024390244, 0.5333333333333333]
    ---------------------
    Expected Values
    [0, 1.2765957446808511, 1.5405405405405403, 1.5294117647058822, 1.1707317073170733, 1.6]
    ---- 2 Pointers ----
    Percentages
    [0.5670103092783505, 0.6, 0, 0.38461538461538464, 0.3333333333333333, 0.5]
    ---------------------
    Expected Values
    [1.134020618556701, 1.2, 0, 0.7692307692307693, 0.6666666666666666, 1.0]



![png](output_77_11.png)


    ---------------------------------------------------------------
    Joe Johnson
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.2, 0.15789473684210525, 0.0, 0.4444444444444444, 0.0]
    ---------------------
    Expected Values
    [0, 0.6000000000000001, 0.47368421052631576, 0.0, 1.3333333333333333, 0.0]
    ---- 2 Pointers ----
    Percentages
    [0.48936170212765956, 0.5555555555555556, 0, 0.4, 0.375, 0.5]
    ---------------------
    Expected Values
    [0.9787234042553191, 1.1111111111111112, 0, 0.8, 0.75, 1.0]



![png](output_77_13.png)


    ---------------------------------------------------------------
    Jonas Jerebko
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.35294117647058826, 0.42857142857142855, 0.4, 0.25, 0.375]
    ---------------------
    Expected Values
    [0, 1.0588235294117647, 1.2857142857142856, 1.2000000000000002, 0.75, 1.125]
    ---- 2 Pointers ----
    Percentages
    [0.5487804878048781, 0.3333333333333333, 0.5, 0.0, 0, 0.25]
    ---------------------
    Expected Values
    [1.0975609756097562, 0.6666666666666666, 1.0, 0.0, 0, 0.5]



![png](output_77_15.png)


    ---------------------------------------------------------------
    Raul Neto
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.3333333333333333, 0.42857142857142855, 0.375, 0.5, 0.0]
    ---------------------
    Expected Values
    [0, 1.0, 1.2857142857142856, 1.125, 1.5, 0.0]
    ---- 2 Pointers ----
    Percentages
    [0.5192307692307693, 0.0, 0, 0.6666666666666666, 0.25, 0]
    ---------------------
    Expected Values
    [1.0384615384615385, 0.0, 0, 1.3333333333333333, 0.5, 0]



![png](output_77_17.png)


    ---------------------------------------------------------------
    Ricky Rubio
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.23529411764705882, 0.45454545454545453, 0.4523809523809524, 0.5882352941176471, 0.25]
    ---------------------
    Expected Values
    [0, 0.7058823529411764, 1.3636363636363635, 1.3571428571428572, 1.7647058823529411, 0.75]
    ---- 2 Pointers ----
    Percentages
    [0.49295774647887325, 0.5, 0.25, 0.43209876543209874, 0.3125, 0.4782608695652174]
    ---------------------
    Expected Values
    [0.9859154929577465, 1.0, 0.5, 0.8641975308641975, 0.625, 0.9565217391304348]



![png](output_77_19.png)


    ---------------------------------------------------------------
    Rodney Hood
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.35714285714285715, 0.1875, 0.4642857142857143, 0.32, 0.5161290322580645]
    ---------------------
    Expected Values
    [0, 1.0714285714285714, 0.5625, 1.3928571428571428, 0.96, 1.5483870967741935]
    ---- 2 Pointers ----
    Percentages
    [0.5581395348837209, 0.5, 0.3333333333333333, 0.5454545454545454, 0.3076923076923077, 0.5714285714285714]
    ---------------------
    Expected Values
    [1.1162790697674418, 1.0, 0.6666666666666666, 1.0909090909090908, 0.6153846153846154, 1.1428571428571428]



![png](output_77_21.png)


    ---------------------------------------------------------------
    Royce O
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.45, 0.375, 0.16666666666666666, 0.13333333333333333, 0.42857142857142855]
    ---------------------
    Expected Values
    [0, 1.35, 1.125, 0.5, 0.4, 1.2857142857142856]
    ---- 2 Pointers ----
    Percentages
    [0.5131578947368421, 0, 0.0, 0.5714285714285714, 0.3333333333333333, 0]
    ---------------------
    Expected Values
    [1.0263157894736843, 0, 0.0, 1.1428571428571428, 0.6666666666666666, 0]



![png](output_77_23.png)


    ---------------------------------------------------------------
    Rudy Gobert
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0, 0, 0, 0, 0]
    ---------------------
    Expected Values
    [0, 0, 0, 0, 0, 0]
    ---- 2 Pointers ----
    Percentages
    [0.5555555555555556, 0, 0, 0.0, 0, 0.5]
    ---------------------
    Expected Values
    [1.1111111111111112, 0, 0, 0.0, 0, 1.0]



![png](output_77_25.png)


    ---------------------------------------------------------------
    Thabo Sefolosha
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.45454545454545453, 0.5, 0.6, 0.0, 1.0]
    ---------------------
    Expected Values
    [0, 1.3636363636363635, 1.5, 1.7999999999999998, 0.0, 3.0]
    ---- 2 Pointers ----
    Percentages
    [0.6226415094339622, 0.5, 0.75, 0.38461538461538464, 0.0, 0.0]
    ---------------------
    Expected Values
    [1.2452830188679245, 1.0, 1.5, 0.7692307692307693, 0.0, 0.0]



![png](output_77_27.png)



```python
## Away

# 3 pointers
PlayerIDs = np.unique(ShotsPD['shooter'])
PlayerNames = np.unique(ShotsPD['shooter_name'])
NumOPlayers = len(PlayerNames)
for Name in range(0,NumOPlayers):
    PercMadeDif3 = []
    NumShots = []
    NumMade = []
    ExpectedValue3 =[]
    AvLeft3 = []
    AvTop3 =[]
    PtVal3 = 3
    PtVal2 = 2
    PercMadeDif2 = []
    NumShots3 = []
    NumShots2 = []
    NumMade3 = []
    NumMade2 = []
    ExpectedValue2 =[]
    ExpectedValueRound = []
 
    AvLeft2 = []
    AvTop2 =[]
    
    for i in range(0,6):
        Location = ShotsPD[(ShotsPD['LocationCluster']==i) & (ShotsPD['ThreePt']==1) 
                           & ( ShotsPD['shooter_name'] == PlayerNames[Name])& ( ShotsPD['home/away'] == 1) ]     
        NumShots3.append(len(Location['made/missed']))
        NumMade3.append(len(Location[Location['made/missed']==1]))
        PercMade3 = 0
        if NumShots3[i] > 1:
            PercMade3 = NumMade3[i] / NumShots3[i]
            PercMadeDif3.append(PercMade3)
        else:
            PercMadeDif3.append(0)
    
        ExpectedValue3.append(PercMadeDif3[i]*PtVal3)
    
        AvLeft3.append(np.mean(Location['left']))
        AvTop3.append(np.mean(Location['top']))
        
        
        
    
        
        Location = ShotsPD[(ShotsPD['LocationCluster']==i) & (ShotsPD['ThreePt']==0) 
                           & ( ShotsPD['shooter_name'] == PlayerNames[Name]) & ( ShotsPD['home/away'] == 1) ]
        NumShots2.append(len(Location['made/missed']))
        NumMade2.append(len(Location[Location['made/missed']==1]))
        PercMade2 = 0
        if NumShots2[i] > 1:
                PercMade2 = NumMade2[i] / NumShots2[i]
                PercMadeDif2.append(PercMade2)
        else:
                PercMadeDif2.append(0)
        ExpectedValue2.append(PercMadeDif2[i]*PtVal2)
    
        AvLeft2.append(np.mean(Location['left']))
        AvTop2.append(np.mean(Location['top']))
        
    print('---------------------------------------------------------------')
    print(PlayerNames[Name])
    print('---------------------')
    print('---- 3 Pointers ----')
    print('Percentages')
    print(PercMadeDif3)
    print('---------------------')
    print('Expected Values')
    print(ExpectedValue3)
    

    
    print('---- 2 Pointers ----')
    print('Percentages')
    print(PercMadeDif2)
    print('---------------------')
    print('Expected Values')
    print(ExpectedValue2)
    
    for mm in range(0,len(ExpectedValue3)):
        if ExpectedValue3[mm] == 0:
            Extra = mm
    del ExpectedValue3[Extra]
    
    xx = np.isnan(AvLeft3)
    for mm in range(0, len(AvLeft3)):
        if  xx[mm] == True:
            DeleteVar = mm
    
    del AvLeft3[DeleteVar] 
    del AvTop3[DeleteVar]
    
    ExpectedValue = ExpectedValue3 + ExpectedValue2
    ExpectedValueRound = np.round_(ExpectedValue, decimals=2) 
    
    AvLefts = AvLeft3 + AvLeft2
    AvTops = AvTop3 + AvTop2
    
    for u in range(0,len(AvLefts)):
        if math.isnan(AvLefts[u]):
            AvLefts[u]=90
        if math.isnan(AvTops[u]):
            AvTops[u]= 0
        
    
    x = np.round(AvLefts,decimals=0)
    y = np.round(AvTops,decimals=0)
    valz = [str(ExpectedValueRound[0]),str(ExpectedValueRound[1]),str(ExpectedValueRound[2]),
            str(ExpectedValueRound[3]),str(ExpectedValueRound[4]),str(ExpectedValueRound[5]),
            str(ExpectedValueRound[6]),str(ExpectedValueRound[7]),str(ExpectedValueRound[8]),
            str(ExpectedValueRound[9]),str(ExpectedValueRound[10])]
    df = pd.DataFrame({
    'x': x,
    'y': y,
    'group': valz
    })
 
    p1=sns.regplot(data=df, x="x", y="y", fit_reg=False, marker="o", color="skyblue", scatter_kws={'s':400})
    for line in range(0,df.shape[0]):
        p1.text(df.x[line]+0.2, df.y[line], df.group[line], horizontalalignment='left', 
                size='medium', color='black', weight='semibold')
    
    plt.imshow(img, zorder=0, extent=[0, 100, 0, 100.0])
    plt.grid(False)

    plt.show()
```

    ---------------------------------------------------------------
    Alec Burks
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.5555555555555556, 0.5, 0.5, 0.3076923076923077, 0.2857142857142857]
    ---------------------
    Expected Values
    [0, 1.6666666666666667, 1.5, 1.5, 0.9230769230769231, 0.8571428571428571]
    ---- 2 Pointers ----
    Percentages
    [0.5875, 0, 0, 0.5625, 0.2857142857142857, 0.3333333333333333]
    ---------------------
    Expected Values
    [1.175, 0, 0, 1.125, 0.5714285714285714, 0.6666666666666666]



![png](output_78_1.png)


    ---------------------------------------------------------------
    Derrick Favors
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.17647058823529413, 0.18181818181818182, 0, 0, 0]
    ---------------------
    Expected Values
    [0, 0.5294117647058824, 0.5454545454545454, 0, 0, 0]
    ---- 2 Pointers ----
    Percentages
    [0.7024793388429752, 0.375, 1.0, 0.2, 0.3125, 0.46511627906976744]
    ---------------------
    Expected Values
    [1.4049586776859504, 0.75, 2.0, 0.4, 0.625, 0.9302325581395349]



![png](output_78_3.png)


    ---------------------------------------------------------------
    Donovan Mitchell
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.358974358974359, 0.6153846153846154, 0.2619047619047619, 0.27941176470588236, 0.30612244897959184]
    ---------------------
    Expected Values
    [0, 1.0769230769230769, 1.8461538461538463, 0.7857142857142858, 0.8382352941176471, 0.9183673469387755]
    ---- 2 Pointers ----
    Percentages
    [0.5590277777777778, 0.75, 0.3333333333333333, 0.5121951219512195, 0.28888888888888886, 0.37209302325581395]
    ---------------------
    Expected Values
    [1.1180555555555556, 1.5, 0.6666666666666666, 1.024390243902439, 0.5777777777777777, 0.7441860465116279]



![png](output_78_5.png)


    ---------------------------------------------------------------
    Ekpe Udoh
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0, 0, 0, 0, 0]
    ---------------------
    Expected Values
    [0, 0, 0, 0, 0, 0]
    ---- 2 Pointers ----
    Percentages
    [0.5625, 0, 0, 0, 0.0, 0]
    ---------------------
    Expected Values
    [1.125, 0, 0, 0, 0.0, 0]



![png](output_78_7.png)


    ---------------------------------------------------------------
    Jae Crowder
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.42857142857142855, 0.058823529411764705, 0.23529411764705882, 0.2727272727272727, 0.4444444444444444]
    ---------------------
    Expected Values
    [0, 1.2857142857142856, 0.1764705882352941, 0.7058823529411764, 0.8181818181818181, 1.3333333333333333]
    ---- 2 Pointers ----
    Percentages
    [0.5344827586206896, 0.5, 0.0, 0.2857142857142857, 0.2222222222222222, 0.4444444444444444]
    ---------------------
    Expected Values
    [1.0689655172413792, 1.0, 0.0, 0.5714285714285714, 0.4444444444444444, 0.8888888888888888]



![png](output_78_9.png)


    ---------------------------------------------------------------
    Joe Ingles
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.4838709677419355, 0.4888888888888889, 0.3541666666666667, 0.34615384615384615, 0.38461538461538464]
    ---------------------
    Expected Values
    [0, 1.4516129032258065, 1.4666666666666666, 1.0625, 1.0384615384615383, 1.153846153846154]
    ---- 2 Pointers ----
    Percentages
    [0.5576923076923077, 0.4, 0.0, 0.3, 0.2727272727272727, 0.42857142857142855]
    ---------------------
    Expected Values
    [1.1153846153846154, 0.8, 0.0, 0.6, 0.5454545454545454, 0.8571428571428571]



![png](output_78_11.png)


    ---------------------------------------------------------------
    Joe Johnson
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.45454545454545453, 0.5, 0.2222222222222222, 0.2, 0.6666666666666666]
    ---------------------
    Expected Values
    [0, 1.3636363636363635, 1.5, 0.6666666666666666, 0.6000000000000001, 2.0]
    ---- 2 Pointers ----
    Percentages
    [0.6785714285714286, 0.5, 0.0, 0.4, 0.5714285714285714, 0.6666666666666666]
    ---------------------
    Expected Values
    [1.3571428571428572, 1.0, 0.0, 0.8, 1.1428571428571428, 1.3333333333333333]



![png](output_78_13.png)


    ---------------------------------------------------------------
    Jonas Jerebko
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.5769230769230769, 0.5217391304347826, 0.3125, 0.4166666666666667, 0.16666666666666666]
    ---------------------
    Expected Values
    [0, 1.7307692307692306, 1.5652173913043477, 0.9375, 1.25, 0.5]
    ---- 2 Pointers ----
    Percentages
    [0.5352112676056338, 0.0, 0.0, 0.3333333333333333, 1.0, 0]
    ---------------------
    Expected Values
    [1.0704225352112675, 0.0, 0.0, 0.6666666666666666, 2.0, 0]



![png](output_78_15.png)


    ---------------------------------------------------------------
    Raul Neto
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.6666666666666666, 0.5, 0, 0.3333333333333333, 0.3333333333333333]
    ---------------------
    Expected Values
    [0, 2.0, 1.5, 0, 1.0, 1.0]
    ---- 2 Pointers ----
    Percentages
    [0.5294117647058824, 0.5, 0, 0.0, 0.0, 0]
    ---------------------
    Expected Values
    [1.0588235294117647, 1.0, 0, 0.0, 0.0, 0]



![png](output_78_17.png)


    ---------------------------------------------------------------
    Ricky Rubio
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.2692307692307692, 0.3548387096774194, 0.3333333333333333, 0.2777777777777778, 0.21739130434782608]
    ---------------------
    Expected Values
    [0, 0.8076923076923077, 1.064516129032258, 1.0, 0.8333333333333334, 0.6521739130434783]
    ---- 2 Pointers ----
    Percentages
    [0.44660194174757284, 0.42857142857142855, 0.75, 0.36666666666666664, 0.5333333333333333, 0.47619047619047616]
    ---------------------
    Expected Values
    [0.8932038834951457, 0.8571428571428571, 1.5, 0.7333333333333333, 1.0666666666666667, 0.9523809523809523]



![png](output_78_19.png)


    ---------------------------------------------------------------
    Rodney Hood
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.46153846153846156, 0.35714285714285715, 0.40540540540540543, 0.2, 0.32142857142857145]
    ---------------------
    Expected Values
    [0, 1.3846153846153846, 1.0714285714285714, 1.2162162162162162, 0.6000000000000001, 0.9642857142857144]
    ---- 2 Pointers ----
    Percentages
    [0.46, 0.25, 1.0, 0.4444444444444444, 0.21739130434782608, 0.37037037037037035]
    ---------------------
    Expected Values
    [0.92, 0.5, 2.0, 0.8888888888888888, 0.43478260869565216, 0.7407407407407407]



![png](output_78_21.png)


    ---------------------------------------------------------------
    Royce O
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.2, 0.36363636363636365, 0.6666666666666666, 0.25, 0.3333333333333333]
    ---------------------
    Expected Values
    [0, 0.6000000000000001, 1.0909090909090908, 2.0, 0.75, 1.0]
    ---- 2 Pointers ----
    Percentages
    [0.42028985507246375, 0, 0, 0.6, 0.6, 0.5]
    ---------------------
    Expected Values
    [0.8405797101449275, 0, 0, 1.2, 1.2, 1.0]



![png](output_78_23.png)


    ---------------------------------------------------------------
    Rudy Gobert
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0, 0, 0, 0, 0]
    ---------------------
    Expected Values
    [0, 0, 0, 0, 0, 0]
    ---- 2 Pointers ----
    Percentages
    [0.706140350877193, 0, 0, 0.5, 0, 0.25]
    ---------------------
    Expected Values
    [1.412280701754386, 0, 0, 1.0, 0, 0.5]



![png](output_78_25.png)


    ---------------------------------------------------------------
    Thabo Sefolosha
    ---------------------
    ---- 3 Pointers ----
    Percentages
    [0, 0.4444444444444444, 0.35714285714285715, 0.2222222222222222, 0.2727272727272727, 0.0]
    ---------------------
    Expected Values
    [0, 1.3333333333333333, 1.0714285714285714, 0.6666666666666666, 0.8181818181818181, 0.0]
    ---- 2 Pointers ----
    Percentages
    [0.6031746031746031, 0.0, 0.5, 0.3333333333333333, 0.25, 0.6666666666666666]
    ---------------------
    Expected Values
    [1.2063492063492063, 0.0, 1.0, 0.6666666666666666, 0.5, 1.3333333333333333]



![png](output_78_27.png)


** Conclusions**

From this, we can see that some players shoot different shots at much different expected valeus based on whether they are home or away. This could come from that players maybe have more nerves at away games and shoot worse altogher, or maybe they are more comfortable with certain courts and stadiums. In the comparison below, we can see that Ricky Rubio is a much better 3-point shooter at home. But interestingly enough, he shoots that baseline jumper 3x better away than at home. It is intersting to think players can shoot better or worse just depending on whether it is a home or an away game





![Ricky Rubio](RickyComparison.jpeg)

### Score Prediction 

Finally, we were able to look at predicting a game based on the knowledge of the shot attempts to guess the team score and individual scores. We actually did quite well by looking at the boxscore listed below. This was the first game of the season. We over predicted Donavan Mitchel's score, probbly as this was his first game in the NBA, and he may have shot worse due to nerves and getting used to the flow. Alec Burks was playing better at the time,  so he actually did better than we predicted. We were able to decently predict a score based upon the expected values we found. However, our methods don't take into account free throws, so our final scores will be a little off depending on free throws.

We were able to be within 7 points for each player. The model predicted Joe Johnson's score perfectly while we were 7 off of Alec Burks actual total. We were 8 short of the team total.


```python
GameOfInterest = 1
GameX = ShotsPD[ShotsPD['game'] == GameOfInterest]
```


```python
# Predicting a game
# 3 pointers
PlayerIDs = np.unique(GameX['shooter'])
PlayerNames = np.unique(GameX['shooter_name'])
NumOPlayers = len(PlayerNames)
TotalPts = []
PlayerToalPts = []
PlayerToalPtsActual = []
TotalPtsActual = []
TotalTotalPts = []
for Name in range(0,NumOPlayers):
    PercMadeDif3 = []
    NumShots = []
    NumMade = []
    ExpectedValue3 =[]
    PtVal3 = 3
    PtVal2 = 2
    PercMadeDif2 = []
    NumShots3 = []
    NumShots2 = []
    NumMade3 = []
    NumMade2 = []
    ExpectedValue2 =[]
    ExpectedValueRound = []

    
    
    NumShotsRecreate3 = []
    NumShotsRecreate2 = []
    LocationRecreate2 = []
    LocationRecreate3 = []
    ExpectedPoints3Recreate = []
    ExpectedPoints2Recreate= []
    PlayerToalPts = []
    
    
    NumShotsRecreate3Actual = []
    NumShotsRecreate2Actual = []
    LocationRecreate2Actual = []
    LocationRecreate3Actual = []
    ExpectedPoints3RecreateActual = []
    ExpectedPoints2RecreateActual= []
    PlayerToalPts = []
     
    
    for i in range(0,6):
        Location = ShotsPD[(ShotsPD['LocationCluster']==i) & (ShotsPD['ThreePt']==1) 
                           & ( ShotsPD['shooter_name'] == PlayerNames[Name])]     
        NumShots3.append(len(Location['made/missed']))
        NumMade3.append(len(Location[Location['made/missed']==1]))
        PercMade3 = 0
        if NumShots3[i] > 1:
            PercMade3 = NumMade3[i] / NumShots3[i]
            PercMadeDif3.append(PercMade3)
        else:
            PercMadeDif3.append(0)
    
        ExpectedValue3.append(PercMadeDif3[i]*PtVal3)
    
        AvLeft3.append(np.mean(Location['left']))
        AvTop3.append(np.mean(Location['top']))
        
        
        
    
        
        Location = ShotsPD[(ShotsPD['LocationCluster']==i) & (ShotsPD['ThreePt']==0) 
                           & ( ShotsPD['shooter_name'] == PlayerNames[Name])]
        NumShots2.append(len(Location['made/missed']))
        NumMade2.append(len(Location[Location['made/missed']==1]))
        PercMade2 = 0
        if NumShots2[i] > 1:
                PercMade2 = NumMade2[i] / NumShots2[i]
                PercMadeDif2.append(PercMade2)
        else:
                PercMadeDif2.append(0)
        ExpectedValue2.append(PercMadeDif2[i]*PtVal2)
    
        AvLeft2.append(np.mean(Location['left']))
        AvTop2.append(np.mean(Location['top']))
        

              
    
    
                         
                         
                         
                         
    
    

    
    
    for i in range(0,6):
        LocationRecreate3 = GameX[(GameX['LocationCluster']==i)& (GameX['ThreePt']==1) 
                                  & ( GameX['shooter_name'] == PlayerNames[Name])] 
        NumShotsRecreate3.append(len(LocationRecreate3['made/missed']))
        ExpectedPoints3Recreate.append(NumShotsRecreate3[i]*ExpectedValue3[i])
        
    for i in range(0,6):
        LocationRecreate2 = GameX[(GameX['LocationCluster']==i)& (GameX['ThreePt']==0) 
                                  & ( GameX['shooter_name'] == PlayerNames[Name])] 
        NumShotsRecreate2.append(len(LocationRecreate2['made/missed']))
        ExpectedPoints2Recreate.append(NumShotsRecreate2[i]*ExpectedValue2[i])
       
    
    Player3pts = np.sum(ExpectedPoints3Recreate)
    Player2pts = np.sum(ExpectedPoints2Recreate)
    
    PlayerToalPts = Player3pts + Player2pts
    
    TotalPts.append(int(PlayerToalPts))
    
    
    
    
    ## Actual Results of the Game
    
    for i in range(0,6):
        LocationRecreate3Actual = GameX[(GameX['LocationCluster']==i)& (GameX['ThreePt']==1) 
                                        & ( GameX['shooter_name'] == PlayerNames[Name]) 
                                        & ( GameX['made/missed'] == 1)] 
        NumShotsRecreate3Actual.append(len(LocationRecreate3Actual['made/missed']))
        ExpectedPoints3RecreateActual.append(NumShotsRecreate3Actual[i]*3)
        
    for i in range(0,6):
        LocationRecreate2Actual = GameX[(GameX['LocationCluster']==i)& (GameX['ThreePt']==0) 
                                        & ( GameX['shooter_name'] == PlayerNames[Name]) 
                                        & ( GameX['made/missed'] == 1)] 
        NumShotsRecreate2Actual.append(len(LocationRecreate2Actual['made/missed']))
        ExpectedPoints2RecreateActual.append(NumShotsRecreate2Actual[i]*2)
    
    
    Player3ptsActual = np.sum(ExpectedPoints3RecreateActual)
    Player2ptsActual = np.sum(ExpectedPoints2RecreateActual)
    
    PlayerToalPtsActual = Player3ptsActual + Player2ptsActual
    
    TotalPtsActual.append(int(PlayerToalPtsActual))
    
    
    
    print('---------------------------------------------------------------')
    print(PlayerNames[Name])
    print('Predicted: ' + str(int(PlayerToalPts)))
    print('Actual: ' + str(PlayerToalPtsActual))
    
    
    
TotalTotalPts = np.sum(TotalPts)
TotalTotalPtsActual = np.sum(TotalPtsActual)
print('---------------------------------------------------------------')
print('---------------------------------------------------------------')
print('Final Jazz Score:')
print('Predicted: ' + str(TotalTotalPts))
print('Actual: ' + str(TotalTotalPtsActual))

    
```

    ---------------------------------------------------------------
    Alec Burks
    Predicted: 9
    Actual: 16
    ---------------------------------------------------------------
    Derrick Favors
    Predicted: 14
    Actual: 14
    ---------------------------------------------------------------
    Donovan Mitchell
    Predicted: 12
    Actual: 6
    ---------------------------------------------------------------
    Ekpe Udoh
    Predicted: 0
    Actual: 0
    ---------------------------------------------------------------
    Joe Ingles
    Predicted: 7
    Actual: 11
    ---------------------------------------------------------------
    Joe Johnson
    Predicted: 10
    Actual: 10
    ---------------------------------------------------------------
    Ricky Rubio
    Predicted: 8
    Actual: 7
    ---------------------------------------------------------------
    Rodney Hood
    Predicted: 4
    Actual: 6
    ---------------------------------------------------------------
    Rudy Gobert
    Predicted: 11
    Actual: 14
    ---------------------------------------------------------------
    Thabo Sefolosha
    Predicted: 7
    Actual: 6
    ---------------------------------------------------------------
    ---------------------------------------------------------------
    Final Jazz Score:
    Predicted: 82
    Actual: 90


![title](JazzFirstGame.jpeg)

## Conclusion:

We successfully were able to:
- Acquire the data needed from ESPN
- Clean the data to extract the needed values for analysis
- Cluster the shots into natural groupings
- Look at expected values for both the team and individuals
- Run significance testing on the data
- Compare home and away games
- Predict a Jazz game outcome using a create model

### Ideas for future study:

 - Effects on fatigue and overshooting in locations
 
 - Correlation between shot selection and winning
 
 - Compare losing streak with winning streak
 
 - Predict fouls and foul shots
 
 - Evolution of Donovan Mitchell over the season
