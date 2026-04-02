# Flood-Event-Frequency-Mapping-in-Bangladesh-2000-2025

Download and prepare spatial data:
Download Bangladesh country boundary from Natural Earth.
Download global flood observation records (Parquet file from Zenodo) containing flood polygon geometries and dates.

Create a 2‑km grid over Bangladesh:
Project country boundary to Bangladesh Transverse Mercator (EPSG:9678).
Generate a regular grid of 2×2 km cells covering the country’s extent.
Keep only grid cells that intersect Bangladesh’s land area.

Filter flood observations:
Spatially filter flood records to those within the country’s bounding box for efficiency.
Identify distinct flood events
Build a graph where nodes are flood polygons.

Connect two nodes if:
Their geometries intersect, and
Their start dates are within 7 days of each other.
Use connected components to assign a unique flood_event ID to each group of intersecting, temporally‑close polygons.
This prevents counting the same flood multiple times across overlapping or consecutive observations.

Count flood events per grid cell:
Spatially join grid cells with flood polygons (projected to the same CRS).
For each grid cell, count the number of unique flood events that intersect it.

Visualize the flood frequency map:
Grid cells with no observations are shown in light grey.
Cells with ≥1 flood event are colored using a yellow‑orange‑red gradient (1 to 100 events).
Add a colorbar and legend, overlay the country boundary, and remove axes for a clean map layout.

Save the result:
Export the grid with flood event counts as a GeoPackage file for further use or sharing.
