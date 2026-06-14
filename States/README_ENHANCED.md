# 📍 India States & Union Territories

## Overview

This directory contains state-level administrative boundaries for all 28 states and 8 Union Territories of India.

## 📄 Files

### `Admin2.shp` (and companion files)
- **Format**: Shapefile (.shp, .shx, .dbf, .prj, .cpg)
- **Type**: Multi-polygon feature collection
- **Content**: State and Union Territory boundaries
- **File Size**: ~5.7 MB (.shp file)
- **CRS**: WGS 84 (EPSG:4326)
- **Coordinate System**: Latitude/Longitude decimal degrees

**Required companion files**:
- `.shx` - Shape index file
- `.dbf` - Attribute database
- `.prj` - Projection information
- `.cpg` - Code page for attributes

## 📊 Data Details

### Coverage

**28 States**:
1. Andhra Pradesh
2. Arunachal Pradesh
3. Assam
4. Bihar
5. Chhattisgarh
6. Goa
7. Gujarat
8. Haryana
9. Himachal Pradesh
10. Jharkhand
11. Karnataka
12. Kerala
13. Madhya Pradesh
14. Maharashtra
15. Manipur
16. Meghalaya
17. Mizoram
18. Nagaland
19. Odisha
20. Punjab
21. Rajasthan
22. Sikkim
23. Tamil Nadu
24. Telangana
25. Tripura
26. Uttar Pradesh
27. Uttarakhand
28. West Bengal

**8 Union Territories** (including post-2019 reorganization):
1. **Andaman and Nicobar Islands**
2. **Chandigarh**
3. **Dadra and Nagar Haveli and Daman and Diu**
4. **Lakshadweep**
5. **Delhi** (National Capital Territory)
6. **Puducherry**
7. **Jammu and Kashmir** (UT, post-August 5, 2019)
8. **Ladakh** (UT, post-August 5, 2019)

### Attributes

Each feature includes:
- `NAME_1` - State/UT name
- `ADM0_A3` - Country code (IND)
- `ADM1_CODE` - State/UT code
- `SHAPE_AREA` - Area in degrees
- `SHAPE_LEN` - Perimeter in degrees
- Geometry column with polygon boundaries

## 🚀 Usage Examples

### Load and Explore
```python
import geopandas as gpd

# Load state boundaries
states = gpd.read_file('Admin2.shp')

# View basic information
print(states.head())
print(f"Total states/UTs: {len(states)}")

# List all state names
print(states['NAME_1'].unique())
```

### Filter Specific State
```python
import geopandas as gpd

states = gpd.read_file('Admin2.shp')

# Get Karnataka
karnataka = states[states['NAME_1'] == 'Karnataka']

# Get Ladakh UT
ladakh = states[states['NAME_1'] == 'Ladakh']

# Get all Union Territories
uts = states[states['ADM0_A3'].isin(['UT', 'Union Territory'])]
```

### Create State Maps
```python
import geopandas as gpd
import matplotlib.pyplot as plt

states = gpd.read_file('Admin2.shp')

# Map all states
fig, ax = plt.subplots(figsize=(15, 12))
states.plot(ax=ax, alpha=0.5, edgecolor='k', column='NAME_1', legend=True)
plt.title('India - States and Union Territories')
plt.show()

# Map individual state
maharashtra = states[states['NAME_1'] == 'Maharashtra']
fig, ax = plt.subplots(figsize=(10, 8))
maharashtra.plot(ax=ax, color='lightblue', edgecolor='black')
plt.title('Maharashtra')
plt.show()
```

### Interactive Web Map
```python
import folium
import geopandas as gpd

states = gpd.read_file('Admin2.shp')

# Create map
m = folium.Map(location=[23, 82], zoom_start=4)

# Add state boundaries with pop-ups
for idx, row in states.iterrows():
    folium.GeoJson(
        gpd.GeoSeries(row.geometry).__geo_interface__,
        popup=row['NAME_1']
    ).add_to(m)

m.save('states_map.html')
```

## 📈 Statistical Analysis

```python
import geopandas as gpd

states = gpd.read_file('Admin2.shp')

# Calculate area in km² (approximate)
states['area_km2'] = states.geometry.area * (111320**2)

# Sort by area
largest_states = states.nlargest(5, 'area_km2')[['NAME_1', 'area_km2']]
print(largest_states)

# Count features
print(f"Total features: {len(states)}")
```

## 🔄 Format Conversion

```bash
# Convert to GeoJSON
ogr2ogr -f GeoJSON states.geojson Admin2.shp

# Convert to KML
ogr2ogr -f KML states.kml Admin2.shp

# Convert to GeoPackage
ogr2ogr -f GPKG states.gpkg Admin2.shp
```

## 📍 Post-2019 Administrative Changes

### Ladakh Union Territory (August 5, 2019)
- Separated from Jammu & Kashmir
- Comprises: Leh and Kargil districts
- Status: Union Territory (directly governed by Central Government)

### Jammu & Kashmir Union Territory (August 5, 2019)
- Previously: Full state
- Now: Union Territory
- Status: Reduced in area after Ladakh separation

**Updated data reflects these changes**

## 📚 Related Data

For more detailed geographic data, see:
- **Districts**: `../Districts/` - Fine-grained state divisions
- **Country**: `../Country/` - National boundary
- **Constituencies**: Electoral divisions by state

## 🛠️ Technical Details

### File Management
Always keep all companion files together in the same directory:
```
Admin2.shp      (main file)
Admin2.shx      (index file)
Admin2.dbf      (attribute database)
Admin2.prj      (projection)
Admin2.cpg      (code page)
```

### Coordinate Reference System
```
WGS 84 (EPSG:4326)
Latitude/Longitude
Geographic Coordinate System
```

## ⚠️ Important Notes

### Data Accuracy
- Boundaries based on administrative definitions
- Some disputed boundaries included as per Indian claims
- Not suitable for boundary demarcation disputes
- Use official Survey of India data for legal purposes

### Disputed Areas
- Jammu & Kashmir: Disputed with Pakistan
- Ladakh: Includes areas claimed by China
- Arunachal Pradesh: Disputed with China
- All included with Indian administrative divisions

### Limitations
- Pre-delimitation data for some states
- May not reflect latest boundary changes
- Subject to interpretation of disputed territories
- Use for reference and analysis only

## 📖 License

Creative Commons Attribution-ShareAlike 2.5 India

## 🔗 Data Sources

- OpenStreetMap
- Indian Census data
- Administrative Atlas of India
- Survey of India
- Community contributions

## 📧 Notes

- Data updated to reflect 2019 administrative reorganization
- Ladakh now included as separate Union Territory
- Boundary accuracy varies by source
- For official purposes, refer to Survey of India

---

**Last Updated**: June 2026  
**Status**: Updated post-2019 reorganization  
**Coverage**: 28 States + 8 Union Territories  
**Format**: Shapefile (WGS 84)
