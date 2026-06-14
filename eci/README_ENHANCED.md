# 🗳️ Election Commission of India (ECI) Data

## Overview

This directory contains electoral data from the Election Commission of India, organized by constituency type. The data includes polling station information, electoral divisions, and related geographic data.

## 📁 Subdirectories

### `AC_Data/`
**Assembly Constituency Electoral Data**
- State-level electoral division data
- Polling station locations and details
- Assembly constituency-wise information
- Population data by constituency
- Electoral registration details

### `PC_Data/`
**Parliamentary Constituency Electoral Data**
- National-level electoral division data
- Lok Sabha constituency information
- Polling station locations
- Parliamentary constituency profiles
- Voter registration by constituency

## 📊 Data Details

### Constituency Types

#### Assembly Constituencies (AC)
- **Level**: State-level
- **Total**: ~4,000+ constituencies
- **Electoral Body**: State Assembly/Legislative Assembly
- **Election Frequency**: Every 5 years (usually)
- **Associated File**: `India_AC.shp` (in parent directory)

#### Parliamentary Constituencies (PC)
- **Level**: National-level
- **Total**: 543 constituencies
- **Electoral Body**: Lok Sabha (Lower House)
- **Election Frequency**: Every 5 years
- **Associated File**: `india_pc_2014.shp` (in parent directory)

## 🚀 Usage Examples

### Explore ECI Data Structure
```bash
# List AC data
ls -la AC_Data/

# List PC data
ls -la PC_Data/

# Check file details
file AC_Data/*
file PC_Data/*
```

### Link with Geospatial Data
```python
import geopandas as gpd
import pandas as pd

# Load parliamentary constituencies
pc = gpd.read_file('../parliamentary-constituencies/india_pc_2014.shp')

# Load assembly constituencies
ac = gpd.read_file('../assembly-constituencies/India_AC.shp')

# Merge with ECI data
# Read ECI data files from PC_Data or AC_Data directories
# Merge on constituency codes for electoral analysis
```

### Electoral Analysis
```python
import geopandas as gpd

# Load data
ac = gpd.read_file('../assembly-constituencies/India_AC.shp')
pc = gpd.read_file('../parliamentary-constituencies/india_pc_2014.shp')

# Get state-wise constituency count
state_ac = ac.groupby('ST_NM').size()
state_pc = pc.groupby('STATE_NAME').size()

# Compare representation
print("Assembly constituencies by state:")
print(state_ac.sort_values(ascending=False).head(10))

print("\nParliamentary constituencies by state:")
print(state_pc.sort_values(ascending=False).head(10))
```

## 🔗 Data Organization

### Assembly Constituency Data (`AC_Data/`)

**Typical Contents**:
- State-wise assembly constituency boundaries
- Polling station mapping
- Voter registration data
- Electoral district information

**File Format**:
- Shapefiles (.shp, .shx, .dbf, .prj)
- CSV/Excel files with electoral data
- GeoJSON format variants

### Parliamentary Constituency Data (`PC_Data/`)

**Typical Contents**:
- Lok Sabha constituency boundaries
- Polling station locations
- National-level electoral data
- Voter statistics by constituency

**File Format**:
- Shapefiles (.shp, .shx, .dbf, .prj)
- Electoral data files
- Aggregated statistics

## 📊 Electoral Data Fields

Common attributes found in ECI data:

- `CONSTITUENCY_NAME` - Name of electoral division
- `CONSTITUENCY_CODE` - Unique identifier
- `STATE_NAME` - Associated state/UT
- `DISTRICT` - District information
- `ELECTORS` - Total registered voters
- `POPULATION` - Total population
- `POLLING_STATIONS` - Number of polling stations
- `AREA` - Geographic area
- Geometry: Polygon boundaries

## 🛠️ Working with ECI Data

### Load and Analyze
```python
import geopandas as gpd
import pandas as pd

# Load AC data
ac_data = gpd.read_file('AC_Data/India_AC.shp')

# Statistical summary
print("Total Assembly Constituencies:", len(ac_data))
print("\nStates/UTs represented:", ac_data['ST_NM'].nunique())
print("\nTop 10 constituencies by population:")
print(ac_data.nlargest(10, 'POPULATION')[['AC_NAME', 'ST_NM', 'POPULATION']])
```

