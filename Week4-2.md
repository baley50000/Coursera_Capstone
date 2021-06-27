# Welcome to Taiwan! Welcome to visit Kaohsiung :D

I live in Taiwan, Kaohsiung City.

Small eats are the big things in Taiwan. Bubble tea, Braised pork rice, Stinky tofu, etc..

In Taiwan, we can find all of them in a special place, **night market**, a big food court out of door and only open at night. 

Not only delicious food but also fun games are provided in night market.

There are many night market in Kaohsiung. How can we visit most of them in 3 nights.

If you are intrested in eatting delicious food in night market, keep watching:D


before we started, let's import packages first!


```python
import requests # library to handle requests
import pandas as pd # library for data analsysis
import numpy as np # library to handle data in a vectorized manner
import random # library for random number generation
from geopy.geocoders import Nominatim # module to convert an address into latitude and longitude values
# libraries for displaying images
from IPython.display import Image 
from IPython.core.display import HTML # tranforming json file into a pandas dataframe library
from pandas.io.json import json_normalize
import folium # plotting library
```


```python
CLIENT_ID = '' # your Foursquare ID
CLIENT_SECRET = '' # your Foursquare Secret
ACCESS_TOKEN = '' # your FourSquare Access Token
VERSION = '20180604'
LIMIT = 100
print('Your credentails:')
print('CLIENT_ID: ' + CLIENT_ID)
print('CLIENT_SECRET:' + CLIENT_SECRET)
```

    Your credentails:
    CLIENT_ID: 
    CLIENT_SECRET:


 The center of Kaohsiung City is [Formosa Boulevard](https://www.google.com.tw/maps/place/%E7%BE%8E%E9%BA%97%E5%B3%B6%E7%AB%99/@22.6310038,120.3000456,16.93z/data=!4m5!3m4!1s0x346e04895cfb44d7:0x1f2f8775d61795a3!8m2!3d22.6313887!4d120.3019131?hl=zh-TW)



```python
longitude = 120.301956
latitude = 22.631388
print(latitude, longitude)
```

    22.631388 120.301956


Fint all of the night market by foursquare


```python
search_query = 'Night Market'
radius = 10000
print(search_query + ' .... OK!')
```

    Night Market .... OK!



```python
url = 'https://api.foursquare.com/v2/venues/search?client_id={}&client_secret={}&ll={},{}&oauth_token={}&v={}&query={}&radius={}&limit={}'.format(CLIENT_ID, CLIENT_SECRET, latitude, longitude,ACCESS_TOKEN, VERSION, search_query, radius, LIMIT)
url
```




    'https://api.foursquare.com/v2/venues/search?client_id=&client_secret=&ll=22.631388,120.301956&oauth_token=&v=20180604&query=Night Market&radius=10000&limit=100'




```python
results = requests.get(url).json()
venues = results['response']['venues']
dataframe = json_normalize(venues)
dataframe.head()
```

    <ipython-input-75-bc0f259b5133>:3: FutureWarning: pandas.io.json.json_normalize is deprecated, use pandas.json_normalize instead
      dataframe = json_normalize(venues)





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
      <th>id</th>
      <th>name</th>
      <th>categories</th>
      <th>referralId</th>
      <th>hasPerk</th>
      <th>location.address</th>
      <th>location.crossStreet</th>
      <th>location.lat</th>
      <th>location.lng</th>
      <th>location.labeledLatLngs</th>
      <th>location.distance</th>
      <th>location.postalCode</th>
      <th>location.cc</th>
      <th>location.city</th>
      <th>location.country</th>
      <th>location.formattedAddress</th>
      <th>venuePage.id</th>
      <th>location.neighborhood</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>4d90ab691a6bba7a9b026c70</td>
      <td>Nanhua Night Market (南華夜市)</td>
      <td>[{'id': '53e510b7498ebcb1801b55d4', 'name': 'N...</td>
      <td>v-1624814324</td>
      <td>False</td>
      <td>高雄市新興區南華一路40號</td>
      <td>新興市場</td>
      <td>22.629036</td>
      <td>120.302810</td>
      <td>[{'label': 'display', 'lat': 22.629036, 'lng':...</td>
      <td>276</td>
      <td>800</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[高雄市新興區南華一路40號 (新興市場), 高雄市,  800]</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>4d5d28051ef2f04d196bfccd</td>
      <td>忠孝夜市 Zhongsiao Night Market</td>
      <td>[{'id': '53e510b7498ebcb1801b55d4', 'name': 'N...</td>
      <td>v-1624814324</td>
      <td>False</td>
      <td>Zhongsiao 2nd Rd.</td>
      <td>btw Qingnian &amp; Lingya</td>
      <td>22.621223</td>
      <td>120.307622</td>
      <td>[{'label': 'display', 'lat': 22.62122295591197...</td>
      <td>1272</td>
      <td>NaN</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[Zhongsiao 2nd Rd. (btw Qingnian &amp; Lingya), 高雄市]</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>4d6e81a39af52c0f3f0293dd</td>
      <td>興中觀光夜市 Sing Jhong Night Market</td>
      <td>[{'id': '53e510b7498ebcb1801b55d4', 'name': 'N...</td>
      <td>v-1624814324</td>
      <td>False</td>
      <td>苓雅區文橫二路</td>
      <td>三多路</td>
      <td>22.614773</td>
      <td>120.306880</td>
      <td>[{'label': 'display', 'lat': 22.61477347737183...</td>
      <td>1917</td>
      <td>802</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[苓雅區文橫二路 (三多路), 高雄市,  802]</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4d18a1446c8b548142a2f2cc</td>
      <td>星期一夜市 Monday Night Market</td>
      <td>[{'id': '4bf58dd8d48988d120951735', 'name': 'F...</td>
      <td>v-1624814324</td>
      <td>False</td>
      <td>高雄市苓雅區廣東一街97巷</td>
      <td>NaN</td>
      <td>22.621032</td>
      <td>120.320212</td>
      <td>[{'label': 'display', 'lat': 22.621032, 'lng':...</td>
      <td>2201</td>
      <td>NaN</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[高雄市苓雅區廣東一街97巷, 高雄市]</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4b9a255cf964a5202da135e3</td>
      <td>光華夜市 Guanghua Night Market</td>
      <td>[{'id': '53e510b7498ebcb1801b55d4', 'name': 'N...</td>
      <td>v-1624814324</td>
      <td>False</td>
      <td>高雄市苓雅區光華二路</td>
      <td>btw Sandou &amp; guangxi</td>
      <td>22.617095</td>
      <td>120.318242</td>
      <td>[{'label': 'display', 'lat': 22.61709490241331...</td>
      <td>2309</td>
      <td>NaN</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[高雄市苓雅區光華二路 (btw Sandou &amp; guangxi), 高雄市]</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Keep columns we need, and delete the row which is not night market.
filtered_columns = ['name', 'categories'] + [col for col in dataframe.columns if col.startswith('location.')] + ['id']
dataframe_filtered = dataframe.loc[:, filtered_columns]
def get_category_type(row):
    try:
        categories_list = row['categories']
    except:
        categories_list = row['venue.categories']  
    if len(categories_list) == 0:
        return None
    else:
        return categories_list[0]['name']
dataframe_filtered['categories'] = dataframe_filtered.apply(get_category_type, axis=1)
dataframe_filtered.columns = [column.split('.')[-1] for column in dataframe_filtered.columns]
dataframe_filtered = dataframe_filtered[dataframe_filtered["categories"] == "Night Market"]
dataframe_filtered
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
      <th>name</th>
      <th>categories</th>
      <th>address</th>
      <th>crossStreet</th>
      <th>lat</th>
      <th>lng</th>
      <th>labeledLatLngs</th>
      <th>distance</th>
      <th>postalCode</th>
      <th>cc</th>
      <th>city</th>
      <th>country</th>
      <th>formattedAddress</th>
      <th>neighborhood</th>
      <th>id</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nanhua Night Market (南華夜市)</td>
      <td>Night Market</td>
      <td>高雄市新興區南華一路40號</td>
      <td>新興市場</td>
      <td>22.629036</td>
      <td>120.302810</td>
      <td>[{'label': 'display', 'lat': 22.629036, 'lng':...</td>
      <td>276</td>
      <td>800</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[高雄市新興區南華一路40號 (新興市場), 高雄市,  800]</td>
      <td>NaN</td>
      <td>4d90ab691a6bba7a9b026c70</td>
    </tr>
    <tr>
      <th>1</th>
      <td>忠孝夜市 Zhongsiao Night Market</td>
      <td>Night Market</td>
      <td>Zhongsiao 2nd Rd.</td>
      <td>btw Qingnian &amp; Lingya</td>
      <td>22.621223</td>
      <td>120.307622</td>
      <td>[{'label': 'display', 'lat': 22.62122295591197...</td>
      <td>1272</td>
      <td>NaN</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[Zhongsiao 2nd Rd. (btw Qingnian &amp; Lingya), 高雄市]</td>
      <td>NaN</td>
      <td>4d5d28051ef2f04d196bfccd</td>
    </tr>
    <tr>
      <th>2</th>
      <td>興中觀光夜市 Sing Jhong Night Market</td>
      <td>Night Market</td>
      <td>苓雅區文橫二路</td>
      <td>三多路</td>
      <td>22.614773</td>
      <td>120.306880</td>
      <td>[{'label': 'display', 'lat': 22.61477347737183...</td>
      <td>1917</td>
      <td>802</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[苓雅區文橫二路 (三多路), 高雄市,  802]</td>
      <td>NaN</td>
      <td>4d6e81a39af52c0f3f0293dd</td>
    </tr>
    <tr>
      <th>4</th>
      <td>光華夜市 Guanghua Night Market</td>
      <td>Night Market</td>
      <td>高雄市苓雅區光華二路</td>
      <td>btw Sandou &amp; guangxi</td>
      <td>22.617095</td>
      <td>120.318242</td>
      <td>[{'label': 'display', 'lat': 22.61709490241331...</td>
      <td>2309</td>
      <td>NaN</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[高雄市苓雅區光華二路 (btw Sandou &amp; guangxi), 高雄市]</td>
      <td>NaN</td>
      <td>4b9a255cf964a5202da135e3</td>
    </tr>
    <tr>
      <th>5</th>
      <td>金鑽觀光夜市 Jin-Zuan Night Market</td>
      <td>Night Market</td>
      <td>前鎮區凱旋四路788號</td>
      <td>NaN</td>
      <td>22.599798</td>
      <td>120.320777</td>
      <td>[{'label': 'display', 'lat': 22.59979817512787...</td>
      <td>4013</td>
      <td>806</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[前鎮區凱旋四路788號, 高雄市,  806]</td>
      <td>NaN</td>
      <td>52c79fed11d217c0b28a5d9f</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Liouhe Night Market (六合夜市)</td>
      <td>Night Market</td>
      <td>六合二路</td>
      <td>NaN</td>
      <td>22.632438</td>
      <td>120.300083</td>
      <td>[{'label': 'display', 'lat': 22.63243761586824...</td>
      <td>225</td>
      <td>NaN</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[六合二路, 高雄市]</td>
      <td>NaN</td>
      <td>4b63112cf964a520bc602ae3</td>
    </tr>
    <tr>
      <th>10</th>
      <td>吉林夜市</td>
      <td>Night Market</td>
      <td>吉林街</td>
      <td>NaN</td>
      <td>22.644094</td>
      <td>120.306300</td>
      <td>[{'label': 'display', 'lat': 22.64409416767027...</td>
      <td>1483</td>
      <td>807</td>
      <td>TW</td>
      <td>三民區</td>
      <td>臺灣</td>
      <td>[吉林街, 三民區,  807]</td>
      <td>NaN</td>
      <td>4caf2bed1168a093bdf81f23</td>
    </tr>
    <tr>
      <th>11</th>
      <td>自強夜市</td>
      <td>Night Market</td>
      <td>自強三路</td>
      <td>Lingya 2nd Rd</td>
      <td>22.617052</td>
      <td>120.298500</td>
      <td>[{'label': 'display', 'lat': 22.617052, 'lng':...</td>
      <td>1634</td>
      <td>802</td>
      <td>TW</td>
      <td>苓雅區</td>
      <td>臺灣</td>
      <td>[自強三路 (Lingya 2nd Rd), 苓雅區,  802]</td>
      <td>NaN</td>
      <td>4d14cfc3e190721ef9640a21</td>
    </tr>
    <tr>
      <th>17</th>
      <td>La Rue Market by Love River</td>
      <td>Night Market</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>22.622690</td>
      <td>120.289186</td>
      <td>[{'label': 'display', 'lat': 22.62269, 'lng': ...</td>
      <td>1630</td>
      <td>80341</td>
      <td>TW</td>
      <td>鹽埕</td>
      <td>臺灣</td>
      <td>[鹽埕,  80341]</td>
      <td>NaN</td>
      <td>5ef89070afc30000086503a0</td>
    </tr>
    <tr>
      <th>18</th>
      <td>凱旋國際觀光夜市</td>
      <td>Night Market</td>
      <td>凱旋四路758號</td>
      <td>NaN</td>
      <td>22.599017</td>
      <td>120.319863</td>
      <td>[{'label': 'display', 'lat': 22.5990168291308,...</td>
      <td>4046</td>
      <td>NaN</td>
      <td>TW</td>
      <td>前鎮區</td>
      <td>臺灣</td>
      <td>[凱旋四路758號, 前鎮區]</td>
      <td>NaN</td>
      <td>51f928a1498e00e66f88d4b8</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Rueifeng Night Market (瑞豐夜市)</td>
      <td>Night Market</td>
      <td>裕誠路1128號</td>
      <td>南屏路口</td>
      <td>22.665931</td>
      <td>120.299845</td>
      <td>[{'label': 'display', 'lat': 22.66593113167786...</td>
      <td>3851</td>
      <td>813</td>
      <td>TW</td>
      <td>左營區</td>
      <td>臺灣</td>
      <td>[裕誠路1128號 (南屏路口), 左營區,  813]</td>
      <td>Zuoying</td>
      <td>4bfe429af7c82d7fca4e8f04</td>
    </tr>
    <tr>
      <th>20</th>
      <td>勞工公園夜市</td>
      <td>Night Market</td>
      <td>前鎮區一德路</td>
      <td>NaN</td>
      <td>22.608355</td>
      <td>120.309942</td>
      <td>[{'label': 'display', 'lat': 22.60835517368099...</td>
      <td>2692</td>
      <td>806</td>
      <td>TW</td>
      <td>高雄市</td>
      <td>臺灣</td>
      <td>[前鎮區一德路, 高雄市,  806]</td>
      <td>NaN</td>
      <td>4eca32149adf9c7bf628bf88</td>
    </tr>
    <tr>
      <th>27</th>
      <td>旗津國小夜市</td>
      <td>Night Market</td>
      <td>復興巷</td>
      <td>NaN</td>
      <td>22.606761</td>
      <td>120.271859</td>
      <td>[{'label': 'display', 'lat': 22.606761, 'lng':...</td>
      <td>4132</td>
      <td>805</td>
      <td>TW</td>
      <td>旗津區</td>
      <td>臺灣</td>
      <td>[復興巷, 旗津區,  805]</td>
      <td>NaN</td>
      <td>55685d72498e635cb82e88cb</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Pier-2 Night Market (駁二夜市)</td>
      <td>Night Market</td>
      <td>建國四路71號</td>
      <td>NaN</td>
      <td>22.622526</td>
      <td>120.278661</td>
      <td>[{'label': 'display', 'lat': 22.62252611925212...</td>
      <td>2588</td>
      <td>803</td>
      <td>TW</td>
      <td>鹽埕區</td>
      <td>臺灣</td>
      <td>[建國四路71號, 鹽埕區,  803]</td>
      <td>NaN</td>
      <td>53088ebd498e40cc25a0eb75</td>
    </tr>
    <tr>
      <th>43</th>
      <td>瑞北夜市</td>
      <td>Night Market</td>
      <td>瑞北路</td>
      <td>NaN</td>
      <td>22.609068</td>
      <td>120.330670</td>
      <td>[{'label': 'display', 'lat': 22.609068, 'lng':...</td>
      <td>3857</td>
      <td>806</td>
      <td>TW</td>
      <td>Qianzhen District</td>
      <td>臺灣</td>
      <td>[瑞北路, Qianzhen District,  806]</td>
      <td>NaN</td>
      <td>4d970434942ba093a401618c</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Kaixuan Qingnian Night Market (凱旋青年觀光夜市)</td>
      <td>Night Market</td>
      <td>凱旋四路758號</td>
      <td>NaN</td>
      <td>22.598960</td>
      <td>120.319790</td>
      <td>[{'label': 'display', 'lat': 22.59896, 'lng': ...</td>
      <td>4048</td>
      <td>806</td>
      <td>TW</td>
      <td>前鎮區</td>
      <td>臺灣</td>
      <td>[凱旋四路758號, 前鎮區,  806]</td>
      <td>NaN</td>
      <td>5b00143da0215b002c262d5b</td>
    </tr>
  </tbody>
</table>
</div>



 Visualization time!


```python
venues_map = folium.Map(location=[latitude, longitude], zoom_start=13)
# add a red circle marker to represent the Formosa Boulevard
folium.CircleMarker(
    [latitude, longitude],
    radius=10,
    color='red',
    popup='Formosa Boulevard',
    fill = True,
    fill_color = 'red',
    fill_opacity = 0.6
).add_to(venues_map)

# add the night markets as blue circle markers
for lat, lng, label in zip(dataframe_filtered.lat, dataframe_filtered.lng, dataframe_filtered.categories):
    folium.CircleMarker(
        [lat, lng],
        radius=5,
        color='blue',
        popup=label,
        fill = True,
        fill_color='blue',
        fill_opacity=0.6
    ).add_to(venues_map)

# display map
venues_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=%3C%21DOCTYPE%20html%3E%0A%3Chead%3E%20%20%20%20%0A%20%20%20%20%3Cmeta%20http-equiv%3D%22content-type%22%20content%3D%22text/html%3B%20charset%3DUTF-8%22%20/%3E%0A%20%20%20%20%3Cscript%3EL_PREFER_CANVAS%20%3D%20false%3B%20L_NO_TOUCH%20%3D%20false%3B%20L_DISABLE_3D%20%3D%20false%3B%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//cdn.jsdelivr.net/npm/leaflet%401.2.0/dist/leaflet.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js%22%3E%3C/script%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdn.jsdelivr.net/npm/leaflet%401.2.0/dist/leaflet.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//rawgit.com/python-visualization/folium/master/folium/templates/leaflet.awesome.rotate.css%22/%3E%0A%20%20%20%20%3Cstyle%3Ehtml%2C%20body%20%7Bwidth%3A%20100%25%3Bheight%3A%20100%25%3Bmargin%3A%200%3Bpadding%3A%200%3B%7D%3C/style%3E%0A%20%20%20%20%3Cstyle%3E%23map%20%7Bposition%3Aabsolute%3Btop%3A0%3Bbottom%3A0%3Bright%3A0%3Bleft%3A0%3B%7D%3C/style%3E%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cstyle%3E%20%23map_6bc2d461121b40f28e78fdc61dbfbbe4%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20position%20%3A%20relative%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20width%20%3A%20100.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20height%3A%20100.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20left%3A%200.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20top%3A%200.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%3C/style%3E%0A%20%20%20%20%20%20%20%20%0A%3C/head%3E%0A%3Cbody%3E%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cdiv%20class%3D%22folium-map%22%20id%3D%22map_6bc2d461121b40f28e78fdc61dbfbbe4%22%20%3E%3C/div%3E%0A%20%20%20%20%20%20%20%20%0A%3C/body%3E%0A%3Cscript%3E%20%20%20%20%0A%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20bounds%20%3D%20null%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20map_6bc2d461121b40f28e78fdc61dbfbbe4%20%3D%20L.map%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%27map_6bc2d461121b40f28e78fdc61dbfbbe4%27%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7Bcenter%3A%20%5B22.631388%2C120.301956%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20zoom%3A%2013%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20maxBounds%3A%20bounds%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20layers%3A%20%5B%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20worldCopyJump%3A%20false%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20crs%3A%20L.CRS.EPSG3857%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20tile_layer_69f95372e390429785b4f536e60d15ec%20%3D%20L.tileLayer%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%27https%3A//%7Bs%7D.tile.openstreetmap.org/%7Bz%7D/%7Bx%7D/%7By%7D.png%27%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22attribution%22%3A%20null%2C%0A%20%20%22detectRetina%22%3A%20false%2C%0A%20%20%22maxZoom%22%3A%2018%2C%0A%20%20%22minZoom%22%3A%201%2C%0A%20%20%22noWrap%22%3A%20false%2C%0A%20%20%22subdomains%22%3A%20%22abc%22%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_31db233a09b346be83a3d092091a5243%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.631388%2C120.301956%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22red%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22red%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%2010%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_f0d761c2f9124271ba2feef2d09828de%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_cfd204b47ce44654ae087c6e332ba9b8%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_cfd204b47ce44654ae087c6e332ba9b8%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EFormosa%20Boulevard%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_f0d761c2f9124271ba2feef2d09828de.setContent%28html_cfd204b47ce44654ae087c6e332ba9b8%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_31db233a09b346be83a3d092091a5243.bindPopup%28popup_f0d761c2f9124271ba2feef2d09828de%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_52de48b84af74d9d96953f2afc440fc5%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.629036%2C120.30281%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_40eb9e1559664577ac2de78a85b20047%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_c61e1a8533f54bc7b4ca2c69466c8b9d%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_c61e1a8533f54bc7b4ca2c69466c8b9d%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_40eb9e1559664577ac2de78a85b20047.setContent%28html_c61e1a8533f54bc7b4ca2c69466c8b9d%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_52de48b84af74d9d96953f2afc440fc5.bindPopup%28popup_40eb9e1559664577ac2de78a85b20047%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_1a26e09fca6a4b1d8d44bf25d24212fc%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.62122295591197%2C120.30762239471629%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_25bc6f132c224b74b63a1ca7f7f2862d%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_a8ead39f20054c6c9ca60a45e7a00ae0%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_a8ead39f20054c6c9ca60a45e7a00ae0%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_25bc6f132c224b74b63a1ca7f7f2862d.setContent%28html_a8ead39f20054c6c9ca60a45e7a00ae0%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_1a26e09fca6a4b1d8d44bf25d24212fc.bindPopup%28popup_25bc6f132c224b74b63a1ca7f7f2862d%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_4863aa04880844d9852303232d043848%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.61477347737183%2C120.30688047409058%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_e5974b40943740e1bc87a3141251cf29%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_6bba7b72eb9040eea874ffaaa766ef15%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_6bba7b72eb9040eea874ffaaa766ef15%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_e5974b40943740e1bc87a3141251cf29.setContent%28html_6bba7b72eb9040eea874ffaaa766ef15%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_4863aa04880844d9852303232d043848.bindPopup%28popup_e5974b40943740e1bc87a3141251cf29%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_0d4b91ecd7754641a32c2d03d822af85%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.61709490241331%2C120.31824162270296%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_3d60d94475214679838c9d4d71da831d%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_2abc6f593b9e4c44b2b6b42da6079f63%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_2abc6f593b9e4c44b2b6b42da6079f63%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_3d60d94475214679838c9d4d71da831d.setContent%28html_2abc6f593b9e4c44b2b6b42da6079f63%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_0d4b91ecd7754641a32c2d03d822af85.bindPopup%28popup_3d60d94475214679838c9d4d71da831d%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_765dbbe4e8314848a1c8be91bfa47d4f%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.599798175127876%2C120.32077729593117%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_43982989ca4c4e0caac1c55caecbe306%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_9fb6ff90c5aa4e338cd0da5374fc26a5%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_9fb6ff90c5aa4e338cd0da5374fc26a5%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_43982989ca4c4e0caac1c55caecbe306.setContent%28html_9fb6ff90c5aa4e338cd0da5374fc26a5%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_765dbbe4e8314848a1c8be91bfa47d4f.bindPopup%28popup_43982989ca4c4e0caac1c55caecbe306%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_5eaa0d521d6e4dfa81759637bb69e1c6%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.63243761586824%2C120.30008299470865%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_85d39cbfe1b746bdb8992c000cf46af1%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_d03ffa7aa35d48fc827f265ebe7ea38a%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_d03ffa7aa35d48fc827f265ebe7ea38a%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_85d39cbfe1b746bdb8992c000cf46af1.setContent%28html_d03ffa7aa35d48fc827f265ebe7ea38a%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_5eaa0d521d6e4dfa81759637bb69e1c6.bindPopup%28popup_85d39cbfe1b746bdb8992c000cf46af1%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_4b4883c5cae849b1b9dc147be814f89c%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.64409416767027%2C120.30630029864416%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_c4f565d254e14811a928aafa6c1c593d%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_950248c3e40246c690888807d4209240%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_950248c3e40246c690888807d4209240%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_c4f565d254e14811a928aafa6c1c593d.setContent%28html_950248c3e40246c690888807d4209240%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_4b4883c5cae849b1b9dc147be814f89c.bindPopup%28popup_c4f565d254e14811a928aafa6c1c593d%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_b5030c2ce92d45fa8ef74ba60d3fc5fb%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.617052%2C120.2985%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_2f5f42082cfd43df9bbde77641199f4b%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_f5f430a9096841198a737a70eb565c15%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_f5f430a9096841198a737a70eb565c15%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_2f5f42082cfd43df9bbde77641199f4b.setContent%28html_f5f430a9096841198a737a70eb565c15%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_b5030c2ce92d45fa8ef74ba60d3fc5fb.bindPopup%28popup_2f5f42082cfd43df9bbde77641199f4b%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_8bd90bbc7fb9461c80ab294676677c9f%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.62269%2C120.289186%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_b30c664c5adf46e6b73cbaecba9af875%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_b038bc6f670948c596f1343b7bd29e9c%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_b038bc6f670948c596f1343b7bd29e9c%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_b30c664c5adf46e6b73cbaecba9af875.setContent%28html_b038bc6f670948c596f1343b7bd29e9c%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_8bd90bbc7fb9461c80ab294676677c9f.bindPopup%28popup_b30c664c5adf46e6b73cbaecba9af875%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_b1de8ec5c6864874b87f6473d249f18c%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.5990168291308%2C120.31986256539282%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_e446bb73e2da45f993d4cb51b4ff356b%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_7e29f0d26d5e4ceebeb96cfa85bf22c1%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_7e29f0d26d5e4ceebeb96cfa85bf22c1%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_e446bb73e2da45f993d4cb51b4ff356b.setContent%28html_7e29f0d26d5e4ceebeb96cfa85bf22c1%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_b1de8ec5c6864874b87f6473d249f18c.bindPopup%28popup_e446bb73e2da45f993d4cb51b4ff356b%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_5019ee49a51f48529c6a781554b5e9d1%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.66593113167786%2C120.29984450584828%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_e00e8ac6f92d416e912a0a219a8e74bb%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_18ebcbbc806e41aebc6fdaff507d90af%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_18ebcbbc806e41aebc6fdaff507d90af%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_e00e8ac6f92d416e912a0a219a8e74bb.setContent%28html_18ebcbbc806e41aebc6fdaff507d90af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_5019ee49a51f48529c6a781554b5e9d1.bindPopup%28popup_e00e8ac6f92d416e912a0a219a8e74bb%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_fa1c4e19d3a54d55a7827b48bb3b32a2%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.60835517368099%2C120.30994244725024%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_1788e077da7f48869b5c028c9576a415%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_33cd381df60149a68142dcf86dc9e496%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_33cd381df60149a68142dcf86dc9e496%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_1788e077da7f48869b5c028c9576a415.setContent%28html_33cd381df60149a68142dcf86dc9e496%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_fa1c4e19d3a54d55a7827b48bb3b32a2.bindPopup%28popup_1788e077da7f48869b5c028c9576a415%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_db0768c4bd93499f8e118637e350067e%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.606761%2C120.271859%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_3c56f50100524bb39acde5496db6dfe2%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_e20bd1bbc8af4982b76b2754cb30de92%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_e20bd1bbc8af4982b76b2754cb30de92%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_3c56f50100524bb39acde5496db6dfe2.setContent%28html_e20bd1bbc8af4982b76b2754cb30de92%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_db0768c4bd93499f8e118637e350067e.bindPopup%28popup_3c56f50100524bb39acde5496db6dfe2%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_3a5807a95ab24fde9d261c927d7908a6%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.622526119252125%2C120.2786605154157%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_7dd63ccd22054ce2be04d347f9cedd24%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_dde3ce394e884f4fa03fbd7dda8ca6b2%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_dde3ce394e884f4fa03fbd7dda8ca6b2%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_7dd63ccd22054ce2be04d347f9cedd24.setContent%28html_dde3ce394e884f4fa03fbd7dda8ca6b2%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_3a5807a95ab24fde9d261c927d7908a6.bindPopup%28popup_7dd63ccd22054ce2be04d347f9cedd24%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_a5edb1c1689e476e8b268d25c17535d9%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.609068%2C120.33067%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_fcdbb3201db64c9daf3a60b9bb35e933%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_de6ae5b7d4814b5c9a3d22b5589ea10b%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_de6ae5b7d4814b5c9a3d22b5589ea10b%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_fcdbb3201db64c9daf3a60b9bb35e933.setContent%28html_de6ae5b7d4814b5c9a3d22b5589ea10b%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_a5edb1c1689e476e8b268d25c17535d9.bindPopup%28popup_fcdbb3201db64c9daf3a60b9bb35e933%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_0b96d17fdb9e4e63b069a78c2dad00b1%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.59896%2C120.31979%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_6bc2d461121b40f28e78fdc61dbfbbe4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_17fcf81781f442339b5ad336e70bea54%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_8425297e58a64c64a4600ee6356bae78%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_8425297e58a64c64a4600ee6356bae78%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENight%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_17fcf81781f442339b5ad336e70bea54.setContent%28html_8425297e58a64c64a4600ee6356bae78%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_0b96d17fdb9e4e63b069a78c2dad00b1.bindPopup%28popup_17fcf81781f442339b5ad336e70bea54%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%3C/script%3E onload="this.contentDocument.open();this.contentDocument.write(    decodeURIComponent(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python
dataframe_filtered.shape[0]
```




    16



We got 16 night markets by query foursquare.

Suppose we can visit many night markets in one night, and stay 3 days in Kaohsiung.

Let's apply K-means on latitude and longitude with K as 3, and find which night markets are in the same night.


```python
from sklearn import cluster, datasets, metrics
from sklearn.cluster import KMeans
```


```python
n = 3
datalist = [pd.DataFrame() for _ in range(n)]
kmeans = KMeans(n_clusters = n, init = 'k-means++', max_iter = 300, n_init = 10, random_state = 0)
pred_y = kmeans.fit_predict(dataframe_filtered[["lat", "lng"]])
for i in range(0, n):
    datalist[i] = dataframe_filtered.loc[pred_y == i, ['name', 'lat', 'lng']]
```


```python
# Day 1
datalist[0]
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
      <th>name</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>忠孝夜市 Zhongsiao Night Market</td>
      <td>22.621223</td>
      <td>120.307622</td>
    </tr>
    <tr>
      <th>2</th>
      <td>興中觀光夜市 Sing Jhong Night Market</td>
      <td>22.614773</td>
      <td>120.306880</td>
    </tr>
    <tr>
      <th>4</th>
      <td>光華夜市 Guanghua Night Market</td>
      <td>22.617095</td>
      <td>120.318242</td>
    </tr>
    <tr>
      <th>5</th>
      <td>金鑽觀光夜市 Jin-Zuan Night Market</td>
      <td>22.599798</td>
      <td>120.320777</td>
    </tr>
    <tr>
      <th>18</th>
      <td>凱旋國際觀光夜市</td>
      <td>22.599017</td>
      <td>120.319863</td>
    </tr>
    <tr>
      <th>20</th>
      <td>勞工公園夜市</td>
      <td>22.608355</td>
      <td>120.309942</td>
    </tr>
    <tr>
      <th>43</th>
      <td>瑞北夜市</td>
      <td>22.609068</td>
      <td>120.330670</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Kaixuan Qingnian Night Market (凱旋青年觀光夜市)</td>
      <td>22.598960</td>
      <td>120.319790</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Day 2
datalist[1]
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
      <th>name</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>11</th>
      <td>自強夜市</td>
      <td>22.617052</td>
      <td>120.298500</td>
    </tr>
    <tr>
      <th>17</th>
      <td>La Rue Market by Love River</td>
      <td>22.622690</td>
      <td>120.289186</td>
    </tr>
    <tr>
      <th>27</th>
      <td>旗津國小夜市</td>
      <td>22.606761</td>
      <td>120.271859</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Pier-2 Night Market (駁二夜市)</td>
      <td>22.622526</td>
      <td>120.278661</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Day 3
datalist[2]
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
      <th>name</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nanhua Night Market (南華夜市)</td>
      <td>22.629036</td>
      <td>120.302810</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Liouhe Night Market (六合夜市)</td>
      <td>22.632438</td>
      <td>120.300083</td>
    </tr>
    <tr>
      <th>10</th>
      <td>吉林夜市</td>
      <td>22.644094</td>
      <td>120.306300</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Rueifeng Night Market (瑞豐夜市)</td>
      <td>22.665931</td>
      <td>120.299845</td>
    </tr>
  </tbody>
</table>
</div>




```python
venues_map = folium.Map(location=[latitude, longitude], zoom_start=13)

# add the day 1 night markets as blue circle markers
for lat, lng, name in zip(datalist[0].lat, datalist[0].lng, datalist[0].name):
    folium.CircleMarker(
        [lat, lng],
        radius=5,
        color='blue',
        popup=name,
        fill = True,
        fill_color='blue',
        fill_opacity=0.6
    ).add_to(venues_map)
# add the day 2 night markets as green circle markers
for lat, lng, name in zip(datalist[1].lat, datalist[1].lng, datalist[1].name):
    folium.CircleMarker(
        [lat, lng],
        radius=5,
        color='green',
        popup=name,
        fill = True,
        fill_color='green',
        fill_opacity=0.6
    ).add_to(venues_map)
# add the day 3 night markets as purple circle markers
for lat, lng, name in zip(datalist[2].lat, datalist[2].lng, datalist[2].name):
    folium.CircleMarker(
        [lat, lng],
        radius=5,
        color='purple',
        popup=name,
        fill = True,
        fill_color='purple',
        fill_opacity=0.6
    ).add_to(venues_map)

# display map
venues_map
```




<div style="width:100%;"><div style="position:relative;width:100%;height:0;padding-bottom:60%;"><span style="color:#565656">Make this Notebook Trusted to load map: File -> Trust Notebook</span><iframe src="about:blank" style="position:absolute;width:100%;height:100%;left:0;top:0;border:none !important;" data-html=%3C%21DOCTYPE%20html%3E%0A%3Chead%3E%20%20%20%20%0A%20%20%20%20%3Cmeta%20http-equiv%3D%22content-type%22%20content%3D%22text/html%3B%20charset%3DUTF-8%22%20/%3E%0A%20%20%20%20%3Cscript%3EL_PREFER_CANVAS%20%3D%20false%3B%20L_NO_TOUCH%20%3D%20false%3B%20L_DISABLE_3D%20%3D%20false%3B%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//cdn.jsdelivr.net/npm/leaflet%401.2.0/dist/leaflet.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//ajax.googleapis.com/ajax/libs/jquery/1.11.1/jquery.min.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/js/bootstrap.min.js%22%3E%3C/script%3E%0A%20%20%20%20%3Cscript%20src%3D%22https%3A//cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.js%22%3E%3C/script%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdn.jsdelivr.net/npm/leaflet%401.2.0/dist/leaflet.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap-theme.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//maxcdn.bootstrapcdn.com/font-awesome/4.6.3/css/font-awesome.min.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//cdnjs.cloudflare.com/ajax/libs/Leaflet.awesome-markers/2.0.2/leaflet.awesome-markers.css%22/%3E%0A%20%20%20%20%3Clink%20rel%3D%22stylesheet%22%20href%3D%22https%3A//rawgit.com/python-visualization/folium/master/folium/templates/leaflet.awesome.rotate.css%22/%3E%0A%20%20%20%20%3Cstyle%3Ehtml%2C%20body%20%7Bwidth%3A%20100%25%3Bheight%3A%20100%25%3Bmargin%3A%200%3Bpadding%3A%200%3B%7D%3C/style%3E%0A%20%20%20%20%3Cstyle%3E%23map%20%7Bposition%3Aabsolute%3Btop%3A0%3Bbottom%3A0%3Bright%3A0%3Bleft%3A0%3B%7D%3C/style%3E%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cstyle%3E%20%23map_e9d34c055a024c3d9488ba26b9e034af%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20position%20%3A%20relative%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20width%20%3A%20100.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20height%3A%20100.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20left%3A%200.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20top%3A%200.0%25%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%3C/style%3E%0A%20%20%20%20%20%20%20%20%0A%3C/head%3E%0A%3Cbody%3E%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%3Cdiv%20class%3D%22folium-map%22%20id%3D%22map_e9d34c055a024c3d9488ba26b9e034af%22%20%3E%3C/div%3E%0A%20%20%20%20%20%20%20%20%0A%3C/body%3E%0A%3Cscript%3E%20%20%20%20%0A%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20bounds%20%3D%20null%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20map_e9d34c055a024c3d9488ba26b9e034af%20%3D%20L.map%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%27map_e9d34c055a024c3d9488ba26b9e034af%27%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7Bcenter%3A%20%5B22.631388%2C120.301956%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20zoom%3A%2013%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20maxBounds%3A%20bounds%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20layers%3A%20%5B%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20worldCopyJump%3A%20false%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20crs%3A%20L.CRS.EPSG3857%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7D%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20tile_layer_033fdcc83f7b47cf9a9bcc40c3bf704e%20%3D%20L.tileLayer%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%27https%3A//%7Bs%7D.tile.openstreetmap.org/%7Bz%7D/%7Bx%7D/%7By%7D.png%27%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22attribution%22%3A%20null%2C%0A%20%20%22detectRetina%22%3A%20false%2C%0A%20%20%22maxZoom%22%3A%2018%2C%0A%20%20%22minZoom%22%3A%201%2C%0A%20%20%22noWrap%22%3A%20false%2C%0A%20%20%22subdomains%22%3A%20%22abc%22%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_1a8963f1efc4438fb91e5aef52101b82%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.62122295591197%2C120.30762239471629%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_d84ce7c76aa2407fafc3776779d5dd3f%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_95a7682d54a7456b9a19380fd4642fd4%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_95a7682d54a7456b9a19380fd4642fd4%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%E5%BF%A0%E5%AD%9D%E5%A4%9C%E5%B8%82%20Zhongsiao%20Night%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_d84ce7c76aa2407fafc3776779d5dd3f.setContent%28html_95a7682d54a7456b9a19380fd4642fd4%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_1a8963f1efc4438fb91e5aef52101b82.bindPopup%28popup_d84ce7c76aa2407fafc3776779d5dd3f%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_04148f3383ae449ca588809452d43508%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.61477347737183%2C120.30688047409058%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_542994866b2d4a1792c8d940107b47f5%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_a4f2362a683e4125aa5613284d32c737%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_a4f2362a683e4125aa5613284d32c737%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%E8%88%88%E4%B8%AD%E8%A7%80%E5%85%89%E5%A4%9C%E5%B8%82%20Sing%20Jhong%20Night%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_542994866b2d4a1792c8d940107b47f5.setContent%28html_a4f2362a683e4125aa5613284d32c737%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_04148f3383ae449ca588809452d43508.bindPopup%28popup_542994866b2d4a1792c8d940107b47f5%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_0b594c3ca398476fbd2f2ddcf8159e86%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.61709490241331%2C120.31824162270296%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_62ba9f6a41a4432ebed83340726e535f%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_3ca645629e4d46539ec72be7986579c3%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_3ca645629e4d46539ec72be7986579c3%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%E5%85%89%E8%8F%AF%E5%A4%9C%E5%B8%82%20Guanghua%20Night%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_62ba9f6a41a4432ebed83340726e535f.setContent%28html_3ca645629e4d46539ec72be7986579c3%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_0b594c3ca398476fbd2f2ddcf8159e86.bindPopup%28popup_62ba9f6a41a4432ebed83340726e535f%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_f2b8747cb4b240548c4b274439cedb88%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.599798175127876%2C120.32077729593117%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_a6cd5e1783964a50b5187774bc5ce0e8%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_0fe0ab71da6947da9d908ac3a652abb8%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_0fe0ab71da6947da9d908ac3a652abb8%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%E9%87%91%E9%91%BD%E8%A7%80%E5%85%89%E5%A4%9C%E5%B8%82%20Jin-Zuan%20Night%20Market%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_a6cd5e1783964a50b5187774bc5ce0e8.setContent%28html_0fe0ab71da6947da9d908ac3a652abb8%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_f2b8747cb4b240548c4b274439cedb88.bindPopup%28popup_a6cd5e1783964a50b5187774bc5ce0e8%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_8b3418e4a5d449098df3c184c25868d1%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.5990168291308%2C120.31986256539282%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_0fffdc01ef9447c1bf046726fe706821%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_b69ade95d016417a9298aca7efa47062%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_b69ade95d016417a9298aca7efa47062%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%E5%87%B1%E6%97%8B%E5%9C%8B%E9%9A%9B%E8%A7%80%E5%85%89%E5%A4%9C%E5%B8%82%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_0fffdc01ef9447c1bf046726fe706821.setContent%28html_b69ade95d016417a9298aca7efa47062%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_8b3418e4a5d449098df3c184c25868d1.bindPopup%28popup_0fffdc01ef9447c1bf046726fe706821%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_c5e6d1fb170c4608b50dd568a12291e4%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.60835517368099%2C120.30994244725024%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_0a5a3934ff054db4a46c01a5492e4d30%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_72a9c1d3bd0d4061bc866b5f3ae137f6%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_72a9c1d3bd0d4061bc866b5f3ae137f6%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%E5%8B%9E%E5%B7%A5%E5%85%AC%E5%9C%92%E5%A4%9C%E5%B8%82%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_0a5a3934ff054db4a46c01a5492e4d30.setContent%28html_72a9c1d3bd0d4061bc866b5f3ae137f6%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_c5e6d1fb170c4608b50dd568a12291e4.bindPopup%28popup_0a5a3934ff054db4a46c01a5492e4d30%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_3457dc2935c54af2a01b2dbd2b54e4a6%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.609068%2C120.33067%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_db044ab5357748fc8d12e0ccf18f90b6%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_b050b8101b854ddc9a987ed80ec11d8b%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_b050b8101b854ddc9a987ed80ec11d8b%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%E7%91%9E%E5%8C%97%E5%A4%9C%E5%B8%82%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_db044ab5357748fc8d12e0ccf18f90b6.setContent%28html_b050b8101b854ddc9a987ed80ec11d8b%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_3457dc2935c54af2a01b2dbd2b54e4a6.bindPopup%28popup_db044ab5357748fc8d12e0ccf18f90b6%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_cf5ef5514c6e4951b192dff63012675c%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.59896%2C120.31979%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22blue%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22blue%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_699ad577250948a3b8d18f5a7852101a%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_453a36c7bd2a486d876fa9de26c18c7e%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_453a36c7bd2a486d876fa9de26c18c7e%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EKaixuan%20Qingnian%20Night%20Market%20%28%E5%87%B1%E6%97%8B%E9%9D%92%E5%B9%B4%E8%A7%80%E5%85%89%E5%A4%9C%E5%B8%82%29%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_699ad577250948a3b8d18f5a7852101a.setContent%28html_453a36c7bd2a486d876fa9de26c18c7e%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_cf5ef5514c6e4951b192dff63012675c.bindPopup%28popup_699ad577250948a3b8d18f5a7852101a%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_f976f625620b445c8365b41150f49bba%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.617052%2C120.2985%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22green%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22green%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_0595b91d5f704e8d8bf71013b1f35979%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_e96e1ef29bf543bc8e278bb29a23cc5d%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_e96e1ef29bf543bc8e278bb29a23cc5d%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%E8%87%AA%E5%BC%B7%E5%A4%9C%E5%B8%82%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_0595b91d5f704e8d8bf71013b1f35979.setContent%28html_e96e1ef29bf543bc8e278bb29a23cc5d%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_f976f625620b445c8365b41150f49bba.bindPopup%28popup_0595b91d5f704e8d8bf71013b1f35979%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_7f0a8582bb83463298f173045295c167%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.62269%2C120.289186%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22green%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22green%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_9bab6ad3f6ae4df1ab0f32379f8be0f3%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_937207c6fa884135876eacde9702c806%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_937207c6fa884135876eacde9702c806%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ELa%20Rue%20Market%20by%20Love%20River%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_9bab6ad3f6ae4df1ab0f32379f8be0f3.setContent%28html_937207c6fa884135876eacde9702c806%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_7f0a8582bb83463298f173045295c167.bindPopup%28popup_9bab6ad3f6ae4df1ab0f32379f8be0f3%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_c1044298296f43b2b6acd541557a8883%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.606761%2C120.271859%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22green%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22green%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_eeaf200c1dbf4e0c9d75c33ae348cf28%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_3d2e5b9408504e6e95332b7190ff83b2%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_3d2e5b9408504e6e95332b7190ff83b2%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%E6%97%97%E6%B4%A5%E5%9C%8B%E5%B0%8F%E5%A4%9C%E5%B8%82%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_eeaf200c1dbf4e0c9d75c33ae348cf28.setContent%28html_3d2e5b9408504e6e95332b7190ff83b2%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_c1044298296f43b2b6acd541557a8883.bindPopup%28popup_eeaf200c1dbf4e0c9d75c33ae348cf28%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_3afbbb7b948a4fe983ac619124c70386%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.622526119252125%2C120.2786605154157%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22green%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22green%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_9b10886a399945d6a9c0f400f3abeff2%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_05141358ae354b1ab7073adc4adcbf00%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_05141358ae354b1ab7073adc4adcbf00%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3EPier-2%20Night%20Market%20%28%E9%A7%81%E4%BA%8C%E5%A4%9C%E5%B8%82%29%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_9b10886a399945d6a9c0f400f3abeff2.setContent%28html_05141358ae354b1ab7073adc4adcbf00%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_3afbbb7b948a4fe983ac619124c70386.bindPopup%28popup_9b10886a399945d6a9c0f400f3abeff2%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_6b19758f43874d17ba9cbc2a6ec92a48%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.629036%2C120.30281%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22purple%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22purple%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_eb382c9708e542948c571862d1577b33%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_9c3f93937ef44d28bd1ff541de0f8ec5%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_9c3f93937ef44d28bd1ff541de0f8ec5%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ENanhua%20Night%20Market%20%28%E5%8D%97%E8%8F%AF%E5%A4%9C%E5%B8%82%29%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_eb382c9708e542948c571862d1577b33.setContent%28html_9c3f93937ef44d28bd1ff541de0f8ec5%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_6b19758f43874d17ba9cbc2a6ec92a48.bindPopup%28popup_eb382c9708e542948c571862d1577b33%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_b9fadb5827ee4d68865d640c4f57c6e5%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.63243761586824%2C120.30008299470865%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22purple%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22purple%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_77c8f108fa634a8a90efad7242497593%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_55fb7b527a8746a6aa653f7cb18d2841%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_55fb7b527a8746a6aa653f7cb18d2841%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ELiouhe%20Night%20Market%20%28%E5%85%AD%E5%90%88%E5%A4%9C%E5%B8%82%29%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_77c8f108fa634a8a90efad7242497593.setContent%28html_55fb7b527a8746a6aa653f7cb18d2841%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_b9fadb5827ee4d68865d640c4f57c6e5.bindPopup%28popup_77c8f108fa634a8a90efad7242497593%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_dceac07bbbf443f78af1c3389836572a%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.64409416767027%2C120.30630029864416%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22purple%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22purple%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_c1b7b77d441c4950be8466c6ff7a4595%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_5d010be86eb94e028a042fa5a24aba51%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_5d010be86eb94e028a042fa5a24aba51%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3E%E5%90%89%E6%9E%97%E5%A4%9C%E5%B8%82%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_c1b7b77d441c4950be8466c6ff7a4595.setContent%28html_5d010be86eb94e028a042fa5a24aba51%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_dceac07bbbf443f78af1c3389836572a.bindPopup%28popup_c1b7b77d441c4950be8466c6ff7a4595%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20circle_marker_0d8e61f6a172406687533a95ccbb97fb%20%3D%20L.circleMarker%28%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%5B22.66593113167786%2C120.29984450584828%5D%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%7B%0A%20%20%22bubblingMouseEvents%22%3A%20true%2C%0A%20%20%22color%22%3A%20%22purple%22%2C%0A%20%20%22dashArray%22%3A%20null%2C%0A%20%20%22dashOffset%22%3A%20null%2C%0A%20%20%22fill%22%3A%20true%2C%0A%20%20%22fillColor%22%3A%20%22purple%22%2C%0A%20%20%22fillOpacity%22%3A%200.6%2C%0A%20%20%22fillRule%22%3A%20%22evenodd%22%2C%0A%20%20%22lineCap%22%3A%20%22round%22%2C%0A%20%20%22lineJoin%22%3A%20%22round%22%2C%0A%20%20%22opacity%22%3A%201.0%2C%0A%20%20%22radius%22%3A%205%2C%0A%20%20%22stroke%22%3A%20true%2C%0A%20%20%22weight%22%3A%203%0A%7D%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%29.addTo%28map_e9d34c055a024c3d9488ba26b9e034af%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20var%20popup_526f61e45ee344718f82aad0e8f7ff6e%20%3D%20L.popup%28%7BmaxWidth%3A%20%27300%27%7D%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20var%20html_60f745af76324de6ae022583d2ceef24%20%3D%20%24%28%27%3Cdiv%20id%3D%22html_60f745af76324de6ae022583d2ceef24%22%20style%3D%22width%3A%20100.0%25%3B%20height%3A%20100.0%25%3B%22%3ERueifeng%20Night%20Market%20%28%E7%91%9E%E8%B1%90%E5%A4%9C%E5%B8%82%29%3C/div%3E%27%29%5B0%5D%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20%20popup_526f61e45ee344718f82aad0e8f7ff6e.setContent%28html_60f745af76324de6ae022583d2ceef24%29%3B%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20circle_marker_0d8e61f6a172406687533a95ccbb97fb.bindPopup%28popup_526f61e45ee344718f82aad0e8f7ff6e%29%3B%0A%0A%20%20%20%20%20%20%20%20%20%20%20%20%0A%20%20%20%20%20%20%20%20%0A%3C/script%3E onload="this.contentDocument.open();this.contentDocument.write(    decodeURIComponent(this.getAttribute('data-html')));this.contentDocument.close();" allowfullscreen webkitallowfullscreen mozallowfullscreen></iframe></div></div>




```python
# Day 1 old city day: 
datalist[0]
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
      <th>name</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>忠孝夜市 Zhongsiao Night Market</td>
      <td>22.621223</td>
      <td>120.307622</td>
    </tr>
    <tr>
      <th>2</th>
      <td>興中觀光夜市 Sing Jhong Night Market</td>
      <td>22.614773</td>
      <td>120.306880</td>
    </tr>
    <tr>
      <th>4</th>
      <td>光華夜市 Guanghua Night Market</td>
      <td>22.617095</td>
      <td>120.318242</td>
    </tr>
    <tr>
      <th>5</th>
      <td>金鑽觀光夜市 Jin-Zuan Night Market</td>
      <td>22.599798</td>
      <td>120.320777</td>
    </tr>
    <tr>
      <th>18</th>
      <td>凱旋國際觀光夜市</td>
      <td>22.599017</td>
      <td>120.319863</td>
    </tr>
    <tr>
      <th>20</th>
      <td>勞工公園夜市</td>
      <td>22.608355</td>
      <td>120.309942</td>
    </tr>
    <tr>
      <th>43</th>
      <td>瑞北夜市</td>
      <td>22.609068</td>
      <td>120.330670</td>
    </tr>
    <tr>
      <th>45</th>
      <td>Kaixuan Qingnian Night Market (凱旋青年觀光夜市)</td>
      <td>22.598960</td>
      <td>120.319790</td>
    </tr>
  </tbody>
</table>
</div>



These Night markey are near by airport. 

Kaixuan Qingnian Night Market are near Kaohsiung Light Rail.

If you need to buy something, you can go to Dream mall. It is the best mall in Taiwan.


```python
# Day 2 Love River day: 
datalist[1]
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
      <th>name</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>11</th>
      <td>自強夜市</td>
      <td>22.617052</td>
      <td>120.298500</td>
    </tr>
    <tr>
      <th>17</th>
      <td>La Rue Market by Love River</td>
      <td>22.622690</td>
      <td>120.289186</td>
    </tr>
    <tr>
      <th>27</th>
      <td>旗津國小夜市</td>
      <td>22.606761</td>
      <td>120.271859</td>
    </tr>
    <tr>
      <th>37</th>
      <td>Pier-2 Night Market (駁二夜市)</td>
      <td>22.622526</td>
      <td>120.278661</td>
    </tr>
  </tbody>
</table>
</div>



Love River lies across Kaohsiung, it is a romantic place. 
High quality hotels and facilities are waiting for you. 

If you like seafood, you can go Cijin District by boat.

Intimate reminder for you, Pier-2 night market just open on Saturday.
More introducion can be seen on [Kaohsiung Travel](https://khh.travel/en/attractions/detail/499)


```python
# Day 3: Downtown day
datalist[2]
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
      <th>name</th>
      <th>lat</th>
      <th>lng</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Nanhua Night Market (南華夜市)</td>
      <td>22.629036</td>
      <td>120.302810</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Liouhe Night Market (六合夜市)</td>
      <td>22.632438</td>
      <td>120.300083</td>
    </tr>
    <tr>
      <th>10</th>
      <td>吉林夜市</td>
      <td>22.644094</td>
      <td>120.306300</td>
    </tr>
    <tr>
      <th>19</th>
      <td>Rueifeng Night Market (瑞豐夜市)</td>
      <td>22.665931</td>
      <td>120.299845</td>
    </tr>
  </tbody>
</table>
</div>




```python
venue_id = '4bfe429af7c82d7fca4e8f04' # ID of Rueifeng Night Market
url = 'https://api.foursquare.com/v2/venues/{}?client_id={}&client_secret={}&oauth_token={}&v={}'.format(venue_id, CLIENT_ID, CLIENT_SECRET,ACCESS_TOKEN, VERSION)
url
result = requests.get(url).json()
result['response']['venue']
# show rating
try:
    print('Rueifeng Night Market rating:', result['response']['venue']['rating'])
except:
    print('This venue has not been rated yet.')
```

    Rueifeng Night Market rating: 6.9


As a local. I can say that [Rueifeng Night Market](https://en.wikipedia.org/wiki/Ruifeng_Night_Market) is the first night market we will think of. It is the best end for your three days trip. 

Taiwan High-Speed Railway are nearby, you can visit another city in Taiwan conveniently.

Click the above link and view Wiki. 


```python
venue_id = '4b63112cf964a520bc602ae3' # ID of Liouhe Night Market
url = 'https://api.foursquare.com/v2/venues/{}?client_id={}&client_secret={}&oauth_token={}&v={}'.format(venue_id, CLIENT_ID, CLIENT_SECRET,ACCESS_TOKEN, VERSION)
url
result = requests.get(url).json()
result['response']['venue']
try:
    print('Liouhe Night Market rating:', result['response']['venue']['rating'])
except:
    print('This venue has not been rated yet.')
```

    Liouhe Night Market rating: 6.0


[Liouhe Night Market](https://en.wikipedia.org/wiki/Liuhe_Night_Market) is a tourist night market in the downtown.

It provides high-quality seafood and small eats. Because of the higher price, people are fewer than Ruifeng. I recommend bald boss fried rice for you, it just cost about 2 USD and delicious.

Click the above link and view Wiki.

# Looking forward to your arrival :)


```python

```
