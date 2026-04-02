# Flood Event Frequency Mapping in Bangladesh (2000–2025)

## Project Overview

This project identifies and maps the frequency of distinct flood events across Bangladesh between 2000 and 2025 by clustering intersecting and temporally close flood observations into unique events, then counting how many such events occurred within each 2‑km grid cell.

## Methodology

The workflow consists of seven main steps:

### 1. Download and Prepare Spatial Data
- Download Bangladesh country boundary from Natural Earth (10m cultural vector data)
- Download global flood observation records from Zenodo (Parquet format) containing flood polygon geometries and dates

### 2. Create a 2‑km Grid Over Bangladesh
- Project country boundary to Bangladesh Transverse Mercator (EPSG:9678) for accurate local measurements
- Generate a regular grid of 2×2 km cells covering the country's extent
- Keep only grid cells that intersect Bangladesh's land area to optimize processing

### 3. Filter Flood Observations
- Spatially filter flood records to those within the country's bounding box for computational efficiency

### 4. Identify Distinct Flood Events
- Build a graph where nodes represent individual flood polygons
- Connect two nodes when:
  - Their geometries intersect
  - Their start dates are within 7 days of each other
- Use connected components algorithm to assign a unique `flood_event` ID to each group of intersecting, temporally‑close polygons
- This prevents counting the same flood multiple times across overlapping or consecutive observations

### 5. Count Flood Events Per Grid Cell
- Spatially join grid cells with flood polygons (both projected to EPSG:9678)
- For each grid cell, count the number of unique flood events that intersect it

### 6. Visualize the Flood Frequency Map
- Grid cells with **no observations** are shown in light grey (`#d9d9d9`)
- Cells with ≥1 flood event are colored using a yellow‑orange‑red gradient (1 to 100 events)
- Add colorbar legend and manual 'No Observations' legend
- Overlay country boundary with a subtle black outline
- Remove axes for a clean map layout

### 7. Save the Result
- Export the grid with flood event counts as a GeoPackage file for further use or sharing

## Data Sources

| Data | Source | Format |
|------|--------|--------|
| Country boundaries | [Natural Earth](https://naturalearthdata.com) (10m cultural) | Shapefile |
| Flood observations | [Zenodo Record 18647054](https://zenodo.org/records/18647054) | Parquet |

## Requirements

Install the required Python packages:

```bash
pip install geopandas matplotlib numpy pandas scipy shapely
