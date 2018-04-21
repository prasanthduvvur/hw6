
## Temperature is maximum near the equator, decreases as you get further away from it.
## Largest Cluster of max humidity % (between 80-100%) at or near the equator.
## Windspeed ranges between 0-15mph near the equator, seems to increase as you get away from the equator.



```python
# Dependencies

#for cities
from citipy import citipy

#for random numbers - used to get random lattitudes and longitudes
import random

#to get city weather
import openweathermapy.core as owm
import requests

#config
from config import api_key
#dataframe
import pandas as pd

#for scatter plotting
import matplotlib.pyplot as plt
#import numpy as np
#from scipy.stats import sem
import datetime
import seaborn as sns
```


```python
#TESTING citypy
# Some random coordinates
coordinates = [(200, 200), (23, 200), (42, 100)]
#coordinates = [(500, 100), (10000, 200), (42, 100)]
city=citipy.nearest_city(200, 200)
print(f"the city is: {city}")
city = citipy.nearest_city(22.99, 120.21)
city.city_name
```

    the city is: <citipy.citipy.City object at 0x00000268D3FBAA20>
    




    'tainan'




```python
#TESTING openweathermap and data frame creation
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
    The temperature information received is: [78.8, 71.06, 60.8, 57.2]
    The humidty information received is: [39, 60, 31, 71]
    The cloudiness % information received is: [0, 0, 0, 90]
    The windspeed information received is: [6.93, 12.75, 16.11, 13.42]
    




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
      <td>78.80</td>
      <td>6.93</td>
    </tr>
    <tr>
      <th>1</th>
      <td>London</td>
      <td>0</td>
      <td>60</td>
      <td>51.51</td>
      <td>71.06</td>
      <td>12.75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Oslo</td>
      <td>0</td>
      <td>31</td>
      <td>59.91</td>
      <td>60.80</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Beijing</td>
      <td>90</td>
      <td>71</td>
      <td>39.91</td>
      <td>57.20</td>
      <td>13.42</td>
    </tr>
  </tbody>
</table>
</div>




```python
#building up city list
#getting more than 500 as some cities don't have all the needed data
# set up lists to hold reponse info
cities = []
lat=[]
count=0
city=''
latnow=0
lonnow=0

