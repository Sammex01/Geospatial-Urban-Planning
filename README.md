# 🌍 Geospatial Urban Planning: Strava Route Analysis

### 🎯 Objective
City planners often lack dynamic data on how pedestrians and cyclists actually navigate urban infrastructure. The goal of this project was to build a proof-of-concept geospatial data pipeline that extracts active transport routes and generates interactive heatmaps, enabling data-driven decisions for new bike lanes and pedestrian corridors.

### 🛠️ Methodology & Technical Stack
This project required navigating complex, token-based API authentication and rendering raw GPS coordinates into an interactive visual format. 

* **OAuth2 Authentication:** Engineered a complete OAuth2 authorization flow (generating authorization URLs, capturing callback codes, and exchanging tokens) to securely access the Strava v3 API.
* **Data Extraction & Parsing:** Pulled raw activity data and decoded Strava's proprietary `polyline` strings into usable Latitude/Longitude coordinate pairs using Python.
* **Synthetic Modeling:** To bypass Strava Metro's enterprise data locks for global public data, I utilized `NumPy` to generate a statistically realistic synthetic dataset (5,000+ coordinates) simulating high-density traffic clusters in a major metropolitan area.
* **Geospatial Visualization:** Integrated the `Folium` library to render an interactive, HTML-based geographical heatmap, transforming scattered coordinate pings into identifiable high-traffic corridors.

```python
# Decoding API Polylines into GPS Coordinates
import polyline

# Applying the decoder to the API response
df_activities['Coordinates'] = df_activities['Polyline'].apply(polyline.decode)

# Injecting coordinates into the Folium HeatMap layer
heat_data = [[row['Latitude'], row['Longitude']] for index, row in df_routes.iterrows()]
HeatMap(heat_data, radius=25, blur=15).add_to(city_map)
```
View map here: [city_infrastructure_heatmap.html](city_infrastructure_heatmap2.html)

### 💡 Key Takeaways

* API Security: Successfully navigating OAuth2 scopes and token exchanges is critical for accessing modern, user-permissioned datasets.

* Data Transformation: Converting encoded spatial data (polylines) into structured dataframes is a necessary intermediate step before any geographical analysis can occur.

* Visual Impact: Interactive, web-based maps (.html) provide significantly more value to non-technical stakeholders (like urban planners) than static charts or raw coordinate tables.