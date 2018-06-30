
# WeatherPy

## Analysis

### Observed trend 1

There is a correlation between latitude and temperature. In the data set analyzed in June, the highest temperatures are found between 20 and 40 degrees latitude, the same latitude range where Sahara, Guadalajara and Gobi deserts are located. The Northern Hemisphere has more data points, which is expected since the northern hemisphere also contains greater land-mass than the Southern hemisphere.

### Observed trend 2

There appears to be no correlation between latitude and cloudiness, and only weak correlation between latitude and wind speed. There are higher wind speeds observed in the southern hemisphere. 

### Observed trend 3

The correlation between latitude and humidity is also not pronounced. The humidity is higher in the northern hemisphere, which is to be expected as it is Summer in the northern hemisphere. 


```python
# Dependencies
import datetime
import time
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import json
from citipy import citipy
# Open Weather Map API key
#from config import api_key

api_key = "02081d218acbea82be600aa7990a6b22"
```

# Generate Cities List


```python
# Get random longitude
lon = np.random.uniform (low=-1.8, high=1.8, size=(2000)) * 100

# Get random Latitude
lat = np.random.uniform (low=-0.9, high=0.9, size=(2000)) * 100

# Combine random latitude and longitude coordinates
coordinates = np.stack((lat, lon), axis=-1)

```


```python
# Create cities data frame
cities_raw = []
for coordinate_pair in coordinates:
    lat, lon = coordinate_pair
    cities_raw.append(citipy.nearest_city(lat, lon))
    
dup_items = set()
cities = []
for x in cities_raw:
    if x not in dup_items:
        cities.append(x)
        dup_items.add(x)
    
print(len(cities))
```

    753


# Perform API Calls


