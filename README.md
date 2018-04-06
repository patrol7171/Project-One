

```python
# Dependencies
import requests
import json
import pandas as pd
import matplotlib.pyplot as plt
import pickle
import seaborn as sns
sns.set(color_codes=True)


# Google developer API key
gkey='AIzaSyApeF7ERvT-pj38Y3qau5D6I7eM6g-MbWc'
headers = {"Authorization": "Bearer MorDAXlhkrcLM04Ek5Ya0vBT7A1rG4OZMGSb8Sqc6rE_AMcSF3pIq5Ed43YrWd6xsaQfi82g-MChCLNhh0Z-G7HrUUXkL7wd4kUesLg3quXFia-B5sBOJhHvAwfEWnYx"}

```


```python
#Don't Run This. It takes forever. Also saved data in pickle file
#lats=[]
#longs=[]
#counter=0
#for i in monuments:
#        # Target city
#    target_city = i

    # Build the endpoint URL
#    target_url = "https://maps.googleapis.com/maps/api/geocode/json?" \
#    "address=%s&key=%s" % (target_city, gkey)
#    geo_data = requests.get(target_url).json()

    # Print the json
#    counter+=1
#    try:
#        lats.append(geo_data['results'][0]['geometry']['location']['lat'])
#        longs.append(geo_data['results'][0]['geometry']['location']['lng'])
#    except:
#        print(counter)
#        break
```


```python
# Unpickling (Saved Lists)
with open("lats.txt", "rb") as fp:   
    lats = pickle.load(fp)
with open("longs.txt", "rb") as fp:   
    longs = pickle.load(fp)
with open("monuments.txt", "rb") as fp:   
    monuments = pickle.load(fp)
```


```python
dist=[]
price=[]
country=[]
rating=[]
reviewcount=[]
types=[]
monument=[]
ids=[]
for i in range(len(lats)):
    a=requests.get(f'https://api.yelp.com/v3/businesses/search?latitude={lats[i]}&longitude={longs[i]}',headers=headers).json()
    for j in a['businesses']:
        dist.append(j['distance'])
        country.append(j['location']['country'])
        rating.append(j['rating'])
        reviewcount.append(j['review_count'])
        monument.append(monuments[i])
        try:
            price.append(len(j['price']))
        except:
            price.append('n/a')
        try:
            types.append(j['categories'][0]['title'])
        except:
            types.append('n/a')
        try:
            ids.append(j['id'])
        except:
            break
    
```


