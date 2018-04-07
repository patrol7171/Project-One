

```python
# Dependencies
import requests
import json
import pandas as pd
import matplotlib.pyplot as plt
import pickle
import seaborn as sns
import numpy as np
sns.set(color_codes=True)
from scipy.stats import linregress


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
distranks=[]
normalized=[]
for i in range(len(lats)):
    a=requests.get(f'https://api.yelp.com/v3/businesses/search?latitude={lats[i]}&longitude={longs[i]}&limit=50',headers=headers).json()
    tempdist=[]
    temps=[]
    for j in a['businesses']:
        tempdist.append(j['distance'])
        dist.append(j['distance'])
        country.append(j['location']['country'])
        rating.append(j['rating'])
        reviewcount.append(j['review_count'])
        monument.append(monuments[i])
        temps.append(j['distance'])
        try:
            price.append(len(j['price']))
        except:
            price.append('n/a')
        try:
            types.append(j['categories'][0]['title'])
        except:
            types.append('n/a')
    #Finding the Ranking of Restaurants
    something=np.argsort(tempdist)
    for k in range(len(something)):
        placeholder=np.where(something==k)
        for l in placeholder:
            for m in l:
                distranks.append(m+1)
    #Finding Normalized Distance Values (Needs Work)
    try:
        mini=min(temps)
        if mini<10:
            mini=10
        for j in temps:
            normalized.append(j/mini)
    except:
        continue

```


```python
normalized=[]
for i in range(len(lats)):
    a=requests.get(f'https://api.yelp.com/v3/businesses/search?latitude={lats[i]}&longitude={longs[i]}&limit=50',headers=headers).json()
    temps=[]
    for j in a['businesses']:  
        temps.append(j['distance'])
    try:
        mini=min(temps)
        for j in temps:
            normalized.append(j/mini)
    except:
        continue
```


```python
e={'Distance':dist,'Price':price,'Country':country,'Rating':rating,'Review Count':reviewcount,'Monument':monument,'Food Type':types, 'Rank of Distance':distranks, 'Normalized Distance':normalized}
f=pd.DataFrame(e)
f.head()
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
      <th>Normalized Distance</th>
      <th>Price</th>
      <th>Rank of Distance</th>
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
      <td>1.000000</td>
      <td>n/a</td>
      <td>1</td>
      <td>4.5</td>
      <td>821</td>
    </tr>
    <tr>
      <th>1</th>
      <td>US</td>
      <td>1131.717451</td>
      <td>Museums</td>
      <td>Statue of Liberty</td>
      <td>14.900837</td>
      <td>n/a</td>
      <td>2</td>
      <td>4.5</td>
      <td>291</td>
    </tr>
    <tr>
      <th>2</th>
      <td>US</td>
      <td>2377.859054</td>
      <td>Landmarks &amp; Historical Buildings</td>
      <td>Statue of Liberty</td>
      <td>31.308247</td>
      <td>n/a</td>
      <td>6</td>
      <td>4.5</td>
      <td>271</td>
    </tr>
    <tr>
      <th>3</th>
      <td>US</td>
      <td>1798.598168</td>
      <td>Parks</td>
      <td>Statue of Liberty</td>
      <td>23.681368</td>
      <td>n/a</td>
      <td>3</td>
      <td>4.5</td>
      <td>178</td>
    </tr>
    <tr>
      <th>4</th>
      <td>US</td>
      <td>2875.471385</td>
      <td>Barbeque</td>
      <td>Statue of Liberty</td>
      <td>37.860094</td>
      <td>2</td>
      <td>16</td>
      <td>4.0</td>
      <td>985</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Removing N/A's, Making Prices floats
g=f[f['Price']!='n/a']
g['Price']=g['Price'].astype(float)
```


```python
#Removing Review Count Less Than 100 (Not sure if necessary)
h=g[g['Review Count']>100]
```


```python
f.to_csv('evenbetterdata.csv')
```


```python
#Averages grouped by price
h=g.groupby('Rank of Distance').mean()
```

# These 3 Graphs are binning distances based on rank of restaurant (As in is the restaurant the closest, 2nd closest, 3rd closest, etc.)



```python
x=h.index
y=h['Price']
fit = np.polyfit(x,y,1)
fit_fn = np.poly1d(fit) 
plt.plot(x,y, 'yo', x, fit_fn(x), '--k')
plt.text(x=0,y=2,s='Y = -.00313X + 2.21')
plt.text(x=0,y=1.96,s='$R^{2}$ = .245, p = .000262')
plt.xlabel('Closeness of Restaurants')
plt.ylabel('Average Price')
plt.title('Price versus Distance of Restaurant (Relative)')
plt.show()
print(linregress(x,y))

```


