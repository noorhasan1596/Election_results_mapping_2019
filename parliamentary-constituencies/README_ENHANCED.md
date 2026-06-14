# 🗳️ India Parliamentary Constituencies

## Overview

This directory contains Lok Sabha (Lower House) constituency boundaries for India's parliamentary system. These are the electoral divisions used for national-level elections.

## 📄 Files

### `india_pc_2014.shp` (and companion files)
- **Format**: Shapefile (.shp, .shx, .dbf, .prj, .qpj)
- **Type**: Multi-polygon feature collection
- **Content**: Parliamentary Constituency boundaries
- **Data Year**: 2014 Delimitation
- **File Size**: ~14.36 MB (.shp file)
- **CRS**: WGS 84 (EPSG:4326)

**Required companion files**:
- `.shx` - Shape index
- `.dbf` - Attribute database (283 KB)
- `.prj` - Projection information
- `.qpj` - QGIS projection file

## 📊 Data Details

### Coverage

**Total Constituencies**: 543 Lok Sabha seats

**Breakdown by Region**:
- Union Territories: ~20 seats
- Major states: Varies by population
- Smallest states: Usually 1-2 seats

### Key States (by seats)

| State | Seats | Notes |
|-------|-------|-------|
| **Uttar Pradesh** | 80 | Largest |
| **Maharashtra** | 48 | Second largest |
| **West Bengal** | 42 | Third largest |
| **Bihar** | 40 | |
| **Madhya Pradesh** | 29 | |
| **Karnataka** | 28 | |
| **Tamil Nadu** | 39 | |
| **Rajasthan** | 25 | |
| **Gujarat** | 26 | |
| **Andhra Pradesh** | 25 | |

### Attributes

Each feature includes:
- `CONS_NAME` - Constituency name
- `STATE_NAME` - State/UT name
- `PC_CODE` - Parliamentary Constituency code
- `ST_CODE` - State code
- `AREA` - Area in square units
- Geometry with polygon boundaries

## 🚀 Usage Examples

### Load Parliamentary Data
```python
import geopandas as gpd

# Load PC constituencies
pc = gpd.read_file('india_pc_2014.shp')

print(f"Total constituencies: {len(pc)}")
print(pc.head())

# Get unique states
states = pc['STATE_NAME'].unique()
print(f"States/UTs: {len(states)}")
```

### Analyze by State
```python
import geopandas as gpd

pc = gpd.read_file('india_pc_2014.shp')

# Get constituencies in a state
karnataka_pc = pc[pc['STATE_NAME'] == 'Karnataka']
print(f"PC in Karnataka: {len(karnataka_pc)}")
print(karnataka_pc['CONS_NAME'].tolist())

# Count constituencies by state
pc_counts = pc.groupby('STATE_NAME').size().sort_values(ascending=False)
print(pc_counts)
```

### Create Electoral Maps
```python
import geopandas as gpd
import matplotlib.pyplot as plt

pc = gpd.read_file('india_pc_2014.shp')

# Plot all constituencies
fig, ax = plt.subplots(figsize=(15, 12))
pc.plot(ax=ax, alpha=0.5, edgecolor='k')
plt.title('India - Lok Sabha Constituencies (2014)')
plt.show()

# Plot specific state
maharashtra_pc = pc[pc['STATE_NAME'] == 'Maharashtra']
fig, ax = plt.subplots(figsize=(12, 10))
maharashtra_pc.plot(ax=ax, alpha=0.5, edgecolor='k', color='lightblue')
plt.title('Maharashtra - Lok Sabha Constituencies')
plt.show()
```

### Interactive Constituency Maps
```python
import folium
import geopandas as gpd

pc = gpd.read_file('india_pc_2014.shp')

# Create map
m = folium.Map(location=[23, 82], zoom_start=4)

# Add constituencies with popups
for idx, row in pc.iterrows():
    folium.GeoJson(
        gpd.GeoSeries(row.geometry).__geo_interface__,
        popup=f"{row['CONS_NAME']}<br>{row['STATE_NAME']}"
    ).add_to(m)

m.save('parliamentary_constituencies_map.html')
```

