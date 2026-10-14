---
layout: default
title: "Understanding UrbanEV"
---

<p class="article-nav"><a href="{{ '/' | relative_url }}">← All articles</a> · <a href="{{ '/notebooks/01_dataset_overview.ipynb' | relative_url }}">Source notebook</a></p>
<p class="article-meta">UrbanEV / Article 01</p>

# Understanding UrbanEV


### Objective

Explore the structure and contents of the UrbanEV dataset,
an open benchmark of electric vehicle charging activity
in Shenzhen, China.

**Source:** Li et al. (2025), Scientific Data

**Repository:** https://github.com/IntelligentSystemsLab/UrbanEV


### Dataset Overview

UrbanEV is an open dataset of electric vehicle charging
activity in Shenzhen, China, published by Li et al. (2025)
in Scientific Data.

#### Temporal coverage

- Period: September 1, 2022 – February 28, 2023
- Duration: 181 days
- Resolution: Hourly
- Observations: 4,344
- Missing timestamps: 0
- Duplicate timestamps: 0

#### Spatial coverage

The cleaned dataset contains:

- 275 Traffic Analysis Zones (TAZs)
- 1,362 charging stations
- 17,532 charging piles

Charging infrastructure is unevenly distributed across
the selected zones. The median zone has 4 charging stations
and 48 charging piles.

#### Available data

The dataset combines:

1. Charging activity: volume, duration and occupancy
2. Charging prices: electricity and service fees
3. Weather: measurements from two meteorological stations
4. Spatial information: zone adjacency and distances
5. Urban context: 712,135 Points of Interest

#### Dataset structure

The six charging-related datasets share the same 4,344
timestamps and 275 zone identifiers.

Both weather datasets share the charging timestamps.

The spatial matrices are 275 × 275, representing
relationships between geographic zones.

#### Important considerations

- Charging volume is estimated rather than necessarily
  directly metered.
- The dataset has undergone preprocessing and filtering.
- Complete timestamps do not guarantee complete measurements.
- POIs are not directly assigned to geographic zones.
- Geographic coordinate systems differ between data sources.
- Occupancy units should be verified before analysis.



### File inventory


```python

from pathlib import Path
import pandas as pd

DATA_DIR = Path("data")

files = sorted(DATA_DIR.glob("*.csv"))

inventory = pd.DataFrame({
    "filename": [f.name for f in files],
    "size_mb": [round(f.stat().st_size / 1024**2, 2) for f in files]
})

display(inventory)

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
      <th>filename</th>
      <th>size_mb</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>adj.csv</td>
      <td>0.15</td>
    </tr>
    <tr>
      <th>1</th>
      <td>distance.csv</td>
      <td>0.86</td>
    </tr>
    <tr>
      <th>2</th>
      <td>duration.csv</td>
      <td>16.98</td>
    </tr>
    <tr>
      <th>3</th>
      <td>e_price.csv</td>
      <td>11.49</td>
    </tr>
    <tr>
      <th>4</th>
      <td>inf.csv</td>
      <td>0.07</td>
    </tr>
    <tr>
      <th>5</th>
      <td>inf_raw.csv</td>
      <td>0.08</td>
    </tr>
    <tr>
      <th>6</th>
      <td>occupancy.csv</td>
      <td>5.26</td>
    </tr>
    <tr>
      <th>7</th>
      <td>poi.csv</td>
      <td>31.13</td>
    </tr>
    <tr>
      <th>8</th>
      <td>s_price.csv</td>
      <td>14.27</td>
    </tr>
    <tr>
      <th>9</th>
      <td>volume-11kW.csv</td>
      <td>18.81</td>
    </tr>
    <tr>
      <th>10</th>
      <td>volume.csv</td>
      <td>15.74</td>
    </tr>
    <tr>
      <th>11</th>
      <td>weather_airport.csv</td>
      <td>0.16</td>
    </tr>
    <tr>
      <th>12</th>
      <td>weather_central.csv</td>
      <td>0.17</td>
    </tr>
  </tbody>
</table>
</div>


### The structure of `volume.csv`

`volume.csv` contains estimated hourly EV charging electricity consumption in kWh.



```python
volume = pd.read_csv(DATA_DIR / "volume.csv")

