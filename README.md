# Lunar Landing Site Assessment and Hazard-Aware Path Planning

## Project Overview

This project simulates an **automated lunar mission planning system** for identifying optimal landing sites and computing safe navigation paths on the Moon’s south pole. It combines **terrain analysis**, **hazard detection**, and **path planning** to select the best landing locations and routes from those sites to scientifically important regions.

The final system ranks candidate landing sites by suitability (based on slope and proximity to ROIs), avoids hazardous areas (e.g., steep slopes), and uses **A\* search** to compute obstacle-free paths from the best site to nearby points of interest (ROIs). The results are visualized on a suitability heatmap with overlaid safe paths.

---

## Data Used

Data is derived from **NASA LRO-LOLA (Lunar Orbiter Laser Altimeter)** and made available via Michael Kenneth Barker’s high-resolution DEM products:

- `Site01_final_adj_5mpp_surf.tif`: Surface elevation map at 5m resolution
- `Site01_ROIs.tgz`: Shapefile of mission-relevant Regions of Interest (ROIs)

**Source:** High-Resolution LOLA Topography for Lunar South Pole Sites  
**DEM Reference Frame:** South Polar Stereographic, MOON_ME, DE421 ephemeris  
**Pixel Scale:** 5 meters/pixel

---

## Tech Stack

| Tool               | Purpose                                      |
|--------------------|----------------------------------------------|
| **Python**         | Main analysis language                       |
| **NumPy**, **SciPy** | Matrix and gradient operations              |
| **Rasterio**       | DEM loading and geospatial transforms        |
| **GeoPandas**, **Shapely** | ROI shapefile handling              |
| **NetworkX**       | A\* pathfinding over safe terrain            |
| **Matplotlib**     | Visualization of slope, suitability, and paths |
| **Colab / Jupyter**| Interactive development environment          |

---

## Analysis 

### A. Landing Zone Assessment
- Extract elevation from the DEM
- Compute terrain slope from elevation gradients
- Define Points of Interest (POIs) as ROI centroids
- Generate a **suitability score** based on:
  - Flatness (low slope)
  - Proximity to ROIs
- Output: Suitability heatmap, top 5 landing candidates

![image](https://github.com/user-attachments/assets/67049598-44b2-4f36-b3f0-bb518f92dd01)
![image](https://github.com/user-attachments/assets/cf40a10d-4304-442c-9bb9-fb18db76c177)
![image](https://github.com/user-attachments/assets/59614ffa-09cb-4d05-8e33-86792ffa61ee)



### B. Hazard Detection
- Define hazardous areas as terrain where slope > 40°
- **NOTE:** Ideal hazarous slope conditions to identify are usually 12-15° however this specific landing site seemed to lack any slopes thus the margin had be significantly dropped to be less strict
- Generate a **hazard map** (binary mask)
- Use it to block unsafe terrain from path planning

![image](https://github.com/user-attachments/assets/88aadec0-3191-47a4-9613-3b604eb962e0)

### C. Hazard-Aware Path Planning
- For the best landing site:
  - Identify 5 closest ROIs
  - Use **A\* search** on a slope-safe graph
  - Reject paths that intersect hazardous areas
- Output: Up to 5 safe, optimal paths

![image](https://github.com/user-attachments/assets/653f9901-059c-4e8b-9eed-d2822c268319)

### D. Visualization
- Suitability heatmap with ROI polygons
- Top landing site
- Colored optimal paths to ROIs with legend

---

## Example Output

- Landing Suitability Map (with top 5 candidates)
- Binary Hazard Map (slope > 40°)
- Terrain overlay with 5 color-coded paths from landing site to nearest ROIs

## Conclusions

- Only a **small subset** of the terrain meets strict flatness and accessibility criteria.
- Many ROIs fall in or near hazardous terrain; this justifies path filtering.
- The best landing sites often lie near but not within ROIs, requiring short, navigable paths.
- This pipeline provides a realistic tool for **lunar site selection and mission simulation** under real elevation and slope constraints.

---

## 📖 Dataset Description & References

The elevation data is derived from **improved high-resolution LOLA DEMs** (LDEMs) created by Michael Kenneth Barker and colleagues. These DEMs cover several key south polar landing sites (e.g., Site01, Site04, Site07) at a resolution of **5 meters per pixel**. They were generated through self-consistent co-adjustment of LRO-LOLA altimetry tracks to reduce orbital error by more than an order of magnitude (~10–20 cm horizontal, ~2–4 cm vertical). The dataset includes slope, uncertainty, and ROI shapefiles — and is well suited for shadow-independent analysis of topographic and illumination conditions.

These maps have significantly fewer interpolation artifacts and improved geodetic control, making them ideal for hazard-aware landing and mobility simulations.

### 📚 References

- Barker, M.K., et al. (2021), *Improved LOLA Elevation Maps for South Pole Landing Sites: Error Estimates and Their Impact on Illumination Conditions*, Planetary & Space Science, 203, 105119. [https://doi.org/10.1016/j.pss.2020.105119](https://doi.org/10.1016/j.pss.2020.105119)

- Mazarico, E., et al. (2011), *Illumination conditions of the lunar polar regions using LOLA topography*, Icarus, 211(2), 1066–1081. [https://doi.org/10.1016/j.icarus.2010.10.030](https://doi.org/10.1016/j.icarus.2010.10.030)

---