## 📈 Electoral Information

### House of Lok Sabha
- **Total Seats**: 543
- **Directly Elected**: 543
- **Term**: 5 years
- **Last Delimitation**: 2014 (based on 2011 Census)
- **Next Delimitation**: Due after 2031 Census

### Delimitation Commission (2014)
- Based on 2011 Census data
- Redrew boundaries to ensure roughly equal populations
- Allocated seats based on population growth
- Some states gained seats, others lost seats

## 🗳️ Related Electoral Data

### Rajya Sabha (Upper House)
- 245 seats (not geographically divided)
- Elected by state legislatures
- No constituency boundaries

### State Assemblies
- Separate constituency system
- See `../assembly-constituencies/` for Assembly seats
- Multiple Assembly seats per Parliamentary Constituency (typically 7-10)

## 🔄 Format Conversion

```bash
# Convert to GeoJSON
ogr2ogr -f GeoJSON pc_constituencies.geojson india_pc_2014.shp

# Convert to KML
ogr2ogr -f KML pc_constituencies.kml india_pc_2014.shp

# Convert to GeoPackage
ogr2ogr -f GPKG pc_constituencies.gpkg india_pc_2014.shp
```

## 📍 Constituency Levels

**Hierarchy**:
```
Country (India)
├── States/UTs
│   ├── Parliamentary Constituencies (Lok Sabha)
│   │   ├── Assembly Constituencies (multiple per PC)
│   │   │   ├── Districts/Taluks
│   │   │   └── Villages
```

## 📚 Related Data

- **Assembly Constituencies**: `../assembly-constituencies/` - State-level electoral units
- **Districts**: `../Districts/` - Administrative divisions
- **States**: `../States/` - State boundaries
- **ECI Data**: `../eci/PC_Data/` - Election data linked to constituencies

## 🛠️ Technical Details

### File Organization
```
india_pc_2014.shp
india_pc_2014.shx
india_pc_2014.dbf
india_pc_2014.prj
india_pc_2014.qpj
```

### Coordinate Reference System
```
WGS 84 (EPSG:4326)
Latitude/Longitude
Geographic Coordinate System
```

### Data Quality
- Derived from official ECI data and OSM
- Boundaries accurate to constituency level
- Suitable for electoral analysis and visualization
- Meets GIS standards for electoral mapping

## ⚠️ Important Notes

### Data Accuracy
- Boundaries based on 2014 Delimitation Commission
- Approved by Election Commission of India
- Subject to boundary disputes in some areas
- Use for electoral analysis and reference purposes

### Delimitation Changes
- **2014 Boundaries**: Current version (based on 2011 Census)
- **Next Change**: After 2031 Census
- **Previous**: 2008 Delimitation
- **Note**: Boundaries remain same unless new delimitation occurs

### Election Timeline
- **Elections Held**: 2014, 2019, 2024
- **Current Boundaries**: Valid for all three elections
- **Next Boundary Change**: Likely after 2031 Census

### Union Territories
Some UTs have limited representation:
- Puducherry: 1 seat
- Andaman & Nicobar: 1 seat
- Lakshadweep: 1 seat
- Chandigarh: 1 seat
- Ladakh: 1 seat (from 2024)
- Daman & Diu with D&N Haveli: 1 seat

## 📖 License

Creative Commons Attribution-ShareAlike 2.5 India

## 🔗 Data Sources

- Election Commission of India (ECI)
- Delimitation Commission 2014
- OpenStreetMap
- Lok Sabha official data

## 📧 Notes

- Data reflects 2014 Delimitation Commission boundaries
- Valid for elections 2014, 2019, 2024
- Used for Lok Sabha (Lower House) elections only
- Assembly constituencies shown separately in `../assembly-constituencies/`
- For latest electoral data and results, refer to ECI website

---

**Last Updated**: June 2026  
**Data Version**: 2014 Delimitation  
**Total Constituencies**: 543  
**Valid For Elections**: 2014, 2019, 2024  
**Format**: Shapefile (WGS 84)  
**Source**: Election Commission of India