print("Shape (rows, columns):", volume.shape)

print("\nFirst 10 column names:")
print(volume.columns[:10].tolist())

print("\nFirst 5 rows (first 6 columns):")
display(volume.iloc[:5, :6])

print("\nData types (first 6 columns):")
display(volume.dtypes.head(6))
```

    Shape (rows, columns): (4344, 276)
    
    First 10 column names:
    ['time', '102', '104', '105', '106', '107', '108', '109', '110', '111']
    
    First 5 rows (first 6 columns):



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
      <th>time</th>
      <th>102</th>
      <th>104</th>
      <th>105</th>
      <th>106</th>
      <th>107</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2022-09-01 00:00:00</td>
      <td>56.291667</td>
      <td>11.083333</td>
      <td>60.375000</td>
      <td>78.750000</td>
      <td>90.125000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022-09-01 01:00:00</td>
      <td>50.458333</td>
      <td>9.333333</td>
      <td>72.625000</td>
      <td>81.666667</td>
      <td>100.333333</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022-09-01 02:00:00</td>
      <td>55.708333</td>
      <td>12.833333</td>
      <td>73.208333</td>
      <td>89.250000</td>
      <td>94.500000</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2022-09-01 03:00:00</td>
      <td>57.750000</td>
      <td>12.541667</td>
      <td>67.666667</td>
      <td>74.083333</td>
      <td>89.541667</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2022-09-01 04:00:00</td>
      <td>58.333333</td>
      <td>11.958333</td>
      <td>69.708333</td>
      <td>78.166667</td>
      <td>88.666667</td>
    </tr>
  </tbody>
</table>
</div>


    
    Data types (first 6 columns):



    time        str
    102     float64
    104     float64
    105     float64
    106     float64
    107     float64
    dtype: object


**How to interpret a row**

Each value represents the estimated charging electricity volume in that zone during that hour.

These volumes are estimated using charging activity and rated charging power, rather than necessarily representing directly metered electricity consumption.

### Verify temporal coverage

We can confirm that our time series is complete and correctly ordered.


```python

# Convert the timestamp column to datetime
volume["time"] = pd.to_datetime(volume["time"])

# Sort timestamps
timestamps = volume["time"].sort_values()

# Create the expected hourly timeline
expected = pd.date_range(
    start=timestamps.min(),
    end=timestamps.max(),
    freq="h"
)

print("First timestamp:", timestamps.min())
print("Last timestamp:", timestamps.max())
print("Total observations:", len(timestamps))

print("\nDuplicate timestamps:", timestamps.duplicated().sum())
print("Missing hourly timestamps:",
      len(expected.difference(pd.DatetimeIndex(timestamps))))

print("\nTime intervals between observations:")
display(timestamps.diff().value_counts().head())

```

    First timestamp: 2022-09-01 00:00:00
    Last timestamp: 2023-02-28 23:00:00
    Total observations: 4344
    
    Duplicate timestamps: 0
    Missing hourly timestamps: 0
    
    Time intervals between observations:



    time
    0 days 01:00:00    4343
    Name: count, dtype: int64


All 4,343 consecutive time intervals are exactly one hour apart.
This means that we can confidently describe the dataset as containing 181 consecutive days of hourly observations.

A complete timeline doesn't necessarily mean all charging measurements are valid or non-missing. We'll investigate that separately.

### Understanding the spatial dimension

We now know that our dataset contains 275 geographic zones. But what exactly are these zones?
According to the authors, they represent Traffic Analysis Zones (TAZs) in Shenzhen. These are geographic areas used in transportation planning and analysis.

The file `inf.csv` contains information about individual charging stations and their associated zones.
Let's inspect it and see how the station identifiers relate to our `volume.csv` columns.


```python

# Load station information
stations = pd.read_csv(DATA_DIR / "inf.csv")

print("Station information shape:", stations.shape)

print("\nColumns:")
print(stations.columns.tolist())

print("\nFirst 5 rows:")
display(stations.head())

# Identify geographic zones
volume_zones = set(map(int, volume.columns.drop("time")))
station_zones = set(stations["TAZID"].dropna().astype(int))