![png](output_11_0.png)


    LinregressResult(slope=-0.0031347108256813308, intercept=2.2103897825041381, rvalue=-0.4944618199207057, pvalue=0.0002623979769123308, stderr=0.0007953595398227768)
    


```python
x=h.index
y=h['Rating']
fit = np.polyfit(x,y,1)
fit_fn = np.poly1d(fit) 
plt.plot(x,y, 'yo', x, fit_fn(x), '--k')
plt.text(x=30,y=4,s='Y = .00447X + 4.05')
plt.text(x=30,y=3.95,s='$R^{2}$ = .628, p = 7.17E-12')
plt.xlabel('Closeness of Restaurants')
plt.ylabel('Average Price')
plt.title('Rating versus Distance of Restaurant (Relative)')
plt.show()
print(linregress(x,y))
```


![png](output_12_0.png)


    LinregressResult(slope=0.0044746399020663945, intercept=4.0479877243904623, rvalue=0.79229336818706375, pvalue=7.1744065514125857e-12, stderr=0.00049737182772943082)
    


```python
x=h.Price
y=h.Rating
fit = np.polyfit(x,y,1)
fit_fn = np.poly1d(fit) 
plt.plot(x,y, 'yo', x, fit_fn(x), '--k')
plt.xlabel('Average Price of Restaurant')
plt.ylabel('Average Rating of Restaurant')
plt.text(x=1.95,y=4,s='Y = -.433X + 5.09')
plt.text(x=1.95,y=3.95,s='$R^{2}$ = .237, p = .000342')
plt.title('Price versus Rating of Restaurant (Relative)')
plt.show()
print(linregress(x,y))

```


![png](output_13_0.png)


    LinregressResult(slope=-0.43332607132890683, intercept=5.0852725883166903, rvalue=-0.48641557188077089, pvalue=0.00034156646341179723, stderr=0.11234733575531028)
    


```python
#Distributions of Prices for each Restaurant Type  (Doesn't look very presentable) Could show a couple largest ones (French, Italian, Pizza, etc.)
#h=g.groupby('Food Type')
#for i in g['Food Type'].unique():
#    j=h.get_group(i)
#    sns.distplot(j['Price'])
#    plt.title(f'Distribution of Prices of {i} Restaurants')
#    plt.show()
```


```python
#Total Distribution of Prices with N/A's included
i=f.groupby('Price').count()
plt.pie(i['Country'],labels=i.index,autopct="%1.1f%%", shadow=True, startangle=140,explode=(0,0,0,0,.1))
plt.axis('equal')
plt.title('Distribution of Restaurant Prices')
plt.show()
```


![png](output_15_0.png)



```python
#Total Distribution of Prices with N/A's excluded
i=h.groupby('Price').count()
plt.pie(i['Country'],labels=i.index,autopct="%1.1f%%", shadow=True, startangle=140,explode=(0,0,0,0))
plt.axis('equal')
plt.title("Distribution of Restaurant Prices (N/A's Removed)")
plt.show()
```


![png](output_16_0.png)



```python
#Creating Dataframe with Top 10 Restaurant Type Counts
i=f.groupby('Food Type').count()
top=pd.DataFrame(i.nlargest(10,'Country'))
j=top['Country']
j['Other']=sum(i['Country'])-sum(j)
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
      <th>Unnamed: 0</th>
      <th>Country</th>
      <th>Distance</th>
      <th>Monument</th>
      <th>Price</th>
      <th>Rank of Distance</th>
      <th>Rating</th>
      <th>Review Count</th>
    </tr>
    <tr>
      <th>Food Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Italian</th>
      <td>454</td>
      <td>454</td>
      <td>454</td>
      <td>454</td>
      <td>454</td>
      <td>454</td>
      <td>454</td>
      <td>454</td>
    </tr>
    <tr>
      <th>Landmarks &amp; Historical Buildings</th>
      <td>300</td>
      <td>300</td>
      <td>300</td>
      <td>300</td>
      <td>300</td>
      <td>300</td>
      <td>300</td>
      <td>300</td>
    </tr>
    <tr>
      <th>French</th>
      <td>245</td>
      <td>245</td>
      <td>245</td>
      <td>245</td>
      <td>245</td>
      <td>245</td>
      <td>245</td>
      <td>245</td>
    </tr>
    <tr>
      <th>Coffee &amp; Tea</th>
      <td>211</td>
      <td>211</td>
      <td>211</td>
      <td>211</td>
      <td>211</td>
      <td>211</td>
      <td>211</td>
      <td>211</td>
    </tr>
    <tr>
      <th>Pizza</th>
      <td>184</td>
      <td>184</td>
      <td>184</td>
      <td>184</td>
      <td>184</td>
      <td>184</td>
      <td>184</td>
      <td>184</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Restaurant Type Distribution
plt.pie(j,labels=j.index,autopct="%1.f%%", shadow=True)
plt.axis('equal')
plt.title('Distribution of Restaurant Types')
plt.show()
```