```python
e={'Distance':dist,'Price':price,'Country':country,'Rating':rating,'Review Count':reviewcount,'Monument':monument,'Food Type':types}
f=pd.DataFrame(e)
f
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Distance</th>
      <th>Food Type</th>
      <th>Monument</th>
      <th>Price</th>
      <th>Rating</th>
      <th>Review Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>US</td>
      <td>75.949926</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Statue of Liberty</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>821</td>
    </tr>
    <tr>
      <th>1</th>
      <td>US</td>
      <td>1131.717451</td>
      <td>Museums</td>
      <td>Statue of Liberty</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>291</td>
    </tr>
    <tr>
      <th>2</th>
      <td>US</td>
      <td>2377.859054</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Statue of Liberty</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>271</td>
    </tr>
    <tr>
      <th>3</th>
      <td>US</td>
      <td>1798.598168</td>
      <td>Parks</td>
      <td>Statue of Liberty</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>178</td>
    </tr>
    <tr>
      <th>4</th>
      <td>US</td>
      <td>2875.471385</td>
      <td>Barbeque</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>984</td>
    </tr>
    <tr>
      <th>5</th>
      <td>US</td>
      <td>2997.020520</td>
      <td>Mexican</td>
      <td>Statue of Liberty</td>
      <td>1</td>
      <td>4.0</td>
      <td>1441</td>
    </tr>
    <tr>
      <th>6</th>
      <td>US</td>
      <td>3169.119140</td>
      <td>Italian</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>595</td>
    </tr>
    <tr>
      <th>7</th>
      <td>US</td>
      <td>3529.086258</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>1</td>
      <td>4.5</td>
      <td>346</td>
    </tr>
    <tr>
      <th>8</th>
      <td>US</td>
      <td>2798.573094</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>522</td>
    </tr>
    <tr>
      <th>9</th>
      <td>US</td>
      <td>3211.041203</td>
      <td>Korean</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>653</td>
    </tr>
    <tr>
      <th>10</th>
      <td>US</td>
      <td>3481.055344</td>
      <td>Mexican</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>796</td>
    </tr>
    <tr>
      <th>11</th>
      <td>US</td>
      <td>3072.465704</td>
      <td>Diners</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>3.5</td>
      <td>1150</td>
    </tr>
    <tr>
      <th>12</th>
      <td>US</td>
      <td>3533.571550</td>
      <td>Latin American</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>660</td>
    </tr>
    <tr>
      <th>13</th>
      <td>US</td>
      <td>2860.960719</td>
      <td>Wine Bars</td>
      <td>Statue of Liberty</td>
      <td>3</td>
      <td>4.0</td>
      <td>546</td>
    </tr>
    <tr>
      <th>14</th>
      <td>US</td>
      <td>3660.664814</td>
      <td>Seafood</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.5</td>
      <td>499</td>
    </tr>
    <tr>
      <th>15</th>
      <td>US</td>
      <td>3387.361990</td>
      <td>Thai</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.5</td>
      <td>253</td>
    </tr>
    <tr>
      <th>16</th>
      <td>US</td>
      <td>2251.398278</td>
      <td>American (New)</td>
      <td>Statue of Liberty</td>
      <td>3</td>
      <td>3.5</td>
      <td>366</td>
    </tr>
    <tr>
      <th>17</th>
      <td>US</td>
      <td>3478.706132</td>
      <td>American (New)</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>839</td>
    </tr>
    <tr>
      <th>18</th>
      <td>US</td>
      <td>3409.467956</td>
      <td>Seafood</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.5</td>
      <td>246</td>
    </tr>
    <tr>
      <th>19</th>
      <td>US</td>
      <td>3002.990723</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>1</td>
      <td>4.5</td>
      <td>159</td>
    </tr>
    <tr>
      <th>20</th>
      <td>FR</td>
      <td>0.245032</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Eiffel Tower</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>1641</td>
    </tr>
    <tr>
      <th>21</th>
      <td>FR</td>
      <td>539.225701</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>3</td>
      <td>4.0</td>
      <td>432</td>
    </tr>
    <tr>
      <th>22</th>
      <td>FR</td>
      <td>28.485776</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>4</td>
      <td>4.0</td>
      <td>353</td>
    </tr>
    <tr>
      <th>23</th>
      <td>FR</td>
      <td>968.852288</td>
      <td>Basque</td>
      <td>Eiffel Tower</td>
      <td>2</td>
      <td>4.5</td>
      <td>169</td>
    </tr>
    <tr>
      <th>24</th>
      <td>FR</td>
      <td>1719.026836</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Eiffel Tower</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>568</td>
    </tr>
    <tr>
      <th>25</th>
      <td>FR</td>
      <td>510.634214</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>3</td>
      <td>4.0</td>
      <td>288</td>
    </tr>
    <tr>
      <th>26</th>
      <td>FR</td>
      <td>1308.196242</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>2</td>
      <td>4.5</td>
      <td>304</td>
    </tr>
    <tr>
      <th>27</th>
      <td>FR</td>
      <td>599.476459</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>3</td>
      <td>4.5</td>
      <td>191</td>
    </tr>
    <tr>
      <th>28</th>
      <td>FR</td>
      <td>872.740810</td>
      <td>Coffee &amp; Tea</td>
      <td>Eiffel Tower</td>
      <td>1</td>
      <td>4.5</td>
      <td>207</td>
    </tr>
    <tr>
      <th>29</th>
      <td>FR</td>
      <td>852.051111</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>2</td>
      <td>4.0</td>
      <td>248</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>GB</td>
      <td>402.476196</td>
      <td>Seafood</td>
      <td>Piccadilly Circus</td>
      <td>2</td>
      <td>4.0</td>
      <td>252</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>GB</td>
      <td>838.766301</td>
      <td>Parks</td>
      <td>Piccadilly Circus</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>166</td>
    </tr>
    <tr>
      <th>2000</th>
      <td>GB</td>
      <td>807.315996</td>
      <td>Indian</td>
      <td>Piccadilly Circus</td>
      <td>2</td>
      <td>4.0</td>
      <td>411</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>GB</td>
      <td>185.009563</td>
      <td>Japanese</td>
      <td>Piccadilly Circus</td>
      <td>2</td>
      <td>4.0</td>
      <td>297</td>
    </tr>
    <tr>
      <th>2002</th>
      <td>GB</td>
      <td>517.739153</td>
      <td>Coffee &amp; Tea</td>
      <td>Piccadilly Circus</td>
      <td>2</td>
      <td>4.0</td>
      <td>420</td>
    </tr>
    <tr>
      <th>2003</th>
      <td>GB</td>
      <td>1429.218319</td>
      <td>Parks</td>
      <td>Piccadilly Circus</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>343</td>
    </tr>
    <tr>
      <th>2004</th>
      <td>GB</td>
      <td>496.144573</td>
      <td>Indian</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>1150</td>
    </tr>
    <tr>
      <th>2005</th>
      <td>GB</td>
      <td>842.060736</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>403</td>
    </tr>
    <tr>
      <th>2006</th>
      <td>GB</td>
      <td>785.012293</td>
      <td>Local Flavor</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.0</td>
      <td>876</td>
    </tr>
    <tr>
      <th>2007</th>
      <td>GB</td>
      <td>324.588203</td>
      <td>Wine Bars</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>371</td>
    </tr>
    <tr>
      <th>2008</th>
      <td>GB</td>
      <td>977.221358</td>
      <td>Indian</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>287</td>
    </tr>
    <tr>
      <th>2009</th>
      <td>GB</td>
      <td>629.437268</td>
      <td>British</td>
      <td>Trafalgar Square</td>
      <td>3</td>
      <td>4.5</td>
      <td>286</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>GB</td>
      <td>970.661308</td>
      <td>Churches</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>296</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>GB</td>
      <td>1073.089143</td>
      <td>French</td>
      <td>Trafalgar Square</td>
      <td>3</td>
      <td>4.0</td>
      <td>530</td>
    </tr>
    <tr>
      <th>2012</th>
      <td>GB</td>
      <td>709.530088</td>
      <td>Coffee &amp; Tea</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>272</td>
    </tr>
    <tr>
      <th>2013</th>
      <td>GB</td>
      <td>797.496882</td>
      <td>Dim Sum</td>
      <td>Trafalgar Square</td>
      <td>3</td>
      <td>4.0</td>
      <td>441</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>GB</td>
      <td>774.335541</td>
      <td>Parks</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>166</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>GB</td>
      <td>896.993856</td>
      <td>Cafes</td>
      <td>Trafalgar Square</td>
      <td>3</td>
      <td>4.0</td>
      <td>428</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>GB</td>
      <td>683.766278</td>
      <td>Burgers</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>238</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>GB</td>
      <td>786.792622</td>
      <td>Indian</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.0</td>
      <td>411</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>GB</td>
      <td>648.707726</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>166</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>GB</td>
      <td>846.856197</td>
      <td>Steakhouses</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>288</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>GB</td>
      <td>666.256934</td>
      <td>Seafood</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.0</td>
      <td>252</td>
    </tr>
    <tr>
      <th>2021</th>
      <td>GB</td>
      <td>3.292181</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.0</td>
      <td>190</td>
    </tr>
    <tr>
      <th>2022</th>
      <td>GB</td>
      <td>825.628896</td>
      <td>Mediterranean</td>
      <td>Trafalgar Square</td>
      <td>3</td>
      <td>4.5</td>
      <td>232</td>
    </tr>
    <tr>
      <th>2023</th>
      <td>GB</td>
      <td>951.950317</td>
      <td>British</td>
      <td>Trafalgar Square</td>
      <td>1</td>
      <td>4.0</td>
      <td>327</td>
    </tr>
    <tr>
      <th>2024</th>
      <td>FR</td>
      <td>1159.465269</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Millau Bridge, France</td>
      <td>n/a</td>
      <td>4.0</td>
      <td>31</td>
    </tr>
    <tr>
      <th>2025</th>
      <td>FR</td>
      <td>461.684361</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Millau Bridge, France</td>
      <td>n/a</td>
      <td>5.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2026</th>
      <td>IE</td>
      <td>7.113237</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Newgrange</td>
      <td>n/a</td>
      <td>5.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2027</th>
      <td>IE</td>
      <td>679.645933</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Newgrange</td>
      <td>n/a</td>
      <td>4.0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
<p>2028 rows × 7 columns</p>
</div>




```python
f.to_csv('betterdata.csv')
```


```python
#Averages grouped by price
f.groupby('Price').mean()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Distance</th>
      <th>Rating</th>
      <th>Review Count</th>
    </tr>
    <tr>
      <th>Price</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>1306.166725</td>
      <td>4.358333</td>
      <td>543.750000</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1116.034476</td>
      <td>4.182857</td>
      <td>659.915429</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1018.255031</td>
      <td>4.192661</td>
      <td>514.519878</td>
    </tr>
    <tr>
      <th>4</th>
      <td>953.133474</td>
      <td>4.351648</td>
      <td>404.769231</td>
    </tr>
    <tr>
      <th>n/a</th>
      <td>1095.372611</td>
      <td>4.385057</td>
      <td>236.825287</td>
    </tr>
  </tbody>