print("\nSpatial summary:")
print("Zones in volume.csv:", len(volume_zones))
print("Zones in inf.csv:", len(station_zones))
print("Unique stations:", stations["station_id"].nunique())
print("Total charging piles:", stations["charge_count"].sum())

print("\nZones in volume.csv but not in inf.csv:")
print(sorted(volume_zones - station_zones))

```

    Station information shape: (1362, 7)
    
    Columns:
    ['station_id', 'longitude', 'latitude', 'charge_count', 'TAZID', 'area', 'perimeter']
    
    First 5 rows:



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
      <th>station_id</th>
      <th>longitude</th>
      <th>latitude</th>
      <th>charge_count</th>
      <th>TAZID</th>
      <th>area</th>
      <th>perimeter</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1001</td>
      <td>113.784724</td>
      <td>22.714121</td>
      <td>20</td>
      <td>559</td>
      <td>5726523.056</td>
      <td>9624.5755</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1002</td>
      <td>113.785002</td>
      <td>22.725900</td>
      <td>22</td>
      <td>558</td>
      <td>3927542.045</td>
      <td>8189.3687</td>
    </tr>
    <tr>
      <th>2</th>
      <td>1003</td>
      <td>113.787971</td>
      <td>22.735538</td>
      <td>6</td>
      <td>558</td>
      <td>3927542.045</td>
      <td>8189.3687</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1004</td>
      <td>113.788126</td>
      <td>22.693449</td>
      <td>11</td>
      <td>596</td>
      <td>2972113.241</td>
      <td>7335.1002</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1006</td>
      <td>113.790987</td>
      <td>22.731351</td>
      <td>10</td>
      <td>594</td>
      <td>2044150.527</td>
      <td>5825.1986</td>
    </tr>
  </tbody>
</table>
</div>


    
    Spatial summary:
    Zones in volume.csv: 275
    Zones in inf.csv: 275
    Unique stations: 1362
    Total charging piles: 17532
    
    Zones in volume.csv but not in inf.csv:
    []


**What Do the Columns Mean?**

| Column                  | Meaning                                 |
| ----------------------- | --------------------------------------- |
| `station_id`            | Unique charging station identifier      |
| `longitude`, `latitude` | Geographic coordinates of the station   |
| `charge_count`          | Number of charging piles at the station |
| `TAZID`                 | Traffic Analysis Zone identifier        |
| `area`                  | Area of the geographic zone (m²)        |
| `perimeter`             | Perimeter of the geographic zone (m)    |


All 275 zones in `volume.csv` are represented in `inf.csv`. This means we can connect hourly charging demand with information about the charging infrastructure in each zone.

`inf.csv` contains station-level records, whereas `volume.csv` contains zone-level measurements. Multiple stations can belong to the same zone.

### How is charging infrastructure distributed across zones?

We know there are 1,362 charging stations across 275 zones.

But are the stations evenly distributed?

The distribution of infrastructure may help explain differences in charging activity between zones.


```python
# Aggregate station information by geographic zone
zone_summary = (
    stations.groupby("TAZID")
    .agg(
        n_stations=("station_id", "nunique"),
        n_piles=("charge_count", "sum")
    )
    .reset_index()
)

print("Summary statistics:")
display(zone_summary[["n_stations", "n_piles"]].describe().round(2))

print("\nTop 5 zones by number of charging piles:")
display(
    zone_summary.nlargest(5, "n_piles")
)

