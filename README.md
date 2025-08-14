# The 24-Hour Cycle: NYC Citi Bike Rides (July-August 2024)

---

## **Introduction**

Data can reveal insights, but it can also create an atmosphere.

A **bike share usage analysis** of this nature reveals patterns in how riders interact with the city's cycling network over time and across locations, painting a picture of its daily pulse and energy.

By understanding these patterns, it is possible to discover **when and where** demand peaks, how long trips last, and how station usage changes throughout the day and week.

This project analyzes **Citi Bike** trip data for New York City during **July-August 2024**, the two full summer months when bike usage typically peaks.

---

## **About the Dataset**

The data comes from **[Citi Bike's official System Data portal](https://citibikenyc.com/system-data)**, which publishes trip history files including ride IDs, bike types, timestamps, station information, geographic coordinates, and more. The data is provided under the **NYC Bike Share Data Use Policy**.

Historical NYC Citi Bike data is available from 2013-2023 and 2025, but this study focuses solely on **2024** to limit processing requirements. For 2024 as a whole, the initial data load contained **~44.3 million rows**, which was filtered down to just **~9.3 million rows** from July and August. This allows for efficient processing in Python and Tableau.

> Tableau Public can only handle 15 million rows of data at most. 

---

## **Project Goals**

- **Identify hourly and daily usage trends** - Detect trip metrics for each time segment, such as number of trips taken, trip distance, trip length, and trip speed.
- **Pinpoint most popular bike stations** - Determine which stations see the most activity at different times of the day/week, with an emphasis on stations where rides begin.
- **Visualize spatial and temporal trends at the same time** - Allow dashboard viewers to explore a density map to see where most rides begin, without having to simply sift through numbers and more traditional charts.

---

## **Table of Contents**

- [Introduction](#introduction)  
- [About the Dataset](#about-the-dataset)  
- [Project Goals](#project-goals)  
- [Tools Used](#tools-used)  
- [Project Files](#project-files)  
- [Data Loading, Preparation, and Cleaning](#data-loading-preparation-and-cleaning)  
- [Step 1: Hourly Usage Trends](#step-1-hourly-usage-trends)  
- [Step 2: Day vs. Hour Heatmap](#step-2-day-vs-hour-heatmap)  
- [Step 3: Key Performance Indicators (KPIs)](#step-3-key-performance-indicators-kpis)  
- [Step 4: Station Density Mapping](#step-4-station-density-mapping)  
- [Tableau Dashboard](#tableau-dashboard)  
- [Conclusion](#conclusion)  
- [Tableau Dashboard Link](#tableau-dashboard-link)

---

## **Tools Used**

- **Python** (via **Jupyter Notebook):** Data loading, cleaning, preliminary visualizations, and analysis
> Libraries imported: `pandas`, `numpy`, `matplotlib`, `seaborn`, `glob`, `os`, `Path`, `FuncFormatter`
- **Tableau:** Interactive dashboard design and final visualizations (including spatial mapping)

---

## **Project Files**

Below is a complete list of files that are included in this repository:

### Jupyter Notebooks
- [`01_citibike_2024_load_data.ipynb`](https://github.com/dylanbarrett-analytics/citi-bike-activity-analysis/blob/main/01_citibike_2024_load_data.ipynb)  
  Loads original data and concatenates all files

- [`02_citibike_2024_data_cleaning_and_preparation.ipynb`](https://github.com/dylanbarrett-analytics/citi-bike-activity-analysis/blob/main/02_citibike_2024_data_cleaning_and_preparation.ipynb)  
  Cleans and prepares the data for analysis 

- [`03_citibike_2024_analysis.ipynb`](https://github.com/dylanbarrett-analytics/citi-bike-activity-analysis/blob/main/03_citibike_2024_analysis.ipynb)  
  Exploratory analysis

### Tableau-Ready CSV files
- [`kpi_summary.csv`](https://github.com/dylanbarrett-analytics/citi-bike-activity-analysis/blob/main/kpi_summary.csv)  
  Main KPI dataset for all day/hour combinations 

- [`start_station_density_map.csv`](https://github.com/dylanbarrett-analytics/citi-bike-activity-analysis/blob/main/start_station_density_map.csv)  
  Every starting point station's trip count for all day/hour combinations (the density map's raw data)

- [`weekday_hour_heatmap.csv`](https://github.com/dylanbarrett-analytics/citi-bike-activity-analysis/blob/main/weekday_hour_heatmap.csv)  
  Heatmap dataset for all day/hour combinations

### Documentation
- [`README.md`](https://github.com/dylanbarrett-analytics/citi-bike-activity-analysis/blob/main/README.md)  
  Project documentation

---

## **Data Loading, Preparation, and Cleaning**

### **Import & Merge**
- **50 CSV files** for 2024 were loaded and merged into a single DataFrame (~44.3 million rows).
- Filtered for only July and August of 2024 to reduce dataset to ~9.3 million rows.

### **Cleaning & Preparation**
- Renamed some columns for readability and dropped irrelevant columns
- Dropped any rows that were missing values
- Calculated trip duration (in minutes) for every row
- Extracted both 'hour of day' and 'day of week' from the datetime columns
  - Converted 'hour of day' integers (0-23) to 12-hour time labels (12AM-11PM)

### **Post-Cleaning Summary**
- **Rows:** 9,295,830
- **Columns:** 16
- **Unique Trips:** 9,295,830
- **Missing Values:** 0
- **Duplicate Rows:** 0
- **Date Range:** July 1, 2024 - August 31, 2024

---

## **Exploratory Analysis**

---

### **Hourly Usage Trends**

**Goal:** Identify any hourly trends where bike trips occur most frequently.

**Key Logic:**
- Grouped all trips by start hour (12AM-11PM) and counted number of trips for every start hour
- Plotted results in bar chart

**Results:**
- Major peak of activity during PM rush hour (5-6PM)
- Minor peak of activity during AM rush hour (8-9AM)

---

### **Day vs. Hour Heatmap**

**Goal:** Visualize usage patterns across every day/hour combination (e.g., Monday at 8am).

**Key Logic:**
- Grouped by day and hour, and counted number of trips for every day/hour combination
- Pivoted data to make it heatmap-friendly
- Plotted results in heatmap

**Results:**
- Weekdays: Sharp peaks of activity during both AM and PM rush hour blocks (mostly commuting trips)
- Weekends: Consistent, higher activity from ~9AM-8PM (mostly recreational trips)

---

### **Key Performance Indicators (KPIs)**

**Goal:** Summarize core metrics.

**Key Logic:**
- For every day/hour combination, calculated:
  - total trips
  - most common start station
  - most common end station
  - loop trip rate (% of trips starting at ending at same station)
    - excluded from final dashboard since percentages were too low to have any significant impact on overall story
  - average trip duration (in minutes)
  - average trip distance (in miles, via Haversine formula)
  - average speed (mph)

---

### **Start Station Density Mapping**

**Goal:** Identify high-demand bike stations and usage clusters.

**Key Logic:**
- Counted trips starting from each bike station
- Mapped these station locations using their coordinates

**Results:**
- Highest density was consistently in Midtown Manhattan and near major transit hubs
- Clusters occur along waterfront and park areas

---

## **Tableau Dashboard**

The final dashboard visualizes Citi Bike usage across New York City, based on July-August 2024 data.
- **Parameter dropdowns:** Breaks down the data by **day of the week** and **hour of the day**
  - Viewers can filter the KPI summary by any day/hour combination (i.e., time segment)
  - The day of the week parameter also filters the density map.
- **Key Performance Indicators (KPIs):**
  - The primary metrics are the bike station where most trips begin and total trips taken (during the selected time segment).
  - The secondary metrics are average trip duration, average trip distance, average speed (during trip), and the bike station where most trips end.
- **Heatmap:** At a glance, reveals peak cycling hours across the week
- **Density Map:** Essentially an animated time-lapse of where bike trips start throughout the day, using the Tableau pages feature
  - This map section has its own "hour selector" since it is not possible to connect this page viewer to the KPIs; therefore, the KPI cards do not update when the "time-lapse" occurs. This may be a bit confusing, so that's why the map is isolated on the right edge of the dashboard with two annotations as guides.

> <sub> I've been wanting to use the full sunrise-sunset Tableau palette because of the atmosphere it can create. It also has a subtle hint of BG3's Underdark... a personal favorite.

---

## **Conclusion**

Citi Bike usage in NYC is driven heavily by time-of-day and day-of-week patterns, with the most activity occurring during commuting hours and weekend afternoon leisure hours around Midtown Manhattan and major transit hubs. There is nothing too unpredictable or "shocking" about these main headline peak-demand insights. However, the opposing perspective is that these temporal rhythms also reveal when and where demand is naturally low (especially when looking at the map).

Analysts do not make the final business decisions, as we only provide the decision-makers with the necessary evidence and scenarios for said decisions. In this case, understanding both the peaks and valleys allows leadership to evaluate not only where activity is concentrated, but also where untapped opportunities may exist.

---

## **Tableau Dashboard Link**

[The 24-Hour Cycle: NYC Citi Bike Rides (July-August 2024) (Tableau Public)](https://public.tableau.com/app/profile/dylan.barrett1539/viz/The24-HourCycleNYCCitiBikeRidesJulyAugust2024/Dashboard)
