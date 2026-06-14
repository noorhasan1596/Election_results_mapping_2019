# 🗺️ India Districts

## Overview

This directory contains comprehensive district-level geospatial data for India. Districts represent the main administrative divisions within states and are subdivided into taluks/tehsils.

## 📁 Subdirectories

### `Census_2001/`
District boundaries and data from the 2001 Census of India
- 620+ districts
- Historical reference data
- Population statistics from 2001

### `Census_2011/`
District boundaries and data from the 2011 Census of India
- 750+ districts (updated count)
- Current reference dataset
- Latest population statistics
- Includes post-2011 administrative changes

## 📄 Data Files

Each subdirectory contains complete shapefiles with:
- `.shp` - Main geometry file
- `.shx` - Shape index
- `.dbf` - Attribute database with population and area data
- `.prj` - Projection information
- Optional metadata and documentation files

## 📊 Data Details

### Coverage

**2001 Census Data**:
- Base year: 2001
- Districts: 620+
- Historical boundaries
- Population data from Census 2001

**2011 Census Data**:
- Base year: 2011
- Districts: 750+ (includes new district formations)
- Current administrative boundaries
- Population data from Census 2011
- Updated to reflect recent administrative changes

### Attributes

Common attributes in each feature:
- `DISTRICT` - District name
- `STATE` / `ST_NM` - State/UT name
- `ST_CEN_CD` - State census code
- `DT_CEN_CD` - District census code
- `POPULATION` - Population from census
- `AREA` - District area
- `DENSITY` - Population density
- Geometry with polygon boundaries

## 🚀 Usage Examples

### Load District Data
```python
import geopandas as gpd

# Load 2011 district boundaries
districts = gpd.read_file('Census_2011/Districts.shp')

print(f"Total districts: {len(districts)}")
print(districts.head())

# Get unique states
states = districts['ST_NM'].unique()
print(f"States/UTs: {len(states)}")
```

### Analyze by State
```python
import geopandas as gpd

districts = gpd.read_file('Census_2011/Districts.shp')

# Get districts in Karnataka
karnataka_districts = districts[districts['ST_NM'] == 'Karnataka']
print(f"Districts in Karnataka: {len(karnataka_districts)}")

# Get districts in Ladakh UT
ladakh_districts = districts[districts['ST_NM'] == 'Ladakh']
print(f"Ladakh districts: {ladakh_districts['DISTRICT'].tolist()}")
```

### Population Analysis
```python
import geopandas as gpd

districts = gpd.read_file('Census_2011/Districts.shp')

# Top 10 most populous districts
top_districts = districts.nlargest(10, 'POPULATION')[['DISTRICT', 'ST_NM', 'POPULATION']]
print(top_districts)

# Calculate state-wise population
state_pop = districts.groupby('ST_NM')['POPULATION'].sum().sort_values(ascending=False)
print(state_pop)

# Districts with population > 1 million
large_districts = districts[districts['POPULATION'] > 1000000]
print(f"Districts with 1M+ population: {len(large_districts)}")
```

### Create Choropleth Maps
```python
import geopandas as gpd
import matplotlib.pyplot as plt

districts = gpd.read_file('Census_2011/Districts.shp')

# Population density map
fig, ax = plt.subplots(figsize=(15, 12))
districts.plot(
    ax=ax,
    column='DENSITY',
    cmap='YlOrRd',
    legend=True,
    legend_kwds={'label': "Population Density", 'shrink': 0.8}
)
plt.title('India - District Population Density (2011)')
plt.show()
```

### Interactive District Maps
```python
import folium
import geopandas as gpd

districts = gpd.read_file('Census_2011/Districts.shp')

# Create map
m = folium.Map(location=[23, 82], zoom_start=4)

# Add districts with popups
for idx, row in districts.iterrows():
    folium.GeoJson(
        gpd.GeoSeries(row.geometry).__geo_interface__,
        popup=f"{row['DISTRICT']}, {row['ST_NM']}<br>Pop: {row['POPULATION']:,}"
    ).add_to(m)

m.save('districts_map.html')
```