print("\nBottom 5 zones by number of charging piles:")
display(
    zone_summary.nsmallest(5, "n_piles")
)
```

    Summary statistics:



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
      <th>n_stations</th>
      <th>n_piles</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>275.00</td>
      <td>275.00</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>4.95</td>
      <td>63.75</td>
    </tr>
    <tr>
      <th>std</th>
      <td>4.45</td>
      <td>56.73</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.00</td>
      <td>4.00</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.00</td>
      <td>24.00</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>4.00</td>
      <td>48.00</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>7.00</td>
      <td>84.00</td>
    </tr>
    <tr>
      <th>max</th>
      <td>41.00</td>
      <td>373.00</td>
    </tr>
  </tbody>
</table>
</div>


    
    Top 5 zones by number of charging piles:



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
      <th>TAZID</th>
      <th>n_stations</th>
      <th>n_piles</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>271</th>
      <td>1167</td>
      <td>19</td>
      <td>373</td>
    </tr>
    <tr>
      <th>207</th>
      <td>1011</td>
      <td>41</td>
      <td>332</td>
    </tr>
    <tr>
      <th>67</th>
      <td>526</td>
      <td>14</td>
      <td>262</td>
    </tr>
    <tr>
      <th>42</th>
      <td>329</td>
      <td>17</td>
      <td>254</td>
    </tr>
    <tr>
      <th>47</th>
      <td>335</td>
      <td>19</td>
      <td>249</td>
    </tr>
  </tbody>
</table>
</div>


    
    Bottom 5 zones by number of charging piles:



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
      <th>TAZID</th>
      <th>n_stations</th>
      <th>n_piles</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>33</th>
      <td>316</td>
      <td>1</td>
      <td>4</td>
    </tr>
    <tr>
      <th>94</th>
      <td>628</td>
      <td>1</td>
      <td>6</td>
    </tr>
    <tr>
      <th>130</th>
      <td>719</td>
      <td>1</td>
      <td>6</td>
    </tr>
    <tr>
      <th>150</th>
      <td>792</td>
      <td>1</td>
      <td>6</td>
    </tr>
    <tr>
      <th>163</th>
      <td>852</td>
      <td>1</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>


**Charging infrastructure is distributed quite unevenly across Shenzhen's 275 zones!!**

1. The typical zone has relatively few charging stations. The median zone contains four stations and 48 charging piles. Half of the zones have between 24 and 84 charging piles.

2. Some zones have considerably more infrastructure. Zone 1167 contains 373 charging piles across 19 stations, whereas zone 316 has only four piles at a single station. That's a substantial difference.

3. More stations doesn't necessarily mean more charging piles. Zone 1011 contains 41 stations and 332 piles, while zone 1167 has just 19 stations but 373 piles. Station sizes clearly differ.

### Check the other charging time series

So far, we've explored `volume.csv`. But UrbanEV provides multiple measurements of charging activity.

First we have to establish whether these files share the same structure:

* `volume.csv` — estimated electricity consumption (kWh)
* `volume-11kW.csv` — alternative electricity consumption estimate (kWh)
* `duration.csv` — charging duration (hours)
* `occupancy.csv` — charging occupancy (we'll verify its units)
* `e_price.csv` — electricity price (yuan/kWh)
* `s_price.csv` — service price (yuan/kWh)


```python

charging_files = [
    "volume.csv",
    "volume-11kW.csv",
    "duration.csv",
    "occupancy.csv",
    "e_price.csv",
    "s_price.csv",
]

reference_zones = set(volume.columns) - {"time"}

results = []

for filename in charging_files:
    path = DATA_DIR / filename

    # Read only the header and timestamps
    columns = pd.read_csv(path, nrows=0).columns
    times = pd.read_csv(path, usecols=["time"])["time"]
    times = pd.to_datetime(times)

    zones = set(columns) - {"time"}

    results.append({
        "file": filename,
        "rows": len(times),
        "zones": len(zones),
        "same_zones": zones == reference_zones,
        "same_timestamps": times.equals(volume["time"])
    })

display(pd.DataFrame(results))

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
      <th>file</th>
      <th>rows</th>
      <th>zones</th>
      <th>same_zones</th>
      <th>same_timestamps</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>volume.csv</td>
      <td>4344</td>
      <td>275</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>1</th>
      <td>volume-11kW.csv</td>
      <td>4344</td>
      <td>275</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>2</th>
      <td>duration.csv</td>
      <td>4344</td>
      <td>275</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>3</th>
      <td>occupancy.csv</td>
      <td>4344</td>
      <td>275</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>4</th>
      <td>e_price.csv</td>
      <td>4344</td>
      <td>275</td>
      <td>True</td>
      <td>True</td>
    </tr>
    <tr>
      <th>5</th>
      <td>s_price.csv</td>
      <td>4344</td>
      <td>275</td>
      <td>True</td>
      <td>True</td>
    </tr>
  </tbody>
</table>
</div>


### Understanding the weather data

Next, we investigate `weather_airport.csv` and `weather_central.csv`.

Unlike the charging data, which has one column per geographic zone, these files contain observations from two meteorological stations in Shenzhen.