#latnow=random.randint(-90,90)   

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
print(f"{cities}")        
#print(f"lat list is {lat}")
```

    final count is:601
    ['ushuaia', 'punta arenas', 'mataura', 'hobart', 'hilo', 'cape town', 'butaritari', 'kyra', 'codrington', 'rikitea', 'mizdah', 'saint george', 'san quintin', 'busselton', 'cidreira', 'fortuna foothills', 'jamestown', 'taolanaro', 'nantucket', 'shenjiamen', 'tygda', 'norman wells', 'severo-kurilsk', 'albany', 'barrow', 'moindou', 'cherskiy', 'wulanhaote', 'ponta do sol', 'port alfred', 'victoria', 'nikolskoye', 'teguise', 'lagoa', 'port elizabeth', 'hermanus', 'chalmette', 'chilliwack', 'deputatskiy', 'georgetown', 'great yarmouth', 'nelson bay', 'esperance', 'mar del plata', 'bluff', 'zhumadian', 'svetlogorsk', 'provideniya', 'umzimvubu', 'tual', 'oussouye', 'preobrazheniye', 'qaanaaq', 'iqaluit', 'saskylakh', 'bredasdorp', 'olafsvik', 'lavrentiya', 'broome', 'vieques', 'san patricio', 'lebu', 'tasiilaq', 'moravsky krumlov', 'togur', 'longyearbyen', 'illoqqortoormiut', 'barentsburg', 'faanui', 'henties bay', 'attawapiskat', 'la ronge', 'chokurdakh', 'saleaula', 'burevestnik', 'mananjary', 'bethel', 'bose', 'cayenne', 'pevek', 'gat', 'karatau', 'katsuura', 'vaini', 'palmer', 'havoysund', 'port lincoln', 'tezu', 'airai', 'oksfjord', 'puerto ayora', 'tabiauea', 'lata', 'artyk', 'zhanaozen', 'vestmannaeyjar', 'dikson', 'kapaa', 'novikovo', 'kaitong', 'tiznit', 'atuona', 'novobiryusinskiy', 'sibirtsevo', 'bajil', 'castro', 'buala', 'yellowknife', 'salinas', 'namibe', 'port blair', 'malia', 'lorengau', 'touros', 'tuktoyaktuk', 'margate', 'bengkulu', 'shimoda', 'souillac', 'alofi', 'quelimane', 'campoverde', 'muravlenko', 'namatanai', 'pemangkat', 'puerto lempira', 'monte patria', 'nemuro', 'presidencia roque saenz pena', 'te anau', 'torbay', 'zhigansk', 'husavik', 'cullera', 'tres arroyos', 'port hedland', 'sandakan', 'nabire', 'klaksvik', 'yulara', 'lompoc', 'abu samrah', 'verkhnyaya inta', 'ongandjera', 'hambantota', 'arraial do cabo', 'vardo', 'thompson', 'naze', 'amderma', 'honningsvag', 'verkhoyansk', 'ussel', 'springbok', 'formoso do araguaia', 'broken hill', 'ostrovnoy', 'havre-saint-pierre', 'fortuna', 'tura', 'birin', 'shiyan', 'saldanha', 'chabahar', 'valle de allende', 'belushya guba', 'atar', 'clyde river', 'samusu', 'yagoua', 'nizhneyansk', 'olga', 'kruisfontein', 'chagda', 'flinders', 'new norfolk', 'luderitz', 'east london', 'ksenyevka', 'aklavik', 'nome', 'morondava', 'kavieng', 'arlit', 'jabiru', 'laguna', 'arrecife', 'huarmey', 'banda aceh', 'visby', 'otane', 'zhigalovo', 'pisco', 'la rioja', 'bud', 'houma', 'itaituba', 'saint-philippe', 'eskilstuna', 'egvekinot', 'ketchikan', 'berlevag', 'san cristobal', 'ixtapa', 'mogok', 'karratha', 'bonfim', 'bayan', 'mitchell', 'bahia honda', 'solnechnyy', 'narsaq', 'kota belud', 'hofn', 'ancud', 'amapa', 'moose factory', 'jining', 'chapais', 'kodiak', 'tsihombe', 'coxim', 'kudahuvadhoo', 'royan', 'manaus', 'khonuu', 'qandala', 'santa cruz', 'anadyr', 'upernavik', 'avarua', 'gorno-chuyskiy', 'acapulco', 'angoram', 'chara', 'bilma', 'katobu', 'filadelfia', 'hithadhoo', 'yerbogachen', 'raudeberg', 'daru', 'samarai', 'prieska', 'virginia beach', 'bassila', 'sao filipe', 'camacha', 'tyulyachi', 'jiblah', 'ventspils', 'gumdag', 'tucupita', 'ginir', 'waipawa', 'eagle pass', 'battle creek', 'leh', 'dalvik', 'harper', 'chuy', 'nevers', 'krasnoyarsk-66', 'quepos', 'great falls', 'caconda', 'wewak', 'camara de lobos', 'finschhafen', 'vaitupu', 'roma', 'kandrian', 'dongsheng', 'moses lake', 'carutapera', 'zhuhai', 'fairbanks', 'along', 'nanortalik', 'ust-nera', 'aksarka', 'mao', 'birao', 'santa rosa', 'maghama', 'dingle', 'ossora', 'emerald', 'vagur', 'college', 'bara', 'manta', 'ulagan', 'marakkanam', 'kaitangata', 'angra dos reis', 'baykit', 'tahe', 'khatanga', 'cairns', 'floro', 'bang bo', 'lazaro cardenas', 'warragul', 'mys shmidta', 'evensk', 'bridgetown', 'voh', 'onega', 'faya', 'grand gaube', 'sao francisco do sul', 'maridi', 'goldap', 'thunder bay', 'nizhniy tsasuchey', 'charters towers', 'awjilah', 'pacific grove', 'bourail', 'sentyabrskiy', 'klyuchevskiy', 'acarau', 'olot', 'riyadh', 'padang', 'thies', 'tiksi', 'altay', 'cozumel', 'oistins', 'khandyga', 'vao', 'moron', 'ponta delgada', 'meulaboh', 'yumen', 'nouadhibou', 'mahebourg', 'gravina in puglia', 'omsukchan', 'guerrero negro', 'gonc', 'semey', 'lucapa', 'kidal', 'los llanos de aridane', 'russell', 'sedelnikovo', 'vila', 'wajid', 'portland', 'shilka', 'baunatal', 'telde', 'kaolinovo', 'tshikapa', 'iseyin', 'hami', 'tautira', 'coquimbo', 'yar-sale', 'pericos', 'shubarkuduk', 'dolbeau', 'rongcheng', 'vostok', 'lengshuijiang', 'hasaki', 'san luis', 'jinchang', 'acuna', 'bima', 'skibbereen', 'margahovit', 'panjakent', 'uruzgan', 'rungata', 'assiniboia', 'mackay', 'hobyo', 'buraydah', 'tecoanapa', 'burns lake', 'kundiawa', 'chicla', 'afareaitu', 'ukiah', 'amuntai', 'coihaique', 'bolu', 'quatre cocos', 'saint-augustin', 'lephepe', 'galiwinku', 'abashiri', 'la union', 'springdale', 'domoni', 'holme', 'petropavlovsk-kamchatskiy', 'saint-georges', 'mora', 'sarakhs', 'dunedin', 'alamogordo', 'yzeure', 'antofagasta', 'lufilufi', 'toliary', 'asau', 'adrar', 'bandarbeyla', 'carmen', 'gunjur', 'gazanjyk', 'carnarvon', 'hay river', 'severnyy', 'chute-aux-outardes', 'aflu', 'corning', 'lesnyye polyany', 'general roca', 'amahai', 'sawakin', 'pietarsaari', 'birobidzhan', 'jagatsinghapur', 'bambous virieux', 'constitucion', 'ribeira grande', 'waycross', 'plouzane', 'palabuhanratu', 'shenkursk', 'kieta', 'makakilo city', 'torma', 'comodoro rivadavia', 'moosomin', 'vanimo', 'hualmay', 'cornelio procopio', 'labuhan', 'talaya', 'salinopolis', 'parasia', 'kemijarvi', 'alindao', 'cape canaveral', 'taoudenni', 'noyabrsk', 'kutum', 'camargo', 'labutta', 'knokke-heist', 'cheuskiny', 'porto de moz', 'odessa', 'mwanza', 'grindavik', 'kloulklubed', 'kailua', 'cabo san lucas', 'bokovskaya', 'havelock', 'podor', 'copiapo', 'opelika', 'vila velha', 'la serena', 'half moon bay', 'tucurui', 'alcala la real', 'kaseda', 'rawson', 'tabriz', 'portobelo', 'batagay-alyta', 'chupa', 'belgrade', 'muzhi', 'opuwo', 'sinnar', 'lar gerd', 'bulawayo', 'rudnyy', 'newtyle', 'urengoy', 'alyangula', 'remanso', 'sayyan', 'soto la marina', 'moerai', 'erdenet', 'tuatapere', 'kavaratti', 'fort smith', 'srednekolymsk', 'nyurba', 'wau', 'sao jose da coroa grande', 'cabra', 'gwanda', 'bathsheba', 'grand river south east', 'kaoma', 'macaboboni', 'panzhihua', 'tacoronte', 'eureka', 'gizo', 'kayseri', 'marsa matruh', 'lasa', 'talnakh', 'linping', 'sijunjung', 'yabrud', 'mana', 'punta de bombon', 'savelugu', 'mbigou', 'montrose', 'sao joao da barra', 'kamenka', 'kholodnyy', 'tessalit', 'praia da vitoria', 'pyay', 'jaca', 'buin', 'shaki', 'dali', 'villazon', 'cap malheureux', 'umm durman', 'mount gambier', 'kommunisticheskiy', 'orange', 'merauke', 'nizhneangarsk', 'matamoros', 'inirida', 'lamu', 'banjar', 'bollnas', 'mrirt', 'koslan', 'leningradskiy', 'luwuk', 'rabat', 'kaeo', 'escanaba', 'sergeyevka', 'pangnirtung', 'port antonio', 'maunabo', 'ust-koksa', 'richards bay', 'alamos', 'haicheng', 'requena', 'vrangel', 'walvis bay', 'rocha', 'minot', 'aksu', 'skalistyy', 'bilibino', 'soyaux', 'utete', 'caiaponia', 'pendleton', 'guajara-mirim', 'kamenskoye', 'udaipura', 'fare', 'lishui', 'west bay', 'khormuj', 'ikalamavony', 'kindu', 'apozol', 'la romana', 'alexandria', 'corn island', 'hefei', 'ozinki', 'necochea', 'malatya', 'ascension', 'nagaur', 'port macquarie', 'mackenzie', 'iquitos', 'karamea', 'wanaka', 'boguchany', 'agirish']
    


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

    The weather API responded with: {'coord': {'lon': -0.13, 'lat': 51.51}, 'weather': [{'id': 800, 'main': 'Clear', 'description': 'clear sky', 'icon': '01d'}], 'base': 'stations', 'main': {'temp': 71.06, 'pressure': 1020, 'humidity': 60, 'temp_min': 64.4, 'temp_max': 75.2}, 'visibility': 10000, 'wind': {'speed': 12.75, 'deg': 90}, 'clouds': {'all': 0}, 'dt': 1524313200, 'sys': {'type': 1, 'id': 5091, 'message': 0.0053, 'country': 'GB', 'sunrise': 1524286282, 'sunset': 1524337679}, 'id': 2643743, 'name': 'London', 'cod': 200}.
    the temp is 71.06
    


```python
# Create settings dictionary with information we're interested in
settings = {"units": "imperial", "appid": api_key}
url = "http://api.openweathermap.org/data/2.5/weather?"

#initializing lists and variaales
count=1

lat = []
temp = []
humid = []
cloud = []
windspeed = []
weathercities = []
country = []

#for loop to get city weather information
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

