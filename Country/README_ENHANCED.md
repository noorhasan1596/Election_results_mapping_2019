# 🌍 India Country Boundaries

## Overview

This directory contains the national-level geospatial data for India, including the country's boundaries and territorial extent.

## 📄 Files

### `india-osm.geojson`
- **Format**: GeoJSON
- **Type**: Single feature polygon
- **Content**: India's land area including disputed territories
- **Source**: OpenStreetMap (OSM) + Overpass queries
- **Size**: ~6.6 MB
- **CRS**: WGS 84 (EPSG:4326)

## 📊 Data Details

### Coverage
- National boundary of India
- Includes disputed territories claimed by India
- Union Territories and States combined
- Land area extent only (no water boundaries)

### Data Processing
The data was created through the following process:

1. **Export India boundary** from [Wambachers OSM Boundaries](https://wambachers-osm.website/boundaries/)
2. **Export disputed areas** claimed by India from [Overpass API](http://overpass-turbo.eu/)
3. **Union operation** in QGIS to combine layers
4. **Merge features** using QGIS digitizing tools
5. **Remove holes** using Delete Ring tool for clean geometry

## 🚀 Usage Examples

### Load and Visualize
```python
import geopandas as gpd
import matplotlib.pyplot as plt

# Load India boundary
india = gpd.read_file('india-osm.geojson')

# Display info
print(india.info())
print(f"Area: {india.geometry.area.values[0]:.2f} square degrees")

# Plot
fig, ax = plt.subplots(figsize=(12, 10))
india.plot(ax=ax, alpha=0.5, edgecolor='k')
plt.title('India - National Boundary')
plt.show()
```

### Create Interactive Map
```python
import folium
import geopandas as gpd

india = gpd.read_file('india-osm.geojson')

# Create map
m = folium.Map(location=[20, 78], zoom_start=4)

# Add boundary
folium.GeoJson(india.to_json()).add_to(m)

m.save('india_boundary_map.html')
```

### Get Bounding Box
```python
import geopandas as gpd

india = gpd.read_file('india-osm.geojson')

# Get bounds
bounds = india.total_bounds
print(f"Min Latitude: {bounds[1]:.2f}")
print(f"Max Latitude: {bounds[3]:.2f}")
print(f"Min Longitude: {bounds[0]:.2f}")
print(f"Max Longitude: {bounds[2]:.2f}")
```

## 📍 Geographic Extent

| Metric | Value |
|--------|-------|
| **Northernmost Point** | Kashmir (Siachen) ~37°N |
| **Southernmost Point** | Kanyakumari ~8°N |
| **Easternmost Point** | Arunachal Pradesh ~97°E |
| **Westernmost Point** | Gujarat ~68°E |
| **Total Area** | ~3.28 million km² |

## ⚠️ Important Notes

### Data Accuracy
- Boundaries derived from OSM (community-sourced)
- Disputed territories included as per Indian claims
- May not match official Survey of India boundaries exactly
- Use for reference and visualization purposes

### Disputed Territories
This dataset includes areas claimed by India but also claimed by neighboring countries:
- **Jammu & Kashmir**: Partially disputed with Pakistan
- **Ladakh**: Includes areas claimed by China (Aksai Chin)
- **Arunachal Pradesh**: Disputed with China (South Tibet claim)
- **Siachen Glacier**: Disputed with Pakistan

### Usage Considerations
- Suitable for national-level mapping and analysis
- Not recommended for boundary demarcation or official purposes
- Use official Survey of India data for legal/administrative decisions
- Cross-reference with other authoritative sources

## 🔄 Format Conversion

```bash
# Convert to Shapefile
ogr2ogr -f "ESRI Shapefile" india_boundary.shp india-osm.geojson

# Convert to KML
ogr2ogr -f KML india_boundary.kml india-osm.geojson

# Convert to GeoPackage
ogr2ogr -f GPKG india_boundary.gpkg india-osm.geojson
```

## 📚 Related Data

For more detailed geographic data, see:
- **States**: `../States/` - State-level boundaries
- **Districts**: `../Districts/` - District-level boundaries
- **Parliamentary Constituencies**: `../parliamentary-constituencies/` - Electoral divisions

## 📖 License

Creative Commons Attribution-ShareAlike 2.5 India

## 🔗 Data Sources

- **OSM Boundaries**: https://wambachers-osm.website/boundaries/
- **Overpass API**: https://overpass-api.de/
- **OpenStreetMap**: https://www.openstreetmap.org/

## 📧 Notes

- Data derived from OpenStreetMap community contributions
- Post-processed in QGIS for accuracy
- Updated periodically to reflect administrative changes
- For questions or corrections, open GitHub issues

---

**Last Updated**: June 2026  
**Data Version**: Latest OSM snapshot  
**Format**: GeoJSON (single feature)
