
## Temperature is maximum near the equator, decreases as you get further away from it.
## Largest Cluster of max humidity % (between 80-100%) at or near the equator.
## Clustering of cloudiness ranging between 40-90% near the equator.
## Windspeed ranges between 0-15mph near the equator, seems to increase as you get away from the equator.



```python
# Dependencies
from citipy import citipy
import random
# Dependencies
import openweathermapy.core as owm

import requests

#config
from config import api_key
#dataframe
import pandas as pd

import matplotlib.pyplot as plt
import numpy as np
from scipy.stats import sem
import datetime
import seaborn as sns
```


```python
#testing citypy
# Some random coordinates
coordinates = [(200, 200), (23, 200), (42, 100)]
#coordinates = [(500, 100), (10000, 200), (42, 100)]
city=citipy.nearest_city(200, 200)
print(f"the city is: {city}")
city = citipy.nearest_city(22.99, 120.21)
city.city_name
```

    the city is: <citipy.citipy.City object at 0x0000026DDCA98C50>
    




    'tainan'




```python
#testing openweathermap and data frame creation
# Save config information
url = "http://api.openweathermap.org/data/2.5/weather?"

# Build query URL
cities = ["Paris", "London", "Oslo", "Beijing"]

# set up lists to hold reponse info
lat = []
temp = []
humid = []
cloud = []
windspeed = []

# Loop through the list of cities and perform a request for data on each
for city in cities:
    query_url = url + "appid=" + api_key + "&q=" + city +"&units=imperial"
    #response = requests.get(query_url + city).json()
    response = requests.get(query_url)
    response_json = response.json()
    lat.append(response_json['coord']['lat'])
    temp.append(response_json['main']['temp'])
    humid.append(response_json['main']['humidity'])
    cloud.append(response_json['clouds']['all'])
    windspeed.append(response_json['wind']['speed'])


print(f"The latitude information received is: {lat}")
print(f"The temperature information received is: {temp}")
print(f"The humidty information received is: {humid}")
print(f"The cloudiness % information received is: {cloud}")
print(f"The windspeed information received is: {windspeed}")

# create a data frame from cities, lat, and temp
weather_dict = {
    "city": cities,
    "lat": lat,
    "temp": temp,
    "humid": humid,
    "cloud": cloud,
    "windspeed":windspeed
}
weather_data = pd.DataFrame(weather_dict)
weather_data.head()
```

    The latitude information received is: [48.86, 51.51, 59.91, 39.91]
    The temperature information received is: [77.43, 70.7, 59, 57.2]
    The humidty information received is: [39, 64, 30, 71]
    The cloudiness % information received is: [0, 0, 0, 90]
    The windspeed information received is: [4.7, 12.75, 18.34, 11.18]
    




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
      <th>city</th>
      <th>cloud</th>
      <th>humid</th>
      <th>lat</th>
      <th>temp</th>
      <th>windspeed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Paris</td>
      <td>0</td>
      <td>39</td>
      <td>48.86</td>
      <td>77.43</td>
      <td>4.70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>London</td>
      <td>0</td>
      <td>64</td>
      <td>51.51</td>
      <td>70.70</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Oslo</td>
      <td>0</td>
      <td>30</td>
      <td>59.91</td>
      <td>59.00</td>
      <td>18.34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Beijing</td>
      <td>90</td>
      <td>71</td>
      <td>39.91</td>
      <td>57.20</td>
      <td>11.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
cities=[]
for coordinate_pair in coordinates:
    lat, lon = coordinate_pair
    cities.append(citipy.nearest_city(lat, lon).city_name)
    
print(f"{cities}")
```

    ['leningradskiy', 'butaritari', 'jiuquan']
    


```python
#print("hello!")

# set up lists to hold reponse info
cities = []

lat=[]
lon=[]

count=0
city=''
latnow=0
lonnow=0

latnow=random.randint(-90,90)   


while count <= 600:

    latnow=round(random.uniform(-90,90),2)
    lonnow=round(random.uniform(-180,180),2)
    city=citipy.nearest_city(latnow, lonnow).city_name

    if ((city not in cities) & (city != '') & (latnow not in lat)):
        try:
            cities.append(city)
            count += 1
            lat.append(latnow)
            lat.sort()
        except exception:
            continue
        

print(f"final count is:{len(cities)}")
#print(f"{cities}")        
#print(f"lat list is {lat}")
```

    final count is:601
    


```python
#testing openweathermap
# Save config information
url = "http://api.openweathermap.org/data/2.5/weather?"
city = "london"

# Build query URL
query_url = url + "appid=" + api_key + "&q=" + city +"&units=imperial"

# Get weather data
weather_response = requests.get(query_url)
weather_json = weather_response.json()
weather_json['main']['temp']

# Get the temperature from the response
print(f"The weather API responded with: {weather_json}.")
print(f"the temp is {weather_json['main']['temp']}")
```

    The weather API responded with: {'coord': {'lon': -0.13, 'lat': 51.51}, 'weather': [{'id': 800, 'main': 'Clear', 'description': 'clear sky', 'icon': '01d'}], 'base': 'stations', 'main': {'temp': 70.7, 'pressure': 1020, 'humidity': 64, 'temp_min': 66.2, 'temp_max': 75.2}, 'visibility': 10000, 'wind': {'speed': 12.75, 'deg': 90}, 'clouds': {'all': 0}, 'dt': 1524311400, 'sys': {'type': 1, 'id': 5091, 'message': 0.0052, 'country': 'GB', 'sunrise': 1524286285, 'sunset': 1524337676}, 'id': 2643743, 'name': 'London', 'cod': 200}.
    the temp is 70.7
    


```python
# Create settings dictionary with information we're interested in
settings = {"units": "imperial", "appid": api_key}
url = "http://api.openweathermap.org/data/2.5/weather?"
count=1

lat = []
temp = []
humid = []
cloud = []
windspeed = []
weathercities = []
country = []

for city in cities:
    print(f"processing record {count} of set 1 | {city}")
    # Build query URL
    query_url = url + "appid=" + api_key + "&q=" + city +"&units=imperial"
    print(f"{query_url}")
    # Get weather data
    
    weather_response = requests.get(query_url)
    weather_json = weather_response.json()
    
    try:
        lat.append(weather_json['coord']['lat'])
        temp.append(weather_json['main']['temp_max'])
        humid.append(weather_json['main']['humidity'])
        cloud.append(weather_json['clouds']['all'])
        windspeed.append(weather_json['wind']['speed'])
        country.append(weather_json['sys']['country'])
    except Exception:
        continue    