</table>
</div>




```python
f.groupby('Country').mean().head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Distance</th>
      <th>Rating</th>
      <th>Review Count</th>
    </tr>
    <tr>
      <th>Country</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>AR</th>
      <td>522.647220</td>
      <td>4.666667</td>
      <td>17.666667</td>
    </tr>
    <tr>
      <th>AT</th>
      <td>628.371613</td>
      <td>4.300000</td>
      <td>54.650000</td>
    </tr>
    <tr>
      <th>AU</th>
      <td>1121.973694</td>
      <td>4.166667</td>
      <td>109.285714</td>
    </tr>
    <tr>
      <th>BE</th>
      <td>2078.000116</td>
      <td>4.125000</td>
      <td>148.350000</td>
    </tr>
    <tr>
      <th>BR</th>
      <td>2006.432972</td>
      <td>4.428571</td>
      <td>43.142857</td>
    </tr>
  </tbody>
</table>
</div>




```python
g=f[f['Price']!='n/a']
```


```python
g['Price']=g['Price'].astype(float)
```

    C:\Users\J\Anaconda3\lib\site-packages\ipykernel_launcher.py:1: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      """Entry point for launching an IPython kernel.
    


```python
labels=["<100","100-500","500-1000","1000+"]
g['Distance']=pd.cut(g['Distance'],[0,100,500,1000,6000],labels=labels)
```

    C:\Users\J\Anaconda3\lib\site-packages\ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    


```python
g
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Distance</th>
      <th>Food Type</th>
      <th>Monument</th>
      <th>Price</th>
      <th>Rating</th>
      <th>Review Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>US</td>
      <td>1000+</td>
      <td>Barbeque</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>984</td>
    </tr>
    <tr>
      <th>5</th>
      <td>US</td>
      <td>1000+</td>
      <td>Mexican</td>
      <td>Statue of Liberty</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>1441</td>
    </tr>
    <tr>
      <th>6</th>
      <td>US</td>
      <td>1000+</td>
      <td>Italian</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>595</td>
    </tr>
    <tr>
      <th>7</th>
      <td>US</td>
      <td>1000+</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>346</td>
    </tr>
    <tr>
      <th>8</th>
      <td>US</td>
      <td>1000+</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>522</td>
    </tr>
    <tr>
      <th>9</th>
      <td>US</td>
      <td>1000+</td>
      <td>Korean</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>653</td>
    </tr>
    <tr>
      <th>10</th>
      <td>US</td>
      <td>1000+</td>
      <td>Mexican</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>796</td>
    </tr>
    <tr>
      <th>11</th>
      <td>US</td>
      <td>1000+</td>
      <td>Diners</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>3.5</td>
      <td>1150</td>
    </tr>
    <tr>
      <th>12</th>
      <td>US</td>
      <td>1000+</td>
      <td>Latin American</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>660</td>
    </tr>
    <tr>
      <th>13</th>
      <td>US</td>
      <td>1000+</td>
      <td>Wine Bars</td>
      <td>Statue of Liberty</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>546</td>
    </tr>
    <tr>
      <th>14</th>
      <td>US</td>
      <td>1000+</td>
      <td>Seafood</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>499</td>
    </tr>
    <tr>
      <th>15</th>
      <td>US</td>
      <td>1000+</td>
      <td>Thai</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>253</td>
    </tr>
    <tr>
      <th>16</th>
      <td>US</td>
      <td>1000+</td>
      <td>American (New)</td>
      <td>Statue of Liberty</td>
      <td>3.0</td>
      <td>3.5</td>
      <td>366</td>
    </tr>
    <tr>
      <th>17</th>
      <td>US</td>
      <td>1000+</td>
      <td>American (New)</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>839</td>
    </tr>
    <tr>
      <th>18</th>
      <td>US</td>
      <td>1000+</td>
      <td>Seafood</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>246</td>
    </tr>
    <tr>
      <th>19</th>
      <td>US</td>
      <td>1000+</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>159</td>
    </tr>
    <tr>
      <th>21</th>
      <td>FR</td>
      <td>500-1000</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>432</td>
    </tr>
    <tr>
      <th>22</th>
      <td>FR</td>
      <td>&lt;100</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>353</td>
    </tr>
    <tr>
      <th>23</th>
      <td>FR</td>
      <td>500-1000</td>
      <td>Basque</td>
      <td>Eiffel Tower</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>169</td>
    </tr>
    <tr>
      <th>25</th>
      <td>FR</td>
      <td>500-1000</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>288</td>
    </tr>
    <tr>
      <th>26</th>
      <td>FR</td>
      <td>1000+</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>304</td>
    </tr>
    <tr>
      <th>27</th>
      <td>FR</td>
      <td>500-1000</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>3.0</td>
      <td>4.5</td>
      <td>191</td>
    </tr>
    <tr>
      <th>28</th>
      <td>FR</td>
      <td>500-1000</td>
      <td>Coffee &amp; Tea</td>
      <td>Eiffel Tower</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>207</td>
    </tr>
    <tr>
      <th>29</th>
      <td>FR</td>
      <td>500-1000</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>248</td>
    </tr>
    <tr>
      <th>30</th>
      <td>FR</td>
      <td>500-1000</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>4.0</td>
      <td>4.0</td>
      <td>201</td>
    </tr>
    <tr>
      <th>31</th>
      <td>FR</td>
      <td>1000+</td>
      <td>Steakhouses</td>
      <td>Eiffel Tower</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>385</td>
    </tr>
    <tr>
      <th>32</th>
      <td>FR</td>
      <td>&lt;100</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>4.0</td>
      <td>3.5</td>
      <td>222</td>
    </tr>
    <tr>
      <th>33</th>
      <td>FR</td>
      <td>100-500</td>
      <td>Italian</td>
      <td>Eiffel Tower</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>176</td>
    </tr>
    <tr>
      <th>34</th>
      <td>FR</td>
      <td>500-1000</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>122</td>
    </tr>
    <tr>
      <th>35</th>
      <td>FR</td>
      <td>500-1000</td>
      <td>Bars</td>
      <td>Eiffel Tower</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>118</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1984</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Indian</td>
      <td>Piccadilly Circus</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>1150</td>
    </tr>
    <tr>
      <th>1985</th>
      <td>GB</td>
      <td>100-500</td>
      <td>Indian</td>
      <td>Piccadilly Circus</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>287</td>
    </tr>
    <tr>
      <th>1986</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>French</td>
      <td>Piccadilly Circus</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>530</td>
    </tr>
    <tr>
      <th>1987</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Wine Bars</td>
      <td>Piccadilly Circus</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>371</td>
    </tr>
    <tr>
      <th>1988</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>British</td>
      <td>Piccadilly Circus</td>
      <td>3.0</td>
      <td>4.5</td>
      <td>286</td>
    </tr>
    <tr>
      <th>1989</th>
      <td>GB</td>
      <td>100-500</td>
      <td>Dim Sum</td>
      <td>Piccadilly Circus</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>441</td>
    </tr>
    <tr>
      <th>1990</th>
      <td>GB</td>
      <td>100-500</td>
      <td>Steakhouses</td>
      <td>Piccadilly Circus</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>288</td>
    </tr>
    <tr>
      <th>1991</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Cafes</td>
      <td>Piccadilly Circus</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>428</td>
    </tr>
    <tr>
      <th>1992</th>
      <td>GB</td>
      <td>100-500</td>
      <td>Burgers</td>
      <td>Piccadilly Circus</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>238</td>
    </tr>
    <tr>
      <th>1993</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Coffee &amp; Tea</td>
      <td>Piccadilly Circus</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>272</td>
    </tr>
    <tr>
      <th>1995</th>
      <td>GB</td>
      <td>100-500</td>
      <td>Mediterranean</td>
      <td>Piccadilly Circus</td>
      <td>3.0</td>
      <td>4.5</td>
      <td>232</td>
    </tr>
    <tr>
      <th>1996</th>
      <td>GB</td>
      <td>100-500</td>
      <td>British</td>
      <td>Piccadilly Circus</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>327</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>GB</td>
      <td>100-500</td>
      <td>Seafood</td>
      <td>Piccadilly Circus</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>252</td>
    </tr>
    <tr>
      <th>2000</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Indian</td>
      <td>Piccadilly Circus</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>411</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>GB</td>
      <td>100-500</td>
      <td>Japanese</td>
      <td>Piccadilly Circus</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>297</td>
    </tr>
    <tr>
      <th>2002</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Coffee &amp; Tea</td>
      <td>Piccadilly Circus</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>420</td>
    </tr>
    <tr>
      <th>2004</th>
      <td>GB</td>
      <td>100-500</td>
      <td>Indian</td>
      <td>Trafalgar Square</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>1150</td>
    </tr>
    <tr>
      <th>2007</th>
      <td>GB</td>
      <td>100-500</td>
      <td>Wine Bars</td>
      <td>Trafalgar Square</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>371</td>
    </tr>
    <tr>
      <th>2008</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Indian</td>
      <td>Trafalgar Square</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>287</td>
    </tr>
    <tr>
      <th>2009</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>British</td>
      <td>Trafalgar Square</td>
      <td>3.0</td>
      <td>4.5</td>
      <td>286</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>GB</td>
      <td>1000+</td>
      <td>French</td>
      <td>Trafalgar Square</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>530</td>
    </tr>
    <tr>
      <th>2012</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Coffee &amp; Tea</td>
      <td>Trafalgar Square</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>272</td>
    </tr>
    <tr>
      <th>2013</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Dim Sum</td>
      <td>Trafalgar Square</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>441</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Cafes</td>
      <td>Trafalgar Square</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>428</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Burgers</td>
      <td>Trafalgar Square</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>238</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Indian</td>
      <td>Trafalgar Square</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>411</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Steakhouses</td>
      <td>Trafalgar Square</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>288</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Seafood</td>
      <td>Trafalgar Square</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>252</td>
    </tr>
    <tr>
      <th>2022</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>Mediterranean</td>
      <td>Trafalgar Square</td>
      <td>3.0</td>
      <td>4.5</td>
      <td>232</td>
    </tr>
    <tr>
      <th>2023</th>
      <td>GB</td>
      <td>500-1000</td>
      <td>British</td>
      <td>Trafalgar Square</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>327</td>
    </tr>
  </tbody>
