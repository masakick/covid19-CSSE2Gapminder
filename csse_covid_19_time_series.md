
# Convert COVID-19 data from CSSE to Gapminder format

load csv  data soruces: https://github.com/CSSEGISandData/COVID-19
CC-BY: Masaki Yamabe (masakiyamabe.com)


```python
#Library
import numpy as np
import pandas as pd

source_confirmed = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Confirmed.csv"
source_death = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Deaths.csv"
source_recoverd = "https://raw.githubusercontent.com/CSSEGISandData/COVID-19/master/csse_covid_19_data/csse_covid_19_time_series/time_series_19-covid-Recovered.csv"

df_confirmed = pd.read_csv(source_confirmed)
df_death = pd.read_csv(source_death)
df_recoverd  = pd.read_csv(source_recoverd)
```


```python
#delete unuse column
del df_confirmed['Province/State']
del df_confirmed['Lat']
del df_confirmed['Long']

del df_death['Province/State']
del df_death['Lat']
del df_death['Long']

del df_recoverd['Province/State']
del df_recoverd['Lat']
del df_recoverd['Long']
df_recoverd.head()
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
      <th>Country/Region</th>
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>1/28/20</th>
      <th>1/29/20</th>
      <th>1/30/20</th>
      <th>...</th>
      <th>3/8/20</th>
      <th>3/9/20</th>
      <th>3/10/20</th>
      <th>3/11/20</th>
      <th>3/12/20</th>
      <th>3/13/20</th>
      <th>3/14/20</th>
      <th>3/15/20</th>
      <th>3/16/20</th>
      <th>3/17/20</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Thailand</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>2</td>
      <td>2</td>
      <td>5</td>
      <td>5</td>
      <td>5</td>
      <td>...</td>
      <td>31</td>
      <td>31</td>
      <td>33</td>
      <td>34</td>
      <td>34</td>
      <td>35</td>
      <td>35</td>
      <td>35</td>
      <td>35</td>
      <td>41</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Japan</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>...</td>
      <td>76</td>
      <td>76</td>
      <td>101</td>
      <td>118</td>
      <td>118</td>
      <td>118</td>
      <td>118</td>
      <td>118</td>
      <td>144</td>
      <td>144</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Singapore</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>78</td>
      <td>78</td>
      <td>78</td>
      <td>96</td>
      <td>96</td>
      <td>97</td>
      <td>105</td>
      <td>105</td>
      <td>109</td>
      <td>114</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Nepal</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Malaysia</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>24</td>
      <td>24</td>
      <td>24</td>
      <td>26</td>
      <td>26</td>
      <td>26</td>
      <td>35</td>
      <td>42</td>
      <td>42</td>
      <td>49</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 57 columns</p>
</div>




```python
# group by "country/region"
df_confirmed = df_confirmed.groupby("Country/Region").apply(lambda x: x.sum()).drop('Country/Region', axis=1).reset_index()
df_death =  df_death.groupby("Country/Region").apply(lambda x: x.sum()).drop('Country/Region', axis=1).reset_index()
df_recoverd =  df_recoverd.groupby("Country/Region").apply(lambda x: x.sum()).drop('Country/Region', axis=1).reset_index()
```


```python
#insert case column

df_confirmed.insert(1,"case",'confirmed')
df_death.insert(1,"case",'deaths')
df_recoverd.insert(1,"case",'recoverd')
```


```python
df_combined = pd.concat([df_confirmed, df_death, df_recoverd])
df_combined.head()
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
      <th>Country/Region</th>
      <th>case</th>
      <th>1/22/20</th>
      <th>1/23/20</th>
      <th>1/24/20</th>
      <th>1/25/20</th>
      <th>1/26/20</th>
      <th>1/27/20</th>
      <th>1/28/20</th>
      <th>1/29/20</th>
      <th>...</th>
      <th>3/8/20</th>
      <th>3/9/20</th>
      <th>3/10/20</th>
      <th>3/11/20</th>
      <th>3/12/20</th>
      <th>3/13/20</th>
      <th>3/14/20</th>
      <th>3/15/20</th>
      <th>3/16/20</th>
      <th>3/17/20</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>confirmed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>11</td>
      <td>16</td>
      <td>21</td>
      <td>22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>confirmed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>2</td>
      <td>10</td>
      <td>12</td>
      <td>23</td>
      <td>33</td>
      <td>38</td>
      <td>42</td>
      <td>51</td>
      <td>55</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>confirmed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>19</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>24</td>
      <td>26</td>
      <td>37</td>
      <td>48</td>
      <td>54</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Andorra</td>
      <td>confirmed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>39</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Antigua and Barbuda</td>
      <td>confirmed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 58 columns</p>
</div>