#     #print(f"The latitude information received is: {lat}")
#     print(f"The temperature information received is: {temp}")
#     print(f"The humidty information received is: {humid}")
#     print(f"The cloudiness % information received is: {cloud}")
#     print(f"The windspeed information received is: {windspeed}")
    
    weathercities.append(city)
    count+=1
    if count > 500:
        break

# create a data frame from cities, lat, and temp
weather_dict = {
    "city": weathercities,
    "Latitude": lat,
    "Max temp": temp,
    "Humidity": humid,
    "Cloudiness": cloud,
    "Windspeed":windspeed,
    "Country":country
}
#weather_dict
weather_data = pd.DataFrame(weather_dict)
weather_data.head()

```

    processing record 1 of set 1 | safaga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=safaga&units=imperial
    processing record 1 of set 1 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=illoqqortoormiut&units=imperial
    processing record 1 of set 1 | kichera
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kichera&units=imperial
    processing record 2 of set 1 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=puerto ayora&units=imperial
    processing record 3 of set 1 | busselton
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=busselton&units=imperial
    processing record 4 of set 1 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ponta do sol&units=imperial
    processing record 5 of set 1 | havoysund
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=havoysund&units=imperial
    processing record 6 of set 1 | kahului
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kahului&units=imperial
    processing record 7 of set 1 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saskylakh&units=imperial
    processing record 8 of set 1 | lorengau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lorengau&units=imperial
    processing record 9 of set 1 | yunhe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yunhe&units=imperial
    processing record 10 of set 1 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kavaratti&units=imperial
    processing record 11 of set 1 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saint-philippe&units=imperial
    processing record 12 of set 1 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mahebourg&units=imperial
    processing record 13 of set 1 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ushuaia&units=imperial
    processing record 14 of set 1 | dongsheng
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dongsheng&units=imperial
    processing record 15 of set 1 | hobart
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hobart&units=imperial
    processing record 16 of set 1 | kaeo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kaeo&units=imperial
    processing record 17 of set 1 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=arraial do cabo&units=imperial
    processing record 18 of set 1 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=punta arenas&units=imperial
    processing record 19 of set 1 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nanortalik&units=imperial
    processing record 20 of set 1 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sao filipe&units=imperial
    processing record 21 of set 1 | hilo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hilo&units=imperial
    processing record 22 of set 1 | ancud
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ancud&units=imperial
    processing record 23 of set 1 | dikson
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dikson&units=imperial
    processing record 24 of set 1 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bredasdorp&units=imperial
    processing record 25 of set 1 | butaritari
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=butaritari&units=imperial
    processing record 26 of set 1 | francistown
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=francistown&units=imperial
    processing record 27 of set 1 | dalby
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dalby&units=imperial
    processing record 28 of set 1 | nalut
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nalut&units=imperial
    processing record 29 of set 1 | chuy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chuy&units=imperial
    processing record 30 of set 1 | thompson
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=thompson&units=imperial
    processing record 31 of set 1 | rikitea
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rikitea&units=imperial
    processing record 32 of set 1 | jamestown
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jamestown&units=imperial
    processing record 33 of set 1 | taburi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=taburi&units=imperial
    processing record 33 of set 1 | avarua
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=avarua&units=imperial
    processing record 34 of set 1 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sao joao da barra&units=imperial
    processing record 35 of set 1 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hithadhoo&units=imperial
    processing record 36 of set 1 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bambous virieux&units=imperial
    processing record 37 of set 1 | camopi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=camopi&units=imperial
    processing record 38 of set 1 | namibe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=namibe&units=imperial
    processing record 39 of set 1 | atuona
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=atuona&units=imperial
    processing record 40 of set 1 | husavik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=husavik&units=imperial
    processing record 41 of set 1 | coihaique
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=coihaique&units=imperial
    processing record 42 of set 1 | kiama
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kiama&units=imperial
    processing record 43 of set 1 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tasiilaq&units=imperial
    processing record 44 of set 1 | grand river south east
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=grand river south east&units=imperial
    processing record 44 of set 1 | vaini
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vaini&units=imperial
    processing record 45 of set 1 | chapleau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chapleau&units=imperial
    processing record 46 of set 1 | east london
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=east london&units=imperial
    processing record 47 of set 1 | albany
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=albany&units=imperial
    processing record 48 of set 1 | palabuhanratu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=palabuhanratu&units=imperial
    processing record 48 of set 1 | vaitupu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vaitupu&units=imperial
    processing record 48 of set 1 | hervey bay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hervey bay&units=imperial
    processing record 49 of set 1 | mirador
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mirador&units=imperial
    processing record 50 of set 1 | rawson
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rawson&units=imperial
    processing record 51 of set 1 | champerico
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=champerico&units=imperial
    processing record 52 of set 1 | vanimo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vanimo&units=imperial
    processing record 53 of set 1 | hermanus
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hermanus&units=imperial
    processing record 54 of set 1 | souillac
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=souillac&units=imperial
    processing record 55 of set 1 | ejido hermosillo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ejido hermosillo&units=imperial
    processing record 56 of set 1 | malinovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=malinovskiy&units=imperial
    processing record 57 of set 1 | kapaa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kapaa&units=imperial
    processing record 58 of set 1 | mataura
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mataura&units=imperial
    processing record 59 of set 1 | vilhena
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vilhena&units=imperial
    processing record 60 of set 1 | emba
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=emba&units=imperial
    processing record 61 of set 1 | formoso do araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=formoso do araguaia&units=imperial
    processing record 61 of set 1 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bengkulu&units=imperial
    processing record 61 of set 1 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=half moon bay&units=imperial
    processing record 62 of set 1 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fort nelson&units=imperial
    processing record 63 of set 1 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ilulissat&units=imperial
    processing record 64 of set 1 | misratah
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=misratah&units=imperial
    processing record 65 of set 1 | punta alta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=punta alta&units=imperial
    processing record 66 of set 1 | palana
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=palana&units=imperial
    processing record 67 of set 1 | gosh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gosh&units=imperial
    processing record 68 of set 1 | that phanom
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=that phanom&units=imperial
    processing record 69 of set 1 | sungaipenuh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sungaipenuh&units=imperial
    processing record 70 of set 1 | talnakh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=talnakh&units=imperial
    processing record 71 of set 1 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cherskiy&units=imperial
    processing record 72 of set 1 | shache
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=shache&units=imperial
    processing record 73 of set 1 | svetlogorsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=svetlogorsk&units=imperial
    processing record 74 of set 1 | saldanha
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saldanha&units=imperial
    processing record 75 of set 1 | traverse city
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=traverse city&units=imperial
    processing record 76 of set 1 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kaitangata&units=imperial
    processing record 77 of set 1 | atambua
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=atambua&units=imperial
    processing record 78 of set 1 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ribeira grande&units=imperial
    processing record 79 of set 1 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nizhneyansk&units=imperial
    processing record 79 of set 1 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=iqaluit&units=imperial
    processing record 80 of set 1 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=taolanaro&units=imperial
    processing record 80 of set 1 | puga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=puga&units=imperial
    processing record 81 of set 1 | georgetown
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=georgetown&units=imperial
    processing record 82 of set 1 | sultanpur
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sultanpur&units=imperial
    processing record 83 of set 1 | tsihombe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tsihombe&units=imperial
    processing record 83 of set 1 | bethel
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bethel&units=imperial
    processing record 84 of set 1 | samusu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=samusu&units=imperial
    processing record 84 of set 1 | karratha
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=karratha&units=imperial
    processing record 85 of set 1 | jacareacanga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jacareacanga&units=imperial
    processing record 86 of set 1 | pleshanovo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pleshanovo&units=imperial
    processing record 87 of set 1 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=meulaboh&units=imperial
    processing record 88 of set 1 | saleaula
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saleaula&units=imperial
    processing record 88 of set 1 | bluff
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bluff&units=imperial
    processing record 89 of set 1 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mar del plata&units=imperial
    processing record 90 of set 1 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chokurdakh&units=imperial
    processing record 91 of set 1 | bathsheba
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bathsheba&units=imperial
    processing record 92 of set 1 | hobyo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hobyo&units=imperial
    processing record 93 of set 1 | nantucket
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nantucket&units=imperial
    processing record 94 of set 1 | aklavik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=aklavik&units=imperial
    processing record 95 of set 1 | cape town
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cape town&units=imperial
    processing record 96 of set 1 | puerto del rosario
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=puerto del rosario&units=imperial
    processing record 97 of set 1 | hambantota
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hambantota&units=imperial
    processing record 98 of set 1 | port alfred
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port alfred&units=imperial
    processing record 99 of set 1 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=coquimbo&units=imperial
    processing record 100 of set 1 | saint anthony
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saint anthony&units=imperial
    processing record 101 of set 1 | umzimvubu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=umzimvubu&units=imperial
    processing record 101 of set 1 | la plata
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=la plata&units=imperial
    processing record 102 of set 1 | portland
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=portland&units=imperial
    processing record 103 of set 1 | mendahara
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mendahara&units=imperial
    processing record 103 of set 1 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=severo-kurilsk&units=imperial
    processing record 104 of set 1 | ust-koksa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ust-koksa&units=imperial
    processing record 105 of set 1 | zhiryatino
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=zhiryatino&units=imperial
    processing record 106 of set 1 | malwan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=malwan&units=imperial
    processing record 106 of set 1 | barrow
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=barrow&units=imperial
    processing record 107 of set 1 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=san cristobal&units=imperial
    processing record 108 of set 1 | puerto lleras
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=puerto lleras&units=imperial
    processing record 109 of set 1 | hofn
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hofn&units=imperial
    processing record 110 of set 1 | vao
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vao&units=imperial
    processing record 111 of set 1 | baruun-urt
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=baruun-urt&units=imperial
    processing record 112 of set 1 | nouadhibou
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nouadhibou&units=imperial
    processing record 113 of set 1 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=carnarvon&units=imperial
    processing record 114 of set 1 | eugene
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=eugene&units=imperial
    processing record 115 of set 1 | saint george
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saint george&units=imperial
    processing record 116 of set 1 | lebu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lebu&units=imperial
    processing record 117 of set 1 | tambopata
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tambopata&units=imperial
    processing record 117 of set 1 | ponta delgada
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ponta delgada&units=imperial
    processing record 118 of set 1 | katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=katsuura&units=imperial
    processing record 119 of set 1 | gizo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gizo&units=imperial
    processing record 120 of set 1 | norman wells
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=norman wells&units=imperial
    processing record 121 of set 1 | okhotsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=okhotsk&units=imperial
    processing record 122 of set 1 | geraldton
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=geraldton&units=imperial
    processing record 123 of set 1 | kodiak
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kodiak&units=imperial
    processing record 124 of set 1 | lusaka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lusaka&units=imperial
    processing record 125 of set 1 | belushya guba
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=belushya guba&units=imperial
    processing record 125 of set 1 | hasaki
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hasaki&units=imperial
    processing record 126 of set 1 | purranque
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=purranque&units=imperial
    processing record 127 of set 1 | ler
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ler&units=imperial
    processing record 128 of set 1 | tommot
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tommot&units=imperial
    processing record 129 of set 1 | wodonga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=wodonga&units=imperial
    processing record 130 of set 1 | port hardy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port hardy&units=imperial
    processing record 131 of set 1 | yonago
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yonago&units=imperial
    processing record 132 of set 1 | oriximina
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=oriximina&units=imperial
    processing record 133 of set 1 | pentaplatanon
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pentaplatanon&units=imperial
    processing record 134 of set 1 | camacha
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=camacha&units=imperial
    processing record 135 of set 1 | nyurba
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nyurba&units=imperial
    processing record 136 of set 1 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=klaksvik&units=imperial
    processing record 137 of set 1 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nikolskoye&units=imperial
    processing record 138 of set 1 | payson
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=payson&units=imperial
    processing record 139 of set 1 | hualmay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hualmay&units=imperial
    processing record 140 of set 1 | billings
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=billings&units=imperial
    processing record 141 of set 1 | sitka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sitka&units=imperial
    processing record 142 of set 1 | gloggnitz
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gloggnitz&units=imperial
    processing record 143 of set 1 | airai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=airai&units=imperial
    processing record 144 of set 1 | narayangarh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=narayangarh&units=imperial
    processing record 145 of set 1 | tessalit
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tessalit&units=imperial
    processing record 146 of set 1 | panorama
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=panorama&units=imperial
    processing record 147 of set 1 | iquique
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=iquique&units=imperial
    processing record 148 of set 1 | padang
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=padang&units=imperial
    processing record 149 of set 1 | villazon
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=villazon&units=imperial
    processing record 149 of set 1 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mys shmidta&units=imperial
    processing record 149 of set 1 | la puebla de cazalla
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=la puebla de cazalla&units=imperial
    processing record 150 of set 1 | burnie
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=burnie&units=imperial
    processing record 151 of set 1 | waspan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=waspan&units=imperial
    processing record 151 of set 1 | cartagena
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cartagena&units=imperial
    processing record 152 of set 1 | leh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=leh&units=imperial
    processing record 153 of set 1 | torbay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=torbay&units=imperial
    processing record 154 of set 1 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cabo san lucas&units=imperial
    processing record 155 of set 1 | attawapiskat
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=attawapiskat&units=imperial
    processing record 155 of set 1 | grand-lahou
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=grand-lahou&units=imperial
    processing record 156 of set 1 | dustlik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dustlik&units=imperial
    processing record 157 of set 1 | fare
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fare&units=imperial
    processing record 158 of set 1 | fukue
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fukue&units=imperial
    processing record 159 of set 1 | yuncheng
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yuncheng&units=imperial
    processing record 160 of set 1 | sainte-rose
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sainte-rose&units=imperial
    processing record 161 of set 1 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port elizabeth&units=imperial
    processing record 162 of set 1 | harbour breton
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=harbour breton&units=imperial
    processing record 163 of set 1 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yellowknife&units=imperial
    processing record 164 of set 1 | nador
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nador&units=imperial
    processing record 165 of set 1 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=qaanaaq&units=imperial
    processing record 166 of set 1 | tumannyy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tumannyy&units=imperial
    processing record 166 of set 1 | borama
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=borama&units=imperial
    processing record 166 of set 1 | pimentel
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pimentel&units=imperial
    processing record 167 of set 1 | mahibadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mahibadhoo&units=imperial
    processing record 168 of set 1 | pacific grove
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pacific grove&units=imperial
    processing record 169 of set 1 | katangli
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=katangli&units=imperial
    processing record 170 of set 1 | kalmunai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kalmunai&units=imperial
    processing record 171 of set 1 | marcona
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=marcona&units=imperial
    processing record 171 of set 1 | trelew
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=trelew&units=imperial
    processing record 172 of set 1 | anadyr
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=anadyr&units=imperial
    processing record 173 of set 1 | virginia beach
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=virginia beach&units=imperial
    processing record 174 of set 1 | russell
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=russell&units=imperial
    processing record 175 of set 1 | esperance
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=esperance&units=imperial
    processing record 176 of set 1 | hamilton
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hamilton&units=imperial
    processing record 177 of set 1 | pecos
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pecos&units=imperial
    processing record 178 of set 1 | quixada
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=quixada&units=imperial
    processing record 178 of set 1 | honningsvag
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=honningsvag&units=imperial
    processing record 179 of set 1 | plettenberg bay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=plettenberg bay&units=imperial
    processing record 180 of set 1 | izhma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=izhma&units=imperial
    processing record 181 of set 1 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=new norfolk&units=imperial
    processing record 182 of set 1 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kruisfontein&units=imperial
    processing record 183 of set 1 | te anau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=te anau&units=imperial
    processing record 184 of set 1 | jinchang
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jinchang&units=imperial
    processing record 185 of set 1 | codrington
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=codrington&units=imperial
    processing record 186 of set 1 | lagoa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lagoa&units=imperial
    processing record 187 of set 1 | touros
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=touros&units=imperial
    processing record 188 of set 1 | port-gentil
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port-gentil&units=imperial
    processing record 189 of set 1 | manitouwadge
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=manitouwadge&units=imperial
    processing record 190 of set 1 | bosaso
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bosaso&units=imperial
    processing record 191 of set 1 | marzuq
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=marzuq&units=imperial
    processing record 192 of set 1 | pisco
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pisco&units=imperial
    processing record 193 of set 1 | walvis bay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=walvis bay&units=imperial
    processing record 194 of set 1 | sorvag
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sorvag&units=imperial
    processing record 194 of set 1 | shimoda
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=shimoda&units=imperial
    processing record 195 of set 1 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=barentsburg&units=imperial
    processing record 195 of set 1 | salalah
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=salalah&units=imperial
    processing record 196 of set 1 | arlit
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=arlit&units=imperial
    processing record 197 of set 1 | tiksi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tiksi&units=imperial
    processing record 198 of set 1 | denpasar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=denpasar&units=imperial
    processing record 199 of set 1 | gualeguay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gualeguay&units=imperial
    processing record 200 of set 1 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mount gambier&units=imperial
    processing record 201 of set 1 | chenghai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chenghai&units=imperial
    processing record 202 of set 1 | marsa matruh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=marsa matruh&units=imperial
    processing record 203 of set 1 | nome
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nome&units=imperial
    processing record 204 of set 1 | sawtell
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sawtell&units=imperial
    processing record 205 of set 1 | lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lincoln&units=imperial
    processing record 206 of set 1 | beroroha
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=beroroha&units=imperial
    processing record 207 of set 1 | egvekinot
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=egvekinot&units=imperial
    processing record 208 of set 1 | caucaia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=caucaia&units=imperial
    processing record 209 of set 1 | mandurah
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mandurah&units=imperial
    processing record 210 of set 1 | amderma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=amderma&units=imperial
    processing record 210 of set 1 | linxia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=linxia&units=imperial
    processing record 211 of set 1 | bousso
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bousso&units=imperial
    processing record 211 of set 1 | yar-sale
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yar-sale&units=imperial
    processing record 212 of set 1 | lodwar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lodwar&units=imperial
    processing record 213 of set 1 | benin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=benin&units=imperial
    processing record 213 of set 1 | ortona
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ortona&units=imperial
    processing record 214 of set 1 | gongzhuling
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gongzhuling&units=imperial
    processing record 215 of set 1 | addis zemen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=addis zemen&units=imperial
    processing record 215 of set 1 | makakilo city
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=makakilo city&units=imperial
    processing record 216 of set 1 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bandarbeyla&units=imperial
    processing record 217 of set 1 | clyde river
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=clyde river&units=imperial
    processing record 218 of set 1 | douentza
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=douentza&units=imperial
    processing record 219 of set 1 | kuche
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kuche&units=imperial
    processing record 219 of set 1 | george
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=george&units=imperial
    processing record 220 of set 1 | raudeberg
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=raudeberg&units=imperial
    processing record 221 of set 1 | salmas
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=salmas&units=imperial
    processing record 222 of set 1 | oussouye
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=oussouye&units=imperial
    processing record 223 of set 1 | coruripe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=coruripe&units=imperial
    processing record 224 of set 1 | carnduff
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=carnduff&units=imperial
    processing record 225 of set 1 | marawi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=marawi&units=imperial
    processing record 226 of set 1 | hue
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hue&units=imperial
    processing record 227 of set 1 | rossland
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rossland&units=imperial
    processing record 228 of set 1 | agadir
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=agadir&units=imperial
    processing record 229 of set 1 | alofi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=alofi&units=imperial
    processing record 230 of set 1 | adrar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=adrar&units=imperial
    processing record 231 of set 1 | saint-pierre
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saint-pierre&units=imperial
    processing record 232 of set 1 | sherghati
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sherghati&units=imperial
    processing record 233 of set 1 | bonthe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bonthe&units=imperial
    processing record 234 of set 1 | nishihara
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nishihara&units=imperial
    processing record 235 of set 1 | bargal
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bargal&units=imperial
    processing record 235 of set 1 | victoria
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=victoria&units=imperial
    processing record 236 of set 1 | noumea
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=noumea&units=imperial
    processing record 237 of set 1 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=beringovskiy&units=imperial
    processing record 238 of set 1 | kouango
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kouango&units=imperial
    processing record 239 of set 1 | bo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bo&units=imperial
    processing record 239 of set 1 | wahpeton
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=wahpeton&units=imperial
    processing record 240 of set 1 | ternate
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ternate&units=imperial
    processing record 241 of set 1 | baie-comeau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=baie-comeau&units=imperial
    processing record 242 of set 1 | tauranga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tauranga&units=imperial
    processing record 243 of set 1 | alice springs
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=alice springs&units=imperial
    processing record 244 of set 1 | santa rosa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=santa rosa&units=imperial
    processing record 245 of set 1 | cidreira
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cidreira&units=imperial
    processing record 246 of set 1 | junin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=junin&units=imperial
    processing record 247 of set 1 | the valley
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=the valley&units=imperial
    processing record 248 of set 1 | rincon
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rincon&units=imperial
    processing record 249 of set 1 | luderitz
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=luderitz&units=imperial
    processing record 250 of set 1 | isangel
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=isangel&units=imperial
    processing record 251 of set 1 | methoni
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=methoni&units=imperial
    processing record 252 of set 1 | jega
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jega&units=imperial
    processing record 253 of set 1 | dingle
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dingle&units=imperial
    processing record 254 of set 1 | tasbuget
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tasbuget&units=imperial
    processing record 254 of set 1 | esso
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=esso&units=imperial
    processing record 255 of set 1 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port lincoln&units=imperial
    processing record 256 of set 1 | castro
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=castro&units=imperial
    processing record 257 of set 1 | cagliari
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cagliari&units=imperial
    processing record 258 of set 1 | bend
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bend&units=imperial
    processing record 259 of set 1 | yima
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yima&units=imperial
    processing record 260 of set 1 | bundaberg
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bundaberg&units=imperial
    processing record 261 of set 1 | sambava
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sambava&units=imperial
    processing record 262 of set 1 | manjo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=manjo&units=imperial
    processing record 263 of set 1 | bilma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bilma&units=imperial
    processing record 264 of set 1 | port hedland
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port hedland&units=imperial
    processing record 265 of set 1 | maghama
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=maghama&units=imperial
    processing record 265 of set 1 | muros
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=muros&units=imperial
    processing record 266 of set 1 | babushkin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=babushkin&units=imperial
    processing record 267 of set 1 | copperas cove
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=copperas cove&units=imperial
    processing record 268 of set 1 | kanepi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kanepi&units=imperial
    processing record 269 of set 1 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tuktoyaktuk&units=imperial
    processing record 270 of set 1 | nukus
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nukus&units=imperial
    processing record 271 of set 1 | maragogi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=maragogi&units=imperial
    processing record 272 of set 1 | nguiu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nguiu&units=imperial
    processing record 272 of set 1 | sabya
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sabya&units=imperial
    processing record 273 of set 1 | ucluelet
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ucluelet&units=imperial
    processing record 274 of set 1 | broome
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=broome&units=imperial
    processing record 275 of set 1 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pangnirtung&units=imperial
    processing record 276 of set 1 | alta floresta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=alta floresta&units=imperial
    processing record 277 of set 1 | viru
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=viru&units=imperial
    processing record 278 of set 1 | tigil
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tigil&units=imperial
    processing record 279 of set 1 | avera
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=avera&units=imperial
    processing record 280 of set 1 | leningradskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=leningradskiy&units=imperial
    processing record 281 of set 1 | cabul-an
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cabul-an&units=imperial
    processing record 282 of set 1 | namatanai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=namatanai&units=imperial
    processing record 283 of set 1 | port macquarie
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port macquarie&units=imperial
    processing record 284 of set 1 | phuntsholing
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=phuntsholing&units=imperial
    processing record 285 of set 1 | aguimes
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=aguimes&units=imperial
    processing record 286 of set 1 | chikoy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chikoy&units=imperial
    processing record 286 of set 1 | les cayes
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=les cayes&units=imperial
    processing record 287 of set 1 | cabra
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cabra&units=imperial
    processing record 288 of set 1 | tutoia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tutoia&units=imperial
    processing record 289 of set 1 | skelleftea
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=skelleftea&units=imperial
    processing record 290 of set 1 | nyeri
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nyeri&units=imperial
    processing record 291 of set 1 | inhambane
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=inhambane&units=imperial
    processing record 292 of set 1 | alenquer
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=alenquer&units=imperial
    processing record 293 of set 1 | warmbad
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=warmbad&units=imperial
    processing record 294 of set 1 | channel-port aux basques
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=channel-port aux basques&units=imperial
    processing record 295 of set 1 | pore
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pore&units=imperial
    processing record 296 of set 1 | verkhnyaya toyma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=verkhnyaya toyma&units=imperial
    processing record 297 of set 1 | matagami
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=matagami&units=imperial
    processing record 298 of set 1 | mendota
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mendota&units=imperial
    processing record 299 of set 1 | wilmington
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=wilmington&units=imperial
    processing record 300 of set 1 | cayenne
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cayenne&units=imperial
    processing record 301 of set 1 | palaiokhora
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=palaiokhora&units=imperial
    processing record 301 of set 1 | los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=los llanos de aridane&units=imperial
    processing record 302 of set 1 | perth
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=perth&units=imperial
    processing record 303 of set 1 | sheridan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sheridan&units=imperial
    processing record 304 of set 1 | vilcun
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vilcun&units=imperial
    processing record 305 of set 1 | yongan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yongan&units=imperial
    processing record 306 of set 1 | xinmin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=xinmin&units=imperial
    processing record 307 of set 1 | chester
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chester&units=imperial
    processing record 308 of set 1 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=guerrero negro&units=imperial
    processing record 309 of set 1 | waipawa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=waipawa&units=imperial
    processing record 310 of set 1 | magalia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=magalia&units=imperial
    processing record 311 of set 1 | gladstone
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gladstone&units=imperial
    processing record 312 of set 1 | barhi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=barhi&units=imperial
    processing record 313 of set 1 | pokrovsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pokrovsk&units=imperial
    processing record 314 of set 1 | oktyabrskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=oktyabrskiy&units=imperial
    processing record 315 of set 1 | puerto escondido
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=puerto escondido&units=imperial
    processing record 316 of set 1 | zhigansk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=zhigansk&units=imperial
    processing record 317 of set 1 | okato
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=okato&units=imperial
    processing record 318 of set 1 | mount isa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mount isa&units=imperial
    processing record 319 of set 1 | ulaangom
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ulaangom&units=imperial
    processing record 320 of set 1 | vardo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vardo&units=imperial
    processing record 321 of set 1 | panguna
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=panguna&units=imperial
    processing record 322 of set 1 | prieska
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=prieska&units=imperial
    processing record 323 of set 1 | pochutla
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pochutla&units=imperial
    processing record 324 of set 1 | cabedelo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cabedelo&units=imperial
    processing record 325 of set 1 | paso de los toros
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=paso de los toros&units=imperial
    processing record 326 of set 1 | susanville
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=susanville&units=imperial
    processing record 327 of set 1 | jiuquan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jiuquan&units=imperial
    processing record 328 of set 1 | rungata
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rungata&units=imperial
    processing record 328 of set 1 | muisne
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=muisne&units=imperial
    processing record 329 of set 1 | fournoi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fournoi&units=imperial
    processing record 329 of set 1 | nuuk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nuuk&units=imperial
    processing record 330 of set 1 | kathmandu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kathmandu&units=imperial
    processing record 331 of set 1 | berdigestyakh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=berdigestyakh&units=imperial
    processing record 332 of set 1 | maraba
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=maraba&units=imperial
    processing record 333 of set 1 | thinadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=thinadhoo&units=imperial
    processing record 334 of set 1 | hengshui
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hengshui&units=imperial
    processing record 335 of set 1 | narsaq
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=narsaq&units=imperial
    processing record 336 of set 1 | skalistyy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=skalistyy&units=imperial
    processing record 336 of set 1 | mayo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mayo&units=imperial
    processing record 337 of set 1 | haibowan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=haibowan&units=imperial
    processing record 337 of set 1 | kingsville
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kingsville&units=imperial
    processing record 338 of set 1 | petropavlovsk-kamchatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=petropavlovsk-kamchatskiy&units=imperial
    processing record 339 of set 1 | emerald
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=emerald&units=imperial
    processing record 340 of set 1 | santa cruz de rosales
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=santa cruz de rosales&units=imperial
    processing record 340 of set 1 | santiago del estero
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=santiago del estero&units=imperial
    processing record 341 of set 1 | vestmannaeyjar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vestmannaeyjar&units=imperial
    processing record 342 of set 1 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tuatapere&units=imperial
    processing record 343 of set 1 | patpata
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=patpata&units=imperial
    processing record 343 of set 1 | askiz
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=askiz&units=imperial
    processing record 344 of set 1 | sisimiut
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sisimiut&units=imperial
    processing record 345 of set 1 | chkalovsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chkalovsk&units=imperial
    processing record 346 of set 1 | caraballeda
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=caraballeda&units=imperial
    processing record 347 of set 1 | porosozero
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=porosozero&units=imperial
    processing record 348 of set 1 | caravelas
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=caravelas&units=imperial
    processing record 349 of set 1 | harper
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=harper&units=imperial
    processing record 350 of set 1 | otradnoye
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=otradnoye&units=imperial
    processing record 351 of set 1 | voh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=voh&units=imperial
    processing record 352 of set 1 | huanren
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=huanren&units=imperial
    processing record 353 of set 1 | conakry
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=conakry&units=imperial
    processing record 354 of set 1 | san patricio
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=san patricio&units=imperial
    processing record 355 of set 1 | ormara
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ormara&units=imperial
    processing record 356 of set 1 | katobu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=katobu&units=imperial
    processing record 357 of set 1 | usinsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=usinsk&units=imperial
    processing record 358 of set 1 | blackwater
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=blackwater&units=imperial
    processing record 359 of set 1 | hare bay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hare bay&units=imperial
    processing record 360 of set 1 | belmonte
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=belmonte&units=imperial
    processing record 361 of set 1 | do gonbadan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=do gonbadan&units=imperial
    processing record 362 of set 1 | provideniya
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=provideniya&units=imperial
    processing record 363 of set 1 | atbasar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=atbasar&units=imperial
    processing record 364 of set 1 | warrington
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=warrington&units=imperial
    processing record 365 of set 1 | yinchuan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yinchuan&units=imperial
    processing record 366 of set 1 | paradwip
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=paradwip&units=imperial
    processing record 366 of set 1 | olafsvik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=olafsvik&units=imperial
    processing record 366 of set 1 | lafia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lafia&units=imperial
    processing record 367 of set 1 | bom jardim
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bom jardim&units=imperial
    processing record 368 of set 1 | xingcheng
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=xingcheng&units=imperial
    processing record 369 of set 1 | haines junction
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=haines junction&units=imperial
    processing record 370 of set 1 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fairbanks&units=imperial
    processing record 371 of set 1 | caucasia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=caucasia&units=imperial
    processing record 372 of set 1 | norsup
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=norsup&units=imperial
    processing record 373 of set 1 | poso
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=poso&units=imperial
    processing record 374 of set 1 | abja-paluoja
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=abja-paluoja&units=imperial
    processing record 375 of set 1 | tautira
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tautira&units=imperial
    processing record 376 of set 1 | safford
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=safford&units=imperial
    processing record 377 of set 1 | novikovo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=novikovo&units=imperial
    processing record 378 of set 1 | goma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=goma&units=imperial
    processing record 379 of set 1 | kambove
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kambove&units=imperial
    processing record 380 of set 1 | kjollefjord
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kjollefjord&units=imperial
    processing record 381 of set 1 | ilanskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ilanskiy&units=imperial
    processing record 382 of set 1 | suzun
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=suzun&units=imperial
    processing record 383 of set 1 | xiaolingwei
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=xiaolingwei&units=imperial
    processing record 384 of set 1 | margate
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=margate&units=imperial
    processing record 385 of set 1 | muzaffarabad
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=muzaffarabad&units=imperial
    processing record 386 of set 1 | magadan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=magadan&units=imperial
    processing record 387 of set 1 | khatanga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=khatanga&units=imperial
    processing record 388 of set 1 | upernavik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=upernavik&units=imperial
    processing record 389 of set 1 | stepnyak
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=stepnyak&units=imperial
    processing record 390 of set 1 | seoul
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=seoul&units=imperial
    processing record 391 of set 1 | rio brilhante
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rio brilhante&units=imperial
    processing record 392 of set 1 | meyungs
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=meyungs&units=imperial
    processing record 392 of set 1 | north myrtle beach
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=north myrtle beach&units=imperial
    processing record 393 of set 1 | znamenskoye
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=znamenskoye&units=imperial
    processing record 394 of set 1 | roma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=roma&units=imperial
    processing record 395 of set 1 | paamiut
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=paamiut&units=imperial
    processing record 396 of set 1 | la sarre
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=la sarre&units=imperial
    processing record 397 of set 1 | williston
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=williston&units=imperial
    processing record 398 of set 1 | fallon
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fallon&units=imperial
    processing record 399 of set 1 | faya
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=faya&units=imperial
    processing record 400 of set 1 | bud
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bud&units=imperial
    processing record 401 of set 1 | dicabisagan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dicabisagan&units=imperial
    processing record 402 of set 1 | kielce
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kielce&units=imperial
    processing record 403 of set 1 | dzhebariki-khaya
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dzhebariki-khaya&units=imperial
    processing record 404 of set 1 | jining
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jining&units=imperial
    processing record 405 of set 1 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=longyearbyen&units=imperial
    processing record 406 of set 1 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ostrovnoy&units=imperial
    processing record 407 of set 1 | bahia blanca
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bahia blanca&units=imperial
    processing record 408 of set 1 | atsiki
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=atsiki&units=imperial
    processing record 408 of set 1 | ambovombe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ambovombe&units=imperial
    processing record 409 of set 1 | leshukonskoye
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=leshukonskoye&units=imperial
    processing record 410 of set 1 | okha
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=okha&units=imperial
    processing record 411 of set 1 | dhamnod
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dhamnod&units=imperial
    processing record 412 of set 1 | cap-aux-meules
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cap-aux-meules&units=imperial
    processing record 413 of set 1 | payakumbuh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=payakumbuh&units=imperial
    processing record 414 of set 1 | halalo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=halalo&units=imperial
    processing record 414 of set 1 | flinders
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=flinders&units=imperial
    processing record 415 of set 1 | mandalgovi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mandalgovi&units=imperial
    processing record 416 of set 1 | sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sentyabrskiy&units=imperial
    processing record 416 of set 1 | zapolyarnyy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=zapolyarnyy&units=imperial
    processing record 417 of set 1 | sibolga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sibolga&units=imperial
    processing record 418 of set 1 | ussel
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ussel&units=imperial
    processing record 419 of set 1 | tuy hoa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tuy hoa&units=imperial
    processing record 420 of set 1 | dukat
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dukat&units=imperial
    processing record 421 of set 1 | port augusta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port augusta&units=imperial
    processing record 422 of set 1 | zonguldak
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=zonguldak&units=imperial
    processing record 423 of set 1 | manokwari
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=manokwari&units=imperial
    processing record 424 of set 1 | salinas
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=salinas&units=imperial
    processing record 425 of set 1 | novopokrovka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=novopokrovka&units=imperial
    processing record 426 of set 1 | juneau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=juneau&units=imperial
    processing record 427 of set 1 | dudinka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dudinka&units=imperial
    processing record 428 of set 1 | warren
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=warren&units=imperial
    processing record 429 of set 1 | xichang
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=xichang&units=imperial
    processing record 430 of set 1 | kortkeros
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kortkeros&units=imperial
    processing record 431 of set 1 | gat
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gat&units=imperial
    processing record 432 of set 1 | pindiga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pindiga&units=imperial
    processing record 433 of set 1 | butajira
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=butajira&units=imperial
    processing record 434 of set 1 | matara
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=matara&units=imperial
    processing record 435 of set 1 | georgiyevka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=georgiyevka&units=imperial
    processing record 436 of set 1 | big rapids
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=big rapids&units=imperial
    processing record 437 of set 1 | charyshskoye
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=charyshskoye&units=imperial
    processing record 438 of set 1 | lasa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lasa&units=imperial
    processing record 439 of set 1 | urubicha
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=urubicha&units=imperial
    processing record 440 of set 1 | faanui
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=faanui&units=imperial
    processing record 441 of set 1 | cabatuan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cabatuan&units=imperial
    processing record 442 of set 1 | fort payne
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fort payne&units=imperial
    processing record 443 of set 1 | chupa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chupa&units=imperial
    processing record 444 of set 1 | lamesa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lamesa&units=imperial
    processing record 445 of set 1 | edd
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=edd&units=imperial
    processing record 446 of set 1 | uray
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=uray&units=imperial
    processing record 447 of set 1 | praia da vitoria
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=praia da vitoria&units=imperial
    processing record 448 of set 1 | ranfurly
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ranfurly&units=imperial
    processing record 449 of set 1 | mingshui
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mingshui&units=imperial
    processing record 450 of set 1 | bredy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bredy&units=imperial
    processing record 451 of set 1 | buenos aires
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=buenos aires&units=imperial
    processing record 452 of set 1 | cap malheureux
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cap malheureux&units=imperial
    processing record 453 of set 1 | swan hill
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=swan hill&units=imperial
    processing record 454 of set 1 | golden
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=golden&units=imperial
    processing record 455 of set 1 | tokamachi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tokamachi&units=imperial
    processing record 456 of set 1 | roald
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=roald&units=imperial
    processing record 457 of set 1 | porto novo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=porto novo&units=imperial
    processing record 458 of set 1 | kavieng
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kavieng&units=imperial
    processing record 459 of set 1 | mogadishu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mogadishu&units=imperial
    processing record 460 of set 1 | mantua
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mantua&units=imperial
    processing record 461 of set 1 | ngukurr
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ngukurr&units=imperial
    processing record 461 of set 1 | serenje
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=serenje&units=imperial
    processing record 462 of set 1 | brae
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=brae&units=imperial
    processing record 463 of set 1 | olinda
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=olinda&units=imperial
    processing record 464 of set 1 | lompoc
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lompoc&units=imperial
    processing record 465 of set 1 | silver city
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=silver city&units=imperial
    processing record 466 of set 1 | hirara
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hirara&units=imperial
    processing record 467 of set 1 | norrtalje
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=norrtalje&units=imperial
    processing record 468 of set 1 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vila franca do campo&units=imperial
    processing record 469 of set 1 | yeppoon
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yeppoon&units=imperial
    processing record 470 of set 1 | baijiantan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=baijiantan&units=imperial
    processing record 471 of set 1 | gazanjyk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gazanjyk&units=imperial
    processing record 472 of set 1 | atar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=atar&units=imperial
    processing record 473 of set 1 | vallenar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vallenar&units=imperial
    processing record 474 of set 1 | canton
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=canton&units=imperial
    processing record 475 of set 1 | baracoa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=baracoa&units=imperial
    processing record 476 of set 1 | tacuarembo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tacuarembo&units=imperial
    processing record 477 of set 1 | vossevangen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vossevangen&units=imperial
    processing record 478 of set 1 | krasnoarmeyskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=krasnoarmeyskiy&units=imperial
    processing record 479 of set 1 | daliao
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=daliao&units=imperial
    processing record 480 of set 1 | sorab
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sorab&units=imperial
    processing record 481 of set 1 | dunedin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dunedin&units=imperial
    processing record 482 of set 1 | scottsbluff
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=scottsbluff&units=imperial
    processing record 483 of set 1 | whitehorse
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=whitehorse&units=imperial
    processing record 484 of set 1 | alekseyevka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=alekseyevka&units=imperial
    processing record 485 of set 1 | fort-shevchenko
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fort-shevchenko&units=imperial
    processing record 486 of set 1 | guilin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=guilin&units=imperial
    processing record 487 of set 1 | tondano
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tondano&units=imperial
    processing record 488 of set 1 | balkanabat
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=balkanabat&units=imperial
    processing record 489 of set 1 | san carlos
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=san carlos&units=imperial
    processing record 490 of set 1 | broken hill
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=broken hill&units=imperial
    processing record 491 of set 1 | bontang
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bontang&units=imperial
    processing record 492 of set 1 | sinnamary
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sinnamary&units=imperial
    processing record 493 of set 1 | pevek
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pevek&units=imperial
    processing record 494 of set 1 | montague
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=montague&units=imperial
    processing record 495 of set 1 | batemans bay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=batemans bay&units=imperial
    processing record 496 of set 1 | constitucion
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=constitucion&units=imperial
    processing record 497 of set 1 | loningen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=loningen&units=imperial
    processing record 498 of set 1 | tezu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tezu&units=imperial
    processing record 499 of set 1 | smolenka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=smolenka&units=imperial
    processing record 500 of set 1 | baykit
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=baykit&units=imperial
    




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
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Humidity</th>
      <th>Latitude</th>
      <th>Max temp</th>
      <th>Windspeed</th>
      <th>city</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>RU</td>
      <td>68</td>
      <td>55.93</td>
      <td>28.28</td>
      <td>2.62</td>
      <td>kichera</td>
    </tr>
    <tr>
      <th>1</th>
      <td>68</td>
      <td>EC</td>
      <td>100</td>
      <td>-0.74</td>
      <td>74.18</td>
      <td>5.53</td>
      <td>puerto ayora</td>
    </tr>
    <tr>
      <th>2</th>
      <td>92</td>
      <td>AU</td>
      <td>100</td>
      <td>-33.64</td>
      <td>65.09</td>
      <td>12.19</td>
      <td>busselton</td>
    </tr>
    <tr>
      <th>3</th>
      <td>80</td>
      <td>BR</td>
      <td>70</td>
      <td>-20.63</td>
      <td>71.21</td>
      <td>4.25</td>
      <td>ponta do sol</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>NO</td>
      <td>69</td>
      <td>71.00</td>
      <td>33.80</td>
      <td>11.41</td>
      <td>havoysund</td>
    </tr>
  </tbody>
</table>
</div>




```python
weather_data.to_csv("city_weather.csv")
```


```python
#plt.scatter(weather_data["lat"],weather_data["temp"],marker=".",color="blue", edgecolors="face")
# use the function regplot to make a scatterplot 
# With regression fit:
sns_plot=sns.regplot(x=weather_data["Latitude"], y=weather_data["Max temp"], fit_reg=True)
# Incorporate the other graph properties
plt.title(f"Latitude vs. Temperature (F)-{datetime.datetime.now():%Y-%m-%d}")
plt.ylabel("Temperature (F)")
plt.xlabel("Latitude")
plt.grid(True)
#plt.legend(bbox_to_anchor=(1.1, 1.05))