Our goal is to understand their structure and whether their timestamps align with the charging data.


```python

weather_files = [
    "weather_airport.csv",
    "weather_central.csv"
]

for filename in weather_files:
    weather = pd.read_csv(DATA_DIR / filename)

    print(f"\n{'=' * 50}")
    print(f"File: {filename}")
    print(f"{'=' * 50}")

    print("Shape:", weather.shape)
    print("\nColumns:", weather.columns.tolist())

    print("\nFirst 5 rows:")
    display(weather.head())

    # Examine temporal coverage
    weather["time"] = pd.to_datetime(weather["time"])

    print("First timestamp:", weather["time"].min())
    print("Last timestamp:", weather["time"].max())
    print("Duplicate timestamps:", weather["time"].duplicated().sum())

    # Compare timestamps with charging observations
    charging_times = set(volume["time"])
    weather_times = set(weather["time"].dropna())

    print("Matching charging timestamps:",
          len(charging_times & weather_times))
    print("Charging timestamps absent from weather:",
          len(charging_times - weather_times))

```

    
    ==================================================
    File: weather_airport.csv
    ==================================================
    Shape: (4344, 7)
    
    Columns: ['time', 'T', 'P0', 'P', 'U', 'nRAIN', 'Td']
    
    First 5 rows:



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
      <th>time</th>
      <th>T</th>
      <th>P0</th>
      <th>P</th>
      <th>U</th>
      <th>nRAIN</th>
      <th>Td</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2022/9/1 0:00</td>
      <td>28</td>
      <td>755.9</td>
      <td>756.2</td>
      <td>89</td>
      <td>0</td>
      <td>26</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022/9/1 1:00</td>
      <td>29</td>
      <td>755.9</td>
      <td>756.2</td>
      <td>84</td>
      <td>0</td>
      <td>26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022/9/1 2:00</td>
      <td>29</td>
      <td>755.1</td>
      <td>755.4</td>
      <td>79</td>
      <td>0</td>
      <td>25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2022/9/1 3:00</td>
      <td>29</td>
      <td>755.1</td>
      <td>755.4</td>
      <td>79</td>
      <td>0</td>
      <td>25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2022/9/1 4:00</td>
      <td>28</td>
      <td>755.1</td>
      <td>755.4</td>
      <td>84</td>
      <td>0</td>
      <td>25</td>
    </tr>
  </tbody>
</table>
</div>


    First timestamp: 2022-09-01 00:00:00
    Last timestamp: 2023-02-28 23:00:00
    Duplicate timestamps: 0
    Matching charging timestamps: 4344
    Charging timestamps absent from weather: 0
    
    ==================================================
    File: weather_central.csv
    ==================================================
    Shape: (4344, 7)
    
    Columns: ['time', 'T', 'P0', 'P', 'U', 'nRAIN', 'Td']
    
    First 5 rows:



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
      <th>time</th>
      <th>T</th>
      <th>P0</th>
      <th>P</th>
      <th>U</th>
      <th>nRAIN</th>
      <th>Td</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2022/9/1 0:00</td>
      <td>27.3</td>
      <td>750.6</td>
      <td>756.1</td>
      <td>87</td>
      <td>0</td>
      <td>24.9</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2022/9/1 1:00</td>
      <td>27.3</td>
      <td>750.6</td>
      <td>756.1</td>
      <td>87</td>
      <td>0</td>
      <td>24.9</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2022/9/1 2:00</td>
      <td>27.3</td>
      <td>750.6</td>
      <td>756.1</td>
      <td>87</td>
      <td>0</td>
      <td>24.9</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2022/9/1 3:00</td>
      <td>27.3</td>
      <td>750.6</td>
      <td>756.1</td>
      <td>87</td>
      <td>0</td>
      <td>24.9</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2022/9/1 4:00</td>
      <td>27.3</td>
      <td>750.6</td>
      <td>756.1</td>
      <td>87</td>
      <td>0</td>
      <td>24.9</td>
    </tr>
  </tbody>
</table>
</div>


    First timestamp: 2022-09-01 00:00:00
    Last timestamp: 2023-02-28 23:00:00
    Duplicate timestamps: 0
    Matching charging timestamps: 4344
    Charging timestamps absent from weather: 0