```python
# Save config information
url = "http://api.openweathermap.org/data/2.5/weather?"
units = "Imperial"

# set up lists to hold reponse info
ow_date = []
ow_city = []
ow_country = []
ow_cloud = []
ow_humid = []
ow_lat = []
ow_lon = []
ow_maxtmp = []
ow_wind = []

x = 1
    
# Build query URL
for city in cities:
    
   
    name = city.city_name
    print("Retreiving data for City #"+ str(x) + " of " + str((len(cities))) + " ... "  + name )
    
    query_url = url + "appid=" + api_key + "&q=" + name + "&units=" + units
    print(query_url)
    print(50 * "-")
    
    # Get weather data
    weather_response = requests.get(query_url)
    weather_json = weather_response.json()
    
   
    # Catch wrong city name exception
    try:
   
        ow_date.append(weather_json['dt'])
        ow_city.append(weather_json['name'])
        ow_country.append(weather_json['sys']['country'])
        ow_lat.append(weather_json['coord']['lat'])
        ow_lon.append(weather_json['coord']['lon'])
        ow_maxtmp.append(weather_json['main']['temp_max'])
        ow_humid.append(weather_json['main']['humidity'])
        ow_cloud.append(weather_json['clouds']['all'])
        ow_wind.append(weather_json['wind']['speed'])
        
        x = x + 1
            
    except:
             print("Oops! That was a wrong city name. Try again...")
             print(50 * "-")
            
   
    # Timer to pause for a second after each record - not too exceed 60 API calls per minute limit
    time.sleep(1)

```

    Retreiving data for City #1 of 753 ... bushehr
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bushehr&units=Imperial
    --------------------------------------------------
    Retreiving data for City #2 of 753 ... tambura
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tambura&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #2 of 753 ... avarua
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=avarua&units=Imperial
    --------------------------------------------------
    Retreiving data for City #3 of 753 ... castro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=castro&units=Imperial
    --------------------------------------------------
    Retreiving data for City #4 of 753 ... guanica
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=guanica&units=Imperial
    --------------------------------------------------
    Retreiving data for City #5 of 753 ... boddam
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=boddam&units=Imperial
    --------------------------------------------------
    Retreiving data for City #6 of 753 ... atuona
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=atuona&units=Imperial
    --------------------------------------------------
    Retreiving data for City #7 of 753 ... albany
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=albany&units=Imperial
    --------------------------------------------------
    Retreiving data for City #8 of 753 ... saint-philippe
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saint-philippe&units=Imperial
    --------------------------------------------------
    Retreiving data for City #9 of 753 ... artesia
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=artesia&units=Imperial
    --------------------------------------------------
    Retreiving data for City #10 of 753 ... portland
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=portland&units=Imperial
    --------------------------------------------------
    Retreiving data for City #11 of 753 ... alyangula
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=alyangula&units=Imperial
    --------------------------------------------------
    Retreiving data for City #12 of 753 ... nadvoitsy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nadvoitsy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #13 of 753 ... hasaki
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hasaki&units=Imperial
    --------------------------------------------------
    Retreiving data for City #14 of 753 ... port elizabeth
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=port elizabeth&units=Imperial
    --------------------------------------------------
    Retreiving data for City #15 of 753 ... gubkinskiy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=gubkinskiy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #16 of 753 ... dalby
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dalby&units=Imperial
    --------------------------------------------------
    Retreiving data for City #17 of 753 ... punta arenas
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=punta arenas&units=Imperial
    --------------------------------------------------
    Retreiving data for City #18 of 753 ... busselton
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=busselton&units=Imperial
    --------------------------------------------------
    Retreiving data for City #19 of 753 ... vaini
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vaini&units=Imperial
    --------------------------------------------------
    Retreiving data for City #20 of 753 ... aklavik
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=aklavik&units=Imperial
    --------------------------------------------------
    Retreiving data for City #21 of 753 ... hobart
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hobart&units=Imperial
    --------------------------------------------------
    Retreiving data for City #22 of 753 ... ulladulla
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ulladulla&units=Imperial
    --------------------------------------------------
    Retreiving data for City #23 of 753 ... ushuaia
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ushuaia&units=Imperial
    --------------------------------------------------
    Retreiving data for City #24 of 753 ... tuatapere
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tuatapere&units=Imperial
    --------------------------------------------------
    Retreiving data for City #25 of 753 ... saint-joseph
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saint-joseph&units=Imperial
    --------------------------------------------------
    Retreiving data for City #26 of 753 ... jamestown
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=jamestown&units=Imperial
    --------------------------------------------------
    Retreiving data for City #27 of 753 ... kalabo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kalabo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #28 of 753 ... nikolskoye
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nikolskoye&units=Imperial
    --------------------------------------------------
    Retreiving data for City #29 of 753 ... mataura
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mataura&units=Imperial
    --------------------------------------------------
    Retreiving data for City #30 of 753 ... hobyo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hobyo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #31 of 753 ... bethel
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bethel&units=Imperial
    --------------------------------------------------
    Retreiving data for City #32 of 753 ... east london
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=east london&units=Imperial
    --------------------------------------------------
    Retreiving data for City #33 of 753 ... tortoli
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tortoli&units=Imperial
    --------------------------------------------------
    Retreiving data for City #34 of 753 ... matagami
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=matagami&units=Imperial
    --------------------------------------------------
    Retreiving data for City #35 of 753 ... rikitea
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rikitea&units=Imperial
    --------------------------------------------------
    Retreiving data for City #36 of 753 ... ailigandi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ailigandi&units=Imperial
    --------------------------------------------------
    Retreiving data for City #37 of 753 ... kapaa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kapaa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #38 of 753 ... grindavik
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=grindavik&units=Imperial
    --------------------------------------------------
    Retreiving data for City #39 of 753 ... iqaluit
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=iqaluit&units=Imperial
    --------------------------------------------------
    Retreiving data for City #40 of 753 ... puerto ayora
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=puerto ayora&units=Imperial
    --------------------------------------------------
    Retreiving data for City #41 of 753 ... suleja
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=suleja&units=Imperial
    --------------------------------------------------
    Retreiving data for City #42 of 753 ... provideniya
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=provideniya&units=Imperial
    --------------------------------------------------
    Retreiving data for City #43 of 753 ... bluff
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bluff&units=Imperial
    --------------------------------------------------
    Retreiving data for City #44 of 753 ... ilo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ilo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #45 of 753 ... tubuala
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tubuala&units=Imperial
    --------------------------------------------------
    Retreiving data for City #46 of 753 ... samusu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=samusu&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #46 of 753 ... cayenne
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cayenne&units=Imperial
    --------------------------------------------------
    Retreiving data for City #47 of 753 ... victoria
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=victoria&units=Imperial
    --------------------------------------------------
    Retreiving data for City #48 of 753 ... shimoda
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=shimoda&units=Imperial
    --------------------------------------------------
    Retreiving data for City #49 of 753 ... bengkulu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bengkulu&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #49 of 753 ... barrow
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=barrow&units=Imperial
    --------------------------------------------------
    Retreiving data for City #50 of 753 ... chernukha
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chernukha&units=Imperial
    --------------------------------------------------
    Retreiving data for City #51 of 753 ... san cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=san cristobal&units=Imperial
    --------------------------------------------------
    Retreiving data for City #52 of 753 ... georgetown
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=georgetown&units=Imperial
    --------------------------------------------------
    Retreiving data for City #53 of 753 ... juba
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=juba&units=Imperial
    --------------------------------------------------
    Retreiving data for City #54 of 753 ... ribeira grande
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ribeira grande&units=Imperial
    --------------------------------------------------
    Retreiving data for City #55 of 753 ... upernavik
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=upernavik&units=Imperial
    --------------------------------------------------
    Retreiving data for City #56 of 753 ... rawannawi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rawannawi&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #56 of 753 ... diego de almagro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=diego de almagro&units=Imperial
    --------------------------------------------------
    Retreiving data for City #57 of 753 ... ferrol
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ferrol&units=Imperial
    --------------------------------------------------
    Retreiving data for City #58 of 753 ... berlevag
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=berlevag&units=Imperial
    --------------------------------------------------
    Retreiving data for City #59 of 753 ... hithadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hithadhoo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #60 of 753 ... taolanaro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=taolanaro&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #60 of 753 ... fort nelson
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=fort nelson&units=Imperial
    --------------------------------------------------
    Retreiving data for City #61 of 753 ... jember
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=jember&units=Imperial
    --------------------------------------------------
    Retreiving data for City #62 of 753 ... nanortalik
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nanortalik&units=Imperial
    --------------------------------------------------
    Retreiving data for City #63 of 753 ... hit
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hit&units=Imperial
    --------------------------------------------------
    Retreiving data for City #64 of 753 ... simbahan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=simbahan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #65 of 753 ... kota kinabalu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kota kinabalu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #66 of 753 ... port hedland
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=port hedland&units=Imperial
    --------------------------------------------------
    Retreiving data for City #67 of 753 ... bam
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bam&units=Imperial
    --------------------------------------------------
    Retreiving data for City #68 of 753 ... mount darwin
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mount darwin&units=Imperial
    --------------------------------------------------
    Retreiving data for City #69 of 753 ... khatanga
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=khatanga&units=Imperial
    --------------------------------------------------
    Retreiving data for City #70 of 753 ... illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=illoqqortoormiut&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #70 of 753 ... cherskiy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cherskiy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #71 of 753 ... westport
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=westport&units=Imperial
    --------------------------------------------------
    Retreiving data for City #72 of 753 ... dongsheng
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dongsheng&units=Imperial
    --------------------------------------------------
    Retreiving data for City #73 of 753 ... bairiki
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bairiki&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #73 of 753 ... barentsburg
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=barentsburg&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #73 of 753 ... kysyl-syr
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kysyl-syr&units=Imperial
    --------------------------------------------------
    Retreiving data for City #74 of 753 ... sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sentyabrskiy&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #74 of 753 ... carnarvon
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=carnarvon&units=Imperial
    --------------------------------------------------
    Retreiving data for City #75 of 753 ... cervo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cervo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #76 of 753 ... tasiilaq
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tasiilaq&units=Imperial
    --------------------------------------------------
    Retreiving data for City #77 of 753 ... bargal
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bargal&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #77 of 753 ... hilo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hilo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #78 of 753 ... urdzhar
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=urdzhar&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #78 of 753 ... saldanha
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saldanha&units=Imperial
    --------------------------------------------------
    Retreiving data for City #79 of 753 ... karratha
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=karratha&units=Imperial
    --------------------------------------------------
    Retreiving data for City #80 of 753 ... naze
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=naze&units=Imperial
    --------------------------------------------------
    Retreiving data for City #81 of 753 ... dwarka
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dwarka&units=Imperial
    --------------------------------------------------
    Retreiving data for City #82 of 753 ... souillac
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=souillac&units=Imperial
    --------------------------------------------------
    Retreiving data for City #83 of 753 ... arlit
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=arlit&units=Imperial
    --------------------------------------------------
    Retreiving data for City #84 of 753 ... saint george
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saint george&units=Imperial
    --------------------------------------------------
    Retreiving data for City #85 of 753 ... bredasdorp
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bredasdorp&units=Imperial
    --------------------------------------------------
    Retreiving data for City #86 of 753 ... new norfolk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=new norfolk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #87 of 753 ... ponta do sol
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ponta do sol&units=Imperial
    --------------------------------------------------
    Retreiving data for City #88 of 753 ... tsihombe
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tsihombe&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #88 of 753 ... pyay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pyay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #89 of 753 ... katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=katsuura&units=Imperial
    --------------------------------------------------
    Retreiving data for City #90 of 753 ... tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tuktoyaktuk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #91 of 753 ... lasa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=lasa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #92 of 753 ... lagoa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=lagoa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #93 of 753 ... anzio
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=anzio&units=Imperial
    --------------------------------------------------
    Retreiving data for City #94 of 753 ... chake chake
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chake chake&units=Imperial
    --------------------------------------------------
    Retreiving data for City #95 of 753 ... butaritari
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=butaritari&units=Imperial
    --------------------------------------------------
    Retreiving data for City #96 of 753 ... padang
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=padang&units=Imperial
    --------------------------------------------------
    Retreiving data for City #97 of 753 ... makakilo city
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=makakilo city&units=Imperial
    --------------------------------------------------
    Retreiving data for City #98 of 753 ... arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=arraial do cabo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #99 of 753 ... belushya guba
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=belushya guba&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #99 of 753 ... half moon bay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=half moon bay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #100 of 753 ... kuche
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kuche&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #100 of 753 ... amderma
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=amderma&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #100 of 753 ... jeremoabo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=jeremoabo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #101 of 753 ... hualmay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hualmay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #102 of 753 ... harper
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=harper&units=Imperial
    --------------------------------------------------
    Retreiving data for City #103 of 753 ... tsabong
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tsabong&units=Imperial
    --------------------------------------------------
    Retreiving data for City #104 of 753 ... ngunguru
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ngunguru&units=Imperial
    --------------------------------------------------
    Retreiving data for City #105 of 753 ... shakiso
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=shakiso&units=Imperial
    --------------------------------------------------
    Retreiving data for City #106 of 753 ... bangkalan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bangkalan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #107 of 753 ... camacha
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=camacha&units=Imperial
    --------------------------------------------------
    Retreiving data for City #108 of 753 ... play cu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=play cu&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #108 of 753 ... sao filipe
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sao filipe&units=Imperial
    --------------------------------------------------
    Retreiving data for City #109 of 753 ... baghmara
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=baghmara&units=Imperial
    --------------------------------------------------
    Retreiving data for City #110 of 753 ... te anau
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=te anau&units=Imperial
    --------------------------------------------------
    Retreiving data for City #111 of 753 ... yellowknife
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yellowknife&units=Imperial
    --------------------------------------------------
    Retreiving data for City #112 of 753 ... hofn
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hofn&units=Imperial
    --------------------------------------------------
    Retreiving data for City #113 of 753 ... kroya
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kroya&units=Imperial
    --------------------------------------------------
    Retreiving data for City #114 of 753 ... leningradskiy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=leningradskiy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #115 of 753 ... coahuayana
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=coahuayana&units=Imperial
    --------------------------------------------------
    Retreiving data for City #116 of 753 ... chagda
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chagda&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #116 of 753 ... severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=severo-kurilsk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #117 of 753 ... aguililla
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=aguililla&units=Imperial
    --------------------------------------------------
    Retreiving data for City #118 of 753 ... port moresby
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=port moresby&units=Imperial
    --------------------------------------------------
    Retreiving data for City #119 of 753 ... deputatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=deputatskiy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #120 of 753 ... west bay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=west bay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #121 of 753 ... saskylakh
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saskylakh&units=Imperial
    --------------------------------------------------
    Retreiving data for City #122 of 753 ... mar del plata
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mar del plata&units=Imperial
    --------------------------------------------------
    Retreiving data for City #123 of 753 ... ishigaki
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ishigaki&units=Imperial
    --------------------------------------------------
    Retreiving data for City #124 of 753 ... hermanus
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hermanus&units=Imperial
    --------------------------------------------------
    Retreiving data for City #125 of 753 ... pemangkat
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pemangkat&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #125 of 753 ... emirdag
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=emirdag&units=Imperial
    --------------------------------------------------
    Retreiving data for City #126 of 753 ... dori
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dori&units=Imperial
    --------------------------------------------------
    Retreiving data for City #127 of 753 ... tema
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tema&units=Imperial
    --------------------------------------------------
    Retreiving data for City #128 of 753 ... channel-port aux basques
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=channel-port aux basques&units=Imperial
    --------------------------------------------------
    Retreiving data for City #129 of 753 ... aflu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=aflu&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #129 of 753 ... san marcos
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=san marcos&units=Imperial
    --------------------------------------------------
    Retreiving data for City #130 of 753 ... namatanai
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=namatanai&units=Imperial
    --------------------------------------------------
    Retreiving data for City #131 of 753 ... kyra
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kyra&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #131 of 753 ... alvdal
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=alvdal&units=Imperial
    --------------------------------------------------
    Retreiving data for City #132 of 753 ... valparaiso
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=valparaiso&units=Imperial
    --------------------------------------------------
    Retreiving data for City #133 of 753 ... bonavista
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bonavista&units=Imperial
    --------------------------------------------------
    Retreiving data for City #134 of 753 ... vaitupu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vaitupu&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #134 of 753 ... saiha
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saiha&units=Imperial
    --------------------------------------------------
    Retreiving data for City #135 of 753 ... tamiahua
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tamiahua&units=Imperial
    --------------------------------------------------
    Retreiving data for City #136 of 753 ... port hardy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=port hardy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #137 of 753 ... sioux lookout
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sioux lookout&units=Imperial
    --------------------------------------------------
    Retreiving data for City #138 of 753 ... killybegs
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=killybegs&units=Imperial
    --------------------------------------------------
    Retreiving data for City #139 of 753 ... cidreira
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cidreira&units=Imperial
    --------------------------------------------------
    Retreiving data for City #140 of 753 ... scarborough
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=scarborough&units=Imperial
    --------------------------------------------------
    Retreiving data for City #141 of 753 ... ponta delgada
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ponta delgada&units=Imperial
    --------------------------------------------------
    Retreiving data for City #142 of 753 ... gwadar
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=gwadar&units=Imperial
    --------------------------------------------------
    Retreiving data for City #143 of 753 ... praia da vitoria
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=praia da vitoria&units=Imperial
    --------------------------------------------------
    Retreiving data for City #144 of 753 ... dikson
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dikson&units=Imperial
    --------------------------------------------------
    Retreiving data for City #145 of 753 ... thompson
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=thompson&units=Imperial
    --------------------------------------------------
    Retreiving data for City #146 of 753 ... constitucion
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=constitucion&units=Imperial
    --------------------------------------------------
    Retreiving data for City #147 of 753 ... airai
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=airai&units=Imperial
    --------------------------------------------------
    Retreiving data for City #148 of 753 ... zurrieq
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=zurrieq&units=Imperial
    --------------------------------------------------
    Retreiving data for City #149 of 753 ... vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vila franca do campo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #150 of 753 ... kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kudahuvadhoo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #151 of 753 ... shelburne
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=shelburne&units=Imperial
    --------------------------------------------------
    Retreiving data for City #152 of 753 ... bathsheba
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bathsheba&units=Imperial
    --------------------------------------------------
    Retreiving data for City #153 of 753 ... klaksvik
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=klaksvik&units=Imperial
    --------------------------------------------------
    Retreiving data for City #154 of 753 ... port alfred
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=port alfred&units=Imperial
    --------------------------------------------------
    Retreiving data for City #155 of 753 ... gouyave
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=gouyave&units=Imperial
    --------------------------------------------------
    Retreiving data for City #156 of 753 ... khani
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=khani&units=Imperial
    --------------------------------------------------
    Retreiving data for City #157 of 753 ... luderitz
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=luderitz&units=Imperial
    --------------------------------------------------
    Retreiving data for City #158 of 753 ... nantucket
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nantucket&units=Imperial
    --------------------------------------------------
    Retreiving data for City #159 of 753 ... cruzilia
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cruzilia&units=Imperial
    --------------------------------------------------
    Retreiving data for City #160 of 753 ... tiarei
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tiarei&units=Imperial
    --------------------------------------------------
    Retreiving data for City #161 of 753 ... viedma
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=viedma&units=Imperial
    --------------------------------------------------
    Retreiving data for City #162 of 753 ... kristiansund
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kristiansund&units=Imperial
    --------------------------------------------------
    Retreiving data for City #163 of 753 ... qaanaaq
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=qaanaaq&units=Imperial
    --------------------------------------------------
    Retreiving data for City #164 of 753 ... keti bandar
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=keti bandar&units=Imperial
    --------------------------------------------------
    Retreiving data for City #165 of 753 ... hirara
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hirara&units=Imperial
    --------------------------------------------------
    Retreiving data for City #166 of 753 ... norman wells
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=norman wells&units=Imperial
    --------------------------------------------------
    Retreiving data for City #167 of 753 ... vyshhorod
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vyshhorod&units=Imperial
    --------------------------------------------------
    Retreiving data for City #168 of 753 ... van buren
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=van buren&units=Imperial
    --------------------------------------------------
    Retreiving data for City #169 of 753 ... mys shmidta
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mys shmidta&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #169 of 753 ... luanda
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=luanda&units=Imperial
    --------------------------------------------------
    Retreiving data for City #170 of 753 ... mapiripan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mapiripan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #171 of 753 ... alampur
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=alampur&units=Imperial
    --------------------------------------------------
    Retreiving data for City #172 of 753 ... tiksi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tiksi&units=Imperial
    --------------------------------------------------
    Retreiving data for City #173 of 753 ... callaway
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=callaway&units=Imperial
    --------------------------------------------------
    Retreiving data for City #174 of 753 ... asayita
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=asayita&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #174 of 753 ... pozo colorado
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pozo colorado&units=Imperial
    --------------------------------------------------
    Retreiving data for City #175 of 753 ... lebu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=lebu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #176 of 753 ... koppies
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=koppies&units=Imperial
    --------------------------------------------------
    Retreiving data for City #177 of 753 ... guiren
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=guiren&units=Imperial
    --------------------------------------------------
    Retreiving data for City #178 of 753 ... chapais
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chapais&units=Imperial
    --------------------------------------------------
    Retreiving data for City #179 of 753 ... marsa matruh
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=marsa matruh&units=Imperial
    --------------------------------------------------
    Retreiving data for City #180 of 753 ... divnomorskoye
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=divnomorskoye&units=Imperial
    --------------------------------------------------
    Retreiving data for City #181 of 753 ... rodrigues alves
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rodrigues alves&units=Imperial
    --------------------------------------------------
    Retreiving data for City #182 of 753 ... ancud
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ancud&units=Imperial
    --------------------------------------------------
    Retreiving data for City #183 of 753 ... port lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=port lincoln&units=Imperial
    --------------------------------------------------
    Retreiving data for City #184 of 753 ... moyale
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=moyale&units=Imperial
    --------------------------------------------------
    Retreiving data for City #185 of 753 ... ambulu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ambulu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #186 of 753 ... cairns
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cairns&units=Imperial
    --------------------------------------------------
    Retreiving data for City #187 of 753 ... catalina
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=catalina&units=Imperial
    --------------------------------------------------
    Retreiving data for City #188 of 753 ... attawapiskat
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=attawapiskat&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #188 of 753 ... yulara
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yulara&units=Imperial
    --------------------------------------------------
    Retreiving data for City #189 of 753 ... cape town
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cape town&units=Imperial
    --------------------------------------------------
    Retreiving data for City #190 of 753 ... port blair
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=port blair&units=Imperial
    --------------------------------------------------
    Retreiving data for City #191 of 753 ... labuhan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=labuhan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #192 of 753 ... voh
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=voh&units=Imperial
    --------------------------------------------------
    Retreiving data for City #193 of 753 ... evanston
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=evanston&units=Imperial
    --------------------------------------------------
    Retreiving data for City #194 of 753 ... santa ines
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=santa ines&units=Imperial
    --------------------------------------------------
    Retreiving data for City #195 of 753 ... sisimiut
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sisimiut&units=Imperial
    --------------------------------------------------
    Retreiving data for City #196 of 753 ... ciudad guayana
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ciudad guayana&units=Imperial
    --------------------------------------------------
    Retreiving data for City #197 of 753 ... hambantota
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hambantota&units=Imperial
    --------------------------------------------------
    Retreiving data for City #198 of 753 ... havre-saint-pierre
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=havre-saint-pierre&units=Imperial
    --------------------------------------------------
    Retreiving data for City #199 of 753 ... waingapu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=waingapu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #200 of 753 ... burica
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=burica&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #200 of 753 ... gornopravdinsk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=gornopravdinsk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #201 of 753 ... ngukurr
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ngukurr&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #201 of 753 ... paka
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=paka&units=Imperial
    --------------------------------------------------
    Retreiving data for City #202 of 753 ... san patricio
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=san patricio&units=Imperial
    --------------------------------------------------
    Retreiving data for City #203 of 753 ... umzimvubu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=umzimvubu&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #203 of 753 ... palabuhanratu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=palabuhanratu&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #203 of 753 ... nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nizhneyansk&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #203 of 753 ... maniitsoq
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=maniitsoq&units=Imperial
    --------------------------------------------------
    Retreiving data for City #204 of 753 ... korla
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=korla&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #204 of 753 ... talnakh
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=talnakh&units=Imperial
    --------------------------------------------------
    Retreiving data for City #205 of 753 ... sabha
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sabha&units=Imperial
    --------------------------------------------------
    Retreiving data for City #206 of 753 ... mahebourg
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mahebourg&units=Imperial
    --------------------------------------------------
    Retreiving data for City #207 of 753 ... torbay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=torbay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #208 of 753 ... vardo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vardo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #209 of 753 ... yeppoon
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yeppoon&units=Imperial
    --------------------------------------------------
    Retreiving data for City #210 of 753 ... port macquarie
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=port macquarie&units=Imperial
    --------------------------------------------------
    Retreiving data for City #211 of 753 ... mackenzie
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mackenzie&units=Imperial
    --------------------------------------------------
    Retreiving data for City #212 of 753 ... bhawanipatna
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bhawanipatna&units=Imperial
    --------------------------------------------------
    Retreiving data for City #213 of 753 ... hami
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hami&units=Imperial
    --------------------------------------------------
    Retreiving data for City #214 of 753 ... asau
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=asau&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #214 of 753 ... toora-khem
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=toora-khem&units=Imperial
    --------------------------------------------------
    Retreiving data for City #215 of 753 ... saleaula
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saleaula&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #215 of 753 ... antsohihy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=antsohihy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #216 of 753 ... kaitangata
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kaitangata&units=Imperial
    --------------------------------------------------
    Retreiving data for City #217 of 753 ... luba
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=luba&units=Imperial
    --------------------------------------------------
    Retreiving data for City #218 of 753 ... nalut
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nalut&units=Imperial
    --------------------------------------------------
    Retreiving data for City #219 of 753 ... chicama
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chicama&units=Imperial
    --------------------------------------------------
    Retreiving data for City #220 of 753 ... longyearbyen
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=longyearbyen&units=Imperial
    --------------------------------------------------
    Retreiving data for City #221 of 753 ... grand river south east
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=grand river south east&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #221 of 753 ... samarina
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=samarina&units=Imperial
    --------------------------------------------------
    Retreiving data for City #222 of 753 ... bambari
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bambari&units=Imperial
    --------------------------------------------------
    Retreiving data for City #223 of 753 ... mankono
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mankono&units=Imperial
    --------------------------------------------------
    Retreiving data for City #224 of 753 ... kangaatsiaq
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kangaatsiaq&units=Imperial
    --------------------------------------------------
    Retreiving data for City #225 of 753 ... kirakira
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kirakira&units=Imperial
    --------------------------------------------------
    Retreiving data for City #226 of 753 ... fukue
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=fukue&units=Imperial
    --------------------------------------------------
    Retreiving data for City #227 of 753 ... one hundred mile house
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=one hundred mile house&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #227 of 753 ... whitehorse
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=whitehorse&units=Imperial
    --------------------------------------------------
    Retreiving data for City #228 of 753 ... sitka
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sitka&units=Imperial
    --------------------------------------------------
    Retreiving data for City #229 of 753 ... manggar
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=manggar&units=Imperial
    --------------------------------------------------
    Retreiving data for City #230 of 753 ... ketchikan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ketchikan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #231 of 753 ... chokurdakh
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chokurdakh&units=Imperial
    --------------------------------------------------
    Retreiving data for City #232 of 753 ... california city
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=california city&units=Imperial
    --------------------------------------------------
    Retreiving data for City #233 of 753 ... bambous virieux
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bambous virieux&units=Imperial
    --------------------------------------------------
    Retreiving data for City #234 of 753 ... marystown
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=marystown&units=Imperial
    --------------------------------------------------
    Retreiving data for City #235 of 753 ... vao
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vao&units=Imperial
    --------------------------------------------------
    Retreiving data for City #236 of 753 ... esperance
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=esperance&units=Imperial
    --------------------------------------------------
    Retreiving data for City #237 of 753 ... kavieng
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kavieng&units=Imperial
    --------------------------------------------------
    Retreiving data for City #238 of 753 ... ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ostrovnoy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #239 of 753 ... banff
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=banff&units=Imperial
    --------------------------------------------------
    Retreiving data for City #240 of 753 ... ilulissat
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ilulissat&units=Imperial
    --------------------------------------------------
    Retreiving data for City #241 of 753 ... kamenskoye
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kamenskoye&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #241 of 753 ... nipawin
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nipawin&units=Imperial
    --------------------------------------------------
    Retreiving data for City #242 of 753 ... kiruna
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kiruna&units=Imperial
    --------------------------------------------------
    Retreiving data for City #243 of 753 ... ust-kamchatsk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ust-kamchatsk&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #243 of 753 ... salalah
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=salalah&units=Imperial
    --------------------------------------------------
    Retreiving data for City #244 of 753 ... palmer
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=palmer&units=Imperial
    --------------------------------------------------
    Retreiving data for City #245 of 753 ... holovyne
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=holovyne&units=Imperial
    --------------------------------------------------
    Retreiving data for City #246 of 753 ... rapid city
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rapid city&units=Imperial
    --------------------------------------------------
    Retreiving data for City #247 of 753 ... hoshcha
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hoshcha&units=Imperial
    --------------------------------------------------
    Retreiving data for City #248 of 753 ... bayburt
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bayburt&units=Imperial
    --------------------------------------------------
    Retreiving data for City #249 of 753 ... pisco
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pisco&units=Imperial
    --------------------------------------------------
    Retreiving data for City #250 of 753 ... pushkino
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pushkino&units=Imperial
    --------------------------------------------------
    Retreiving data for City #251 of 753 ... shebunino
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=shebunino&units=Imperial
    --------------------------------------------------
    Retreiving data for City #252 of 753 ... ahipara
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ahipara&units=Imperial
    --------------------------------------------------
    Retreiving data for City #253 of 753 ... marquette
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=marquette&units=Imperial
    --------------------------------------------------
    Retreiving data for City #254 of 753 ... honningsvag
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=honningsvag&units=Imperial
    --------------------------------------------------
    Retreiving data for City #255 of 753 ... hanzhong
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hanzhong&units=Imperial
    --------------------------------------------------
    Retreiving data for City #256 of 753 ... guilin
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=guilin&units=Imperial
    --------------------------------------------------
    Retreiving data for City #257 of 753 ... belyy yar
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=belyy yar&units=Imperial
    --------------------------------------------------
    Retreiving data for City #258 of 753 ... sept-iles
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sept-iles&units=Imperial
    --------------------------------------------------
    Retreiving data for City #259 of 753 ... mananjary
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mananjary&units=Imperial
    --------------------------------------------------
    Retreiving data for City #260 of 753 ... atasu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=atasu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #261 of 753 ... guerrero negro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=guerrero negro&units=Imperial
    --------------------------------------------------
    Retreiving data for City #262 of 753 ... adrar
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=adrar&units=Imperial
    --------------------------------------------------
    Retreiving data for City #263 of 753 ... rocha
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rocha&units=Imperial
    --------------------------------------------------
    Retreiving data for City #264 of 753 ... bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bandarbeyla&units=Imperial
    --------------------------------------------------
    Retreiving data for City #265 of 753 ... stonewall
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=stonewall&units=Imperial
    --------------------------------------------------
    Retreiving data for City #266 of 753 ... placido de castro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=placido de castro&units=Imperial
    --------------------------------------------------
    Retreiving data for City #267 of 753 ... palmas bellas
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=palmas bellas&units=Imperial
    --------------------------------------------------
    Retreiving data for City #268 of 753 ... tautira
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tautira&units=Imperial
    --------------------------------------------------
    Retreiving data for City #269 of 753 ... ouesso
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ouesso&units=Imperial
    --------------------------------------------------
    Retreiving data for City #270 of 753 ... bintulu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bintulu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #271 of 753 ... kruisfontein
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kruisfontein&units=Imperial
    --------------------------------------------------
    Retreiving data for City #272 of 753 ... barcelona
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=barcelona&units=Imperial
    --------------------------------------------------
    Retreiving data for City #273 of 753 ... olinda
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=olinda&units=Imperial
    --------------------------------------------------
    Retreiving data for City #274 of 753 ... ati
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ati&units=Imperial
    --------------------------------------------------
    Retreiving data for City #275 of 753 ... brigantine
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=brigantine&units=Imperial
    --------------------------------------------------
    Retreiving data for City #276 of 753 ... barabai
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=barabai&units=Imperial
    --------------------------------------------------
    Retreiving data for City #277 of 753 ... guangyuan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=guangyuan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #278 of 753 ... nisia floresta
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nisia floresta&units=Imperial
    --------------------------------------------------
    Retreiving data for City #279 of 753 ... oktyabrskiy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=oktyabrskiy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #280 of 753 ... fort saint james
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=fort saint james&units=Imperial
    --------------------------------------------------
    Retreiving data for City #281 of 753 ... suez
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=suez&units=Imperial
    --------------------------------------------------
    Retreiving data for City #282 of 753 ... minab
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=minab&units=Imperial
    --------------------------------------------------
    Retreiving data for City #283 of 753 ... puerto del rosario
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=puerto del rosario&units=Imperial
    --------------------------------------------------
    Retreiving data for City #284 of 753 ... am timan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=am timan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #285 of 753 ... la tuque
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=la tuque&units=Imperial
    --------------------------------------------------
    Retreiving data for City #286 of 753 ... vostok
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vostok&units=Imperial
    --------------------------------------------------
    Retreiving data for City #287 of 753 ... loiza
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=loiza&units=Imperial
    --------------------------------------------------
    Retreiving data for City #288 of 753 ... alice springs
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=alice springs&units=Imperial
    --------------------------------------------------
    Retreiving data for City #289 of 753 ... sokolo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sokolo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #290 of 753 ... baykit
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=baykit&units=Imperial
    --------------------------------------------------
    Retreiving data for City #291 of 753 ... eyl
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=eyl&units=Imperial
    --------------------------------------------------
    Retreiving data for City #292 of 753 ... hazorasp
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hazorasp&units=Imperial
    --------------------------------------------------
    Retreiving data for City #293 of 753 ... santo antonio do ica
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=santo antonio do ica&units=Imperial
    --------------------------------------------------
    Retreiving data for City #294 of 753 ... bubaque
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bubaque&units=Imperial
    --------------------------------------------------
    Retreiving data for City #295 of 753 ... hamilton
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hamilton&units=Imperial
    --------------------------------------------------
    Retreiving data for City #296 of 753 ... high rock
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=high rock&units=Imperial
    --------------------------------------------------
    Retreiving data for City #297 of 753 ... jaisalmer
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=jaisalmer&units=Imperial
    --------------------------------------------------
    Retreiving data for City #298 of 753 ... kungurtug
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kungurtug&units=Imperial
    --------------------------------------------------
    Retreiving data for City #299 of 753 ... jimma
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=jimma&units=Imperial
    --------------------------------------------------
    Retreiving data for City #300 of 753 ... paramonga
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=paramonga&units=Imperial
    --------------------------------------------------
    Retreiving data for City #301 of 753 ... pasighat
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pasighat&units=Imperial
    --------------------------------------------------
    Retreiving data for City #302 of 753 ... novosergiyevka
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=novosergiyevka&units=Imperial
    --------------------------------------------------
    Retreiving data for City #303 of 753 ... phuket
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=phuket&units=Imperial
    --------------------------------------------------
    Retreiving data for City #304 of 753 ... veraval
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=veraval&units=Imperial
    --------------------------------------------------
    Retreiving data for City #305 of 753 ... sirjan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sirjan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #306 of 753 ... broxburn
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=broxburn&units=Imperial
    --------------------------------------------------
    Retreiving data for City #307 of 753 ... freeport
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=freeport&units=Imperial
    --------------------------------------------------
    Retreiving data for City #308 of 753 ... pevek
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pevek&units=Imperial
    --------------------------------------------------
    Retreiving data for City #309 of 753 ... cotonou
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cotonou&units=Imperial
    --------------------------------------------------
    Retreiving data for City #310 of 753 ... santa barbara
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=santa barbara&units=Imperial
    --------------------------------------------------
    Retreiving data for City #311 of 753 ... thinadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=thinadhoo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #312 of 753 ... rawah
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rawah&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #312 of 753 ... xinyu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=xinyu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #313 of 753 ... alofi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=alofi&units=Imperial
    --------------------------------------------------
    Retreiving data for City #314 of 753 ... lerwick
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=lerwick&units=Imperial
    --------------------------------------------------
    Retreiving data for City #315 of 753 ... ust-kuyga
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ust-kuyga&units=Imperial
    --------------------------------------------------
    Retreiving data for City #316 of 753 ... payo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=payo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #317 of 753 ... road town
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=road town&units=Imperial
    --------------------------------------------------
    Retreiving data for City #318 of 753 ... cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cabo san lucas&units=Imperial
    --------------------------------------------------
    Retreiving data for City #319 of 753 ... ust-omchug
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ust-omchug&units=Imperial
    --------------------------------------------------
    Retreiving data for City #320 of 753 ... hanmer springs
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hanmer springs&units=Imperial
    --------------------------------------------------
    Retreiving data for City #321 of 753 ... rio grande
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rio grande&units=Imperial
    --------------------------------------------------
    Retreiving data for City #322 of 753 ... namibe
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=namibe&units=Imperial
    --------------------------------------------------
    Retreiving data for City #323 of 753 ... sambava
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sambava&units=Imperial
    --------------------------------------------------
    Retreiving data for City #324 of 753 ... olafsvik
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=olafsvik&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #324 of 753 ... zapolyarnyy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=zapolyarnyy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #325 of 753 ... samandag
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=samandag&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #325 of 753 ... tidore
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tidore&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #325 of 753 ... kutum
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kutum&units=Imperial
    --------------------------------------------------
    Retreiving data for City #326 of 753 ... makung
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=makung&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #326 of 753 ... narsaq
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=narsaq&units=Imperial
    --------------------------------------------------
    Retreiving data for City #327 of 753 ... punta de bombon
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=punta de bombon&units=Imperial
    --------------------------------------------------
    Retreiving data for City #328 of 753 ... butembo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=butembo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #329 of 753 ... krasnyy oktyabr
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=krasnyy oktyabr&units=Imperial
    --------------------------------------------------
    Retreiving data for City #330 of 753 ... safaqis
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=safaqis&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #330 of 753 ... severnyy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=severnyy&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #330 of 753 ... san rafael
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=san rafael&units=Imperial
    --------------------------------------------------
    Retreiving data for City #331 of 753 ... bandar
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bandar&units=Imperial
    --------------------------------------------------
    Retreiving data for City #332 of 753 ... anadyr
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=anadyr&units=Imperial
    --------------------------------------------------
    Retreiving data for City #333 of 753 ... kununurra
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kununurra&units=Imperial
    --------------------------------------------------
    Retreiving data for City #334 of 753 ... leh
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=leh&units=Imperial
    --------------------------------------------------
    Retreiving data for City #335 of 753 ... venice
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=venice&units=Imperial
    --------------------------------------------------
    Retreiving data for City #336 of 753 ... pribelskiy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pribelskiy&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #336 of 753 ... louisbourg
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=louisbourg&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #336 of 753 ... lolua
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=lolua&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #336 of 753 ... shubarshi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=shubarshi&units=Imperial
    --------------------------------------------------
    Retreiving data for City #337 of 753 ... vagur
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vagur&units=Imperial
    --------------------------------------------------
    Retreiving data for City #338 of 753 ... ponta do sol
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ponta do sol&units=Imperial
    --------------------------------------------------
    Retreiving data for City #339 of 753 ... thilogne
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=thilogne&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #339 of 753 ... zhangjiakou
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=zhangjiakou&units=Imperial
    --------------------------------------------------
    Retreiving data for City #340 of 753 ... wanxian
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=wanxian&units=Imperial
    --------------------------------------------------
    Retreiving data for City #341 of 753 ... aitape
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=aitape&units=Imperial
    --------------------------------------------------
    Retreiving data for City #342 of 753 ... san juan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=san juan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #343 of 753 ... falavarjan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=falavarjan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #344 of 753 ... revelstoke
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=revelstoke&units=Imperial
    --------------------------------------------------
    Retreiving data for City #345 of 753 ... abu samrah
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=abu samrah&units=Imperial
    --------------------------------------------------
    Retreiving data for City #346 of 753 ... iskateley
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=iskateley&units=Imperial
    --------------------------------------------------
    Retreiving data for City #347 of 753 ... raudeberg
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=raudeberg&units=Imperial
    --------------------------------------------------
    Retreiving data for City #348 of 753 ... correntina
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=correntina&units=Imperial
    --------------------------------------------------
    Retreiving data for City #349 of 753 ... tumannyy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tumannyy&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #349 of 753 ... faanui
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=faanui&units=Imperial
    --------------------------------------------------
    Retreiving data for City #350 of 753 ... waddan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=waddan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #351 of 753 ... rantauprapat
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rantauprapat&units=Imperial
    --------------------------------------------------
    Retreiving data for City #352 of 753 ... shubarkuduk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=shubarkuduk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #353 of 753 ... the pas
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=the pas&units=Imperial
    --------------------------------------------------
    Retreiving data for City #354 of 753 ... tamandare
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tamandare&units=Imperial
    --------------------------------------------------
    Retreiving data for City #355 of 753 ... mahibadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mahibadhoo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #356 of 753 ... anloga
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=anloga&units=Imperial
    --------------------------------------------------
    Retreiving data for City #357 of 753 ... vigia del fuerte
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vigia del fuerte&units=Imperial
    --------------------------------------------------
    Retreiving data for City #358 of 753 ... stari bohorodchany
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=stari bohorodchany&units=Imperial
    --------------------------------------------------
    Retreiving data for City #359 of 753 ... geraldton
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=geraldton&units=Imperial
    --------------------------------------------------
    Retreiving data for City #360 of 753 ... buqayq
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=buqayq&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #360 of 753 ... salinas
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=salinas&units=Imperial
    --------------------------------------------------
    Retreiving data for City #361 of 753 ... margate
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=margate&units=Imperial
    --------------------------------------------------
    Retreiving data for City #362 of 753 ... riverton
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=riverton&units=Imperial
    --------------------------------------------------
    Retreiving data for City #363 of 753 ... shieli
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=shieli&units=Imperial
    --------------------------------------------------
    Retreiving data for City #364 of 753 ... mecca
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mecca&units=Imperial
    --------------------------------------------------
    Retreiving data for City #365 of 753 ... kaseda
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kaseda&units=Imperial
    --------------------------------------------------
    Retreiving data for City #366 of 753 ... beringovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=beringovskiy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #367 of 753 ... muros
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=muros&units=Imperial
    --------------------------------------------------
    Retreiving data for City #368 of 753 ... grand centre
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=grand centre&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #368 of 753 ... nome
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nome&units=Imperial
    --------------------------------------------------
    Retreiving data for City #369 of 753 ... rinteln
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rinteln&units=Imperial
    --------------------------------------------------
    Retreiving data for City #370 of 753 ... banepa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=banepa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #371 of 753 ... santa cruz
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=santa cruz&units=Imperial
    --------------------------------------------------
    Retreiving data for City #372 of 753 ... haibowan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=haibowan&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #372 of 753 ... shepsi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=shepsi&units=Imperial
    --------------------------------------------------
    Retreiving data for City #373 of 753 ... mezhdurechensk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mezhdurechensk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #374 of 753 ... muisne
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=muisne&units=Imperial
    --------------------------------------------------
    Retreiving data for City #375 of 753 ... khrebtovaya
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=khrebtovaya&units=Imperial
    --------------------------------------------------
    Retreiving data for City #376 of 753 ... micheweni
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=micheweni&units=Imperial
    --------------------------------------------------
    Retreiving data for City #377 of 753 ... blagoyevo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=blagoyevo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #378 of 753 ... fairbanks
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=fairbanks&units=Imperial
    --------------------------------------------------
    Retreiving data for City #379 of 753 ... argir
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=argir&units=Imperial
    --------------------------------------------------
    Retreiving data for City #380 of 753 ... kaeo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kaeo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #381 of 753 ... torbat-e jam
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=torbat-e jam&units=Imperial
    --------------------------------------------------
    Retreiving data for City #382 of 753 ... noumea
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=noumea&units=Imperial
    --------------------------------------------------
    Retreiving data for City #383 of 753 ... bonthe
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bonthe&units=Imperial
    --------------------------------------------------
    Retreiving data for City #384 of 753 ... garowe
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=garowe&units=Imperial
    --------------------------------------------------
    Retreiving data for City #385 of 753 ... butte
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=butte&units=Imperial
    --------------------------------------------------
    Retreiving data for City #386 of 753 ... lompoc
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=lompoc&units=Imperial
    --------------------------------------------------
    Retreiving data for City #387 of 753 ... burlington
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=burlington&units=Imperial
    --------------------------------------------------
    Retreiving data for City #388 of 753 ... ust-nera
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ust-nera&units=Imperial
    --------------------------------------------------
    Retreiving data for City #389 of 753 ... yarmouth
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yarmouth&units=Imperial
    --------------------------------------------------
    Retreiving data for City #390 of 753 ... wiarton
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=wiarton&units=Imperial
    --------------------------------------------------
    Retreiving data for City #391 of 753 ... pouebo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pouebo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #392 of 753 ... tabiauea
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tabiauea&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #392 of 753 ... sola
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sola&units=Imperial
    --------------------------------------------------
    Retreiving data for City #393 of 753 ... mudgee
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mudgee&units=Imperial
    --------------------------------------------------
    Retreiving data for City #394 of 753 ... dingle
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dingle&units=Imperial
    --------------------------------------------------
    Retreiving data for City #395 of 753 ... saint anthony
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saint anthony&units=Imperial
    --------------------------------------------------
    Retreiving data for City #396 of 753 ... krasnyy kut
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=krasnyy kut&units=Imperial
    --------------------------------------------------
    Retreiving data for City #397 of 753 ... krasnoselkup
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=krasnoselkup&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #397 of 753 ... kokopo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kokopo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #398 of 753 ... mendoza
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mendoza&units=Imperial
    --------------------------------------------------
    Retreiving data for City #399 of 753 ... russell
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=russell&units=Imperial
    --------------------------------------------------
    Retreiving data for City #400 of 753 ... northam
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=northam&units=Imperial
    --------------------------------------------------
    Retreiving data for City #401 of 753 ... coquimbo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=coquimbo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #402 of 753 ... muli
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=muli&units=Imperial
    --------------------------------------------------
    Retreiving data for City #403 of 753 ... sovetsk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sovetsk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #404 of 753 ... mirnyy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mirnyy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #405 of 753 ... ambilobe
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ambilobe&units=Imperial
    --------------------------------------------------
    Retreiving data for City #406 of 753 ... talcahuano
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=talcahuano&units=Imperial
    --------------------------------------------------
    Retreiving data for City #407 of 753 ... fortuna
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=fortuna&units=Imperial
    --------------------------------------------------
    Retreiving data for City #408 of 753 ... turukhansk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=turukhansk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #409 of 753 ... clyde river
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=clyde river&units=Imperial
    --------------------------------------------------
    Retreiving data for City #410 of 753 ... lata
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=lata&units=Imperial
    --------------------------------------------------
    Retreiving data for City #411 of 753 ... kawalu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kawalu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #412 of 753 ... tokur
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tokur&units=Imperial
    --------------------------------------------------
    Retreiving data for City #413 of 753 ... luganville
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=luganville&units=Imperial
    --------------------------------------------------
    Retreiving data for City #414 of 753 ... amapa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=amapa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #415 of 753 ... vostok
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vostok&units=Imperial
    --------------------------------------------------
    Retreiving data for City #416 of 753 ... sabang
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sabang&units=Imperial
    --------------------------------------------------
    Retreiving data for City #417 of 753 ... hobe sound
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hobe sound&units=Imperial
    --------------------------------------------------
    Retreiving data for City #418 of 753 ... mayo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mayo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #419 of 753 ... valverde del camino
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=valverde del camino&units=Imperial
    --------------------------------------------------
    Retreiving data for City #420 of 753 ... samarai
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=samarai&units=Imperial
    --------------------------------------------------
    Retreiving data for City #421 of 753 ... chipinge
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chipinge&units=Imperial
    --------------------------------------------------
    Retreiving data for City #422 of 753 ... dudinka
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dudinka&units=Imperial
    --------------------------------------------------
    Retreiving data for City #423 of 753 ... nogliki
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nogliki&units=Imperial
    --------------------------------------------------
    Retreiving data for City #424 of 753 ... havoysund
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=havoysund&units=Imperial
    --------------------------------------------------
    Retreiving data for City #425 of 753 ... alakurtti
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=alakurtti&units=Imperial
    --------------------------------------------------
    Retreiving data for City #426 of 753 ... sumbawa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sumbawa&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #426 of 753 ... vanavara
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vanavara&units=Imperial
    --------------------------------------------------
    Retreiving data for City #427 of 753 ... saint-augustin
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saint-augustin&units=Imperial
    --------------------------------------------------
    Retreiving data for City #428 of 753 ... darhan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=darhan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #429 of 753 ... lekoni
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=lekoni&units=Imperial
    --------------------------------------------------
    Retreiving data for City #430 of 753 ... kwinana
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kwinana&units=Imperial
    --------------------------------------------------
    Retreiving data for City #431 of 753 ... pilar
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pilar&units=Imperial
    --------------------------------------------------
    Retreiving data for City #432 of 753 ... labutta
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=labutta&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #432 of 753 ... bud
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bud&units=Imperial
    --------------------------------------------------
    Retreiving data for City #433 of 753 ... aksarka
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=aksarka&units=Imperial
    --------------------------------------------------
    Retreiving data for City #434 of 753 ... touros
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=touros&units=Imperial
    --------------------------------------------------
    Retreiving data for City #435 of 753 ... tomatlan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tomatlan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #436 of 753 ... yanchukan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yanchukan&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #436 of 753 ... isla mujeres
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=isla mujeres&units=Imperial
    --------------------------------------------------
    Retreiving data for City #437 of 753 ... altay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=altay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #438 of 753 ... zihuatanejo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=zihuatanejo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #439 of 753 ... husavik
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=husavik&units=Imperial
    --------------------------------------------------
    Retreiving data for City #440 of 753 ... svirstroy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=svirstroy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #441 of 753 ... west plains
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=west plains&units=Imperial
    --------------------------------------------------
    Retreiving data for City #442 of 753 ... kondinskoye
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kondinskoye&units=Imperial
    --------------------------------------------------
    Retreiving data for City #443 of 753 ... caravelas
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=caravelas&units=Imperial
    --------------------------------------------------
    Retreiving data for City #444 of 753 ... prainha
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=prainha&units=Imperial
    --------------------------------------------------
    Retreiving data for City #445 of 753 ... mount isa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mount isa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #446 of 753 ... kachug
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kachug&units=Imperial
    --------------------------------------------------
    Retreiving data for City #447 of 753 ... cap malheureux
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cap malheureux&units=Imperial
    --------------------------------------------------
    Retreiving data for City #448 of 753 ... taoudenni
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=taoudenni&units=Imperial
    --------------------------------------------------
    Retreiving data for City #449 of 753 ... babu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=babu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #450 of 753 ... honolulu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=honolulu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #451 of 753 ... netrakona
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=netrakona&units=Imperial
    --------------------------------------------------
    Retreiving data for City #452 of 753 ... yumen
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yumen&units=Imperial
    --------------------------------------------------
    Retreiving data for City #453 of 753 ... roald
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=roald&units=Imperial
    --------------------------------------------------
    Retreiving data for City #454 of 753 ... zabinka
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=zabinka&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #454 of 753 ... nueva gerona
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nueva gerona&units=Imperial
    --------------------------------------------------
    Retreiving data for City #455 of 753 ... magadi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=magadi&units=Imperial
    --------------------------------------------------
    Retreiving data for City #456 of 753 ... srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=srednekolymsk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #457 of 753 ... careiro da varzea
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=careiro da varzea&units=Imperial
    --------------------------------------------------
    Retreiving data for City #458 of 753 ... we
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=we&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #458 of 753 ... isangel
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=isangel&units=Imperial
    --------------------------------------------------
    Retreiving data for City #459 of 753 ... tunduru
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tunduru&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #459 of 753 ... teavaro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=teavaro&units=Imperial
    --------------------------------------------------
    Retreiving data for City #460 of 753 ... whitianga
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=whitianga&units=Imperial
    --------------------------------------------------
    Retreiving data for City #461 of 753 ... oistins
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=oistins&units=Imperial
    --------------------------------------------------
    Retreiving data for City #462 of 753 ... port-gentil
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=port-gentil&units=Imperial
    --------------------------------------------------
    Retreiving data for City #463 of 753 ... pangnirtung
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pangnirtung&units=Imperial
    --------------------------------------------------
    Retreiving data for City #464 of 753 ... pingliang
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pingliang&units=Imperial
    --------------------------------------------------
    Retreiving data for City #465 of 753 ... kamaishi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kamaishi&units=Imperial
    --------------------------------------------------
    Retreiving data for City #466 of 753 ... naftah
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=naftah&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #466 of 753 ... yomou
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yomou&units=Imperial
    --------------------------------------------------
    Retreiving data for City #467 of 753 ... kamphaeng phet
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kamphaeng phet&units=Imperial
    --------------------------------------------------
    Retreiving data for City #468 of 753 ... caronport
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=caronport&units=Imperial
    --------------------------------------------------
    Retreiving data for City #469 of 753 ... yuryev-polskiy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yuryev-polskiy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #470 of 753 ... timra
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=timra&units=Imperial
    --------------------------------------------------
    Retreiving data for City #471 of 753 ... san pedro de cajas
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=san pedro de cajas&units=Imperial
    --------------------------------------------------
    Retreiving data for City #472 of 753 ... taitung
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=taitung&units=Imperial
    --------------------------------------------------
    Retreiving data for City #473 of 753 ... malmesbury
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=malmesbury&units=Imperial
    --------------------------------------------------
    Retreiving data for City #474 of 753 ... yashkul
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yashkul&units=Imperial
    --------------------------------------------------
    Retreiving data for City #475 of 753 ... codrington
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=codrington&units=Imperial
    --------------------------------------------------
    Retreiving data for City #476 of 753 ... mana
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mana&units=Imperial
    --------------------------------------------------
    Retreiving data for City #477 of 753 ... abong mbang
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=abong mbang&units=Imperial
    --------------------------------------------------
    Retreiving data for City #478 of 753 ... loei
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=loei&units=Imperial
    --------------------------------------------------
    Retreiving data for City #479 of 753 ... quatre cocos
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=quatre cocos&units=Imperial
    --------------------------------------------------
    Retreiving data for City #480 of 753 ... atar
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=atar&units=Imperial
    --------------------------------------------------
    Retreiving data for City #481 of 753 ... mad
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mad&units=Imperial
    --------------------------------------------------
    Retreiving data for City #482 of 753 ... trim
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=trim&units=Imperial
    --------------------------------------------------
    Retreiving data for City #483 of 753 ... burnie
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=burnie&units=Imperial
    --------------------------------------------------
    Retreiving data for City #484 of 753 ... gazli
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=gazli&units=Imperial
    --------------------------------------------------
    Retreiving data for City #485 of 753 ... ruatoria
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ruatoria&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #485 of 753 ... kathmandu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kathmandu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #486 of 753 ... mount gambier
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mount gambier&units=Imperial
    --------------------------------------------------
    Retreiving data for City #487 of 753 ... los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=los llanos de aridane&units=Imperial
    --------------------------------------------------
    Retreiving data for City #488 of 753 ... constitucion
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=constitucion&units=Imperial
    --------------------------------------------------
    Retreiving data for City #489 of 753 ... warqla
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=warqla&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #489 of 753 ... cosala
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cosala&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #489 of 753 ... chifeng
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chifeng&units=Imperial
    --------------------------------------------------
    Retreiving data for City #490 of 753 ... nanakuli
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nanakuli&units=Imperial
    --------------------------------------------------
    Retreiving data for City #491 of 753 ... kuhestan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kuhestan&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #491 of 753 ... charters towers
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=charters towers&units=Imperial
    --------------------------------------------------
    Retreiving data for City #492 of 753 ... batouri
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=batouri&units=Imperial
    --------------------------------------------------
    Retreiving data for City #493 of 753 ... chalchihuites
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chalchihuites&units=Imperial
    --------------------------------------------------
    Retreiving data for City #494 of 753 ... asfi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=asfi&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #494 of 753 ... ampanihy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ampanihy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #495 of 753 ... yei
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yei&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #495 of 753 ... moussoro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=moussoro&units=Imperial
    --------------------------------------------------
    Retreiving data for City #496 of 753 ... tornio
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tornio&units=Imperial
    --------------------------------------------------
    Retreiving data for City #497 of 753 ... waldoboro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=waldoboro&units=Imperial
    --------------------------------------------------
    Retreiving data for City #498 of 753 ... calvinia
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=calvinia&units=Imperial
    --------------------------------------------------
    Retreiving data for City #499 of 753 ... campbellsville
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=campbellsville&units=Imperial
    --------------------------------------------------
    Retreiving data for City #500 of 753 ... damghan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=damghan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #501 of 753 ... yarada
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yarada&units=Imperial
    --------------------------------------------------
    Retreiving data for City #502 of 753 ... ixtapa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ixtapa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #503 of 753 ... shu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=shu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #504 of 753 ... tubruq
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tubruq&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #504 of 753 ... athabasca
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=athabasca&units=Imperial
    --------------------------------------------------
    Retreiving data for City #505 of 753 ... erzin
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=erzin&units=Imperial
    --------------------------------------------------
    Retreiving data for City #506 of 753 ... walvis bay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=walvis bay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #507 of 753 ... maningrida
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=maningrida&units=Imperial
    --------------------------------------------------
    Retreiving data for City #508 of 753 ... rognan
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rognan&units=Imperial
    --------------------------------------------------
    Retreiving data for City #509 of 753 ... banjar
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=banjar&units=Imperial
    --------------------------------------------------
    Retreiving data for City #510 of 753 ... valdivia
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=valdivia&units=Imperial
    --------------------------------------------------
    Retreiving data for City #511 of 753 ... celestun
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=celestun&units=Imperial
    --------------------------------------------------
    Retreiving data for City #512 of 753 ... tiznit
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tiznit&units=Imperial
    --------------------------------------------------
    Retreiving data for City #513 of 753 ... marzuq
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=marzuq&units=Imperial
    --------------------------------------------------
    Retreiving data for City #514 of 753 ... sungai udang
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sungai udang&units=Imperial
    --------------------------------------------------
    Retreiving data for City #515 of 753 ... mehamn
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mehamn&units=Imperial
    --------------------------------------------------
    Retreiving data for City #516 of 753 ... sao gabriel da cachoeira
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sao gabriel da cachoeira&units=Imperial
    --------------------------------------------------
    Retreiving data for City #517 of 753 ... cabra
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cabra&units=Imperial
    --------------------------------------------------
    Retreiving data for City #518 of 753 ... manokwari
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=manokwari&units=Imperial
    --------------------------------------------------
    Retreiving data for City #519 of 753 ... envira
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=envira&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #519 of 753 ... lewistown
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=lewistown&units=Imperial
    --------------------------------------------------
    Retreiving data for City #520 of 753 ... resistencia
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=resistencia&units=Imperial
    --------------------------------------------------
    Retreiving data for City #521 of 753 ... itarantim
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=itarantim&units=Imperial
    --------------------------------------------------
    Retreiving data for City #522 of 753 ... zhangye
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=zhangye&units=Imperial
    --------------------------------------------------
    Retreiving data for City #523 of 753 ... hals
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hals&units=Imperial
    --------------------------------------------------
    Retreiving data for City #524 of 753 ... weymouth
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=weymouth&units=Imperial
    --------------------------------------------------
    Retreiving data for City #525 of 753 ... sokoni
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sokoni&units=Imperial
    --------------------------------------------------
    Retreiving data for City #526 of 753 ... carberry
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=carberry&units=Imperial
    --------------------------------------------------
    Retreiving data for City #527 of 753 ... jeremie
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=jeremie&units=Imperial
    --------------------------------------------------
    Retreiving data for City #528 of 753 ... huarmey
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=huarmey&units=Imperial
    --------------------------------------------------
    Retreiving data for City #529 of 753 ... saint-medard-en-jalles
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saint-medard-en-jalles&units=Imperial
    --------------------------------------------------
    Retreiving data for City #530 of 753 ... paamiut
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=paamiut&units=Imperial
    --------------------------------------------------
    Retreiving data for City #531 of 753 ... novikovo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=novikovo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #532 of 753 ... phoenix
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=phoenix&units=Imperial
    --------------------------------------------------
    Retreiving data for City #533 of 753 ... merauke
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=merauke&units=Imperial
    --------------------------------------------------
    Retreiving data for City #534 of 753 ... almaty
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=almaty&units=Imperial
    --------------------------------------------------
    Retreiving data for City #535 of 753 ... grosse pointe farms
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=grosse pointe farms&units=Imperial
    --------------------------------------------------
    Retreiving data for City #536 of 753 ... isla vista
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=isla vista&units=Imperial
    --------------------------------------------------
    Retreiving data for City #537 of 753 ... nsanje
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nsanje&units=Imperial
    --------------------------------------------------
    Retreiving data for City #538 of 753 ... udachnyy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=udachnyy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #539 of 753 ... artyk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=artyk&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #539 of 753 ... coro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=coro&units=Imperial
    --------------------------------------------------
    Retreiving data for City #540 of 753 ... marsh harbour
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=marsh harbour&units=Imperial
    --------------------------------------------------
    Retreiving data for City #541 of 753 ... nguiu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nguiu&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #541 of 753 ... imeni poliny osipenko
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=imeni poliny osipenko&units=Imperial
    --------------------------------------------------
    Retreiving data for City #542 of 753 ... turochak
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=turochak&units=Imperial
    --------------------------------------------------
    Retreiving data for City #543 of 753 ... gwanda
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=gwanda&units=Imperial
    --------------------------------------------------
    Retreiving data for City #544 of 753 ... ilebo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ilebo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #545 of 753 ... abu dhabi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=abu dhabi&units=Imperial
    --------------------------------------------------
    Retreiving data for City #546 of 753 ... vestmanna
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vestmanna&units=Imperial
    --------------------------------------------------
    Retreiving data for City #547 of 753 ... flinders
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=flinders&units=Imperial
    --------------------------------------------------
    Retreiving data for City #548 of 753 ... iquitos
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=iquitos&units=Imperial
    --------------------------------------------------
    Retreiving data for City #549 of 753 ... suzun
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=suzun&units=Imperial
    --------------------------------------------------
    Retreiving data for City #550 of 753 ... tanout
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tanout&units=Imperial
    --------------------------------------------------
    Retreiving data for City #551 of 753 ... simao
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=simao&units=Imperial
    --------------------------------------------------
    Retreiving data for City #552 of 753 ... ankang
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ankang&units=Imperial
    --------------------------------------------------
    Retreiving data for City #553 of 753 ... kodiak
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kodiak&units=Imperial
    --------------------------------------------------
    Retreiving data for City #554 of 753 ... malanje
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=malanje&units=Imperial
    --------------------------------------------------
    Retreiving data for City #555 of 753 ... malko tarnovo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=malko tarnovo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #556 of 753 ... barcelos
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=barcelos&units=Imperial
    --------------------------------------------------
    Retreiving data for City #557 of 753 ... chitral
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chitral&units=Imperial
    --------------------------------------------------
    Retreiving data for City #558 of 753 ... nemuro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nemuro&units=Imperial
    --------------------------------------------------
    Retreiving data for City #559 of 753 ... yatou
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=yatou&units=Imperial
    --------------------------------------------------
    Retreiving data for City #560 of 753 ... thunder bay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=thunder bay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #561 of 753 ... batagay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=batagay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #562 of 753 ... richards bay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=richards bay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #563 of 753 ... san cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=san cristobal&units=Imperial
    --------------------------------------------------
    Retreiving data for City #564 of 753 ... sobolevo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sobolevo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #565 of 753 ... qingdao
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=qingdao&units=Imperial
    --------------------------------------------------
    Retreiving data for City #566 of 753 ... belogorsk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=belogorsk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #567 of 753 ... gushikawa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=gushikawa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #568 of 753 ... oskaloosa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=oskaloosa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #569 of 753 ... avera
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=avera&units=Imperial
    --------------------------------------------------
    Retreiving data for City #570 of 753 ... maykain
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=maykain&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #570 of 753 ... sainte-marie
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sainte-marie&units=Imperial
    --------------------------------------------------
    Retreiving data for City #571 of 753 ... kangaba
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kangaba&units=Imperial
    --------------------------------------------------
    Retreiving data for City #572 of 753 ... tromso
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tromso&units=Imperial
    --------------------------------------------------
    Retreiving data for City #573 of 753 ... raga
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=raga&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #573 of 753 ... rawson
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rawson&units=Imperial
    --------------------------------------------------
    Retreiving data for City #574 of 753 ... nushki
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nushki&units=Imperial
    --------------------------------------------------
    Retreiving data for City #575 of 753 ... todos santos
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=todos santos&units=Imperial
    --------------------------------------------------
    Retreiving data for City #576 of 753 ... onega
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=onega&units=Imperial
    --------------------------------------------------
    Retreiving data for City #577 of 753 ... predeal
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=predeal&units=Imperial
    --------------------------------------------------
    Retreiving data for City #578 of 753 ... oga
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=oga&units=Imperial
    --------------------------------------------------
    Retreiving data for City #579 of 753 ... marawi
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=marawi&units=Imperial
    --------------------------------------------------
    Retreiving data for City #580 of 753 ... kisangani
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kisangani&units=Imperial
    --------------------------------------------------
    Retreiving data for City #581 of 753 ... batagay-alyta
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=batagay-alyta&units=Imperial
    --------------------------------------------------
    Retreiving data for City #582 of 753 ... teahupoo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=teahupoo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #583 of 753 ... huanuni
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=huanuni&units=Imperial
    --------------------------------------------------
    Retreiving data for City #584 of 753 ... solnechnyy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=solnechnyy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #585 of 753 ... zavyalovo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=zavyalovo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #586 of 753 ... dickinson
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dickinson&units=Imperial
    --------------------------------------------------
    Retreiving data for City #587 of 753 ... luangwa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=luangwa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #588 of 753 ... miguel hidalgo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=miguel hidalgo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #589 of 753 ... xapuri
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=xapuri&units=Imperial
    --------------------------------------------------
    Retreiving data for City #590 of 753 ... sao domingos
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=sao domingos&units=Imperial
    --------------------------------------------------
    Retreiving data for City #591 of 753 ... pakhtakoron
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pakhtakoron&units=Imperial
    --------------------------------------------------
    Retreiving data for City #592 of 753 ... verkhoyansk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=verkhoyansk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #593 of 753 ... venissieux
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=venissieux&units=Imperial
    --------------------------------------------------
    Retreiving data for City #594 of 753 ... kasongo-lunda
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kasongo-lunda&units=Imperial
    --------------------------------------------------
    Retreiving data for City #595 of 753 ... leshukonskoye
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=leshukonskoye&units=Imperial
    --------------------------------------------------
    Retreiving data for City #596 of 753 ... dmitriyevka
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dmitriyevka&units=Imperial
    --------------------------------------------------
    Retreiving data for City #597 of 753 ... taywarah
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=taywarah&units=Imperial
    --------------------------------------------------
    Retreiving data for City #598 of 753 ... kiunga
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kiunga&units=Imperial
    --------------------------------------------------
    Retreiving data for City #599 of 753 ... newport
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=newport&units=Imperial
    --------------------------------------------------
    Retreiving data for City #600 of 753 ... skagastrond
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=skagastrond&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #600 of 753 ... crab hill
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=crab hill&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #600 of 753 ... mariental
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mariental&units=Imperial
    --------------------------------------------------
    Retreiving data for City #601 of 753 ... damaturu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=damaturu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #602 of 753 ... ambodifototra
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ambodifototra&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #602 of 753 ... grenaa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=grenaa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #603 of 753 ... champerico
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=champerico&units=Imperial
    --------------------------------------------------
    Retreiving data for City #604 of 753 ... dunedin
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dunedin&units=Imperial
    --------------------------------------------------
    Retreiving data for City #605 of 753 ... salinas
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=salinas&units=Imperial
    --------------------------------------------------
    Retreiving data for City #606 of 753 ... krasnyy chikoy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=krasnyy chikoy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #607 of 753 ... emba
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=emba&units=Imperial
    --------------------------------------------------
    Retreiving data for City #608 of 753 ... porto belo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=porto belo&units=Imperial
    --------------------------------------------------
    Retreiving data for City #609 of 753 ... listvyanka
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=listvyanka&units=Imperial
    --------------------------------------------------
    Retreiving data for City #610 of 753 ... marcona
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=marcona&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #610 of 753 ... hopkinsville
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hopkinsville&units=Imperial
    --------------------------------------------------
    Retreiving data for City #611 of 753 ... tilichiki
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tilichiki&units=Imperial
    --------------------------------------------------
    Retreiving data for City #612 of 753 ... henties bay
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=henties bay&units=Imperial
    --------------------------------------------------
    Retreiving data for City #613 of 753 ... tazovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tazovskiy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #614 of 753 ... fort saint john
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=fort saint john&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #614 of 753 ... necochea
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=necochea&units=Imperial
    --------------------------------------------------
    Retreiving data for City #615 of 753 ... tyssedal
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tyssedal&units=Imperial
    --------------------------------------------------
    Retreiving data for City #616 of 753 ... saint andrews
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=saint andrews&units=Imperial
    --------------------------------------------------
    Retreiving data for City #617 of 753 ... poum
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=poum&units=Imperial
    --------------------------------------------------
    Retreiving data for City #618 of 753 ... san quintin
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=san quintin&units=Imperial
    --------------------------------------------------
    Retreiving data for City #619 of 753 ... hirado
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hirado&units=Imperial
    --------------------------------------------------
    Retreiving data for City #620 of 753 ... pitimbu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pitimbu&units=Imperial
    --------------------------------------------------
    Retreiving data for City #621 of 753 ... samalaeulu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=samalaeulu&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #621 of 753 ... general salgado
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=general salgado&units=Imperial
    --------------------------------------------------
    Retreiving data for City #622 of 753 ... kristiinankaupunki
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kristiinankaupunki&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #622 of 753 ... pemba
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pemba&units=Imperial
    --------------------------------------------------
    Retreiving data for City #623 of 753 ... okhotsk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=okhotsk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #624 of 753 ... constantine
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=constantine&units=Imperial
    --------------------------------------------------
    Retreiving data for City #625 of 753 ... san juan de la maguana
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=san juan de la maguana&units=Imperial
    --------------------------------------------------
    Retreiving data for City #626 of 753 ... nara
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=nara&units=Imperial
    --------------------------------------------------
    Retreiving data for City #627 of 753 ... halalo
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=halalo&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #627 of 753 ... silver city
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=silver city&units=Imperial
    --------------------------------------------------
    Retreiving data for City #628 of 753 ... isfana
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=isfana&units=Imperial
    --------------------------------------------------
    Retreiving data for City #629 of 753 ... uyuni
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=uyuni&units=Imperial
    --------------------------------------------------
    Retreiving data for City #630 of 753 ... otradnoye
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=otradnoye&units=Imperial
    --------------------------------------------------
    Retreiving data for City #631 of 753 ... hefei
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hefei&units=Imperial
    --------------------------------------------------
    Retreiving data for City #632 of 753 ... havelock
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=havelock&units=Imperial
    --------------------------------------------------
    Retreiving data for City #633 of 753 ... hays
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hays&units=Imperial
    --------------------------------------------------
    Retreiving data for City #634 of 753 ... kalmunai
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kalmunai&units=Imperial
    --------------------------------------------------
    Retreiving data for City #635 of 753 ... mapiri
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=mapiri&units=Imperial
    --------------------------------------------------
    Retreiving data for City #636 of 753 ... hovd
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=hovd&units=Imperial
    --------------------------------------------------
    Retreiving data for City #637 of 753 ... dali
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=dali&units=Imperial
    --------------------------------------------------
    Retreiving data for City #638 of 753 ... waipawa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=waipawa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #639 of 753 ... manta
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=manta&units=Imperial
    --------------------------------------------------
    Retreiving data for City #640 of 753 ... qujing
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=qujing&units=Imperial
    --------------------------------------------------
    Retreiving data for City #641 of 753 ... vermillion
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vermillion&units=Imperial
    --------------------------------------------------
    Retreiving data for City #642 of 753 ... evensk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=evensk&units=Imperial
    --------------------------------------------------
    Retreiving data for City #643 of 753 ... gallup
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=gallup&units=Imperial
    --------------------------------------------------
    Retreiving data for City #644 of 753 ... shenjiamen
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=shenjiamen&units=Imperial
    --------------------------------------------------
    Retreiving data for City #645 of 753 ... bafoulabe
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bafoulabe&units=Imperial
    --------------------------------------------------
    Retreiving data for City #646 of 753 ... bilibino
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=bilibino&units=Imperial
    --------------------------------------------------
    Retreiving data for City #647 of 753 ... kavaratti
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=kavaratti&units=Imperial
    --------------------------------------------------
    Retreiving data for City #648 of 753 ... karera
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=karera&units=Imperial
    --------------------------------------------------
    Retreiving data for City #649 of 753 ... litija
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=litija&units=Imperial
    --------------------------------------------------
    Retreiving data for City #650 of 753 ... cockburn harbour
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=cockburn harbour&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #650 of 753 ... toba tek singh
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=toba tek singh&units=Imperial
    --------------------------------------------------
    Retreiving data for City #651 of 753 ... geraldton
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=geraldton&units=Imperial
    --------------------------------------------------
    Retreiving data for City #652 of 753 ... boyuibe
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=boyuibe&units=Imperial
    --------------------------------------------------
    Retreiving data for City #653 of 753 ... tahta
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tahta&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #653 of 753 ... general roca
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=general roca&units=Imperial
    --------------------------------------------------
    Retreiving data for City #654 of 753 ... paulo ramos
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=paulo ramos&units=Imperial
    --------------------------------------------------
    Retreiving data for City #655 of 753 ... san vicente
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=san vicente&units=Imperial
    --------------------------------------------------
    Retreiving data for City #656 of 753 ... okandja
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=okandja&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #656 of 753 ... barmer
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=barmer&units=Imperial
    --------------------------------------------------
    Retreiving data for City #657 of 753 ... inhambane
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=inhambane&units=Imperial
    --------------------------------------------------
    Retreiving data for City #658 of 753 ... rio gallegos
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=rio gallegos&units=Imperial
    --------------------------------------------------
    Retreiving data for City #659 of 753 ... anchorage
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=anchorage&units=Imperial
    --------------------------------------------------
    Retreiving data for City #660 of 753 ... lao cai
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=lao cai&units=Imperial
    --------------------------------------------------
    Retreiving data for City #661 of 753 ... canitas
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=canitas&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #661 of 753 ... pizarro
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=pizarro&units=Imperial
    --------------------------------------------------
    Retreiving data for City #662 of 753 ... haines junction
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=haines junction&units=Imperial
    --------------------------------------------------
    Retreiving data for City #663 of 753 ... khonuu
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=khonuu&units=Imperial
    --------------------------------------------------
    Oops! That was a wrong city name. Try again...
    --------------------------------------------------
    Retreiving data for City #663 of 753 ... chuy
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=chuy&units=Imperial
    --------------------------------------------------
    Retreiving data for City #664 of 753 ... ejido
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=ejido&units=Imperial
    --------------------------------------------------
    Retreiving data for City #665 of 753 ... vaasa
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=vaasa&units=Imperial
    --------------------------------------------------
    Retreiving data for City #666 of 753 ... la libertad
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=la libertad&units=Imperial
    --------------------------------------------------
    Retreiving data for City #667 of 753 ... tuckahoe
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=tuckahoe&units=Imperial
    --------------------------------------------------
    Retreiving data for City #668 of 753 ... svetlogorsk
    http://api.openweathermap.org/data/2.5/weather?appid=02081d218acbea82be600aa7990a6b22&q=svetlogorsk&units=Imperial
    --------------------------------------------------