plt.ylim([-20,110])
plt.xlim([-60,85])

# Show plot
plt.show()
# Save an image of the chart and print to screen
#plt.savefig("Latitude_vs_Temperature.png")
#sns_plot = sns.pairplot(df, hue='species', size=2.5)
sns_plot.figure.savefig("Latitude_vs_Temperature.png")
```


![png](output_9_0.png)



```python
#plt.scatter(weather_data["lat"],weather_data["humid"],marker=".",color="blue", edgecolors="face")
# use the function regplot to make a scatterplot 
# With regression fit:
sns_plot=sns.regplot(x=weather_data["Latitude"], y=weather_data["Humidity"], fit_reg=True)
# Incorporate the other graph properties
plt.title(f"Latitude vs. Humidity (%)-{datetime.datetime.now():%Y-%m-%d}")
plt.ylabel("Humidity (%)")
plt.xlabel("Latitude")
plt.grid(True)

plt.ylim([10,105])
plt.xlim([-60,85])

# Show plot
plt.show()
# Save an image of the chart and print to screen
sns_plot.figure.savefig("Latitude_vs_Humidity.png")
```


![png](output_10_0.png)



```python
#plt.scatter(weather_data["lat"],weather_data["cloud"],marker=".",color="blue", edgecolors="face")
# use the function regplot to make a scatterplot 
# With regression fit:
sns_plot=sns.regplot(x=weather_data["Latitude"], y=weather_data["Cloudiness"], fit_reg=True)
# Incorporate the other graph properties
plt.title(f"Latitude vs. Cloudiness (%)-{datetime.datetime.now():%Y-%m-%d}")
plt.ylabel("Cloudiness (%)")
plt.xlabel("Latitude")
plt.grid(True)

plt.ylim([-5,105])
plt.xlim([-60,85])

# Show plot
plt.show()
# Save an image of the chart and print to screen
sns_plot.figure.savefig("Latitude_vs_Cloudiness.png")
```


![png](output_11_0.png)



```python
#plt.scatter(weather_data["lat"],weather_data["windspeed"],marker="D")
# use the function regplot to make a scatterplot 
# With regression fit:
sns_plot=sns.regplot(x=weather_data["Latitude"], y=weather_data["Windspeed"], fit_reg=True)
# Incorporate the other graph properties
plt.title(f"Latitude vs. Windspeed (mph)-{datetime.datetime.now():%Y-%m-%d}")
plt.ylabel("Windspeed (mph)")
plt.xlabel("Latitude")
plt.grid(True)

plt.ylim([-2,32])
plt.xlim([-65,85])

# Show plot
plt.show()
# Save an image of the chart and print to screen
sns_plot.figure.savefig("Latitude_vs_Windspeed.png")
```


![png](output_12_0.png)

