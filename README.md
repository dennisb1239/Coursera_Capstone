# IBM Data Science - Capstone project 
## "WHERE TO MOVE DUE TO BREXIT?"

This project was created to finish the IBM Data Science spezialization offered on Coursera. 

### BUSINESS PROBLEM
This project deals with the question to which EU city banks should move their HQ to, if the current location of their HQ is in the UK (more specifically London as it is the financial centre of the UK). The reason why banks are asking themselves this question is linked to the upcoming Brexit, which means that banks have higher hurdles to overcome when wanting to be involved in the EU market. 
The target audience therefore is clearly consisting of decision-makers at the boards of these banks. 
The main question which will be tackled through this project is: "which candidate cities offer a similar environment compared to London?".

### DATA
In order to be able to answer this question, data was gathered mainly from the eurostat database, which offers publicly available data on various topics related to the EU [(https://ec.europa.eu/eurostat/data/database)](https://ec.europa.eu/eurostat/data/database). 

The following parameters were identified to be relevant for the project scope: 
- The percentage of country inhabitants with an education level of 5-8 acc. the International Standard Classification of Education (ISCED) (2018)
- The labor costs in the respective country [€] (2018)
- The average monthly rent in the candidate city [€/m^2] (2018)
- Consumer prices acc. to the Harmonized Index of Consumer Prices (HICP) (2018)
- Crime rate / 100'000 inhabitants for the respective country 
- The number of universities in the city area
- The difference in time zones compared to London
- The life satisfaction index (2018) for the respective country
- The number of active companies for the respective country (2017)

The following candidate cities were evaluated as potential new locations for banks, all of them being locations of stock exchanges: 
- Dublin, Ireland
- Frankfurt, Germany
- Paris, France
- Warsaw, Poland
- Madrid, Spain
- Lisbon, Portugal
- Stockholm, Sweden
- Helsinki, Finland
- Milan, Italy
- Brussels, Belgium
- Amsterdam, Netherlands
- Copenhagen, Denmark
- Vienna, Austria
- Prague, Czech Republic

First, we need to import all relevant libraries. 

```
# import all relevant libraries
import pandas as pd
import numpy as np
from sklearn import preprocessing
from sklearn.cluster import KMeans
from sklearn.cluster import DBSCAN
import json

!conda install -c conda-forge geopy --yes
# convert an address into latitude and longitude values
from geopy.geocoders import Nominatim 

import requests 
from pandas.io.json import json_normalize

import matplotlib.cm as cm
import matplotlib.colors as colors

!conda install -c conda-forge folium=0.5.0 --yes
import folium 

print('Libraries imported.')
```

Then we get the data file prepared for the analysis. To do so, we clone my github repo

```
# Clone the entire repo.
!git clone -l -s git://github.com/dennisb1239/Coursera_Capstone.git cloned-repo
%cd cloned-repo
!ls
```

### Methodology

In this project we will try to find an alternative city, to which banks with their current HQ in London / UK can move to, in order to avoid negative consequences due to the upcoming Brexit. To do so, we followed the steps below:

1. We first imported a prepared data set through the cloning of my git repository, and transformed it into a pandas DataFrame
2. We created a second DataFrame, and store the geographical coordinates of the evaluated cities in it
3. A first map is created, to visualize the geographical locations of the cities
4. 2 different clustering algorithms are tested on the first Dataframe: KMeans & DBScan
5. The results of the 2 cluster analyses are shown on separate maps, in order to make it easier to identify the alternative candidates

### Analysis

The data is saved in a pandas DataFrame and we check the dataframe
```
df = pd.DataFrame()
df = pd.read_excel("Data_C9W5_normalized.xlsx", index_col = 0)
```
Now let's have a look at the data types of the features in the dataframe
```
df.dtypes
```
In order to have a starting point for the visualization, we request the geographical coordinates of a center position in Europe. For this the city of Munich is choosen.
```
address_Muc = 'Munich, Germany'

geolocator = Nominatim(user_agent="to_explorer")
location = geolocator.geocode(address_Muc)
latitude_Muc = location.latitude
longitude_Muc = location.longitude
print('The geograpical coordinate of Munich are {}, {}.'.format(latitude_Muc, longitude_Muc))
```
_The geograpical coordinates of Munich are 48.1371079, 11.5753822._

Now we to get the get the geographical coordinates of the cities which are to be evaluated
`
cities = ['London, England', 'Dublin, Ireland', 'Frankfurt, Germany', 'Paris, France', 'Warsaw, Poland', 'Madrid, Spain', 'Lisbon, Portugal', 'Stockholm, Sweden', 'Helsinki, Finland', 'Milan, Italy', 'Brussels, Belgium', 'Amsterdam, Netherlands', 'Copenhagen, Denmark', 'Vienna, Austria', 'Prague, Czech Republic']
city_locs = pd.DataFrame(columns = ('city','latitude','longitude'))

for city in cities: 
    address = city
    location = geolocator.geocode(address)
    latitude = location.latitude
    longitude = location.longitude
    data = {'city':[city], 'latitude':[latitude], 'longitude':[longitude]}
    data = pd.DataFrame(data = data)
    city_locs = city_locs.append(data)
    
city_locs
`