```python

# create a data frame from retreived weather data
weather_dict = {
    "Date": ow_date,
    "City": ow_city,
    "Country": ow_country,
    "Latitude": ow_lat,
    "Longitude": ow_lon,
    "MaxTemp": ow_maxtmp,
    "Humidity": ow_humid,
    "Cloudiness": ow_cloud,
    "Wind Speed": ow_wind
}


weather_data = pd.DataFrame(weather_dict)

# Remove duplicates
weather_data = weather_data.drop_duplicates(["City"], keep ='first')

weather_data.head(10)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Date</th>
      <th>Humidity</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>MaxTemp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Bushehr</td>
      <td>0</td>
      <td>IR</td>
      <td>1530320400</td>
      <td>58</td>
      <td>28.97</td>
      <td>50.84</td>
      <td>84.20</td>
      <td>3.36</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Avarua</td>
      <td>8</td>
      <td>CK</td>
      <td>1530325009</td>
      <td>100</td>
      <td>-21.21</td>
      <td>-159.78</td>
      <td>73.56</td>
      <td>14.65</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Castro</td>
      <td>92</td>
      <td>CL</td>
      <td>1530325275</td>
      <td>100</td>
      <td>-42.48</td>
      <td>-73.76</td>
      <td>40.35</td>
      <td>2.91</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Guanica</td>
      <td>75</td>
      <td>PR</td>
      <td>1530323400</td>
      <td>88</td>
      <td>17.97</td>
      <td>-66.91</td>
      <td>78.80</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Boddam</td>
      <td>12</td>
      <td>GB</td>
      <td>1530323400</td>
      <td>87</td>
      <td>57.47</td>
      <td>-1.78</td>
      <td>51.80</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Atuona</td>
      <td>8</td>
      <td>PF</td>
      <td>1530325279</td>
      <td>100</td>
      <td>-9.80</td>
      <td>-139.03</td>
      <td>79.86</td>
      <td>19.46</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Albany</td>
      <td>75</td>
      <td>US</td>
      <td>1530323700</td>
      <td>51</td>
      <td>42.65</td>
      <td>-73.75</td>
      <td>80.60</td>
      <td>13.87</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Saint-Philippe</td>
      <td>1</td>
      <td>CA</td>
      <td>1530324000</td>
      <td>57</td>
      <td>45.36</td>
      <td>-73.48</td>
      <td>77.00</td>
      <td>2.24</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Artesia</td>
      <td>1</td>
      <td>US</td>
      <td>1530323700</td>
      <td>22</td>
      <td>32.84</td>
      <td>-104.40</td>
      <td>91.40</td>
      <td>10.29</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Portland</td>
      <td>20</td>
      <td>US</td>
      <td>1530323760</td>
      <td>44</td>
      <td>45.52</td>
      <td>-122.67</td>
      <td>77.00</td>
      <td>9.17</td>
    </tr>
  </tbody>
</table>
</div>




```python
weather_data.count()
```




    City          662
    Cloudiness    662
    Country       662
    Date          662
    Humidity      662
    Latitude      662
    Longitude     662
    MaxTemp       662
    Wind Speed    662
    dtype: int64




```python
# Save csv file
weather_data.to_csv("Weather_Data.csv", index=False)
```


```python
# Convert Unix Time Stamp to regular date
tmpDate = datetime.datetime.fromtimestamp(int(weather_data["Date"][0])).strftime('%Y-%m-%d')
```

# Latitude vs Temperature Plot


```python
# Build a scatter plot for each data type
plt.scatter(weather_data["Latitude"],weather_data["MaxTemp"], marker="o", alpha = 0.75)

