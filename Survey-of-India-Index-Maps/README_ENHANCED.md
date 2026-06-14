# 🗺️ Survey of India Index Maps

## Overview

This directory contains historical Survey of India index maps and coordinate grid references. These are reference maps used for geographic indexing, mapping reference systems, and historical map sheet organization.

## 📁 Subdirectories

### `Boundaries/`
**Survey of India Boundaries and Reference Maps**
- Historical boundary maps
- Administrative boundary index
- Geographic reference sheets
- Grid system reference maps
- Index sheet organization

### `OSM_UTM_WGS84/`
**OpenStreetMap with UTM and WGS84 Reference**
- Modern OSM data with coordinate grids
- Universal Transverse Mercator (UTM) grid overlays
- WGS 84 coordinate system reference
- Modern coordinate reference system maps
- Conversion and reference maps

### `OldSystem_Everest_Polyconic/`
**Historical Survey System - Everest and Polyconic Projection**
- Legacy Survey of India coordinate system
- Everest datum reference
- Polyconic projection grids
- Historical map sheet reference
- Pre-modern geodetic system
- Historical boundary interpretations

## 📄 Files

### `Topo2OSM.pdf`
- **Type**: PDF documentation
- **Size**: ~671 KB
- **Content**: Topographic to OpenStreetMap conversion guide
- **Purpose**: Reference for converting between coordinate systems and map sources

## 📊 Coordinate Systems Explained

### Modern Systems

#### WGS 84 (Global Standard)
- **System**: Geographic (Latitude/Longitude)
- **Datum**: World Geodetic System 1984
- **EPSG Code**: 4326
- **Used By**: GPS, Google Maps, OSM, modern GIS
- **Accuracy**: Global consistency

#### UTM (Universal Transverse Mercator)
- **System**: Projected (Easting/Northing)
- **Grid**: 60 zones covering Earth
- **Datum**: WGS 84
- **Zone for India**: Zones 43-45 (primarily)
- **Advantages**: Local accuracy, minimal distortion
- **Used By**: Military, professional surveying

### Historical Systems

#### Everest Datum
- **Historical**: Used by Survey of India until recently
- **Named After**: George Everest (Surveyor General)
- **Coverage**: Indian subcontinent
- **Accuracy**: Excellent for India region
- **Legacy**: Many historical maps use this

#### Polyconic Projection
- **Type**: Hybrid projection
- **Characteristics**: Multiple cone projections
- **Historical Use**: Survey of India historical maps
- **Properties**: Minimal distortion along central meridian
- **Modern**: Largely replaced by UTM and Web Mercator

## 🗂️ Directory Structure

```
Survey-of-India-Index-Maps/
├── Boundaries/
│   ├── Historical boundary references
│   ├── Index sheet grids
│   ├── Administrative divisions
│   └── Reference maps
│
├── OSM_UTM_WGS84/
│   ├── Modern coordinate grid references
│   ├── UTM zone maps
│   ├── WGS 84 index sheets
│   └── Conversion references
│
├── OldSystem_Everest_Polyconic/
│   ├── Historical Survey sheet index
│   ├── Everest datum reference
│   ├── Polyconic projection grids
│   └── Legacy coordinate systems
│
├── Topo2OSM.pdf
│   └── Conversion guide documentation
│
└── README.md
    └── Original documentation
```

## 🚀 Usage Examples

### Working with Modern Coordinate Systems (WGS84/UTM)

```python
import geopandas as gpd
from pyproj import Transformer

# Transform from WGS 84 to UTM Zone 43N
transformer = Transformer.from_proj(
    'EPSG:4326',      # WGS 84
    'EPSG:32643',     # UTM Zone 43N
    always_xy=True
)

# Convert coordinates
lon, lat = 77.5, 28.5  # Delhi approximate
easting, northing = transformer.transform(lon, lat)
print(f"Delhi in UTM Zone 43N: E={easting:.0f}, N={northing:.0f}")
```

### Reference Mapping
```python
import geopandas as gpd
import matplotlib.pyplot as plt

# Load boundaries/index maps from Boundaries/ subdirectory
boundaries = gpd.read_file('Boundaries/index_map.shp')

# Create reference map
fig, ax = plt.subplots(figsize=(12, 10))
boundaries.plot(ax=ax, alpha=0.3, edgecolor='k')

# Add grid reference
ax.set_xlabel('Longitude')
ax.set_ylabel('Latitude')
ax.grid(True, alpha=0.3)
ax.set_title('Survey of India Index Map Reference')

plt.show()
```

### Historical to Modern Conversion
```python
from pyproj import Transformer

# Everest to WGS 84 conversion
everest_to_wgs84 = Transformer.from_proj(
    'EPSG:4154',  # Indian Datum (Everest-based)
    'EPSG:4326'   # WGS 84
)

# Historical coordinates to modern
# (Everest Datum coordinates)
lon_old, lat_old = 77.5, 28.5

lon_new, lat_new = everest_to_wgs84.transform(lat_old, lon_old)
print(f"Historical: {lat_old}, {lon_old}")
print(f"Modern WGS84: {lat_new}, {lon_new}")
print(f"Difference: {abs(lat_new - lat_old):.6f}°")
```

## 📊 Coordinate System Information

### India's Geodetic Position

| Aspect | Details |
|--------|---------|
| **Primary UTM Zones** | 43, 44, 45 |
| **Central Meridians** | 75°E, 81°E, 87°E |
| **Latitude Range** | 8°N to 37°N |
| **Longitude Range** | 68°E to 97°E |
| **Historical Datum** | Everest (now Everest 1830 Modified) |
| **Modern Datum** | WGS 84 |