![png](output_18_0.png)



```python
#Total Distribution of Distances
g=f[f['Distance']<6000]
rs = np.random.RandomState(0)
n, p = 40, 8
d = rs.normal(0, 2, (n, p))
d += np.log(np.arange(1, p + 1)) * -5 + 10
pal = sns.cubehelix_palette(p, rot=-.5, dark=.3)
sns.violinplot(data=g['Distance'], palette=pal, inner="points")
plt.title('Distribution of Distances')
plt.ylabel('Distance (meters)')
plt.xlabel('Restaurants')
plt.show()
```


![png](output_19_0.png)



```python
#Binning Distances
g=f[f['Price']!='n/a']
g['Price']=g['Price'].astype(float)
labels=[i for i in range(0,4000,100)]
g['Distance']=pd.cut(g['Distance'],[i for i in range(0,4100,100)],labels=labels)
i=g.groupby('Distance').mean()
```

    C:\Users\J\Anaconda3\lib\site-packages\ipykernel_launcher.py:2: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      
    C:\Users\J\Anaconda3\lib\site-packages\ipykernel_launcher.py:4: SettingWithCopyWarning: 
    A value is trying to be set on a copy of a slice from a DataFrame.
    Try using .loc[row_indexer,col_indexer] = value instead
    
    See the caveats in the documentation: http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-view-versus-copy
      after removing the cwd from sys.path.
    

# These 3 Graphs are binning the distances based purely on meters away


```python
x=i.index.categories
y=i['Price']
fit = np.polyfit(x,y,1)
fit_fn = np.poly1d(fit) 
plt.plot(x,y, 'yo', x, fit_fn(x), '--k')
plt.text(x=0,y=2,s='Y = -.00313X + 2.21')
plt.text(x=0,y=1.96,s='$R^{2}$ = .245, p = .000262')
plt.xlabel('Distance of Restaurants (Meters)')
plt.ylabel('Average Price')
plt.title('Price versus Distance of Restaurant (Absolute)')
plt.show()
print(linregress(x,y))
```


![png](output_22_0.png)


    LinregressResult(slope=-4.1932818082761443e-05, intercept=2.1712041704624463, rvalue=-0.37512291237330669, pvalue=0.01707539812091562, stderr=1.6809574477544837e-05)
    


```python
x=i.index.categories
y=i['Rating']
fit = np.polyfit(x,y,1)
fit_fn = np.poly1d(fit) 
plt.plot(x,y, 'yo', x, fit_fn(x), '--k')
plt.text(x=500,y=4,s='Y = -8.27E-6*X + 4.17')
plt.text(x=500,y=3.95,s='$R^{2}$ = .011, p = .511')
plt.xlabel('Distance of Restaurants (Meters)')
plt.ylabel('Average Rating')
plt.title('Rating versus Distance of Restaurant (Absolute)')
plt.show()
print(linregress(x,y))
```


![png](output_23_0.png)


    LinregressResult(slope=-8.2769916301354592e-06, intercept=4.1672725017609462, rvalue=-0.10694244864306066, pvalue=0.51130722728678979, stderr=1.2483398612665412e-05)
    


```python
x=i.Price
y=i.Rating
fit = np.polyfit(x,y,1)
fit_fn = np.poly1d(fit) 
plt.plot(x,y, 'yo', x, fit_fn(x), '--k')
plt.text(x=1.8,y=4,s='Y = -.112X + 4.38')
plt.text(x=1.8,y=3.96,s='$R^{2}$ = .026, p = .320')
plt.xlabel('Average Price of Restaurant')
plt.ylabel('Average Rating of Restaurant')
plt.title('Price versus Rating of Restaurant (Absolute)')
plt.show()
print(linregress(x,y))
```


![png](output_24_0.png)


    LinregressResult(slope=-0.11169806513701161, intercept=4.3845182343813534, rvalue=-0.16132575881406544, pvalue=0.31998275552793715, stderr=0.11084696768090577)
    
