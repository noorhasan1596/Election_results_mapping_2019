# 🗺️ India Maps Repository - Complete Guide

## 📍 Overview

The **Maps Repository** is a comprehensive geospatial database containing administrative boundaries and spatial data for India at multiple levels including country, states, districts, and constituencies.
---

## 📦 Repository Contents

### 1. **Country-Level Data** (`Country/`)
- India national boundaries
- Overall geographic extent
- File: `IND_adm0.shp`

### 2. **State-Level Data** (`States/`)
- State boundaries for all 28 states and 8 Union Territories
- State administrative divisions
- File format: `IND_adm1.shp`

**Coverage Includes:**
- Union Territories (UT):
  - Ladakh UT (post-2019)
  - Jammu & Kashmir UT (post-2019)
  - Delhi (NCT)
  - Puducherry
  - Andaman & Nicobar Islands
  - Lakshadweep
  - Daman & Diu
  - Dadra & Nagar Haveli

- Major States:
  - Uttar Pradesh
  - Maharashtra
  - Karnataka
  - Rajasthan
  - Tamil Nadu
  - And 23 more states

### 3. **District-Level Data** (`Districts/`)
- ~750+ districts across India
- Fine-grained administrative divisions
- File format: `IND_adm2.shp`
- Updated with 2019 administrative reorganization

### 4. **Assembly Constituencies** (`assembly-constituencies/`)
- State Assembly/Legislative Assembly boundaries
- Electoral division data for state-level elections
- File format: `.shp` format for each state

### 5. **Parliamentary Constituencies** (`parliamentary-constituencies/`)
- Lok Sabha constituency boundaries
- Rajya Sabha divisions
- Electoral districts for national elections
- Current boundaries (updated post-delimitation)

### 6. **Election Commission Data** (`eci/`)
- Election Commission of India official geospatial data
- Constituency-wise electoral data
- Voting district information

### 7. **Survey of India Index Maps** (`Survey-of-India-Index-Maps/`)
- Historical Survey of India map sheets
- Reference maps and grid indices
- Administrative map references

### 8. **Documentation** (`docs/`)
- Data dictionaries
- Attribute descriptions
- Metadata and data specifications
- Usage guidelines

### 9. **Website Resources** (`website/`)
- Web-friendly map data
- Visualization resources
- GeoJSON converted versions

---

## 📊 Data Specifications

### Coordinate Reference System (CRS)
- **Default**: WGS 84 (EPSG:4326)
- Latitude/Longitude in decimal degrees
- Global standard for GIS data

### File Formats
**Primary Format**: Shapefile (.shp)
- Requires companion files: `.shx`, `.dbf`, `.prj`, `.shp`

**Available Conversions**:
- GeoJSON
- KML/KMZ
- TopoJSON
- GeoPackage

### Attributes Included
Each shapefile typically includes:
- `NAME` - Administrative unit name
- `ADM0_A3` - Country code (IND for India)
- `ADM1` - State code
- `ADM2` - District code
- `POPULATION` - Census population data
- `AREA_SQ_KM` - Area in square kilometers
- Geometry columns for boundaries

---

## 🚀 Quick Start Guide

### 1. **Download Data**
```bash
# Crowdsources sources and datameet clone some fo files

# Navigate to directory
cd maps
```

### 2. **Using the Data**

#### Load with GeoPandas (Python)
```python
import geopandas as gpd

# Load state boundaries
states = gpd.read_file('States/IND_adm1.shp')
print(states.head())

# Load district boundaries
districts = gpd.read_file('Districts/IND_adm2.shp')
print(districts.head())

# Filter specific state
maharashtra = states[states['NAME_1'] == 'Maharashtra']
```

#### Create Interactive Map
```python
import folium
import geopandas as gpd

# Load data
districts = gpd.read_file('Districts/IND_adm2.shp')

# Create map
m = folium.Map(location=[23, 82], zoom_start=4)

# Add data
folium.GeoJson(districts.to_json()).add_to(m)

# Save
m.save('india_map.html')
```

#### Convert to GeoJSON
```bash
# Using GDAL ogr2ogr
ogr2ogr -f GeoJSON states.geojson States/IND_adm1.shp

# Using GeoPandas
gdf = gpd.read_file('States/IND_adm1.shp')
gdf.to_file('states.geojson', driver='GeoJSON')
```