```python
#convert date format
from datetime import datetime as dt
cols = np.array(df_combined.columns)


for i,word in enumerate(cols):
    if i>1:
        s = cols[i]
        tdatetime = dt.strptime(s, '%m/%d/%y')
        cols[i] = tdatetime.strftime('%Y%m%d')
        
# set new columns to df
df_combined.columns = cols
df_combined.head()
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
      <th>Country/Region</th>
      <th>case</th>
      <th>20200122</th>
      <th>20200123</th>
      <th>20200124</th>
      <th>20200125</th>
      <th>20200126</th>
      <th>20200127</th>
      <th>20200128</th>
      <th>20200129</th>
      <th>...</th>
      <th>20200308</th>
      <th>20200309</th>
      <th>20200310</th>
      <th>20200311</th>
      <th>20200312</th>
      <th>20200313</th>
      <th>20200314</th>
      <th>20200315</th>
      <th>20200316</th>
      <th>20200317</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Afghanistan</td>
      <td>confirmed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>4</td>
      <td>4</td>
      <td>5</td>
      <td>7</td>
      <td>7</td>
      <td>7</td>
      <td>11</td>
      <td>16</td>
      <td>21</td>
      <td>22</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Albania</td>
      <td>confirmed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>2</td>
      <td>10</td>
      <td>12</td>
      <td>23</td>
      <td>33</td>
      <td>38</td>
      <td>42</td>
      <td>51</td>
      <td>55</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Algeria</td>
      <td>confirmed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>19</td>
      <td>20</td>
      <td>20</td>
      <td>20</td>
      <td>24</td>
      <td>26</td>
      <td>37</td>
      <td>48</td>
      <td>54</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Andorra</td>
      <td>confirmed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>2</td>
      <td>39</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Antigua and Barbuda</td>
      <td>confirmed</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>...</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
<p>5 rows × 58 columns</p>
</div>




```python
df_combined.describe()
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
      <th>20200122</th>
      <th>20200123</th>
      <th>20200124</th>
      <th>20200125</th>
      <th>20200126</th>
      <th>20200127</th>
      <th>20200128</th>
      <th>20200129</th>
      <th>20200130</th>
      <th>20200131</th>
      <th>...</th>
      <th>20200308</th>
      <th>20200309</th>
      <th>20200310</th>
      <th>20200311</th>
      <th>20200312</th>
      <th>20200313</th>
      <th>20200314</th>
      <th>20200315</th>
      <th>20200316</th>
      <th>20200317</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>...</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
      <td>456.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>1.315789</td>
      <td>1.537281</td>
      <td>2.199561</td>
      <td>3.322368</td>
      <td>4.881579</td>
      <td>6.732456</td>
      <td>12.754386</td>
      <td>14.089912</td>
      <td>18.745614</td>
      <td>22.723684</td>
      <td>...</td>
      <td>382.217105</td>
      <td>394.831140</td>
      <td>410.653509</td>
      <td>433.076754</td>
      <td>441.638158</td>
      <td>484.315789</td>
      <td>514.342105</td>
      <td>548.076754</td>
      <td>584.967105</td>
      <td>626.953947</td>
    </tr>
    <tr>
      <th>std</th>
      <td>25.703231</td>
      <td>30.150296</td>
      <td>43.125904</td>
      <td>65.886843</td>
      <td>97.219516</td>
      <td>134.791523</td>
      <td>258.068789</td>
      <td>285.140063</td>
      <td>381.333388</td>
      <td>459.182769</td>
      <td>...</td>
      <td>4671.441320</td>
      <td>4720.435238</td>
      <td>4767.374188</td>
      <td>4826.384583</td>
      <td>4867.117631</td>
      <td>4952.609674</td>
      <td>5037.890988</td>
      <td>5129.598975</td>
      <td>5216.159370</td>
      <td>5315.703096</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>1.000000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>...</td>
      <td>2.250000</td>
      <td>3.000000</td>
      <td>4.000000</td>
      <td>6.000000</td>
      <td>6.250000</td>
      <td>8.250000</td>
      <td>12.000000</td>
      <td>14.000000</td>
      <td>17.000000</td>
      <td>26.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>548.000000</td>
      <td>643.000000</td>
      <td>920.000000</td>
      <td>1406.000000</td>
      <td>2075.000000</td>
      <td>2877.000000</td>
      <td>5509.000000</td>
      <td>6087.000000</td>
      <td>8141.000000</td>
      <td>9802.000000</td>
      <td>...</td>
      <td>80823.000000</td>
      <td>80860.000000</td>
      <td>80887.000000</td>
      <td>80921.000000</td>
      <td>80932.000000</td>
      <td>80945.000000</td>
      <td>80977.000000</td>
      <td>81003.000000</td>
      <td>81033.000000</td>
      <td>81058.000000</td>
    </tr>
  </tbody>
</table>
<p>8 rows × 56 columns</p>
</div>




```python
df_combined.to_csv("covid19_gapminder.csv",index=False)
```