**Weather Station Data Summary**

| Property                    | Airport station     | Central station     |
| --------------------------- | ------------------- | ------------------- |
| Rows                        | 4,344               | 4,344               |
| Columns                     | 7                   | 7                   |
| Missing timestamps          | 0                   | 0                   |
| Duplicate timestamps        | 0                   | 0                   |
| Matches charging timestamps | Yes                 | Yes                 |
| Observation period          | Sep 2022 – Feb 2023 | Sep 2022 – Feb 2023 |

The two datasets contain six weather variables. Their definitions are documented in the repository's `weather_header.txt`.

**Weather Variable Descriptions**

| Variable | Meaning                                     | Unit |
| -------- | ------------------------------------------- | ---- |
| `T`      | Air temperature                             | °C   |
| `P0`     | Atmospheric pressure at the weather station | mmHg |
| `P`      | Atmospheric pressure reduced to sea level   | mmHg |
| `U`      | Relative humidity                           | %    |
| `nRAIN`  | Rain intensity category                     | 0–3  |
| `Td`     | Dew point temperature                       | °C   |

**Note:** the weather measurements come from two locations, not from all 275 zones. 

### Understanding the spatial matrices

We now examine two files that describe relationships between geographic zones:

- `adj.csv`: Which zones are geographically connected or adjacent?
- `distance.csv`: How far apart are the zones?


```python

# Load geographic matrices
adj = pd.read_csv(DATA_DIR / "adj.csv")
distance = pd.read_csv(DATA_DIR / "distance.csv")

for name, df in [
    ("Adjacency matrix", adj),
    ("Distance matrix", distance)
]:
    print(f"\n{'=' * 45}")
    print(name)
    print(f"{'=' * 45}")

    print("Shape:", df.shape)

    print("\nFirst 5 column names:")
    print(df.columns[:5].tolist())

    print("\nFirst 5 rows and columns:")
    display(df.iloc[:5, :5])

    print("Columns match charging zones:",
          set(df.columns) == set(volume.columns) - {"time"})

```

    
    =============================================
    Adjacency matrix
    =============================================
    Shape: (275, 275)
    
    First 5 column names:
    ['102', '104', '105', '106', '107']
    
    First 5 rows and columns:



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
      <th>102</th>
      <th>104</th>
      <th>105</th>
      <th>106</th>
      <th>107</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0</td>
      <td>0</td>
      <td>1</td>
      <td>1</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>


    Columns match charging zones: True
    
    =============================================
    Distance matrix
    =============================================
    Shape: (275, 275)
    
    First 5 column names:
    ['102', '104', '105', '106', '107']
    
    First 5 rows and columns:



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
      <th>102</th>
      <th>104</th>
      <th>105</th>
      <th>106</th>
      <th>107</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.000000</td>
      <td>1809.441169</td>
      <td>2196.926362</td>
      <td>1991.889627</td>
      <td>2863.407789</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1809.441169</td>
      <td>0.000000</td>
      <td>734.061580</td>
      <td>1274.183390</td>
      <td>1919.472076</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2196.926362</td>
      <td>734.061580</td>
      <td>0.000000</td>
      <td>782.769630</td>
      <td>1211.703811</td>
    </tr>
    <tr>
      <th>3</th>
      <td>1991.889627</td>
      <td>1274.183390</td>
      <td>782.769630</td>
      <td>0.000000</td>
      <td>877.584708</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2863.407789</td>
      <td>1919.472076</td>
      <td>1211.703811</td>
      <td>877.584708</td>
      <td>0.000000</td>
    </tr>
  </tbody>
</table>
</div>


    Columns match charging zones: True


**Adjacency and Distance Matrix Summary**

| Property                  | `adj.csv`         | `distance.csv`         |
| ------------------------- | ----------------- | ---------------------- |
| Shape                     | 275 × 275         | 275 × 275              |
| Represents                | Zone connectivity | Distance between zones |
| Values                    | 0 or 1            | Numerical distances    |
| Matches charging zone IDs | Yes               | Yes                    |
| Diagonal values shown     | 1                 | 0                      |