#debug code
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

    processing record 1 of set 1 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ushuaia&units=imperial
    processing record 2 of set 1 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=punta arenas&units=imperial
    processing record 3 of set 1 | mataura
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mataura&units=imperial
    processing record 4 of set 1 | hobart
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hobart&units=imperial
    processing record 5 of set 1 | hilo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hilo&units=imperial
    processing record 6 of set 1 | cape town
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cape town&units=imperial
    processing record 7 of set 1 | butaritari
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=butaritari&units=imperial
    processing record 8 of set 1 | kyra
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kyra&units=imperial
    processing record 8 of set 1 | codrington
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=codrington&units=imperial
    processing record 9 of set 1 | rikitea
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rikitea&units=imperial
    processing record 10 of set 1 | mizdah
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mizdah&units=imperial
    processing record 11 of set 1 | saint george
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saint george&units=imperial
    processing record 12 of set 1 | san quintin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=san quintin&units=imperial
    processing record 13 of set 1 | busselton
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=busselton&units=imperial
    processing record 14 of set 1 | cidreira
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cidreira&units=imperial
    processing record 15 of set 1 | fortuna foothills
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fortuna foothills&units=imperial
    processing record 16 of set 1 | jamestown
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jamestown&units=imperial
    processing record 17 of set 1 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=taolanaro&units=imperial
    processing record 17 of set 1 | nantucket
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nantucket&units=imperial
    processing record 18 of set 1 | shenjiamen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=shenjiamen&units=imperial
    processing record 19 of set 1 | tygda
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tygda&units=imperial
    processing record 20 of set 1 | norman wells
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=norman wells&units=imperial
    processing record 21 of set 1 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=severo-kurilsk&units=imperial
    processing record 22 of set 1 | albany
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=albany&units=imperial
    processing record 23 of set 1 | barrow
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=barrow&units=imperial
    processing record 24 of set 1 | moindou
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=moindou&units=imperial
    processing record 25 of set 1 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cherskiy&units=imperial
    processing record 26 of set 1 | wulanhaote
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=wulanhaote&units=imperial
    processing record 26 of set 1 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ponta do sol&units=imperial
    processing record 27 of set 1 | port alfred
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port alfred&units=imperial
    processing record 28 of set 1 | victoria
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=victoria&units=imperial
    processing record 29 of set 1 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nikolskoye&units=imperial
    processing record 30 of set 1 | teguise
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=teguise&units=imperial
    processing record 31 of set 1 | lagoa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lagoa&units=imperial
    processing record 32 of set 1 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port elizabeth&units=imperial
    processing record 33 of set 1 | hermanus
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hermanus&units=imperial
    processing record 34 of set 1 | chalmette
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chalmette&units=imperial
    processing record 35 of set 1 | chilliwack
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chilliwack&units=imperial
    processing record 36 of set 1 | deputatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=deputatskiy&units=imperial
    processing record 37 of set 1 | georgetown
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=georgetown&units=imperial
    processing record 38 of set 1 | great yarmouth
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=great yarmouth&units=imperial
    processing record 39 of set 1 | nelson bay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nelson bay&units=imperial
    processing record 40 of set 1 | esperance
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=esperance&units=imperial
    processing record 41 of set 1 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mar del plata&units=imperial
    processing record 42 of set 1 | bluff
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bluff&units=imperial
    processing record 43 of set 1 | zhumadian
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=zhumadian&units=imperial
    processing record 44 of set 1 | svetlogorsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=svetlogorsk&units=imperial
    processing record 45 of set 1 | provideniya
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=provideniya&units=imperial
    processing record 46 of set 1 | umzimvubu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=umzimvubu&units=imperial
    processing record 46 of set 1 | tual
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tual&units=imperial
    processing record 47 of set 1 | oussouye
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=oussouye&units=imperial
    processing record 48 of set 1 | preobrazheniye
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=preobrazheniye&units=imperial
    processing record 49 of set 1 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=qaanaaq&units=imperial
    processing record 50 of set 1 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=iqaluit&units=imperial
    processing record 51 of set 1 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saskylakh&units=imperial
    processing record 52 of set 1 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bredasdorp&units=imperial
    processing record 53 of set 1 | olafsvik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=olafsvik&units=imperial
    processing record 53 of set 1 | lavrentiya
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lavrentiya&units=imperial
    processing record 54 of set 1 | broome
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=broome&units=imperial
    processing record 55 of set 1 | vieques
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vieques&units=imperial
    processing record 56 of set 1 | san patricio
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=san patricio&units=imperial
    processing record 57 of set 1 | lebu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lebu&units=imperial
    processing record 58 of set 1 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tasiilaq&units=imperial
    processing record 59 of set 1 | moravsky krumlov
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=moravsky krumlov&units=imperial
    processing record 60 of set 1 | togur
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=togur&units=imperial
    processing record 61 of set 1 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=longyearbyen&units=imperial
    processing record 62 of set 1 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=illoqqortoormiut&units=imperial
    processing record 62 of set 1 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=barentsburg&units=imperial
    processing record 62 of set 1 | faanui
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=faanui&units=imperial
    processing record 63 of set 1 | henties bay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=henties bay&units=imperial
    processing record 64 of set 1 | attawapiskat
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=attawapiskat&units=imperial
    processing record 64 of set 1 | la ronge
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=la ronge&units=imperial
    processing record 65 of set 1 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chokurdakh&units=imperial
    processing record 66 of set 1 | saleaula
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saleaula&units=imperial
    processing record 66 of set 1 | burevestnik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=burevestnik&units=imperial
    processing record 67 of set 1 | mananjary
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mananjary&units=imperial
    processing record 68 of set 1 | bethel
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bethel&units=imperial
    processing record 69 of set 1 | bose
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bose&units=imperial
    processing record 70 of set 1 | cayenne
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cayenne&units=imperial
    processing record 71 of set 1 | pevek
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pevek&units=imperial
    processing record 72 of set 1 | gat
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gat&units=imperial
    processing record 73 of set 1 | karatau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=karatau&units=imperial
    processing record 74 of set 1 | katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=katsuura&units=imperial
    processing record 75 of set 1 | vaini
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vaini&units=imperial
    processing record 76 of set 1 | palmer
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=palmer&units=imperial
    processing record 77 of set 1 | havoysund
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=havoysund&units=imperial
    processing record 78 of set 1 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port lincoln&units=imperial
    processing record 79 of set 1 | tezu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tezu&units=imperial
    processing record 80 of set 1 | airai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=airai&units=imperial
    processing record 81 of set 1 | oksfjord
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=oksfjord&units=imperial
    processing record 82 of set 1 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=puerto ayora&units=imperial
    processing record 83 of set 1 | tabiauea
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tabiauea&units=imperial
    processing record 83 of set 1 | lata
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lata&units=imperial
    processing record 84 of set 1 | artyk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=artyk&units=imperial
    processing record 84 of set 1 | zhanaozen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=zhanaozen&units=imperial
    processing record 85 of set 1 | vestmannaeyjar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vestmannaeyjar&units=imperial
    processing record 86 of set 1 | dikson
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dikson&units=imperial
    processing record 87 of set 1 | kapaa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kapaa&units=imperial
    processing record 88 of set 1 | novikovo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=novikovo&units=imperial
    processing record 89 of set 1 | kaitong
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kaitong&units=imperial
    processing record 90 of set 1 | tiznit
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tiznit&units=imperial
    processing record 91 of set 1 | atuona
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=atuona&units=imperial
    processing record 92 of set 1 | novobiryusinskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=novobiryusinskiy&units=imperial
    processing record 93 of set 1 | sibirtsevo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sibirtsevo&units=imperial
    processing record 93 of set 1 | bajil
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bajil&units=imperial
    processing record 94 of set 1 | castro
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=castro&units=imperial
    processing record 95 of set 1 | buala
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=buala&units=imperial
    processing record 96 of set 1 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yellowknife&units=imperial
    processing record 97 of set 1 | salinas
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=salinas&units=imperial
    processing record 98 of set 1 | namibe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=namibe&units=imperial
    processing record 99 of set 1 | port blair
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port blair&units=imperial
    processing record 100 of set 1 | malia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=malia&units=imperial
    processing record 101 of set 1 | lorengau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lorengau&units=imperial
    processing record 102 of set 1 | touros
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=touros&units=imperial
    processing record 103 of set 1 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tuktoyaktuk&units=imperial
    processing record 104 of set 1 | margate
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=margate&units=imperial
    processing record 105 of set 1 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bengkulu&units=imperial
    processing record 105 of set 1 | shimoda
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=shimoda&units=imperial
    processing record 106 of set 1 | souillac
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=souillac&units=imperial
    processing record 107 of set 1 | alofi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=alofi&units=imperial
    processing record 108 of set 1 | quelimane
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=quelimane&units=imperial
    processing record 109 of set 1 | campoverde
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=campoverde&units=imperial
    processing record 110 of set 1 | muravlenko
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=muravlenko&units=imperial
    processing record 111 of set 1 | namatanai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=namatanai&units=imperial
    processing record 112 of set 1 | pemangkat
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pemangkat&units=imperial
    processing record 112 of set 1 | puerto lempira
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=puerto lempira&units=imperial
    processing record 113 of set 1 | monte patria
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=monte patria&units=imperial
    processing record 114 of set 1 | nemuro
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nemuro&units=imperial
    processing record 115 of set 1 | presidencia roque saenz pena
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=presidencia roque saenz pena&units=imperial
    processing record 116 of set 1 | te anau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=te anau&units=imperial
    processing record 117 of set 1 | torbay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=torbay&units=imperial
    processing record 118 of set 1 | zhigansk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=zhigansk&units=imperial
    processing record 119 of set 1 | husavik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=husavik&units=imperial
    processing record 120 of set 1 | cullera
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cullera&units=imperial
    processing record 121 of set 1 | tres arroyos
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tres arroyos&units=imperial
    processing record 122 of set 1 | port hedland
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port hedland&units=imperial
    processing record 123 of set 1 | sandakan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sandakan&units=imperial
    processing record 124 of set 1 | nabire
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nabire&units=imperial
    processing record 125 of set 1 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=klaksvik&units=imperial
    processing record 126 of set 1 | yulara
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yulara&units=imperial
    processing record 127 of set 1 | lompoc
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lompoc&units=imperial
    processing record 128 of set 1 | abu samrah
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=abu samrah&units=imperial
    processing record 129 of set 1 | verkhnyaya inta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=verkhnyaya inta&units=imperial
    processing record 130 of set 1 | ongandjera
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ongandjera&units=imperial
    processing record 131 of set 1 | hambantota
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hambantota&units=imperial
    processing record 132 of set 1 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=arraial do cabo&units=imperial
    processing record 133 of set 1 | vardo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vardo&units=imperial
    processing record 134 of set 1 | thompson
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=thompson&units=imperial
    processing record 135 of set 1 | naze
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=naze&units=imperial
    processing record 136 of set 1 | amderma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=amderma&units=imperial
    processing record 136 of set 1 | honningsvag
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=honningsvag&units=imperial
    processing record 137 of set 1 | verkhoyansk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=verkhoyansk&units=imperial
    processing record 138 of set 1 | ussel
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ussel&units=imperial
    processing record 139 of set 1 | springbok
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=springbok&units=imperial
    processing record 140 of set 1 | formoso do araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=formoso do araguaia&units=imperial
    processing record 140 of set 1 | broken hill
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=broken hill&units=imperial
    processing record 141 of set 1 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ostrovnoy&units=imperial
    processing record 142 of set 1 | havre-saint-pierre
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=havre-saint-pierre&units=imperial
    processing record 143 of set 1 | fortuna
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fortuna&units=imperial
    processing record 144 of set 1 | tura
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tura&units=imperial
    processing record 145 of set 1 | birin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=birin&units=imperial
    processing record 146 of set 1 | shiyan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=shiyan&units=imperial
    processing record 147 of set 1 | saldanha
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saldanha&units=imperial
    processing record 148 of set 1 | chabahar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chabahar&units=imperial
    processing record 149 of set 1 | valle de allende
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=valle de allende&units=imperial
    processing record 150 of set 1 | belushya guba
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=belushya guba&units=imperial
    processing record 150 of set 1 | atar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=atar&units=imperial
    processing record 151 of set 1 | clyde river
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=clyde river&units=imperial
    processing record 152 of set 1 | samusu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=samusu&units=imperial
    processing record 152 of set 1 | yagoua
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yagoua&units=imperial
    processing record 153 of set 1 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nizhneyansk&units=imperial
    processing record 153 of set 1 | olga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=olga&units=imperial
    processing record 154 of set 1 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kruisfontein&units=imperial
    processing record 155 of set 1 | chagda
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chagda&units=imperial
    processing record 155 of set 1 | flinders
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=flinders&units=imperial
    processing record 156 of set 1 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=new norfolk&units=imperial
    processing record 157 of set 1 | luderitz
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=luderitz&units=imperial
    processing record 158 of set 1 | east london
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=east london&units=imperial
    processing record 159 of set 1 | ksenyevka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ksenyevka&units=imperial
    processing record 159 of set 1 | aklavik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=aklavik&units=imperial
    processing record 160 of set 1 | nome
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nome&units=imperial
    processing record 161 of set 1 | morondava
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=morondava&units=imperial
    processing record 162 of set 1 | kavieng
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kavieng&units=imperial
    processing record 163 of set 1 | arlit
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=arlit&units=imperial
    processing record 164 of set 1 | jabiru
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jabiru&units=imperial
    processing record 164 of set 1 | laguna
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=laguna&units=imperial
    processing record 165 of set 1 | arrecife
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=arrecife&units=imperial
    processing record 165 of set 1 | huarmey
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=huarmey&units=imperial
    processing record 166 of set 1 | banda aceh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=banda aceh&units=imperial
    processing record 167 of set 1 | visby
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=visby&units=imperial
    processing record 168 of set 1 | otane
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=otane&units=imperial
    processing record 169 of set 1 | zhigalovo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=zhigalovo&units=imperial
    processing record 170 of set 1 | pisco
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pisco&units=imperial
    processing record 171 of set 1 | la rioja
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=la rioja&units=imperial
    processing record 172 of set 1 | bud
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bud&units=imperial
    processing record 173 of set 1 | houma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=houma&units=imperial
    processing record 174 of set 1 | itaituba
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=itaituba&units=imperial
    processing record 175 of set 1 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saint-philippe&units=imperial
    processing record 176 of set 1 | eskilstuna
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=eskilstuna&units=imperial
    processing record 177 of set 1 | egvekinot
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=egvekinot&units=imperial
    processing record 178 of set 1 | ketchikan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ketchikan&units=imperial
    processing record 179 of set 1 | berlevag
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=berlevag&units=imperial
    processing record 180 of set 1 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=san cristobal&units=imperial
    processing record 181 of set 1 | ixtapa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ixtapa&units=imperial
    processing record 182 of set 1 | mogok
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mogok&units=imperial
    processing record 183 of set 1 | karratha
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=karratha&units=imperial
    processing record 184 of set 1 | bonfim
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bonfim&units=imperial
    processing record 185 of set 1 | bayan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bayan&units=imperial
    processing record 186 of set 1 | mitchell
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mitchell&units=imperial
    processing record 187 of set 1 | bahia honda
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bahia honda&units=imperial
    processing record 188 of set 1 | solnechnyy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=solnechnyy&units=imperial
    processing record 189 of set 1 | narsaq
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=narsaq&units=imperial
    processing record 190 of set 1 | kota belud
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kota belud&units=imperial
    processing record 191 of set 1 | hofn
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hofn&units=imperial
    processing record 192 of set 1 | ancud
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ancud&units=imperial
    processing record 193 of set 1 | amapa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=amapa&units=imperial
    processing record 194 of set 1 | moose factory
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=moose factory&units=imperial
    processing record 195 of set 1 | jining
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jining&units=imperial
    processing record 196 of set 1 | chapais
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chapais&units=imperial
    processing record 197 of set 1 | kodiak
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kodiak&units=imperial
    processing record 198 of set 1 | tsihombe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tsihombe&units=imperial
    processing record 198 of set 1 | coxim
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=coxim&units=imperial
    processing record 199 of set 1 | kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kudahuvadhoo&units=imperial
    processing record 200 of set 1 | royan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=royan&units=imperial
    processing record 201 of set 1 | manaus
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=manaus&units=imperial
    processing record 202 of set 1 | khonuu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=khonuu&units=imperial
    processing record 202 of set 1 | qandala
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=qandala&units=imperial
    processing record 203 of set 1 | santa cruz
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=santa cruz&units=imperial
    processing record 204 of set 1 | anadyr
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=anadyr&units=imperial
    processing record 205 of set 1 | upernavik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=upernavik&units=imperial
    processing record 206 of set 1 | avarua
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=avarua&units=imperial
    processing record 207 of set 1 | gorno-chuyskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gorno-chuyskiy&units=imperial
    processing record 207 of set 1 | acapulco
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=acapulco&units=imperial
    processing record 208 of set 1 | angoram
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=angoram&units=imperial
    processing record 209 of set 1 | chara
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chara&units=imperial
    processing record 210 of set 1 | bilma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bilma&units=imperial
    processing record 211 of set 1 | katobu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=katobu&units=imperial
    processing record 212 of set 1 | filadelfia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=filadelfia&units=imperial
    processing record 213 of set 1 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hithadhoo&units=imperial
    processing record 214 of set 1 | yerbogachen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yerbogachen&units=imperial
    processing record 215 of set 1 | raudeberg
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=raudeberg&units=imperial
    processing record 216 of set 1 | daru
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=daru&units=imperial
    processing record 217 of set 1 | samarai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=samarai&units=imperial
    processing record 218 of set 1 | prieska
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=prieska&units=imperial
    processing record 219 of set 1 | virginia beach
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=virginia beach&units=imperial
    processing record 220 of set 1 | bassila
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bassila&units=imperial
    processing record 221 of set 1 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sao filipe&units=imperial
    processing record 222 of set 1 | camacha
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=camacha&units=imperial
    processing record 223 of set 1 | tyulyachi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tyulyachi&units=imperial
    processing record 224 of set 1 | jiblah
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jiblah&units=imperial
    processing record 225 of set 1 | ventspils
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ventspils&units=imperial
    processing record 226 of set 1 | gumdag
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gumdag&units=imperial
    processing record 227 of set 1 | tucupita
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tucupita&units=imperial
    processing record 228 of set 1 | ginir
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ginir&units=imperial
    processing record 229 of set 1 | waipawa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=waipawa&units=imperial
    processing record 230 of set 1 | eagle pass
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=eagle pass&units=imperial
    processing record 231 of set 1 | battle creek
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=battle creek&units=imperial
    processing record 232 of set 1 | leh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=leh&units=imperial
    processing record 233 of set 1 | dalvik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dalvik&units=imperial
    processing record 234 of set 1 | harper
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=harper&units=imperial
    processing record 235 of set 1 | chuy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chuy&units=imperial
    processing record 236 of set 1 | nevers
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nevers&units=imperial
    processing record 237 of set 1 | krasnoyarsk-66
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=krasnoyarsk-66&units=imperial
    processing record 237 of set 1 | quepos
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=quepos&units=imperial
    processing record 238 of set 1 | great falls
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=great falls&units=imperial
    processing record 239 of set 1 | caconda
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=caconda&units=imperial
    processing record 240 of set 1 | wewak
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=wewak&units=imperial
    processing record 241 of set 1 | camara de lobos
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=camara de lobos&units=imperial
    processing record 242 of set 1 | finschhafen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=finschhafen&units=imperial
    processing record 243 of set 1 | vaitupu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vaitupu&units=imperial
    processing record 243 of set 1 | roma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=roma&units=imperial
    processing record 244 of set 1 | kandrian
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kandrian&units=imperial
    processing record 245 of set 1 | dongsheng
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dongsheng&units=imperial
    processing record 246 of set 1 | moses lake
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=moses lake&units=imperial
    processing record 247 of set 1 | carutapera
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=carutapera&units=imperial
    processing record 248 of set 1 | zhuhai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=zhuhai&units=imperial
    processing record 249 of set 1 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fairbanks&units=imperial
    processing record 250 of set 1 | along
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=along&units=imperial
    processing record 251 of set 1 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nanortalik&units=imperial
    processing record 252 of set 1 | ust-nera
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ust-nera&units=imperial
    processing record 253 of set 1 | aksarka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=aksarka&units=imperial
    processing record 254 of set 1 | mao
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mao&units=imperial
    processing record 255 of set 1 | birao
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=birao&units=imperial
    processing record 256 of set 1 | santa rosa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=santa rosa&units=imperial
    processing record 257 of set 1 | maghama
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=maghama&units=imperial
    processing record 257 of set 1 | dingle
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dingle&units=imperial
    processing record 258 of set 1 | ossora
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ossora&units=imperial
    processing record 259 of set 1 | emerald
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=emerald&units=imperial
    processing record 260 of set 1 | vagur
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vagur&units=imperial
    processing record 261 of set 1 | college
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=college&units=imperial
    processing record 262 of set 1 | bara
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bara&units=imperial
    processing record 263 of set 1 | manta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=manta&units=imperial
    processing record 264 of set 1 | ulagan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ulagan&units=imperial
    processing record 265 of set 1 | marakkanam
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=marakkanam&units=imperial
    processing record 266 of set 1 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kaitangata&units=imperial
    processing record 267 of set 1 | angra dos reis
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=angra dos reis&units=imperial
    processing record 268 of set 1 | baykit
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=baykit&units=imperial
    processing record 269 of set 1 | tahe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tahe&units=imperial
    processing record 270 of set 1 | khatanga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=khatanga&units=imperial
    processing record 271 of set 1 | cairns
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cairns&units=imperial
    processing record 272 of set 1 | floro
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=floro&units=imperial
    processing record 273 of set 1 | bang bo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bang bo&units=imperial
    processing record 274 of set 1 | lazaro cardenas
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lazaro cardenas&units=imperial
    processing record 275 of set 1 | warragul
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=warragul&units=imperial
    processing record 276 of set 1 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mys shmidta&units=imperial
    processing record 276 of set 1 | evensk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=evensk&units=imperial
    processing record 277 of set 1 | bridgetown
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bridgetown&units=imperial
    processing record 278 of set 1 | voh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=voh&units=imperial
    processing record 279 of set 1 | onega
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=onega&units=imperial
    processing record 280 of set 1 | faya
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=faya&units=imperial
    processing record 281 of set 1 | grand gaube
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=grand gaube&units=imperial
    processing record 282 of set 1 | sao francisco do sul
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sao francisco do sul&units=imperial
    processing record 283 of set 1 | maridi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=maridi&units=imperial
    processing record 283 of set 1 | goldap
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=goldap&units=imperial
    processing record 284 of set 1 | thunder bay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=thunder bay&units=imperial
    processing record 285 of set 1 | nizhniy tsasuchey
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nizhniy tsasuchey&units=imperial
    processing record 286 of set 1 | charters towers
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=charters towers&units=imperial
    processing record 287 of set 1 | awjilah
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=awjilah&units=imperial
    processing record 288 of set 1 | pacific grove
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pacific grove&units=imperial
    processing record 289 of set 1 | bourail
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bourail&units=imperial
    processing record 290 of set 1 | sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sentyabrskiy&units=imperial
    processing record 290 of set 1 | klyuchevskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=klyuchevskiy&units=imperial
    processing record 291 of set 1 | acarau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=acarau&units=imperial
    processing record 291 of set 1 | olot
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=olot&units=imperial
    processing record 292 of set 1 | riyadh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=riyadh&units=imperial
    processing record 293 of set 1 | padang
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=padang&units=imperial
    processing record 294 of set 1 | thies
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=thies&units=imperial
    processing record 295 of set 1 | tiksi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tiksi&units=imperial
    processing record 296 of set 1 | altay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=altay&units=imperial
    processing record 297 of set 1 | cozumel
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cozumel&units=imperial
    processing record 297 of set 1 | oistins
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=oistins&units=imperial
    processing record 298 of set 1 | khandyga
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=khandyga&units=imperial
    processing record 299 of set 1 | vao
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vao&units=imperial
    processing record 300 of set 1 | moron
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=moron&units=imperial
    processing record 301 of set 1 | ponta delgada
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ponta delgada&units=imperial
    processing record 302 of set 1 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=meulaboh&units=imperial
    processing record 303 of set 1 | yumen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yumen&units=imperial
    processing record 304 of set 1 | nouadhibou
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nouadhibou&units=imperial
    processing record 305 of set 1 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mahebourg&units=imperial
    processing record 306 of set 1 | gravina in puglia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gravina in puglia&units=imperial
    processing record 307 of set 1 | omsukchan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=omsukchan&units=imperial
    processing record 308 of set 1 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=guerrero negro&units=imperial
    processing record 309 of set 1 | gonc
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gonc&units=imperial
    processing record 310 of set 1 | semey
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=semey&units=imperial
    processing record 311 of set 1 | lucapa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lucapa&units=imperial
    processing record 312 of set 1 | kidal
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kidal&units=imperial
    processing record 313 of set 1 | los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=los llanos de aridane&units=imperial
    processing record 314 of set 1 | russell
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=russell&units=imperial
    processing record 315 of set 1 | sedelnikovo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sedelnikovo&units=imperial
    processing record 315 of set 1 | vila
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vila&units=imperial
    processing record 316 of set 1 | wajid
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=wajid&units=imperial
    processing record 317 of set 1 | portland
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=portland&units=imperial
    processing record 318 of set 1 | shilka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=shilka&units=imperial
    processing record 319 of set 1 | baunatal
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=baunatal&units=imperial
    processing record 320 of set 1 | telde
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=telde&units=imperial
    processing record 321 of set 1 | kaolinovo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kaolinovo&units=imperial
    processing record 322 of set 1 | tshikapa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tshikapa&units=imperial
    processing record 323 of set 1 | iseyin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=iseyin&units=imperial
    processing record 324 of set 1 | hami
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hami&units=imperial
    processing record 325 of set 1 | tautira
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tautira&units=imperial
    processing record 326 of set 1 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=coquimbo&units=imperial
    processing record 327 of set 1 | yar-sale
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yar-sale&units=imperial
    processing record 328 of set 1 | pericos
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pericos&units=imperial
    processing record 329 of set 1 | shubarkuduk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=shubarkuduk&units=imperial
    processing record 330 of set 1 | dolbeau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dolbeau&units=imperial
    processing record 330 of set 1 | rongcheng
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rongcheng&units=imperial
    processing record 331 of set 1 | vostok
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vostok&units=imperial
    processing record 332 of set 1 | lengshuijiang
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lengshuijiang&units=imperial
    processing record 333 of set 1 | hasaki
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hasaki&units=imperial
    processing record 334 of set 1 | san luis
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=san luis&units=imperial
    processing record 335 of set 1 | jinchang
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jinchang&units=imperial
    processing record 336 of set 1 | acuna
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=acuna&units=imperial
    processing record 336 of set 1 | bima
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bima&units=imperial
    processing record 337 of set 1 | skibbereen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=skibbereen&units=imperial
    processing record 338 of set 1 | margahovit
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=margahovit&units=imperial
    processing record 339 of set 1 | panjakent
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=panjakent&units=imperial
    processing record 340 of set 1 | uruzgan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=uruzgan&units=imperial
    processing record 341 of set 1 | rungata
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rungata&units=imperial
    processing record 341 of set 1 | assiniboia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=assiniboia&units=imperial
    processing record 342 of set 1 | mackay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mackay&units=imperial
    processing record 343 of set 1 | hobyo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hobyo&units=imperial
    processing record 344 of set 1 | buraydah
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=buraydah&units=imperial
    processing record 345 of set 1 | tecoanapa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tecoanapa&units=imperial
    processing record 346 of set 1 | burns lake
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=burns lake&units=imperial
    processing record 347 of set 1 | kundiawa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kundiawa&units=imperial
    processing record 348 of set 1 | chicla
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chicla&units=imperial
    processing record 349 of set 1 | afareaitu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=afareaitu&units=imperial
    processing record 350 of set 1 | ukiah
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ukiah&units=imperial
    processing record 351 of set 1 | amuntai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=amuntai&units=imperial
    processing record 352 of set 1 | coihaique
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=coihaique&units=imperial
    processing record 353 of set 1 | bolu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bolu&units=imperial
    processing record 354 of set 1 | quatre cocos
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=quatre cocos&units=imperial
    processing record 355 of set 1 | saint-augustin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saint-augustin&units=imperial
    processing record 356 of set 1 | lephepe
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lephepe&units=imperial
    processing record 356 of set 1 | galiwinku
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=galiwinku&units=imperial
    processing record 356 of set 1 | abashiri
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=abashiri&units=imperial
    processing record 357 of set 1 | la union
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=la union&units=imperial
    processing record 358 of set 1 | springdale
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=springdale&units=imperial
    processing record 359 of set 1 | domoni
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=domoni&units=imperial
    processing record 359 of set 1 | holme
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=holme&units=imperial
    processing record 360 of set 1 | petropavlovsk-kamchatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=petropavlovsk-kamchatskiy&units=imperial
    processing record 361 of set 1 | saint-georges
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=saint-georges&units=imperial
    processing record 362 of set 1 | mora
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mora&units=imperial
    processing record 363 of set 1 | sarakhs
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sarakhs&units=imperial
    processing record 364 of set 1 | dunedin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dunedin&units=imperial
    processing record 365 of set 1 | alamogordo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=alamogordo&units=imperial
    processing record 366 of set 1 | yzeure
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yzeure&units=imperial
    processing record 367 of set 1 | antofagasta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=antofagasta&units=imperial
    processing record 368 of set 1 | lufilufi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lufilufi&units=imperial
    processing record 369 of set 1 | toliary
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=toliary&units=imperial
    processing record 369 of set 1 | asau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=asau&units=imperial
    processing record 369 of set 1 | adrar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=adrar&units=imperial
    processing record 370 of set 1 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bandarbeyla&units=imperial
    processing record 371 of set 1 | carmen
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=carmen&units=imperial
    processing record 372 of set 1 | gunjur
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gunjur&units=imperial
    processing record 373 of set 1 | gazanjyk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gazanjyk&units=imperial
    processing record 374 of set 1 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=carnarvon&units=imperial
    processing record 375 of set 1 | hay river
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hay river&units=imperial
    processing record 376 of set 1 | severnyy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=severnyy&units=imperial
    processing record 376 of set 1 | chute-aux-outardes
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chute-aux-outardes&units=imperial
    processing record 377 of set 1 | aflu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=aflu&units=imperial
    processing record 377 of set 1 | corning
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=corning&units=imperial
    processing record 378 of set 1 | lesnyye polyany
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lesnyye polyany&units=imperial
    processing record 379 of set 1 | general roca
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=general roca&units=imperial
    processing record 380 of set 1 | amahai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=amahai&units=imperial
    processing record 381 of set 1 | sawakin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sawakin&units=imperial
    processing record 382 of set 1 | pietarsaari
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pietarsaari&units=imperial
    processing record 382 of set 1 | birobidzhan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=birobidzhan&units=imperial
    processing record 383 of set 1 | jagatsinghapur
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jagatsinghapur&units=imperial
    processing record 384 of set 1 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bambous virieux&units=imperial
    processing record 385 of set 1 | constitucion
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=constitucion&units=imperial
    processing record 386 of set 1 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=ribeira grande&units=imperial
    processing record 387 of set 1 | waycross
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=waycross&units=imperial
    processing record 388 of set 1 | plouzane
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=plouzane&units=imperial
    processing record 389 of set 1 | palabuhanratu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=palabuhanratu&units=imperial
    processing record 389 of set 1 | shenkursk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=shenkursk&units=imperial
    processing record 390 of set 1 | kieta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kieta&units=imperial
    processing record 391 of set 1 | makakilo city
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=makakilo city&units=imperial
    processing record 392 of set 1 | torma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=torma&units=imperial
    processing record 393 of set 1 | comodoro rivadavia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=comodoro rivadavia&units=imperial
    processing record 394 of set 1 | moosomin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=moosomin&units=imperial
    processing record 395 of set 1 | vanimo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vanimo&units=imperial
    processing record 396 of set 1 | hualmay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=hualmay&units=imperial
    processing record 397 of set 1 | cornelio procopio
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cornelio procopio&units=imperial
    processing record 397 of set 1 | labuhan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=labuhan&units=imperial
    processing record 398 of set 1 | talaya
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=talaya&units=imperial
    processing record 399 of set 1 | salinopolis
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=salinopolis&units=imperial
    processing record 400 of set 1 | parasia
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=parasia&units=imperial
    processing record 401 of set 1 | kemijarvi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kemijarvi&units=imperial
    processing record 401 of set 1 | alindao
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=alindao&units=imperial
    processing record 402 of set 1 | cape canaveral
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cape canaveral&units=imperial
    processing record 403 of set 1 | taoudenni
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=taoudenni&units=imperial
    processing record 404 of set 1 | noyabrsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=noyabrsk&units=imperial
    processing record 405 of set 1 | kutum
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kutum&units=imperial
    processing record 406 of set 1 | camargo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=camargo&units=imperial
    processing record 407 of set 1 | labutta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=labutta&units=imperial
    processing record 407 of set 1 | knokke-heist
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=knokke-heist&units=imperial
    processing record 408 of set 1 | cheuskiny
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cheuskiny&units=imperial
    processing record 408 of set 1 | porto de moz
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=porto de moz&units=imperial
    processing record 409 of set 1 | odessa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=odessa&units=imperial
    processing record 410 of set 1 | mwanza
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mwanza&units=imperial
    processing record 411 of set 1 | grindavik
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=grindavik&units=imperial
    processing record 412 of set 1 | kloulklubed
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kloulklubed&units=imperial
    processing record 413 of set 1 | kailua
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kailua&units=imperial
    processing record 414 of set 1 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cabo san lucas&units=imperial
    processing record 415 of set 1 | bokovskaya
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bokovskaya&units=imperial
    processing record 416 of set 1 | havelock
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=havelock&units=imperial
    processing record 417 of set 1 | podor
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=podor&units=imperial
    processing record 418 of set 1 | copiapo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=copiapo&units=imperial
    processing record 419 of set 1 | opelika
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=opelika&units=imperial
    processing record 420 of set 1 | vila velha
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=vila velha&units=imperial
    processing record 421 of set 1 | la serena
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=la serena&units=imperial
    processing record 422 of set 1 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=half moon bay&units=imperial
    processing record 423 of set 1 | tucurui
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tucurui&units=imperial
    processing record 424 of set 1 | alcala la real
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=alcala la real&units=imperial
    processing record 425 of set 1 | kaseda
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kaseda&units=imperial
    processing record 426 of set 1 | rawson
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rawson&units=imperial
    processing record 427 of set 1 | tabriz
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tabriz&units=imperial
    processing record 428 of set 1 | portobelo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=portobelo&units=imperial
    processing record 429 of set 1 | batagay-alyta
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=batagay-alyta&units=imperial
    processing record 430 of set 1 | chupa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=chupa&units=imperial
    processing record 431 of set 1 | belgrade
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=belgrade&units=imperial
    processing record 432 of set 1 | muzhi
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=muzhi&units=imperial
    processing record 433 of set 1 | opuwo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=opuwo&units=imperial
    processing record 434 of set 1 | sinnar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sinnar&units=imperial
    processing record 435 of set 1 | lar gerd
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lar gerd&units=imperial
    processing record 435 of set 1 | bulawayo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bulawayo&units=imperial
    processing record 436 of set 1 | rudnyy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rudnyy&units=imperial
    processing record 437 of set 1 | newtyle
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=newtyle&units=imperial
    processing record 438 of set 1 | urengoy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=urengoy&units=imperial
    processing record 439 of set 1 | alyangula
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=alyangula&units=imperial
    processing record 440 of set 1 | remanso
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=remanso&units=imperial
    processing record 441 of set 1 | sayyan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sayyan&units=imperial
    processing record 442 of set 1 | soto la marina
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=soto la marina&units=imperial
    processing record 443 of set 1 | moerai
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=moerai&units=imperial
    processing record 444 of set 1 | erdenet
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=erdenet&units=imperial
    processing record 445 of set 1 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tuatapere&units=imperial
    processing record 446 of set 1 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kavaratti&units=imperial
    processing record 447 of set 1 | fort smith
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=fort smith&units=imperial
    processing record 448 of set 1 | srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=srednekolymsk&units=imperial
    processing record 449 of set 1 | nyurba
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nyurba&units=imperial
    processing record 450 of set 1 | wau
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=wau&units=imperial
    processing record 450 of set 1 | sao jose da coroa grande
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sao jose da coroa grande&units=imperial
    processing record 451 of set 1 | cabra
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cabra&units=imperial
    processing record 452 of set 1 | gwanda
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gwanda&units=imperial
    processing record 453 of set 1 | bathsheba
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bathsheba&units=imperial
    processing record 454 of set 1 | grand river south east
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=grand river south east&units=imperial
    processing record 454 of set 1 | kaoma
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kaoma&units=imperial
    processing record 455 of set 1 | macaboboni
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=macaboboni&units=imperial
    processing record 455 of set 1 | panzhihua
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=panzhihua&units=imperial
    processing record 456 of set 1 | tacoronte
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tacoronte&units=imperial
    processing record 457 of set 1 | eureka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=eureka&units=imperial
    processing record 458 of set 1 | gizo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=gizo&units=imperial
    processing record 459 of set 1 | kayseri
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kayseri&units=imperial
    processing record 460 of set 1 | marsa matruh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=marsa matruh&units=imperial
    processing record 461 of set 1 | lasa
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lasa&units=imperial
    processing record 462 of set 1 | talnakh
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=talnakh&units=imperial
    processing record 463 of set 1 | linping
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=linping&units=imperial
    processing record 463 of set 1 | sijunjung
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sijunjung&units=imperial
    processing record 464 of set 1 | yabrud
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=yabrud&units=imperial
    processing record 465 of set 1 | mana
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mana&units=imperial
    processing record 466 of set 1 | punta de bombon
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=punta de bombon&units=imperial
    processing record 467 of set 1 | savelugu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=savelugu&units=imperial
    processing record 468 of set 1 | mbigou
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mbigou&units=imperial
    processing record 469 of set 1 | montrose
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=montrose&units=imperial
    processing record 470 of set 1 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sao joao da barra&units=imperial
    processing record 471 of set 1 | kamenka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kamenka&units=imperial
    processing record 472 of set 1 | kholodnyy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kholodnyy&units=imperial
    processing record 473 of set 1 | tessalit
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=tessalit&units=imperial
    processing record 474 of set 1 | praia da vitoria
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=praia da vitoria&units=imperial
    processing record 475 of set 1 | pyay
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pyay&units=imperial
    processing record 476 of set 1 | jaca
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=jaca&units=imperial
    processing record 477 of set 1 | buin
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=buin&units=imperial
    processing record 478 of set 1 | shaki
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=shaki&units=imperial
    processing record 479 of set 1 | dali
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=dali&units=imperial
    processing record 480 of set 1 | villazon
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=villazon&units=imperial
    processing record 480 of set 1 | cap malheureux
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=cap malheureux&units=imperial
    processing record 481 of set 1 | umm durman
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=umm durman&units=imperial
    processing record 481 of set 1 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mount gambier&units=imperial
    processing record 482 of set 1 | kommunisticheskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kommunisticheskiy&units=imperial
    processing record 483 of set 1 | orange
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=orange&units=imperial
    processing record 484 of set 1 | merauke
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=merauke&units=imperial
    processing record 485 of set 1 | nizhneangarsk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=nizhneangarsk&units=imperial
    processing record 486 of set 1 | matamoros
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=matamoros&units=imperial
    processing record 487 of set 1 | inirida
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=inirida&units=imperial
    processing record 488 of set 1 | lamu
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=lamu&units=imperial
    processing record 489 of set 1 | banjar
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=banjar&units=imperial
    processing record 490 of set 1 | bollnas
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=bollnas&units=imperial
    processing record 491 of set 1 | mrirt
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=mrirt&units=imperial
    processing record 491 of set 1 | koslan
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=koslan&units=imperial
    processing record 492 of set 1 | leningradskiy
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=leningradskiy&units=imperial
    processing record 493 of set 1 | luwuk
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=luwuk&units=imperial
    processing record 494 of set 1 | rabat
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=rabat&units=imperial
    processing record 495 of set 1 | kaeo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=kaeo&units=imperial
    processing record 496 of set 1 | escanaba
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=escanaba&units=imperial
    processing record 497 of set 1 | sergeyevka
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=sergeyevka&units=imperial
    processing record 498 of set 1 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=pangnirtung&units=imperial
    processing record 499 of set 1 | port antonio
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=port antonio&units=imperial
    processing record 500 of set 1 | maunabo
    http://api.openweathermap.org/data/2.5/weather?appid=25bc90a1196e6f153eece0bc0b0fc9eb&q=maunabo&units=imperial
    




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
      <td>40</td>
      <td>AR</td>
      <td>60</td>
      <td>-54.81</td>
      <td>42.80</td>
      <td>4.70</td>
      <td>ushuaia</td>
    </tr>
    <tr>
      <th>1</th>
      <td>75</td>
      <td>CL</td>
      <td>100</td>
      <td>-53.16</td>
      <td>39.20</td>
      <td>11.41</td>
      <td>punta arenas</td>
    </tr>
    <tr>
      <th>2</th>
      <td>92</td>
      <td>NZ</td>
      <td>77</td>
      <td>-46.19</td>
      <td>45.83</td>
      <td>14.65</td>
      <td>mataura</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>AU</td>
      <td>93</td>
      <td>-42.88</td>
      <td>53.60</td>
      <td>2.28</td>
      <td>hobart</td>
    </tr>
    <tr>
      <th>4</th>
      <td>75</td>
      <td>US</td>
      <td>77</td>
      <td>19.71</td>
      <td>69.80</td>
      <td>4.70</td>
      <td>hilo</td>
    </tr>
  </tbody>
</table>
</div>




```python
#saving city weather information to csv file
weather_data.to_csv("city_weather.csv")
```


```python
#plotting latitude vs. other criteria
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


![png](output_8_0.png)



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


![png](output_9_0.png)



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


![png](output_10_0.png)



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


![png](output_11_0.png)