## 📈 Comparison: 2001 vs 2011

### Key Differences

| Aspect | 2001 | 2011 |
|--------|------|------|
| **Districts** | 620+ | 750+ |
| **New Districts** | - | 130+ newly formed |
| **Population Base** | 2001 Census | 2011 Census |
| **Administrative Changes** | Baseline | Includes recent changes |
| **Recommended Use** | Historical analysis | Current analysis |

### District Formation Trends
- Multiple new districts created between 2001-2011
- Bifurcation of large districts
- Administrative reorganization
- **2019 Addition**: Ladakh UT created from J&K

## 🔄 Format Conversion

```bash
# Convert 2011 districts to GeoJSON
ogr2ogr -f GeoJSON districts_2011.geojson Census_2011/Districts.shp

# Convert to KML
ogr2ogr -f KML districts_2011.kml Census_2011/Districts.shp

# Convert to GeoPackage
ogr2ogr -f GPKG districts_2011.gpkg Census_2011/Districts.shp
```

## 📍 Sub-Administrative Divisions

Districts are subdivided into:
- **Taluks** / **Tehsils** - Revenue subdivision
- **Blocks** - Development blocks
- **Mandals** - Administrative units (varies by state)
- **Villages** - Lowest administrative unit

(Note: Taluk/Mandal-level data not included in this directory, check `docs/` for references)

## 📚 Related Data

- **States**: `../States/` - State-level boundaries (aggregated)
- **Assembly Constituencies**: `../assembly-constituencies/` - Electoral divisions
- **Parliamentary Constituencies**: `../parliamentary-constituencies/` - National electoral units
- **Country**: `../Country/` - National boundary

## 🛠️ Technical Details

### File Organization
```
Census_2001/
  ├── Districts_2001.shp
  ├── Districts_2001.shx
  ├── Districts_2001.dbf
  ├── Districts_2001.prj
  └── README.md

Census_2011/
  ├── Districts_2011.shp
  ├── Districts_2011.shx
  ├── Districts_2011.dbf
  ├── Districts_2011.prj
  └── README.md
```

### Coordinate Reference System
```
WGS 84 (EPSG:4326)
Latitude/Longitude
Geographic Coordinate System
```

## ⚠️ Important Notes

### Data Accuracy
- Boundaries derived from multiple sources (topo maps, OSM, official data)
- Accuracy varies by district
- Some areas have disputed boundaries
- Not suitable for legal boundary demarcation

### Census Updates
- 2001 Census data fixed to that year
- 2011 Census data reflects 2011 boundaries and population
- Note: 2021 Census results released but boundaries not updated

### Disputed Territories
- Jammu & Kashmir districts: Now in J&K UT (post-2019)
- Ladakh districts: Leh and Kargil (post-2019)
- Arunachal Pradesh: Disputed with China
- All shown with Indian administrative definitions

### Administrative Changes
- Check 2019 reorganization for latest changes
- New UT of Ladakh: Contains Leh and Kargil
- Jammu & Kashmir UT: Remaining J&K districts
- Use post-2019 data for current analysis

## 📖 License

Creative Commons Attribution-ShareAlike 2.5 India

## 🔗 Data Sources

- Census of India (2001, 2011)
- Administrative Atlas of India
- Survey of India
- Taluk Boundaries from Bhuvan Geoserver
- OpenStreetMap community contributions

## 📧 Notes

- Data available for both 2001 and 2011 Census years
- Use 2011 data for most current analysis
- Post-2019 updates incorporated for new UTs
- Boundary accuracy adequate for thematic mapping
- For precise boundary demarcation, use official Survey of India data

---

**Last Updated**: June 2026  
**Status**: Updated for 2019 administrative changes  
**Coverage**: 750+ districts across 36 states/UTs  
**Primary Format**: Shapefile (WGS 84)  
**Census Years**: 2001, 2011