</table>
<p>1592 rows × 7 columns</p>
</div>




```python
sns.boxplot(x=g.Price,y=f.Distance)
plt.show()
```


![png](output_12_0.png)



```python
dir(sns)
```




    ['FacetGrid',
     'JointGrid',
     'PairGrid',
     '__builtins__',
     '__cached__',
     '__doc__',
     '__file__',
     '__loader__',
     '__name__',
     '__package__',
     '__path__',
     '__spec__',
     '__version__',
     '_orig_rc_params',
     'algorithms',
     'axes_style',
     'axisgrid',
     'barplot',
     'blend_palette',
     'boxplot',
     'categorical',
     'choose_colorbrewer_palette',
     'choose_cubehelix_palette',
     'choose_dark_palette',
     'choose_diverging_palette',
     'choose_light_palette',
     'clustermap',
     'cm',
     'color_palette',
     'countplot',
     'crayon_palette',
     'crayons',
     'cubehelix_palette',
     'dark_palette',
     'desaturate',
     'despine',
     'distplot',
     'distributions',
     'diverging_palette',
     'external',
     'factorplot',
     'get_dataset_names',
     'heatmap',
     'hls_palette',
     'husl_palette',
     'jointplot',
     'kdeplot',
     'light_palette',
     'lmplot',
     'load_dataset',
     'lvplot',
     'matrix',
     'miscplot',
     'mpl',
     'mpl_palette',
     'pairplot',
     'palettes',
     'palplot',
     'plotting_context',
     'pointplot',
     'rcmod',
     'regplot',
     'regression',
     'reset_defaults',
     'reset_orig',
     'residplot',
     'rugplot',
     'saturate',
     'set',
     'set_color_codes',
     'set_context',
     'set_hls_values',
     'set_palette',
     'set_style',
     'stripplot',
     'swarmplot',
     'timeseries',
     'tsplot',
     'utils',
     'violinplot',
     'widgets',
     'xkcd_palette',
     'xkcd_rgb']




```python
o=f.groupby('Rating')
for i in f.Rating.unique():
    h=o.get_group(i)
    h.boxplot('Distance', ax=)
    plt.title(i)
plt.show()
```


![png](output_14_0.png)



```python
h=g.groupby('Distance')
for i in labels:
    j=h.get_group(i)
    j.boxplot('Rating')
    plt.title(i)
    plt.show()
```


![png](output_15_0.png)



![png](output_15_1.png)



![png](output_15_2.png)