### Create Electoral Maps
```python
import folium
import geopandas as gpd

# Load data
ac = gpd.read_file('AC_Data/India_AC.shp')
pc = gpd.read_file('PC_Data/india_pc_2014.shp')

# Create assembly constituency map
m_ac = folium.Map(location=[23, 82], zoom_start=4)
folium.GeoJson(ac.to_json()).add_to(m_ac)
m_ac.save('ac_map.html')

# Create parliamentary constituency map
m_pc = folium.Map(location=[23, 82], zoom_start=4)
folium.GeoJson(pc.to_json()).add_to(m_pc)
m_pc.save('pc_map.html')
```

### Spatial Analysis
```python
import geopandas as gpd

# Load constituencies
ac = gpd.read_file('AC_Data/India_AC.shp')

# Calculate area statistics
ac['area_km2'] = ac.geometry.area * (111320**2)

# Population density
ac['density'] = ac['POPULATION'] / ac['area_km2']

# Filter high-density constituencies
high_density = ac[ac['density'] > 1000]
print(f"High-density ACs (>1000/km²): {len(high_density)}")

# Sort by various metrics
print("\nLargest constituencies by area:")
print(ac.nlargest(5, 'area_km2')[['AC_NAME', 'ST_NM', 'area_km2']])
```

## 📚 Related Resources

### Constituency Geospatial Data
- **Assembly Constituencies**: `../assembly-constituencies/India_AC.shp`
- **Parliamentary Constituencies**: `../parliamentary-constituencies/india_pc_2014.shp`

### Administrative Data
- **States**: `../States/` - State boundaries
- **Districts**: `../Districts/` - District divisions
- **Country**: `../Country/` - National boundary

## 🔄 Data Integration

### Combine ECI with Geospatial
```python
import geopandas as gpd
import pandas as pd

# Load geospatial constituencies
pc_geo = gpd.read_file('../parliamentary-constituencies/india_pc_2014.shp')

# Load ECI election data (if available as CSV)
# eci_data = pd.read_csv('PC_Data/election_results.csv')

# Merge on constituency code
# merged = pc_geo.merge(eci_data, on='PC_CODE')

# Analyze combined data
# result_summary = merged.groupby('STATE_NAME').agg({...})
```

## 📊 Electoral System

### Two-Tier Electoral System

```
National Elections (Lok Sabha)
├── 543 Parliamentary Constituencies
│   └── Fixed term: 5 years
│
State Elections (Assembly)
├── 28 States with ~4,000+ Assembly Constituencies
└── Fixed term: 5 years (usually)
```

### Voting Process
1. **Voter Registration**: Citizens register with local constituency
2. **Polling Stations**: Located in constituencies
3. **Voting**: On scheduled election date
4. **Counting**: At central location per constituency
5. **Result**: Winner declared by majority

## 🏛️ Electoral Bodies

### National Level
- **Lok Sabha**: Lower House (543 seats)
  - 530 from states
  - 13 from Union Territories
- **Rajya Sabha**: Upper House (245 seats)
  - Elected by state legislatures
  - No geographic constituencies

### State Level
- **State Assemblies**: ~4,000+ seats
- **Local Bodies**: Municipalities, Panchayats
- Electoral representation varies by state

## ⚠️ Important Notes

### Data Accuracy
- ECI data is official source for electoral information
- Boundaries may be updated after delimitation
- Subject to periodic review and changes
- Use latest available version

### Data Updates
- **Current Boundaries**: Based on 2014 Delimitation
- **Valid Until**: Next Delimitation (after 2031 Census)
- **Previous**: 2008 Delimitation
- **Note**: Boundaries same for 2014, 2019, 2024 elections

### Caveats
- Some inconsistencies noted in assembly data
- Check state-specific sources for accuracy
- ECI website authoritative for official data
- Use for analysis and visualization

## 📖 License

As per Election Commission of India

## 🔗 Data Sources

- Election Commission of India (ECI)
- https://eci.gov.in/
- Constituency-level electoral data
- Geospatial data from official sources
- Integration with OSM community data

## 📧 Contact & Updates

- **ECI Official**: https://eci.gov.in/
- **Corrections**: Submit via GitHub issues
- **Updates**: Check repository for latest versions

---

**Last Updated**: June 2026  
**Data Type**: Electoral constituencies and divisions  
**Coverage**: All states/UTs and electoral divisions  
**Source**: Election Commission of India  
**Associated Data**: Assembly constituencies (~4,000+), Parliamentary constituencies (543)
