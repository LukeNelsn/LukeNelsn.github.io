---
layout: post
title: Wines of the World
subtitle: Where do the world's best wines come from?
author: Luke
categories: projects
banner:
  video: https://assets.mixkit.co/videos/preview/mixkit-aerial-footage-of-a-vineyard-8197-large.mp4
  loop: true
  volume: 0
  start_at: 8.5
  image: ""
  opacity: 0.618
  background: "#161c1e"
  height: "100vh"
  min_height: "38vh"
  heading_style: "font-size: 4.25em; font-weight: bold"
  subheading_style: "color: white"
tags: projects python jupyter wine
sidebar: []
---

Here, we will be analyzing the review data from over 150,000 wine reviews. Originating from ([WineEnthisiast][WineEnthisiast] ↗), and found on ([Kaggle.com][kaggle] ↗). All reviews originate from each of the 6 populated continents, at least those where wine could be produced! Each wine was rated on a scale of 1-100 by 19 different reviewers, only those with a rating greater than 80 are included here.

The data is categorized by wine variety, location, winery, price, and description. We will be primarily interested in the wine **variety** and its **location**.

I highly reccomend partitioning this analysis within a Jupyter notebook, processing all the data as one combined python file would be more cumbersome than needed. 

This analysis only requires the additional `winemag-data_first150k.csv` file to be stored in the same directory as your `winereviews.ipynb` (Jupyter) file, unless you want to reference the aforementioned file another way. 

> Each of these files can also be downloaded from my GitHub account ([here][github-winereviews] ↗)

## Wines of the World

Required modules: `numpy` `pandas` `matplotlib` `pycountry_convert`

pycountry_convert can be found ([here][pycountry_convert] ↗)

#### Preliminary importing and organization

Let's also import continent codes, country names, and establish a dictionary of continents

Related to our pandas dataframe, we will allow pandas to output all columns

```python
# import packages
import numpy as np
import pandas as pd
from matplotlib import pyplot as plt
from matplotlib import figure as fig
import pycountry_convert as pc
from pycountry_convert import country_alpha2_to_continent_code, country_name_to_country_alpha2

# establish continents naming dictionary
continents = {
    'NA': 'North America',
    'SA': 'South America', 
    'AS': 'Asia',
    'OC': 'Australia',
    'AF': 'Africa',
    'EU': 'Europe'
}

# set pandas to show us all columns
pd.set_option('display.max_columns', None)
pd.set_option("expand_frame_repr", False)

# importing the 150k CSV file
wine = pd.read_csv('winesfolder\winemag-data_first150k.csv')
wine = pd.DataFrame(wine)
wine.head()
```

#### Partition the data by continent

Here, we formalize each country name, remove null values, and segment the data by continent

> Note: we are defaulting each wine from the region "US-France" to the country of France


```python
# formalize each country name with lambda function --'Bosnia and Herzegovina':'Bosnia'
countryFixer = lambda name: {'US':'United States', 'England':'United Kingdom','US-France':'France'}.get(name,name)
wine['Country'] = wine.country.apply(countryFixer)

# remove nan values (5 found)
np.where(pd.isnull(wine.Country))
noNan_wine = wine.drop([wine.index[1133], wine.index[1440], wine.index[68226], wine.index[113016], wine.index[135696]])

# create full list of each continent
continents_list = []
continents = {
    'NA': 'North America',
    'SA': 'South America', 
    'AS': 'Asia',
    'OC': 'Australia',
    'AF': 'Africa',
    'EU': 'Europe'
}

for country_name in noNan_wine.Country:
    country_alpha2 = pc.country_name_to_country_alpha2(country_name)
    country_continent_code = pc.country_alpha2_to_continent_code(country_alpha2)
    country_continent_name = pc.convert_continent_code_to_continent_name(country_continent_code)
    continents_list.append(country_continent_name)

# add new column, assign it the values from the complete list of continents
noNan_wine['Continent'] = continents_list
noNan_wine.head()

# separate all wines into new dataframes by continents
NA_wines = noNan_wine[noNan_wine.Continent=='North America'].reset_index()
EU_wines = noNan_wine[noNan_wine.Continent=='Europe'].reset_index()
OC_wines = noNan_wine[noNan_wine.Continent=='Oceania'].reset_index()
SA_wines = noNan_wine[noNan_wine.Continent=='South America'].reset_index()
AS_wines = noNan_wine[noNan_wine.Continent=='Asia'].reset_index()
AF_wines = noNan_wine[noNan_wine.Continent=='Africa'].reset_index()
```

#### Let's see how many reviews originate from each continent

```python
print('NA:',len(NA_wines))
print('EU:',len(EU_wines))
print('OC:',len(OC_wines))
print('SA:',len(SA_wines))
print('AS:',len(AS_wines))
print('AF:',len(AF_wines))
```

#### Calculating average wine rating by continent

Here, we will calculate each continent's average review points then plot the data in a neat bar chart   