```python
h.get_group('1600+')
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Distance</th>
      <th>Food Type</th>
      <th>Monument</th>
      <th>Price</th>
      <th>Rating</th>
      <th>Review Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>4</th>
      <td>US</td>
      <td>1600+</td>
      <td>Barbeque</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>982</td>
    </tr>
    <tr>
      <th>5</th>
      <td>US</td>
      <td>1600+</td>
      <td>Mexican</td>
      <td>Statue of Liberty</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>1440</td>
    </tr>
    <tr>
      <th>6</th>
      <td>US</td>
      <td>1600+</td>
      <td>Italian</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>595</td>
    </tr>
    <tr>
      <th>7</th>
      <td>US</td>
      <td>1600+</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>346</td>
    </tr>
    <tr>
      <th>8</th>
      <td>US</td>
      <td>1600+</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>522</td>
    </tr>
    <tr>
      <th>9</th>
      <td>US</td>
      <td>1600+</td>
      <td>Korean</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>652</td>
    </tr>
    <tr>
      <th>10</th>
      <td>US</td>
      <td>1600+</td>
      <td>Mexican</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>793</td>
    </tr>
    <tr>
      <th>11</th>
      <td>US</td>
      <td>1600+</td>
      <td>Diners</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>3.5</td>
      <td>1149</td>
    </tr>
    <tr>
      <th>12</th>
      <td>US</td>
      <td>1600+</td>
      <td>Latin American</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>660</td>
    </tr>
    <tr>
      <th>13</th>
      <td>US</td>
      <td>1600+</td>
      <td>Wine Bars</td>
      <td>Statue of Liberty</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>546</td>
    </tr>
    <tr>
      <th>14</th>
      <td>US</td>
      <td>1600+</td>
      <td>Seafood</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>499</td>
    </tr>
    <tr>
      <th>15</th>
      <td>US</td>
      <td>1600+</td>
      <td>Thai</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>252</td>
    </tr>
    <tr>
      <th>16</th>
      <td>US</td>
      <td>1600+</td>
      <td>American (New)</td>
      <td>Statue of Liberty</td>
      <td>3.0</td>
      <td>3.5</td>
      <td>366</td>
    </tr>
    <tr>
      <th>17</th>
      <td>US</td>
      <td>1600+</td>
      <td>American (New)</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>837</td>
    </tr>
    <tr>
      <th>18</th>
      <td>US</td>
      <td>1600+</td>
      <td>Seafood</td>
      <td>Statue of Liberty</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>245</td>
    </tr>
    <tr>
      <th>19</th>
      <td>US</td>
      <td>1600+</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>159</td>
    </tr>
    <tr>
      <th>52</th>
      <td>GB</td>
      <td>1600+</td>
      <td>Dim Sum</td>
      <td>Big Ben</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>441</td>
    </tr>
    <tr>
      <th>57</th>
      <td>GB</td>
      <td>1600+</td>
      <td>Indian</td>
      <td>Big Ben</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>287</td>
    </tr>
    <tr>
      <th>89</th>
      <td>IT</td>
      <td>1600+</td>
      <td>Ice Cream &amp; Frozen Yogurt</td>
      <td>Colosseum</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>387</td>
    </tr>
    <tr>
      <th>92</th>
      <td>IT</td>
      <td>1600+</td>
      <td>Gluten-Free</td>
      <td>Colosseum</td>
      <td>3.0</td>
      <td>4.5</td>
      <td>178</td>
    </tr>
    <tr>
      <th>94</th>
      <td>IT</td>
      <td>1600+</td>
      <td>Italian</td>
      <td>Colosseum</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>440</td>
    </tr>
    <tr>
      <th>99</th>
      <td>IT</td>
      <td>1600+</td>
      <td>Pizza</td>
      <td>Colosseum</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>160</td>
    </tr>
    <tr>
      <th>115</th>
      <td>US</td>
      <td>1600+</td>
      <td>Ramen</td>
      <td>Empire State Building</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>9234</td>
    </tr>
    <tr>
      <th>121</th>
      <td>US</td>
      <td>1600+</td>
      <td>Bakeries</td>
      <td>Hollywood Sign</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>7204</td>
    </tr>
    <tr>
      <th>122</th>
      <td>US</td>
      <td>1600+</td>
      <td>Breakfast &amp; Brunch</td>
      <td>Hollywood Sign</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>2815</td>
    </tr>
    <tr>
      <th>123</th>
      <td>US</td>
      <td>1600+</td>
      <td>Mediterranean</td>
      <td>Hollywood Sign</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>1663</td>
    </tr>
    <tr>
      <th>125</th>
      <td>US</td>
      <td>1600+</td>
      <td>Hot Dogs</td>
      <td>Hollywood Sign</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>663</td>
    </tr>
    <tr>
      <th>126</th>
      <td>US</td>
      <td>1600+</td>
      <td>Cafes</td>
      <td>Hollywood Sign</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>532</td>
    </tr>
    <tr>
      <th>129</th>
      <td>US</td>
      <td>1600+</td>
      <td>Performing Arts</td>
      <td>Hollywood Sign</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>2065</td>
    </tr>
    <tr>
      <th>130</th>
      <td>US</td>
      <td>1600+</td>
      <td>Sandwiches</td>
      <td>Hollywood Sign</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>496</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1835</th>
      <td>BE</td>
      <td>1600+</td>
      <td>Cafes</td>
      <td>Atomium, Brussels</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>65</td>
    </tr>
    <tr>
      <th>1836</th>
      <td>BE</td>
      <td>1600+</td>
      <td>Japanese</td>
      <td>Atomium, Brussels</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>71</td>
    </tr>
    <tr>
      <th>1837</th>
      <td>BE</td>
      <td>1600+</td>
      <td>Seafood</td>
      <td>Atomium, Brussels</td>
      <td>3.0</td>
      <td>4.5</td>
      <td>32</td>
    </tr>
    <tr>
      <th>1838</th>
      <td>BE</td>
      <td>1600+</td>
      <td>Burgers</td>
      <td>Atomium, Brussels</td>
      <td>2.0</td>
      <td>3.5</td>
      <td>172</td>
    </tr>
    <tr>
      <th>1839</th>
      <td>BE</td>
      <td>1600+</td>
      <td>Greek</td>
      <td>Atomium, Brussels</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>59</td>
    </tr>
    <tr>
      <th>1840</th>
      <td>BE</td>
      <td>1600+</td>
      <td>Belgian</td>
      <td>Atomium, Brussels</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>44</td>
    </tr>
    <tr>
      <th>1841</th>
      <td>BE</td>
      <td>1600+</td>
      <td>Brasseries</td>
      <td>Atomium, Brussels</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>24</td>
    </tr>
    <tr>
      <th>1843</th>
      <td>BE</td>
      <td>1600+</td>
      <td>Belgian</td>
      <td>Atomium, Brussels</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>57</td>
    </tr>
    <tr>
      <th>1865</th>
      <td>US</td>
      <td>1600+</td>
      <td>Seafood</td>
      <td>Hollywood Walk of Fame</td>
      <td>4.0</td>
      <td>4.5</td>
      <td>2341</td>
    </tr>
    <tr>
      <th>1868</th>
      <td>US</td>
      <td>1600+</td>
      <td>Breakfast &amp; Brunch</td>
      <td>Hollywood Walk of Fame</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>5045</td>
    </tr>
    <tr>
      <th>1870</th>
      <td>US</td>
      <td>1600+</td>
      <td>American (New)</td>
      <td>Hollywood Walk of Fame</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>4169</td>
    </tr>
    <tr>
      <th>1873</th>
      <td>US</td>
      <td>1600+</td>
      <td>Pizza</td>
      <td>Hollywood Walk of Fame</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>3782</td>
    </tr>
    <tr>
      <th>1886</th>
      <td>DK</td>
      <td>1600+</td>
      <td>Cafes</td>
      <td>Amalienborg Palace</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>165</td>
    </tr>
    <tr>
      <th>1890</th>
      <td>DK</td>
      <td>1600+</td>
      <td>Danish</td>
      <td>Amalienborg Palace</td>
      <td>3.0</td>
      <td>4.5</td>
      <td>166</td>
    </tr>
    <tr>
      <th>1906</th>
      <td>US</td>
      <td>1600+</td>
      <td>Cocktail Bars</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>1215</td>
    </tr>
    <tr>
      <th>1907</th>
      <td>US</td>
      <td>1600+</td>
      <td>Mexican</td>
      <td>Ellis Island Immigration Museum</td>
      <td>1.0</td>
      <td>4.0</td>
      <td>1440</td>
    </tr>
    <tr>
      <th>1908</th>
      <td>US</td>
      <td>1600+</td>
      <td>Seafood</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>915</td>
    </tr>
    <tr>
      <th>1910</th>
      <td>US</td>
      <td>1600+</td>
      <td>Cafes</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>522</td>
    </tr>
    <tr>
      <th>1911</th>
      <td>US</td>
      <td>1600+</td>
      <td>Cafes</td>
      <td>Ellis Island Immigration Museum</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>346</td>
    </tr>
    <tr>
      <th>1912</th>
      <td>US</td>
      <td>1600+</td>
      <td>Italian</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>595</td>
    </tr>
    <tr>
      <th>1913</th>
      <td>US</td>
      <td>1600+</td>
      <td>Korean</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>652</td>
    </tr>
    <tr>
      <th>1914</th>
      <td>US</td>
      <td>1600+</td>
      <td>Wine Bars</td>
      <td>Ellis Island Immigration Museum</td>
      <td>3.0</td>
      <td>4.0</td>
      <td>546</td>
    </tr>
    <tr>
      <th>1915</th>
      <td>US</td>
      <td>1600+</td>
      <td>Mexican</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>793</td>
    </tr>
    <tr>
      <th>1917</th>
      <td>US</td>
      <td>1600+</td>
      <td>Barbeque</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>982</td>
    </tr>
    <tr>
      <th>1918</th>
      <td>US</td>
      <td>1600+</td>
      <td>Diners</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>3.5</td>
      <td>1149</td>
    </tr>
    <tr>
      <th>1920</th>
      <td>US</td>
      <td>1600+</td>
      <td>Latin American</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>4.0</td>
      <td>660</td>
    </tr>
    <tr>
      <th>1922</th>
      <td>US</td>
      <td>1600+</td>
      <td>Seafood</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>499</td>
    </tr>
    <tr>
      <th>1923</th>
      <td>US</td>
      <td>1600+</td>
      <td>Thai</td>
      <td>Ellis Island Immigration Museum</td>
      <td>2.0</td>
      <td>4.5</td>
      <td>252</td>
    </tr>
    <tr>
      <th>1977</th>
      <td>TR</td>
      <td>1600+</td>
      <td>Turkish</td>
      <td>Blue Mosque, Istanbul</td>
      <td>3.0</td>
      <td>4.5</td>
      <td>50</td>
    </tr>
    <tr>
      <th>1980</th>
      <td>TR</td>
      <td>1600+</td>
      <td>Coffee &amp; Tea</td>
      <td>Blue Mosque, Istanbul</td>
      <td>1.0</td>
      <td>4.5</td>
      <td>40</td>
    </tr>
  </tbody>
</table>
<p>337 rows × 7 columns</p>
</div>




```python
f
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Country</th>
      <th>Distance</th>
      <th>Food Type</th>
      <th>Monument</th>
      <th>Price</th>
      <th>Rating</th>
      <th>Review Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>US</td>
      <td>75.949926</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Statue of Liberty</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>821</td>
    </tr>
    <tr>
      <th>1</th>
      <td>US</td>
      <td>1131.717451</td>
      <td>Museums</td>
      <td>Statue of Liberty</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>291</td>
    </tr>
    <tr>
      <th>2</th>
      <td>US</td>
      <td>2377.859054</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Statue of Liberty</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>271</td>
    </tr>
    <tr>
      <th>3</th>
      <td>US</td>
      <td>1798.598168</td>
      <td>Parks</td>
      <td>Statue of Liberty</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>178</td>
    </tr>
    <tr>
      <th>4</th>
      <td>US</td>
      <td>2875.471385</td>
      <td>Barbeque</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>982</td>
    </tr>
    <tr>
      <th>5</th>
      <td>US</td>
      <td>2997.020520</td>
      <td>Mexican</td>
      <td>Statue of Liberty</td>
      <td>1</td>
      <td>4.0</td>
      <td>1440</td>
    </tr>
    <tr>
      <th>6</th>
      <td>US</td>
      <td>3169.119140</td>
      <td>Italian</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>595</td>
    </tr>
    <tr>
      <th>7</th>
      <td>US</td>
      <td>3529.086258</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>1</td>
      <td>4.5</td>
      <td>346</td>
    </tr>
    <tr>
      <th>8</th>
      <td>US</td>
      <td>2798.573094</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>522</td>
    </tr>
    <tr>
      <th>9</th>
      <td>US</td>
      <td>3211.041203</td>
      <td>Korean</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>652</td>
    </tr>
    <tr>
      <th>10</th>
      <td>US</td>
      <td>3481.055344</td>
      <td>Mexican</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>793</td>
    </tr>
    <tr>
      <th>11</th>
      <td>US</td>
      <td>3072.465704</td>
      <td>Diners</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>3.5</td>
      <td>1149</td>
    </tr>
    <tr>
      <th>12</th>
      <td>US</td>
      <td>3533.571550</td>
      <td>Latin American</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>660</td>
    </tr>
    <tr>
      <th>13</th>
      <td>US</td>
      <td>2860.960719</td>
      <td>Wine Bars</td>
      <td>Statue of Liberty</td>
      <td>3</td>
      <td>4.0</td>
      <td>546</td>
    </tr>
    <tr>
      <th>14</th>
      <td>US</td>
      <td>3660.664814</td>
      <td>Seafood</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.5</td>
      <td>499</td>
    </tr>
    <tr>
      <th>15</th>
      <td>US</td>
      <td>3387.361990</td>
      <td>Thai</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.5</td>
      <td>252</td>
    </tr>
    <tr>
      <th>16</th>
      <td>US</td>
      <td>2251.398278</td>
      <td>American (New)</td>
      <td>Statue of Liberty</td>
      <td>3</td>
      <td>3.5</td>
      <td>366</td>
    </tr>
    <tr>
      <th>17</th>
      <td>US</td>
      <td>3478.706132</td>
      <td>American (New)</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.0</td>
      <td>837</td>
    </tr>
    <tr>
      <th>18</th>
      <td>US</td>
      <td>3409.467956</td>
      <td>Seafood</td>
      <td>Statue of Liberty</td>
      <td>2</td>
      <td>4.5</td>
      <td>245</td>
    </tr>
    <tr>
      <th>19</th>
      <td>US</td>
      <td>3002.990723</td>
      <td>Cafes</td>
      <td>Statue of Liberty</td>
      <td>1</td>
      <td>4.5</td>
      <td>159</td>
    </tr>
    <tr>
      <th>20</th>
      <td>FR</td>
      <td>0.245032</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Eiffel Tower</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>1641</td>
    </tr>
    <tr>
      <th>21</th>
      <td>FR</td>
      <td>539.225701</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>3</td>
      <td>4.0</td>
      <td>432</td>
    </tr>
    <tr>
      <th>22</th>
      <td>FR</td>
      <td>28.485776</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>4</td>
      <td>4.0</td>
      <td>353</td>
    </tr>
    <tr>
      <th>23</th>
      <td>FR</td>
      <td>968.852288</td>
      <td>Basque</td>
      <td>Eiffel Tower</td>
      <td>2</td>
      <td>4.5</td>
      <td>169</td>
    </tr>
    <tr>
      <th>24</th>
      <td>FR</td>
      <td>1719.026836</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Eiffel Tower</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>568</td>
    </tr>
    <tr>
      <th>25</th>
      <td>FR</td>
      <td>510.634214</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>3</td>
      <td>4.0</td>
      <td>288</td>
    </tr>
    <tr>
      <th>26</th>
      <td>FR</td>
      <td>1308.196242</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>2</td>
      <td>4.5</td>
      <td>304</td>
    </tr>
    <tr>
      <th>27</th>
      <td>FR</td>
      <td>599.476459</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>3</td>
      <td>4.5</td>
      <td>191</td>
    </tr>
    <tr>
      <th>28</th>
      <td>FR</td>
      <td>872.740810</td>
      <td>Coffee &amp; Tea</td>
      <td>Eiffel Tower</td>
      <td>1</td>
      <td>4.5</td>
      <td>206</td>
    </tr>
    <tr>
      <th>29</th>
      <td>FR</td>
      <td>852.051111</td>
      <td>French</td>
      <td>Eiffel Tower</td>
      <td>2</td>
      <td>4.0</td>
      <td>247</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1998</th>
      <td>GB</td>
      <td>402.476196</td>
      <td>Seafood</td>
      <td>Piccadilly Circus</td>
      <td>2</td>
      <td>4.0</td>
      <td>252</td>
    </tr>
    <tr>
      <th>1999</th>
      <td>GB</td>
      <td>838.766301</td>
      <td>Parks</td>
      <td>Piccadilly Circus</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>166</td>
    </tr>
    <tr>
      <th>2000</th>
      <td>GB</td>
      <td>807.315996</td>
      <td>Indian</td>
      <td>Piccadilly Circus</td>
      <td>2</td>
      <td>4.0</td>
      <td>410</td>
    </tr>
    <tr>
      <th>2001</th>
      <td>GB</td>
      <td>185.009563</td>
      <td>Japanese</td>
      <td>Piccadilly Circus</td>
      <td>2</td>
      <td>4.0</td>
      <td>297</td>
    </tr>
    <tr>
      <th>2002</th>
      <td>GB</td>
      <td>1429.218319</td>
      <td>Parks</td>
      <td>Piccadilly Circus</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>343</td>
    </tr>
    <tr>
      <th>2003</th>
      <td>GB</td>
      <td>517.739153</td>
      <td>Coffee &amp; Tea</td>
      <td>Piccadilly Circus</td>
      <td>2</td>
      <td>4.0</td>
      <td>420</td>
    </tr>
    <tr>
      <th>2004</th>
      <td>GB</td>
      <td>496.144573</td>
      <td>Indian</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>1147</td>
    </tr>
    <tr>
      <th>2005</th>
      <td>GB</td>
      <td>842.060736</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>403</td>
    </tr>
    <tr>
      <th>2006</th>
      <td>GB</td>
      <td>785.012293</td>
      <td>Local Flavor</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.0</td>
      <td>876</td>
    </tr>
    <tr>
      <th>2007</th>
      <td>GB</td>
      <td>324.588203</td>
      <td>Wine Bars</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>371</td>
    </tr>
    <tr>
      <th>2008</th>
      <td>GB</td>
      <td>977.221358</td>
      <td>Indian</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>287</td>
    </tr>
    <tr>
      <th>2009</th>
      <td>GB</td>
      <td>629.437268</td>
      <td>British</td>
      <td>Trafalgar Square</td>
      <td>3</td>
      <td>4.5</td>
      <td>286</td>
    </tr>
    <tr>
      <th>2010</th>
      <td>GB</td>
      <td>970.661308</td>
      <td>Churches</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>296</td>
    </tr>
    <tr>
      <th>2011</th>
      <td>GB</td>
      <td>1073.089143</td>
      <td>French</td>
      <td>Trafalgar Square</td>
      <td>3</td>
      <td>4.0</td>
      <td>529</td>
    </tr>
    <tr>
      <th>2012</th>
      <td>GB</td>
      <td>709.530088</td>
      <td>Coffee &amp; Tea</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>272</td>
    </tr>
    <tr>
      <th>2013</th>
      <td>GB</td>
      <td>797.496882</td>
      <td>Dim Sum</td>
      <td>Trafalgar Square</td>
      <td>3</td>
      <td>4.0</td>
      <td>441</td>
    </tr>
    <tr>
      <th>2014</th>
      <td>GB</td>
      <td>774.335541</td>
      <td>Parks</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>166</td>
    </tr>
    <tr>
      <th>2015</th>
      <td>GB</td>
      <td>896.993856</td>
      <td>Cafes</td>
      <td>Trafalgar Square</td>
      <td>3</td>
      <td>4.0</td>
      <td>427</td>
    </tr>
    <tr>
      <th>2016</th>
      <td>GB</td>
      <td>683.766278</td>
      <td>Burgers</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>238</td>
    </tr>
    <tr>
      <th>2017</th>
      <td>GB</td>
      <td>648.707726</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.5</td>
      <td>167</td>
    </tr>
    <tr>
      <th>2018</th>
      <td>GB</td>
      <td>786.792622</td>
      <td>Indian</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.0</td>
      <td>410</td>
    </tr>
    <tr>
      <th>2019</th>
      <td>GB</td>
      <td>846.856197</td>
      <td>Steakhouses</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.5</td>
      <td>287</td>
    </tr>
    <tr>
      <th>2020</th>
      <td>GB</td>
      <td>666.256934</td>
      <td>Seafood</td>
      <td>Trafalgar Square</td>
      <td>2</td>
      <td>4.0</td>
      <td>252</td>
    </tr>
    <tr>
      <th>2021</th>
      <td>GB</td>
      <td>3.292181</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Trafalgar Square</td>
      <td>n/a</td>
      <td>4.0</td>
      <td>190</td>
    </tr>
    <tr>
      <th>2022</th>
      <td>GB</td>
      <td>825.628896</td>
      <td>Mediterranean</td>
      <td>Trafalgar Square</td>
      <td>3</td>
      <td>4.5</td>
      <td>232</td>
    </tr>
    <tr>
      <th>2023</th>
      <td>GB</td>
      <td>951.950317</td>
      <td>British</td>
      <td>Trafalgar Square</td>
      <td>1</td>
      <td>4.0</td>
      <td>327</td>
    </tr>
    <tr>
      <th>2024</th>
      <td>FR</td>
      <td>1159.465269</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Millau Bridge, France</td>
      <td>n/a</td>
      <td>4.0</td>
      <td>31</td>
    </tr>
    <tr>
      <th>2025</th>
      <td>FR</td>
      <td>461.684361</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Millau Bridge, France</td>
      <td>n/a</td>
      <td>5.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2026</th>
      <td>IE</td>
      <td>7.113237</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Newgrange</td>
      <td>n/a</td>
      <td>5.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2027</th>
      <td>IE</td>
      <td>679.645933</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Newgrange</td>
      <td>n/a</td>
      <td>4.0</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