# Incorporate the other graph properties
plt.title("City Latitude vs. Max Temperature (" + tmpDate + ")")
plt.xlabel("Latitude")
plt.ylabel("Max Temperature (F)")
plt.grid(True)

# Save the figure
plt.savefig("Images/MaxTmp.png")

# Show plot
plt.show()
```


![png](output_14_0.png)


# Latitude vs. Humidity Plot


```python
# Build a scatter plot for each data type
plt.scatter(weather_data["Latitude"],weather_data["Humidity"], marker="o", alpha = 0.75)

# Incorporate the other graph properties
plt.title("City Latitude vs. Humidity (" + tmpDate + ")")
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
plt.grid(True)

# Save the figure
plt.savefig("Images/Humidity.png")

# Show plot
plt.show()

```


![png](output_16_0.png)


# Latitude vs. Cloudiness Plot


```python
# Build a scatter plot for each data type
plt.scatter(weather_data["Latitude"],weather_data["Cloudiness"], marker="o", alpha = 0.75)

# Incorporate the other graph properties
plt.title("City Latitude vs. Cloudiness (" + tmpDate + ")")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
plt.grid(True)

# Save the figure
plt.savefig("Images/Cloudiness.png")

# Show plot
plt.show()
```


![png](output_18_0.png)


# Latitude vs. Wind Speed Plot


```python
# Build a scatter plot for each data type
plt.scatter(weather_data["Latitude"],weather_data["Wind Speed"], marker="o", alpha = 0.75)

# Incorporate the other graph properties
plt.title("City Latitude vs. Wind Speed (" + tmpDate + ")")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed (mph)")
plt.grid(True)

# Save the figure
plt.savefig("Images/WindSpeed.png")

# Show plot
plt.show()
```


![png](output_20_0.png)

