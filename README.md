# Kings Creek Soil Moisture Network

**Konza Prairie Biological Station · Manhattan, Kansas**

This is the repository for a long-term in situ soil moisture monitoring network embedded within the Kings Creek watershed at the Konza Prairie Biological Station. The network became operational on **July 1, 2021** and continues to operate today.

---

## Network Overview

| | |
|---|---|
| **Stations** | 16 |
| **Watershed** | Kings Creek, Konza Prairie Biological Station |
| **Soil depths monitored** | 5 cm, 20 cm, 40 cm |
| **Soil sensor** | Meter Teros 12 |
| **Datalogger** | Meter ZL6 |
| **Sampling interval** | Hourly |
| **Operational since** | July 1, 2021 |

### Instrumentation History

**July 2021 – November 2025**  
All 16 stations recorded soil variables (volumetric water content, soil temperature, bulk electrical conductivity at 3 depths) plus atmospheric variables (precipitation, air temperature, relative humidity). Three stations were additionally equipped with Atmos 41 sensors recording solar radiation, wind speed, and wind direction. The remaining 13 stations used Atmos 14 sensors.

**November 2025 – present**  
Atmospheric sensors (rain gauges and external sensors) were removed from all stations to simplify datalogger shielding during prescribed burns. All stations continue to record soil variables. Internal logger temperature and atmospheric pressure (measured inside the logger enclosure) are also recorded.

---

## Repository Structure

```
.
├── metadta/stations.csv           # Coordinates, elevation, and IDs for all 16 stations
├── metadata/kings_creek_boundary.geojson    # Watershed boundary (EPSG:4326, GEE-compatible)
├── metadata/images/                    # Field photos (N/E/S/W) for each station
├── series/soil/                      # Hourly soil VWC, temperature, EC — all stations
├── series/atm/                       # Hourly atmospheric data (Jul 2021 – Nov 2025)
├── series/mesonet/                   # Hourly atmospheric data from the Kansas Mesonet (Jul 2021 - Mar 2026).
├── series/ndvi/                # Sentinel-2 NDVI time series (point extraction)
├── series/streamflow/                # USGS gauge 06879650 — 15-min discharge
├── maps/ndvi/                      # Sentinel-2 NDVI rasters at 10-m resolution (~150 dates)
├── maps/elevation/                # Elevation, slope, aspect at 10-m resolution (USGS 3DEP)
└── raw/                       # Original datalogger files (~500 MB, not in repo but available upon request)
```

---

## Data Descriptions

### `metadata/stations.csv`
Station-level metadata:

| Column | Description |
|---|---|
| `name` | Station identifier (station_01 … station_16) |
| `latitude` | Decimal degrees (WGS84) |
| `longitude` | Decimal degrees (WGS84) |
| `altitude` | Elevation above sea level (m) |
| `serial_number` | Datalogger serial number |
| `watershed` | Watershed name |

### `metadata/kings_creek_boundary.geojson`
Watershed boundary polygon in GeoJSON format. Compatible with Google Earth Engine, Folium, Leaflet, and GeoPandas. Coordinate reference system: EPSG:4326 (WGS84).

### `metadata/images/`
Field photographs collected during maintenance visits. Images are organized by station and include views looking north, east, south, and west from each station location. These are not systematic or time-lapse images — they serve as landscape context for each monitoring site.

### `series/soil/`
Hourly observations for all soil variables across all 16 stations. Each file contains:

| Column | Description |
|---|---|
| `timestamp` | Hourly datetime (local time) |
| `vwc_5cm` / `vwc_20cm` / `vwc_40cm` | Volumetric water content (m³/m³) |
| `temp_5cm` / `temp_20cm` / `temp_40cm` | Soil temperature (°C) |
| `ec_5cm` / `ec_20cm` / `ec_40cm` | Bulk electrical conductivity (dS/m) |


### `series/atm/`
Hourly atmospheric observations from July 2021 through November 2025.

| Column | Description | Stations |
|---|---|---|
| `precip_total` | Total precipitation (mm) | All 16 |
| `temp_air` | Air temperature (°C) | All 16 |
| `vapor_pressure` | Vapor pressure (kPa) | All 16 |
| `vpd` | Vapor pressure deficit (kPa) | All 16 |
| `precip_rate_max` | Max precipitation rate (mm/hr) | All 16 |
| `battery_percent` / `battery_voltage` | Logger battery status | All 16 |
| `solar_rad`, `wind_speed`, `wind_dir` | Full met variables | Stations 3, 9, and 16 were equipped with Atmos 41 |

### `series/mesonet/`
Hourly atmospheric data obtained from a the Ashland Bottoms station of the Kansas Mesonet. File contains hourly precipitation (mm), air temperature (°C), relative humidity (%), wind speed (m/s), wind direction, and solar radiation (W/m²). The station is located at 39.125846, -96.636446, which is about 4.4 km from station 1 and 7.8 km from station 5.

### `series/ndvi/`
Sentinel-2 NDVI time series extracted at each station location. Only cloud-free observations are included (tiles with >5% cloud cover over the watershed were excluded). Columns: `timestamp`, `station_id`, `NDVI`.

- **Source**: `COPERNICUS/S2_SR_HARMONIZED`
- **Resolution**: 10 m
- **Cloud filtering**: SCL pixel-level mask + tile-level cloud cover < 5%

### `series/streamflow/`
15-minute streamflow data from USGS gauge **06879650** (Kings Creek near Manhattan, KS). Columns follow USGS tab-delimited format; the processed file includes `timestamp` and `discharge_cfs`.

### `maps/elevation/`
Terrain rasters at 10-m resolution clipped to the watershed boundary:
- `elevation.tif` — elevation (m), source: `USGS/3DEP/10m`
- `slope.tif` — slope (degrees)
- `aspect.tif` — aspect (degrees from north)

### `maps/ndvi/`
Sentinel-2 NDVI raster images clipped to the Kings Creek watershed at 10-m resolution. Approximately 150 dates are available. Naming convention: `ndvi_YYYY-MM-DD.tif`.

---

## Notes

- **Missing data**: Some stations experienced outages due to prescribed burns causing hardware damage or failures. The temporal hourly record is complete in each file and missing data are represented as `NaN`.
- **Raw data**: The `raw/` folder (~500 MB) contains the original files as downloaded from the dataloggers and is not tracked in this repository. Contact the authors if you need these files that include logger configuration and a few additional variables like lightning strikes.
- **Temporal coverage**: All timestamps are in UTC time. The UTC offset for local time is -6 hours (Central Standard Time).

---

## Articles

A short article with some preliminary data analysis from the network can be found here: Patrignani, A., Parker, N., & Cominelli, S. (2022). Upland rootzone soil water deficit regulates streamflow in a catchment dominated by North American tallgrass prairie. Water, 14(5), 759. https://doi.org/10.3390/w14050759

## Contact

For questions, collaboration, or to request additional files, please contact: **andrespatrignani@ksu.edu**