<p>2028 rows × 7 columns</p>
</div>




```python
url=f'https://api.yelp.com/v3/businesses/wB45aXh2cQlRWoKQzhWgeA/reviews'
c=requests.get(url,headers=headers).json()
c
```




    {'possible_languages': ['fr', 'en', 'nl', 'pt', 'de', 'tr', 'it', 'ja', 'es'],
     'reviews': [{'id': 'HyuACkSh6EhMW1pi3NVGFw',
       'rating': 5,
       'text': 'I have had the privilege of seeing the Statue of Liberty twice in my lifetime and it is absolutely breathtaking. To me, it symbolizes America and everything...',
       'time_created': '2018-02-19 23:08:48',
       'url': 'https://www.yelp.com/biz/statue-of-liberty-new-york-3?hrid=HyuACkSh6EhMW1pi3NVGFw&adjust_creative=M7mV67MJ1KtmqjorxVLbyA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=M7mV67MJ1KtmqjorxVLbyA',
       'user': {'image_url': 'https://s3-media3.fl.yelpcdn.com/photo/c6KFcSP8zXpybm21TjzUgQ/o.jpg',
        'name': 'Lance L.'}},
      {'id': 'gQsSSfTNWHnKTX2pDgVJIg',
       'rating': 4,
       'text': 'Bottom Line:  A neat experience but there are lots of lines involved to visit this 140+ year old National Monument.\n\nLiberty Enlightening the World is a...',
       'time_created': '2018-03-23 15:10:38',
       'url': 'https://www.yelp.com/biz/statue-of-liberty-new-york-3?hrid=gQsSSfTNWHnKTX2pDgVJIg&adjust_creative=M7mV67MJ1KtmqjorxVLbyA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=M7mV67MJ1KtmqjorxVLbyA',
       'user': {'image_url': 'https://s3-media3.fl.yelpcdn.com/photo/DmBWEceAO1mc6Hk7RD8KwA/o.jpg',
        'name': 'Courtney K.'}},
      {'id': 'eciy912dmgvIVyRvH6-QoA',
       'rating': 4,
       'text': "First time seeing the Statue! With the City Pass you have the choice of the cruise or the ferry to the island. Since the cruise is longer and you don't get...",
       'time_created': '2018-03-22 11:44:51',
       'url': 'https://www.yelp.com/biz/statue-of-liberty-new-york-3?hrid=eciy912dmgvIVyRvH6-QoA&adjust_creative=M7mV67MJ1KtmqjorxVLbyA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=M7mV67MJ1KtmqjorxVLbyA',
       'user': {'image_url': 'https://s3-media4.fl.yelpcdn.com/photo/EkxtCSywVHH0KNW9-NhK4Q/o.jpg',
        'name': 'Justine S.'}}],
     'total': 821}