1. Adjacency matrix (`adj.csv`)
This matrix describes which geographic zones are connected.
For example, the first rows indicate that zone 104 is connected to zones 105 and 106, while zone 102 is not connected to 104.
A value of 1 on the diagonal means that a zone is considered connected to itself in this matrix representation.

2. Distance matrix (`distance.csv`)
This matrix describes the distance between pairs of zones.
For example, the output shows:
- Zones 102 and 104: approximately 1,809 distance units.
- Zones 104 and 105: approximately 734 distance units.
- A zone's distance from itself: 0.

### Explore the Points of Interest (POI) data

Now we examine `poi.csv`, the largest file in our dataset.

Points of Interest describe urban locations such as restaurants, residential buildings and lifestyle services.

According to the paper, these were collected in December 2022 using AMap and grouped into three broad categories.


```python
poi = pd.read_csv(DATA_DIR / "poi.csv")

print("Shape:", poi.shape)

print("\nColumns:")
print(poi.columns.tolist())

print("\nFirst 5 rows:")
display(poi.head())

print("\nData types:")
display(poi.dtypes.to_frame("dtype"))

```

    Shape: (712135, 3)
    
    Columns:
    ['primary_types', 'longitude', 'latitude']
    
    First 5 rows:



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
      <th>primary_types</th>
      <th>longitude</th>
      <th>latitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>lifestyle services</td>
      <td>114.576628</td>
      <td>22.494887</td>
    </tr>
    <tr>
      <th>1</th>
      <td>lifestyle services</td>
      <td>114.579614</td>
      <td>22.492192</td>
    </tr>
    <tr>
      <th>2</th>
      <td>lifestyle services</td>
      <td>114.565933</td>
      <td>22.500528</td>
    </tr>
    <tr>
      <th>3</th>
      <td>lifestyle services</td>
      <td>114.577400</td>
      <td>22.492460</td>
    </tr>
    <tr>
      <th>4</th>
      <td>lifestyle services</td>
      <td>114.574730</td>
      <td>22.494576</td>
    </tr>
  </tbody>
</table>
</div>


    
    Data types:



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
      <th>dtype</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>primary_types</th>
      <td>str</td>
    </tr>
    <tr>
      <th>longitude</th>
      <td>float64</td>
    </tr>
    <tr>
      <th>latitude</th>
      <td>float64</td>
    </tr>
  </tbody>
</table>
</div>


Each row represents a Point of Interest (POI), with its category and geographic location.

There is no TAZID column. Unlike `inf.csv`, the POIs aren't directly assigned to the 275 charging zones.

To examine how many POIs are located within each zone, we'd eventually need geographic zone boundaries and a spatial join.

There's another detail worth remembering. The repository documents different coordinate systems for station-level locations (GCJ-02) and POIs (WGS84). We'll need to account for that when mapping or combining them.

#### What types of POIs are included?


```python

# Count POIs by category
poi_categories = (
    poi["primary_types"]
    .value_counts(dropna=False)
    .rename_axis("category")
    .reset_index(name="count")
)

# Calculate percentage of total
poi_categories["percentage"] = (
    poi_categories["count"] / len(poi) * 100
).round(2)

display(poi_categories)

print("Total POI records:", len(poi))
print("Number of categories:", poi["primary_types"].nunique())

print("\nMissing values:")
display(poi.isna().sum())

print("\nDuplicate rows:", poi.duplicated().sum())

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
      <th>category</th>
      <th>count</th>
      <th>percentage</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>lifestyle services</td>
      <td>393428</td>
      <td>55.25</td>
    </tr>
    <tr>
      <th>1</th>
      <td>business and residential</td>
      <td>183383</td>
      <td>25.75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>food and beverage services</td>
      <td>135324</td>
      <td>19.00</td>
    </tr>
  </tbody>
</table>
</div>


    Total POI records: 712135
    Number of categories: 3
    
    Missing values:



    primary_types    0
    longitude        0
    latitude         0
    dtype: int64


    
    Duplicate rows: 12


1. Lifestyle services dominate the POI dataset. They account for more than half of all POI records. However, this doesn't necessarily mean they occupy more geographic area.

2. There are no missing values in the POI file. All 712,135 records contain a category, longitude and latitude.

3. Only 12 rows are exact duplicates. That's a tiny fraction of the dataset.

### Dataset catalog

We finish with a compact catalog that summarizes the dimensions of every CSV file, including `inf_raw.csv`, which we haven't inspected individually.


```python