### Map Sheet Systems

#### Historical Index (Everest System)
- **Grid Size**: Variable (typically 0.5° × 0.5°)
- **Naming**: Quadrangle-based
- **Resolution**: 1:1,000,000 and larger scales
- **Coverage**: Complete India coverage

#### Modern Index (WGS84/UTM)
- **Grid Size**: UTM grid (1km squares)
- **Naming**: Zone + grid reference
- **Resolution**: All scales supported
- **Coverage**: Global consistency

## 🔄 Format Conversion References

```bash
# Everest to WGS 84 (using GDAL)
ogr2ogr -s_srs 'EPSG:4154' -t_srs 'EPSG:4326' \
  output_wgs84.shp input_everest.shp

# WGS 84 to UTM Zone 43N
ogr2ogr -s_srs 'EPSG:4326' -t_srs 'EPSG:32643' \
  output_utm43n.shp input_wgs84.shp

# Polyconic to Web Mercator
ogr2ogr -s_srs 'EPSG:54003' -t_srs 'EPSG:3857' \
  output_webmercator.shp input_polyconic.shp
```

## 📚 Technical Reference

### EPSG Codes for India

| Name | EPSG Code | Type | Datum |
|------|-----------|------|-------|
| WGS 84 | 4326 | Geographic | WGS 84 |
| UTM Zone 43N | 32643 | Projected | WGS 84 |
| UTM Zone 44N | 32644 | Projected | WGS 84 |
| UTM Zone 45N | 32645 | Projected | WGS 84 |
| Indian Datum | 4154 | Geographic | Everest |
| Everest / India Zone I | 20131 | Projected | Everest |
| Everest / India Zone II | 20132 | Projected | Everest |
| Everest / India Zone III | 20133 | Projected | Everest |

### Projection Details

**UTM (Universal Transverse Mercator)**:
- Local projection (6° width per zone)
- Minimal distortion
- Standard for modern mapping
- Used worldwide

**Polyconic**:
- Historical Survey projection
- Hybrid of conic projections
- Good for continental areas
- Rarely used now

**Everest Datum**:
- High accuracy for Indian region
- Slight differences from WGS 84
- Difference: ~200 meters at worst
- Historical baseline for Indian maps

## 📖 Reference Documentation

### Main Reference
- **File**: `Topo2OSM.pdf`
- **Purpose**: Conversion between topographic and OSM coordinate systems
- **Contents**: Transformation guidelines, datum conversions, projection explanations

### Key Concepts
1. **Datum Transformation**: From Everest to WGS 84
2. **Projection Systems**: UTM, Polyconic, Geographic
3. **Map Sheet Index**: Historical and modern organization
4. **Coordinate Conversion**: Between systems and standards

## 🛠️ Working with Index Maps

### Load Index Maps
```python
import geopandas as gpd

# Load index boundaries
index_map = gpd.read_file('Boundaries/index_grid.shp')

# Display index grid
print(f"Total index sheets: {len(index_map)}")
print(index_map.head())

# Get specific region
print(index_map[index_map['REGION'] == 'Kashmir'])
```

### Create Reference Overlays
```python
import folium
import geopandas as gpd

# Load index
index_map = gpd.read_file('Boundaries/index_grid.shp')

# Create map with index overlay
m = folium.Map(location=[23, 82], zoom_start=4)

# Add index grid
folium.GeoJson(index_map.to_json(), 
               style_function=lambda x: {'color': 'blue', 'weight': 1}).add_to(m)

m.save('survey_index_reference.html')
```

## 📚 Related Data

- **All Geospatial Data**: Parent directory
- **Districts**: `../Districts/` - Administrative divisions
- **States**: `../States/` - State boundaries
- **Country**: `../Country/` - National boundary

## 🔗 External Resources

### Coordinate Systems
- **EPSG Registry**: https://epsg.io/
- **Proj Library**: https://proj.org/
- **GDAL Docs**: https://gdal.org/

### Survey of India
- **Official Website**: https://surveyofindia.gov.in/
- **Map Sheets**: Historical index available
- **Documentation**: Historical geodetic data

### Map Conversion Tools
- **Map Shaper**: https://www.mapshaper.org/
- **GDAL**: Open source geospatial tools

## ⚠️ Important Notes

### Datum Transformation
- **Everest to WGS 84**: Difference ~200m maximum
- **Affects Accuracy**: For precise boundary work
- **Modern Standard**: Always use WGS 84 for new work
- **Historical Data**: Note original datum when available

### Projection Considerations
- **UTM Zones**: India spans zones 43-45
- **Zone Boundaries**: Can affect coordinate calculations
- **Local vs Global**: UTM better for local, WGS 84 for global
- **Web Mapping**: Usually uses Web Mercator (EPSG:3857)

### Historical Maps
- **Legacy System**: Everest datum + Polyconic
- **Modern**: WGS 84 + UTM
- **Conversion**: Use documented transformation parameters
- **Accuracy**: Sufficient for most purposes (~100-200m)

## 📧 Notes

- Index maps serve as reference for geographic organization
- Useful for historical map sheet lookup
- Modern replacements: OSM grid, UTM zones
- Original Survey system: Everest/Polyconic
- Current standard: WGS 84/UTM

---

**Last Updated**: June 2026  
**Content**: Index maps, coordinate systems, grid references  
**Historical Datum**: Everest (pre-modern)  
**Modern Datum**: WGS 84  
**Primary Projection**: UTM Zones 43-45  
**Format**: Shapefiles, PDF documentation  
**Source**: Survey of India, OpenStreetMap