```python
# calculate each continent's average points
continent_avgs = (NA_wines.points.mean(), EU_wines.points.mean(), OC_wines.points.mean(), SA_wines.points.mean(), AS_wines.points.mean(), AF_wines.points.mean())

# calculate the dataset's average out of curiosity
cumulative = NA_wines.points.mean()+EU_wines.points.mean()+OC_wines.points.mean()+SA_wines.points.mean()+AS_wines.points.mean()+AF_wines.points.mean()
cumulative_avg = cumulative/6

# begin plotting the data, neatly organized
error = noNan_wine.groupby('Continent').points.sem()
ax = plt.subplot()
plot = plt.bar(range(len(continent_avgs)), continent_avgs, yerr = error, capsize = 5, color='firebrick')

# let's give the bar chart some nice labels and visual cues
plt.ylim(ymin=85, ymax=89)
ax.set_ylabel('Average Wine Points')
ax.set_title('Average Wine Points by Continent')
ax.set_xticks(range(len(continent_avgs)))
ax.set_xticklabels(noNan_wine.Continent.unique(), rotation=22.5)
#['North America', 'Europe', 'Oceania', 'South America', 'Asia', 'Africa'])

# let's plot each continent's average above its respective bar chart
for value in plot:
    height = value.get_height()
    plt.text(value.get_x() + value.get_width()/2.,
             1.002*height,'%d' % int(height), ha='center', va='bottom')

# saving the chart as a .jpeg file, and displaying the output below this jupyer code cell
plt.savefig(fname='contAverage.jpeg', dpi=300, bbox_inches='tight')
plt.show()
```

![contAverage](https://raw.githubusercontent.com/LukeNelsn/winereviews/main/contAverage.jpeg)

Unsuprisingly, Europe tends to receive high ratings for their wines. Let's drill down furhter, and find out where the best are coming from!

#### Drill down Europe 

We will loop over this many times as the user plays the game

```python
# biring in the Europe wines dataframe, and group the ratings by country
EU_wines
EU_Country_wines = EU_wines.groupby('Country').points.mean().reset_index()

# gather the top 5 (highly rated) countries, this can be changed as you see fit
EU_Country_wines_top5 = EU_Country_wines[EU_Country_wines['points'] > 88.3]

# begin plotting the top 5 data
ax = plt.subplot()
plot = plt.scatter(range(len(EU_Country_wines_top5)),EU_Country_wines_top5.points, color='firebrick')

# let's give the chart some nice labels and visual cues
plt.ylim(ymin=80, ymax=100)
ax.set_ylabel('Average Wine Points')
ax.set_title('Average Wine Points by Country')
ax.set_xticks(range(len(EU_Country_wines_top5)))
ax.set_xticklabels(EU_Country_wines_top5.Country.unique(), rotation=22.5)

# plotting each data point along the chart
y = list(EU_Country_wines_top5.points)
z = range(len(EU_Country_wines_top5))
n = list(EU_Country_wines_top5.points.round(decimals=2))

for i, txt in enumerate(n):
    ax.annotate(txt, (z[i]-.175, y[i]+.75))

# saving the chart as a .jpeg file, and displaying the output below this jupyer code cell
plt.savefig(fname='countAverages.jpeg', dpi=300, bbox_inches='tight')
plt.show()
```
![countAverages](https://raw.githubusercontent.com/LukeNelsn/winereviews/main/countAverages.jpeg)

Wow, it looks like the United Kingdom takes the cake for producing the highest-rated wines! How many reviews originate from each of these top 5 countries?

#### How many reviews come from each country?

```python
# partition each country into its own dataframe
AUT_wines = noNan_wine[noNan_wine.Country == 'Austria']
FRA_wines = noNan_wine[noNan_wine.Country == 'France']
GER_wines = noNan_wine[noNan_wine.Country == 'Germany']
ITY_wines = noNan_wine[noNan_wine.Country == 'Italy']
UK_wines = noNan_wine[noNan_wine.Country == 'United Kingdom']

# print the number of reviews from each country
print(len(AUT_wines),len(FRA_wines),len(GER_wines),len(ITY_wines),len(UK_wines))
```
Looks like the United Kingdom only represents **9** reviews out of 150,000+ total reviews. This fails to provide a significant basis to determine which nation produces the best wines. 

## Conclusion

In total, great wines originate from around the world, and your tates will ultimately guide your purchasing decisions. However, based on the reviews and data compiled here, it appears you won't go wrong purchasing wine from Europe, and more specifically, Austria :austria:!

Thank you for taking the time to check out this wine review analysis, all built in python, and best organized in a jupyter, all processed on your machine!

If you're interested in more, checkout my other projects on this site or visit my GitHub account ([here][github-account] ↗)

[WineEnthisiast]: http://www.winemag.com/?s=&drink_type=wine
[kaggle]: https://www.kaggle.com/datasets/zynicide/wine-reviews
[github-winereviews]: https://github.com/LukeNelsn/winereviews
[pycountry_convert]: https://pypi.org/project/pycountry-convert/
[github-account]: https://github.com/LukeNelsn/