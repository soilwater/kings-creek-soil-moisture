# Kings Creek Watershed Dataset

**Konza Prairie Biological Station · Manhattan, Kansas**

A long-term soil and atmosphere monitoring network embedded within the Kings Creek watershed at the Konza Prairie Biological Station. The network became operational on **July 1, 2021** and continues to operate today.

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

**July 2021 – October 2025**  
All 16 stations recorded soil variables (VWC, soil temperature, electrical conductivity at 3 depths) plus atmospheric variables (precipitation, air temperature, relative humidity). Three stations were additionally equipped with Atmos 41 sensors recording solar radiation, wind speed, and wind direction. The remaining 13 stations used Atmos 14 sensors.

**November 2025 – present**  
Atmospheric sensors (rain gauges and external sensors) were removed from all stations to simplify datalogger shielding during prescribed burns. All stations continue to record soil variables. Internal logger temperature and atmospheric pressure (measured inside the logger enclosure) are also recorded.

---

## Repository Structure

```
.
├── stations_metadata.csv           # Coordinates, elevation, and IDs for all 16 stations
├── kings_creek_boundary.geojson    # Watershed boundary (EPSG:4326, GEE-compatible)
│
├── soil_data/                      # Hourly soil VWC, temperature, EC — all stations
├── atm_data/                       # Hourly atmospheric data (Jul 2021 – Oct 2025)
│                                   #   includes Kansas Mesonet Ashland Bottoms reference
├── vegetation_data/                # Sentinel-2 NDVI time series (point extraction)
├── ndvi_maps/                      # Sentinel-2 NDVI rasters at 10-m resolution (~150 dates)
├── streamflow_data/                # USGS gauge 06879650 — 15-min discharge
├── elevations_data/                # Elevation, slope, aspect at 10-m resolution (USGS 3DEP)
├── images_data/                    # Field photos (N/E/S/W) for each station
└── raw_data/                       # Original datalogger files (~500 MB, not in repo)
```

---

## Data Descriptions

### `soil_data/`
Hourly observations for all soil variables across all 16 stations. Each file contains:

| Column | Description |
|---|---|
| `timestamp` | Hourly datetime (local time) |
| `record_id` | Station identifier |
| `vwc_5cm` / `vwc_20cm` / `vwc_40cm` | Volumetric water content (m³/m³) |
| `temp_5cm` / `temp_20cm` / `temp_40cm` | Soil temperature (°C) |
| `ec_5cm` / `ec_20cm` / `ec_40cm` | Bulk electrical conductivity (dS/m) |
| `temp_logger` | Internal logger temperature (°C) |
| `atm_pressure` | Atmospheric pressure inside logger enclosure (kPa) |

### `atm_data/`
Hourly atmospheric observations from July 2021 through October 2025.

| Column | Description | Stations |
|---|---|---|
| `precip_total` | Total precipitation (mm) | All 16 |
| `temp_air` | Air temperature (°C) | All 16 |
| `vapor_pressure` | Vapor pressure (kPa) | All 16 |
| `vpd` | Vapor pressure deficit (kPa) | All 16 |
| `precip_rate_max` | Max precipitation rate (mm/hr) | All 16 |
| `battery_percent` / `battery_voltage` | Logger battery status | All 16 |
| Solar radiation, wind speed, wind direction | Full met variables | 3 stations (Atmos 41) |

Also includes a reference file from the **Kansas Mesonet Ashland Bottoms** station with hourly precipitation, air temperature, relative humidity, wind speed, wind direction, and solar radiation (W/m²).

### `vegetation_data/`
Sentinel-2 NDVI time series extracted at each station location. Only cloud-free observations are included (tiles with >5% cloud cover over the watershed were excluded). Columns: `timestamp`, `station_id`, `NDVI`.

- **Source**: `COPERNICUS/S2_SR_HARMONIZED`
- **Resolution**: 10 m
- **Cloud filtering**: SCL pixel-level mask + tile-level cloud cover < 5%

### `ndvi_maps/`
Sentinel-2 NDVI raster images clipped to the Kings Creek watershed at 10-m resolution. Approximately 150 dates are available. Naming convention: `ndvi_YYYY-MM-DD.tif`.

### `streamflow_data/`
15-minute streamflow data from USGS gauge **06879650** (Kings Creek near Manhattan, KS). Columns follow USGS tab-delimited format; the processed file includes `timestamp` and `discharge_cfs`.

### `elevations_data/`
Terrain rasters at 10-m resolution clipped to the watershed boundary:
- `elevation.tif` — elevation (m), source: `USGS/3DEP/10m`
- `slope.tif` — slope (degrees)
- `aspect.tif` — aspect (degrees from north)

### `images_data/`
Field photographs collected during maintenance visits. Images are organized by station and include views looking north, east, south, and west from each station location. These are not systematic or time-lapse images — they serve as landscape context for each monitoring site.

### `stations_metadata.csv`
Station-level metadata:

| Column | Description |
|---|---|
| `record_id` | Station identifier (station_01 … station_16) |
| `latitude` | Decimal degrees (WGS84) |
| `longitude` | Decimal degrees (WGS84) |
| `altitude_m` | Elevation above sea level (m) |
| `watershed` | Watershed name |

### `kings_creek_boundary.geojson`
Watershed boundary polygon in GeoJSON format. Compatible with Google Earth Engine, Folium, Leaflet, and GeoPandas. Coordinate reference system: EPSG:4326 (WGS84), 2D coordinates.

---

## Notes

- **Missing data**: Some stations experienced outages due to prescribed burns, hardware failures, or logger battery issues. Missing timestamps are represented as `NaN`.
- **Raw data**: The `raw_data/` folder (~500 MB) contains the original files as downloaded from the dataloggers and is not tracked in this repository. Contact the authors if you need these files.
- **Temporal coverage**: All timestamps are in local time (CDT/CST). UTC offset is available in the raw data.

---

## Citation

If you use this dataset, please cite:

> *[Your citation here]*

---

## Contact

For questions, collaboration, or to request raw datalogger files, please contact: **your@email.edu**