# Categorize all CSV files
file_groups = {
    "volume.csv": "Charging time series",
    "volume-11kW.csv": "Charging time series",
    "duration.csv": "Charging time series",
    "occupancy.csv": "Charging time series",
    "e_price.csv": "Charging time series",
    "s_price.csv": "Charging time series",
    "weather_airport.csv": "Weather",
    "weather_central.csv": "Weather",
    "inf.csv": "Station information",
    "inf_raw.csv": "Station information",
    "poi.csv": "Points of interest",
    "adj.csv": "Spatial matrices",
    "distance.csv": "Spatial matrices",
}

# Build a catalog of files and their dimensions
records = []

for filename, group in file_groups.items():
    path = DATA_DIR / filename
    df = pd.read_csv(path)

    records.append({
        "file": filename,
        "group": group,
        "rows": df.shape[0],
        "columns": df.shape[1],
        "size_mb": round(path.stat().st_size / 1024**2, 2)
    })

catalog = pd.DataFrame(records)

display(catalog)

print("Total files:", len(catalog))
print("Total size (MB):", catalog["size_mb"].sum().round(2))

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
      <th>file</th>
      <th>group</th>
      <th>rows</th>
      <th>columns</th>
      <th>size_mb</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>volume.csv</td>
      <td>Charging time series</td>
      <td>4344</td>
      <td>276</td>
      <td>15.74</td>
    </tr>
    <tr>
      <th>1</th>
      <td>volume-11kW.csv</td>
      <td>Charging time series</td>
      <td>4344</td>
      <td>276</td>
      <td>18.81</td>
    </tr>
    <tr>
      <th>2</th>
      <td>duration.csv</td>
      <td>Charging time series</td>
      <td>4344</td>
      <td>276</td>
      <td>16.98</td>
    </tr>
    <tr>
      <th>3</th>
      <td>occupancy.csv</td>
      <td>Charging time series</td>
      <td>4344</td>
      <td>276</td>
      <td>5.26</td>
    </tr>
    <tr>
      <th>4</th>
      <td>e_price.csv</td>
      <td>Charging time series</td>
      <td>4344</td>
      <td>276</td>
      <td>11.49</td>
    </tr>
    <tr>
      <th>5</th>
      <td>s_price.csv</td>
      <td>Charging time series</td>
      <td>4344</td>
      <td>276</td>
      <td>14.27</td>
    </tr>
    <tr>
      <th>6</th>
      <td>weather_airport.csv</td>
      <td>Weather</td>
      <td>4344</td>
      <td>7</td>
      <td>0.16</td>
    </tr>
    <tr>
      <th>7</th>
      <td>weather_central.csv</td>
      <td>Weather</td>
      <td>4344</td>
      <td>7</td>
      <td>0.17</td>
    </tr>
    <tr>
      <th>8</th>
      <td>inf.csv</td>
      <td>Station information</td>
      <td>1362</td>
      <td>7</td>
      <td>0.07</td>
    </tr>
    <tr>
      <th>9</th>
      <td>inf_raw.csv</td>
      <td>Station information</td>
      <td>1587</td>
      <td>7</td>
      <td>0.08</td>
    </tr>
    <tr>
      <th>10</th>
      <td>poi.csv</td>
      <td>Points of interest</td>
      <td>712135</td>
      <td>3</td>
      <td>31.13</td>
    </tr>
    <tr>
      <th>11</th>
      <td>adj.csv</td>
      <td>Spatial matrices</td>
      <td>275</td>
      <td>275</td>
      <td>0.15</td>
    </tr>
    <tr>
      <th>12</th>
      <td>distance.csv</td>
      <td>Spatial matrices</td>
      <td>275</td>
      <td>275</td>
      <td>0.86</td>
    </tr>
  </tbody>
</table>
</div>


    Total files: 13
    Total size (MB): 115.17


- `inf.csv` contains 1,362 station records after filtering.

- `inf_raw.csv` contains 1,587 station records associated with the selected 275 zones.

This is different from the 1,682 stations described in the original dataset. The repository's `inf_raw.csv` is not necessarily the complete original raw dataset.
