# 🏛️ India Assembly Constituencies

## Overview

This directory contains State Assembly/Legislative Assembly constituency boundaries for India's state-level electoral system. These divisions are used for state elections and assembly representations.

## 📄 Files

### `India_AC.shp` (and companion files)
- **Format**: Shapefile (.shp, .shx, .dbf, .prj)
- **Type**: Multi-polygon feature collection
- **Content**: State Assembly Constituency boundaries
- **File Size**: ~6.2 MB (.shp file)
- **CRS**: WGS 84 (EPSG:4326)

**Required companion files**:
- `.shx` - Shape index (~33.5 KB)
- `.dbf` - Attribute database (~5.7 MB)
- `.prj` - Projection information
- `.shp.xml` - Metadata (~11 KB)

## 📊 Data Details

### Coverage

**Total Assembly Constituencies**: ~4,000+

**Breakdown**:
- State Assemblies (28 states)
- Union Territory Assemblies (select UTs)
- Territorial divisions: 4,000+ constituencies

### Major States (by assembly seats)

| State | Assembly Seats |
|-------|-----------------|
| **Uttar Pradesh** | 403 |
| **Maharashtra** | 288 |
| **West Bengal** | 294 |
| **Karnataka** | 224 |
| **Andhra Pradesh** | 295 |
| **Tamil Nadu** | 234 |
| **Gujarat** | 182 |
| **Rajasthan** | 200 |
| **Madhya Pradesh** | 230 |
| **Odisha** | 147 |

### Attributes

Each feature includes:
- `AC_NAME` - Assembly Constituency name
- `STATE` / `ST_NM` - State/UT name
- `AC_CODE` - Constituency code
- `ST_CODE` - State code
- `PC_NAME` - Associated Parliamentary Constituency
- `POPULATION` - Census population
- `AREA` - Area in square units
- Geometry with polygon boundaries

## 🚀 Usage Examples

### Load Assembly Constituency Data
```python
import geopandas as gpd

# Load AC constituencies
ac = gpd.read_file('India_AC.shp')

print(f"Total constituencies: {len(ac)}")
print(ac.head())

# Get unique states
states = ac['ST_NM'].unique()
print(f"States/UTs: {len(states)}")
```

### Analyze by State
```python
import geopandas as gpd

ac = gpd.read_file('India_AC.shp')

# Get constituencies in a state
maharashtra_ac = ac[ac['ST_NM'] == 'Maharashtra']
print(f"Assembly seats in Maharashtra: {len(maharashtra_ac)}")

# Count constituencies by state
ac_counts = ac.groupby('ST_NM').size().sort_values(ascending=False)
print(ac_counts.head(10))

# Get constituencies with largest population
top_ac = ac.nlargest(10, 'POPULATION')[['AC_NAME', 'ST_NM', 'POPULATION']]
print(top_ac)
```

### Create Electoral Maps
```python
import geopandas as gpd
import matplotlib.pyplot as plt

ac = gpd.read_file('India_AC.shp')

# Plot all constituencies
fig, ax = plt.subplots(figsize=(15, 12))
ac.plot(ax=ax, alpha=0.5, edgecolor='k')
plt.title('India - Assembly Constituencies')
plt.show()

# Plot constituencies for specific state
karnataka_ac = ac[ac['ST_NM'] == 'Karnataka']
fig, ax = plt.subplots(figsize=(12, 10))
karnataka_ac.plot(ax=ax, alpha=0.5, edgecolor='k', color='lightblue')
plt.title('Karnataka - Assembly Constituencies')
plt.show()
```

### Interactive Assembly Constituency Maps
```python
import folium
import geopandas as gpd

ac = gpd.read_file('India_AC.shp')

# Create map
m = folium.Map(location=[23, 82], zoom_start=5)

# Add constituencies with popups
for idx, row in ac.iterrows():
    folium.GeoJson(
        gpd.GeoSeries(row.geometry).__geo_interface__,
        popup=f"{row['AC_NAME']}<br>{row['ST_NM']}"
    ).add_to(m)

m.save('assembly_constituencies_map.html')
```

## 🏦 Assembly Information

