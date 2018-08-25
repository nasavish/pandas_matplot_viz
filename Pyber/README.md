

```python
# Importing dependencies
import os
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
```

# Observable trends

1. There are two outliers in this dataset. Manuelchester had the highest average fare at $49.62 and Port James had the most number of rides at 64
2. Urban areas have the most drivers and riders out of the three city types.
3. Rural areas have the lowest number or rides but the highest fares, on average.  


```python
# Locate data
city = os.path.join('raw_data', 'city_data.csv')
ride = os.path.join('raw_data', 'ride_data.csv')

# Read/ show data
city_df = pd.read_csv(city)
ride_df = pd.read_csv(ride)
#ride_df
#city_df

# Merge data
df = ride_df.merge(city_df, on='city', how='left')
df_city = df.set_index('city')
df_city.head()
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
      <th>date</th>
      <th>fare</th>
      <th>ride_id</th>
      <th>driver_count</th>
      <th>type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Sarabury</th>
      <td>2016-01-16 13:49:27</td>
      <td>38.35</td>
      <td>5403689035038</td>
      <td>46</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>South Roy</th>
      <td>2016-01-02 18:42:34</td>
      <td>17.49</td>
      <td>4036272335942</td>
      <td>35</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Wiseborough</th>
      <td>2016-01-21 17:35:29</td>
      <td>44.18</td>
      <td>3645042422587</td>
      <td>55</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Spencertown</th>
      <td>2016-07-31 14:53:22</td>
      <td>6.87</td>
      <td>2242596575892</td>
      <td>68</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Nguyenbury</th>
      <td>2016-07-09 04:42:44</td>
      <td>6.28</td>
      <td>1543057793673</td>
      <td>8</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Average fair/ city
# Group data by city
city_group_df = df_city.groupby('city')

# Calculate mean fare
avg_fare = city_group_df['fare'].mean()
avg_fare = avg_fare.to_frame()
avg_fare = avg_fare.rename(columns={'fare':'Average Fare'})

avg_fare

# Calculate # of rides/ city
city_rides = df['city'].value_counts()
city_rides = city_rides.to_frame()
city_rides = city_rides.rename(columns={'city':'Number of Rides'})
city_rides.index.names = ['city']
city_rides.head()

# Total num of drivers/ city
city_drivers = city_group_df['driver_count'].max()
city_drivers = city_drivers.to_frame()
city_drivers = city_drivers.rename(columns={'driver_count':'Number of Drivers'})
city_drivers.head()

# City and type of city
city_type = city_group_df['type'].max()
city_type = city_type.to_frame()
city_type.head()

# Merge data
merge1 = avg_fare.merge(city_drivers, left_index=True, right_index=True)
merge2 = merge1.merge(city_rides, left_index=True, right_index=True)
merge3 = merge2.merge(city_type, left_index=True, right_index=True)
merge3.head()

# Drivers
drivers = city_drivers['Number of Drivers']

# Plot city, average fare, number of drivers, number of rides]

sns.lmplot('Number of Rides','Average Fare', data=merge3, fit_reg=False, hue='type', scatter_kws={'s': drivers*4, 'edgecolors':'black'})
plt.title("Pyber Ride Sharing Data (2016)")
```




    Text(0.5,1,'Pyber Ride Sharing Data (2016)')




![png](output_4_1.png)



```python
merge3.head()

# ### % of Total Fares by City Type
# # grouping by city type
# g_city = df.groupby('type')

# # Calculating city fare totals by type
# city_fare = g_city['fare'].sum()
# city_fare = city_fare.to_frame()
# total_city_fare = city_fare['fare'].sum()
# city_fare['fare percentage'] = city_fare['fare']/ total_city_fare
# city_fare

# ### % of Total Rides by City Type


# ### % of Total Drivers by City Type
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
      <th>Average Fare</th>
      <th>Number of Drivers</th>
      <th>Number of Rides</th>
      <th>type</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Alvarezhaven</th>
      <td>23.928710</td>
      <td>21</td>
      <td>31</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Alyssaberg</th>
      <td>20.609615</td>
      <td>67</td>
      <td>26</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Anitamouth</th>
      <td>37.315556</td>
      <td>16</td>
      <td>9</td>
      <td>Suburban</td>
    </tr>
    <tr>
      <th>Antoniomouth</th>
      <td>23.625000</td>
      <td>21</td>
      <td>22</td>
      <td>Urban</td>
    </tr>
    <tr>
      <th>Aprilchester</th>
      <td>21.981579</td>
      <td>49</td>
      <td>19</td>
      <td>Urban</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.head()

total_fare = df['fare'].sum()

city_type = df.groupby('type')
city_type = city_type['fare'].sum()
city_type = city_type.to_frame()
city_type.head()
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
      <th>fare</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>4255.09</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>20335.69</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>40078.34</td>
    </tr>
  </tbody>
</table>
</div>




```python
city_type['fare ratio'] = (city_type['fare']/(total_fare))*100
city_type
fare_ratios = city_type
fare_ratios
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
      <th>fare</th>
      <th>fare ratio</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>4255.09</td>
      <td>6.579786</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>20335.69</td>
      <td>31.445750</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>40078.34</td>
      <td>61.974463</td>
    </tr>
  </tbody>
</table>
</div>




```python
new_df = fare_ratios.reset_index()
plt.pie(new_df['fare ratio'], labels=new_df['type'],
        autopct='{:.1f}%'.format, shadow=True, startangle=140)
plt.axis("equal")
plt.title('Percentage of Fairs by City Type')
plt.show()
```


![png](output_8_0.png)



```python
df = merge3

```


```python
total_drivers = df

total_drivers = df.groupby('type')
total_drivers = total_drivers['Number of Drivers'].sum()
total_drivers = total_drivers.to_frame()
total_drivers

total_number_of_drivers = total_drivers['Number of Drivers'].sum()

total_drivers['driver ratio'] = (total_drivers['Number of Drivers']/(total_number_of_drivers))*100
total_drivers
driver_ratios = total_drivers
driver_ratios

new_df = driver_ratios.reset_index()
plt.pie(new_df['driver ratio'], labels=new_df['type'],
        autopct='{:.1f}%'.format, shadow=True, startangle=140)
plt.axis("equal")
plt.title('Percentage of Drivers by City Type')
plt.show()
```


![png](output_10_0.png)



```python
total_rides = df

total_rides = df.groupby('type')
total_rides = total_rides['Number of Rides'].sum()
total_rides = total_rides.to_frame()
total_rides

total_number_of_riders = total_rides['Number of Rides'].sum()

total_rides['rider ratio'] = (total_rides['Number of Rides']/(total_number_of_riders))*100
total_rides
rider_ratios = total_rides
rider_ratios

new_df = rider_ratios.reset_index()
plt.pie(new_df['rider ratio'], labels=new_df['type'],
        autopct='{:.1f}%'.format, shadow=True, startangle=140)
plt.axis("equal")
plt.title('Percentage of Riders by City Type')
plt.show()
```


![png](output_11_0.png)



```python
df.loc[df['Average Fare'].idxmax()]
# df.head()
```




    Average Fare         49.62
    Number of Drivers        7
    Number of Rides          1
    type                 Rural
    Name: Manuelchester, dtype: object