```python
review=[]
tweets=[]
vader=[]
for i in ids:
    url=f'https://api.yelp.com/v3/businesses/{i}/reviews'
    c=requests.get(url,headers=headers).json()
    try:
        for j in c['reviews']:
            review.append(j['rating'])
            tweets.append(j['text'])
            vader.append(analyzer.polarity_scores(j["text"])["compound"])
    except:
        break
```


```python
plt.scatter(vader,review)
plt.show()
```




    [5]




```python
c['reviews'][0]
```




    {'id': 'HyuACkSh6EhMW1pi3NVGFw',
     'rating': 5,
     'text': 'I have had the privilege of seeing the Statue of Liberty twice in my lifetime and it is absolutely breathtaking. To me, it symbolizes America and everything...',
     'time_created': '2018-02-19 23:08:48',
     'url': 'https://www.yelp.com/biz/statue-of-liberty-new-york-3?hrid=HyuACkSh6EhMW1pi3NVGFw&adjust_creative=M7mV67MJ1KtmqjorxVLbyA&utm_campaign=yelp_api_v3&utm_medium=api_v3_business_reviews&utm_source=M7mV67MJ1KtmqjorxVLbyA',
     'user': {'image_url': 'https://s3-media3.fl.yelpcdn.com/photo/c6KFcSP8zXpybm21TjzUgQ/o.jpg',
      'name': 'Lance L.'}}




```python
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
analyzer = SentimentIntensityAnalyzer()
```