### State Legislatures
- **Total Assembly Seats**: ~4,000+
- **Term**: 5 years (usually)
- **Election**: State-level democratic process
- **Representation**: Based on state population and geography

### Delimitation Notes

**Important Limitations** (as noted in original README):

1. **Pre-delimitation Boundaries**: 
   - Jammu and Kashmir
   - Jharkhand
   - Assam
   - Manipur
   - Nagaland
   - Arunachal Pradesh
   - ⚠️ May not reflect current constituency divisions

2. **Data Accuracy Issues**:
   - Some shift in data (geographic misalignment possible)
   - Constituency names: Some may be incorrect or missing
   - Telangana: Some constituencies still marked as Andhra Pradesh

3. **Union Territories Without Assemblies**:
   - Andaman and Nicobar Islands
   - Chandigarh
   - Dadra and Nagar Haveli
   - Daman and Diu
   - Lakshadweep
   - Ladakh (post-2019)

## 📊 Hierarchical Structure

**Electoral Hierarchy**:
```
Country (India)
├── States/UTs
│   ├── State Assembly (4,000+ seats total)
│   │   └── Divided into Assembly Constituencies
│   │
│   └── Parliamentary Constituencies (543 total)
│       └── Typically comprise 7-10 Assembly Constituencies
```

### Multiple ACs per PC
- One Lok Sabha seat typically covers 7-10 Assembly seats
- State assemblies are more granular than national parliament
- Better representation at state level

## 🔄 Format Conversion

```bash
# Convert to GeoJSON
ogr2ogr -f GeoJSON ac_constituencies.geojson India_AC.shp

# Convert to KML
ogr2ogr -f KML ac_constituencies.kml India_AC.shp

# Convert to GeoPackage
ogr2ogr -f GPKG ac_constituencies.gpkg India_AC.shp
```

## 📚 Related Data

- **Parliamentary Constituencies**: `../parliamentary-constituencies/` - National-level divisions
- **Districts**: `../Districts/` - Administrative divisions
- **States**: `../States/` - State boundaries
- **ECI Data**: `../eci/AC_Data/` - Election data linked to AC

## 🛠️ Technical Details

### File Organization
```
India_AC.shp
India_AC.shx
India_AC.dbf
India_AC.prj
India_AC.shp.xml
```

### Coordinate Reference System
```
WGS 84 (EPSG:4326)
Latitude/Longitude
Geographic Coordinate System
```

### Data Quality Notes
- Scraped from ECI's Polling Station Locations Website
- Aggregated from point data to constituency boundaries
- Subject to interpretation and accuracy limitations
- Use for reference and visualization

## ⚠️ Important Notes

### Known Issues (from original README)

1. **Pre-Delimitation States**: 
   - J&K, Jharkhand, Assam, Manipur, Nagaland, Arunachal Pradesh
   - Boundaries may be outdated
   - Check state-specific sources for updates

2. **Data Shifts**: 
   - Geographic misalignments possible
   - Some boundaries may not be perfectly accurate
   - Suitable for thematic mapping, not for precise analysis

3. **Telangana State**: 
   - Separated from Andhra Pradesh in 2014
   - Some data may still show old boundaries
   - Check separately for Telangana constituencies

4. **Missing/Incorrect Names**: 
   - Some AC names may be incorrect
   - Constituency name spellings may vary
   - Refer to ECI for authoritative names

### Union Territories
Some UTs do not have state assemblies (no constituencies):
- Andaman & Nicobar
- Chandigarh
- Dadra & N.H.
- Daman & Diu
- Lakshadweep
- Ladakh

## 📖 License

Creative Commons Attribution 2.5 India

## 🔗 Data Sources

- Election Commission of India (ECI)
- ECI Polling Station Locations Website
- OpenStreetMap contributions
- State government records

## 📧 Notes

- Data compiled from ECI sources
- Subject to updates and corrections
- Use current ECI website for latest official data
- Known limitations noted above
- Submit corrections via GitHub issues

---

**Last Updated**: June 2026  
**Total Constituencies**: 4,000+  
**States Covered**: 28 states + select UTs  
**Known Issues**: See Important Notes section  
**Format**: Shapefile (WGS 84)  
**Source**: Election Commission of India