### 3. **View in QGIS**
1. Download QGIS (https://qgis.org/)
2. Open QGIS
3. File → Open → Select any `.shp` file
4. Visualize and analyze spatial data

---

## 📈 Data Coverage Comparison

| Level | File | Coverage | Records |
|-------|------|----------|---------|
| Country | `IND_adm0.shp` | India | 1 |
| State | `IND_adm1.shp` | States + UTs | 36 |
| District | `IND_adm2.shp` | Districts | 750+ |
| Assembly | `*.shp` (per state) | State constituencies | ~4000 |
| Parliament | `*.shp` | Lok Sabha seats | 543 |

---

## 🔄 Format Conversion

### Using GDAL Commands

```bash
# Shapefile to GeoJSON
ogr2ogr -f GeoJSON output.geojson input.shp

# Shapefile to KML
ogr2ogr -f KML output.kml input.shp

# Shapefile to GeoPackage
ogr2ogr -f GPKG output.gpkg input.shp

# Shapefile to CSV (with geometry as WKT)
ogr2ogr -f CSV output.csv input.shp
```

### Using Python (GeoPandas)

```python
import geopandas as gpd

# Read any format
gdf = gpd.read_file('input.shp')

# Write to different formats
gdf.to_file('output.geojson', driver='GeoJSON')
gdf.to_file('output.kml', driver='KML')
gdf.to_file('output.gpkg', driver='GPKG')
```

### Using Online Tools
- **MapShaper**: https://www.mapshaper.org/
  - Upload zipped shapefile (.shp, .shx, .dbf, .prj)
  - Convert to GeoJSON, KML, TopoJSON
  - Edit and simplify geometries

---

## 📚 Practical Use Cases

### 1. **Create State-wise Maps**
```python
import geopandas as gpd
import matplotlib.pyplot as plt

states = gpd.read_file('States/IND_adm1.shp')

# Map each state
for state in states['NAME_1'].unique():
    state_data = states[states['NAME_1'] == state]
    
    fig, ax = plt.subplots(figsize=(10, 8))
    state_data.plot(ax=ax, alpha=0.5, edgecolor='k')
    plt.title(f'{state} Boundary')
    plt.savefig(f'{state}.png')
    plt.close()
```

### 2. **Analyze District Population**
```python
import geopandas as gpd
import pandas as pd

districts = gpd.read_file('Districts/IND_adm2.shp')

# Get top 10 most populous districts
top_districts = districts.nlargest(10, 'POPULATION')
print(top_districts[['NAME_2', 'POPULATION', 'AREA_SQ_KM']])

# Calculate population density
districts['DENSITY'] = districts['POPULATION'] / districts['AREA_SQ_KM']
```

### 3. **Spatial Analysis & Queries**
```python
import geopandas as gpd

districts = gpd.read_file('Districts/IND_adm2.shp')

# Get districts in a specific state
maharashtra = districts[districts['NAME_1'] == 'Maharashtra']

# Get districts with area > 5000 sq km
large_districts = districts[districts['AREA_SQ_KM'] > 5000]

# Count districts by state
districts_per_state = districts.groupby('NAME_1').size()
print(districts_per_state)
```

---

## 📖 Data Quality & Accuracy

### Strengths
✅ Comprehensive coverage of all administrative levels  
✅ Regularly updated (especially post-2019 reorganization)  
✅ Open source and freely available  
✅ Community-maintained with active contributions  
✅ Multiple format support  

### Limitations
⚠️ Boundary accuracy varies by source  
⚠️ Some older datasets may lack recent changes  
⚠️ Subject to interpretation of disputed territories  
⚠️ Population data based on latest census available  

---

## 🔗 Integration with Kashmir Repository

The **Kashmir & Ladakh repository** (noorhasan1596/kashmir) complements this maps repository by providing:

- **Focused Data**: Specific attention to Kashmir and Ladakh regions
- **Detailed Boundaries**: Siachen Glacier, district-level analysis
- **Updated Information**: Post-2019 administrative changes
- **Visualization Scripts**: Ready-to-use mapping tools
- **Documentation**: Historical and geopolitical context

**Combined Usage**:
```python
# Load general India data from maps
india_districts = gpd.read_file('maps/Districts/IND_adm2.shp')

# Load specific Kashmir/Ladakh data
kashmir_data = gpd.read_file('kashmir/kashmir_geo_final.geojson')
ladakh_data = gpd.read_file('kashmir/2011_districts_ladakh_ut.geojson')

# Combine and analyze
merged = pd.concat([india_districts, kashmir_data])
```

---

## 🛠️ Tools & Software Compatibility

| Tool | Support | Notes |
|------|---------|-------|
| **QGIS** | ✅ Full | Desktop GIS, native shapefile support |
| **ArcGIS** | ✅ Full | Professional GIS software |
| **GeoPandas** | ✅ Full | Python library for spatial data |
| **Folium** | ✅ Full (via conversion) | Interactive web maps |
| **Leaflet.js** | ✅ Full (via GeoJSON) | JavaScript mapping library |
| **Google Earth** | ✅ Full (via KML) | Convert and view in Earth |
| **PostGIS** | ✅ Full | Database-based spatial analysis |

---

## 📥 Local Setup

### Installation Requirements
```bash
# Python packages
pip install geopandas folium matplotlib pandas shapely

# GDAL (for format conversion)
# macOS:
brew install gdal

# Linux (Ubuntu/Debian):
sudo apt-get install gdal-bin

# Windows:
# Download from OSGeo4W (https://trac.osgeo.org/osgeo4w/)
```

### Workspace Setup
```bash
# Navigate to workspace
cd /Users/noorhasan/GIS_Workspace

# Create project directory
mkdir my_gis_project
cd my_gis_project

# Create Python virtual environment
python3 -m venv venv
source venv/bin/activate

# Install dependencies
pip install geopandas folium matplotlib

# Download maps data
# from Datameet
```

---

## 📝 Important Notes

### File Organization
Always keep shapefile companion files together:
- `.shp` - Main file (geometry)
- `.shx` - Shape index
- `.dbf` - Attribute database
- `.prj` - Projection information

**Missing any of these files will prevent loading!**

### Attribution
When using this data:
- License: CC-BY-SA 2.5 India
- Include license text in your project

### Contributing
The repository is community-driven:
- Report issues on GitHub
- Submit corrections and updates
- Contribute improvements

---

## 🔗 Related Resources

- **GDAL Documentation**: https://gdal.org/
- **GeoPandas Docs**: https://geopandas.org/
- **QGIS Tutorials**: https://qgis.org/en/site/forusers/trainingmaterials/

---

## 📞 Support & Community

- **GitHub Issues**: Report bugs and request features
- **Stack Overflow**: Use tag `gdal`, `geopandas`, `shapefile`

---

## 📋 Quick Reference Checklist

- [x] Country-level boundaries available
- [x] State and district data included
- [x] Constituency data for elections
- [x] Multiple format support
- [x] Python/GDAL conversion tools
- [x] Open source license
- [x] Community maintained
- [x] Regular updates

---

**Last Updated**: June 2026  
**Status**: Active and maintained  
**For**: GIS analysis, mapping, and spatial research
