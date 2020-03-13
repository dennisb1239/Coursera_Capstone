# IBM Data Science - Capstone project 
## "WHERE TO MOVE DUE TO BREXIT?"

This project was created to finish the IBM Data Science spezialization offered on Coursera. 

### BUSINESS PROBLEM
This project deals with the question to which EU city banks should move their HQ to, if the current location of their HQ is in the UK (more specifically London as it is the financial centre of the UK). The reason why banks are asking themselves this question is linked to the upcoming Brexit, which means that banks have higher hurdles to overcome when wanting to be involved in the EU market. 
The target audience therefore is clearly consisting of decision-makers at the boards of these banks. 
The main question which will be tackled through this project is: "which candidate cities offer a similar environment compared to London?".

### DATA
In order to be able to answer this question, data was gathered mainly from the eurostat database, which offers publicly available data on various topics related to the EU [https://ec.europa.eu/eurostat/data/database](https://ec.europa.eu/eurostat/data/database). 

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


