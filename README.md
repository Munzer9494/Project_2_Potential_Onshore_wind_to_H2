# Potential Onshore Wind to Hydrogen (DE)

> Reproducible workflow and notebooks to assess onshore wind resource suitability in Germany and convert to H₂ production potential and levelized cost of hydrogen (LCOH), with Monte Carlo (MC) uncertainty analysis.

---

## Overview

This repository estimates spatially resolved onshore wind suitability and derives hydrogen supply potential and costs. It combines raster/vector GIS processing with engineering and other stochastic & optimization models.

**Core steps**

1. **Resource assessment**: ingest power‑density, elevation/slope, and land‑cover to build a suitability surface.
2. **Constraints**: apply protected areas and other exclusion rules to derive feasible wind tiles.
3. **Wind‑to‑H₂ conversion**: map feasible wind output to electrolyzer production and compute LCOH.
4. **Uncertainty**: optional Monte Carlo runs on key parameters (capacity factor (weather variability), wind turbines spacing, electrolyzer efficiency, etc.).
5. **Linear Programing (LP) & system sizing**: CAPEX & OPEX assumptions, stochastic wind speeds, electrolyser availabilty & the WTG power curves to calculate LCoH distribution based on optimal sizing & creatign a H2 supply curve
6. **H2 Grid analysis**: Evaluates the adequacy of the projected regional hydrogen network and its alignment with the estimated potentials as well as supply and demand locations 


---

## Repository status

Current content is notebook‑driven, with several input rasters and shapefiles already included. See **Notebooks index** and **Data** below

---

## Software stack

* Python 3.11 (Conda recommended)
* Geo/PyData: `geopandas`, `rasterio`, `rioxarray`, `xarray`, `shapely`, `pyproj`, `rtree`
* Analysis: `numpy`, `pandas`, `scipy`, `tqdm`
* Plotting: `matplotlib`
* Optimization (optional): `pyomo` (+ solver such as GLPK/Ipopt)
* Notebooks: `jupyterlab`

An `environment.yml` is provided to reproduce the setup.

```bash
conda env create -f environment.yml
conda activate wind2h2
```

> If you do not use Conda, adapt `requirements.txt` accordingly.

---

## Quick start

```bash
# 1) Clone
git clone https://github.com/Munzer9494/Project_2_Potential_Onshore_wind_to_H2.git
cd Project_2_Potential_Onshore_wind_to_H2

# 2) Create the environment
conda env create -f environment.yml && conda activate wind2h2

# 3) Launch notebooks
jupyter lab
```

Open the notebooks listed below.

---

## Notebooks index

| Notebook                             | Purpose                                                           |
| ------------------------------------ | ----------------------------------------------------------------- |
| `resource_assessment_1.ipynb`        | Build suitability layers (Here we process the power density & land cover layers and calculate the first theretical potentials).      |
| `RSTM_PA_notebook.ipynb`             | Preparing & Raster‑to‑mask processing (Here we process the Digital Elevation Model + protected areas layers).                      |
| `WP_Protected_Areas.ipynb`           | Apply protected areas exclusions (Masking out/ excluding the protected areas - based on WDPA).                          |
| `resource_assessment_2.ipynb`        | Resource assessment 2 - including Elevation & Slope Processing (Here we integrate the elevation and slope masks and calculate the potentials afterwards) .                                              |
| `wind_variability2.ipynb`            | Analyze wind variability impacts & building the stochastic H2 model (Here we start analysing the wind speed data, making assumptions on the distributions of WTG spacing, availabilities & efficiencies for the Monte Carlo simulation).                                 |
| `project_2_LCoH.ipynb`               | Compute levelized cost of hydrogen & the building H2 supply curve (Here we set up the LP for optimal wind/electrolyser sizing to minimize the LCoH. Then we build the H2 supply curve from the stochastic LCoH & the stochastic H2 supply volumes).                               |
| `pipelines_supply_demand_view.ipynb` | Explore hydrogen pipeline supply–demand overlays (Here we investigate the H2 grid layout, the H2 demand and supply locations and estimate the regional flow capcity for adequacy)                 |
| `scanario_analysis.ipynb`            | Scenario/Monte Carlo experiments (Here is the analysis of the adequacy of H2 potentials to meet the demand scenarios). |




---

## Data (current)

Large files are tracked directly in the repo for now. Brief descriptions based on filenames:

* Rasters: `DEU_power-density_100m.tif`, `DE_power_density_clipped1.tif`, `wind_suitability_final.tif`, `wind_suitability_polygons_masked.tif`, `DE_clipped_elevation.tif`, `DE_clipped_elevation_WGS84.tif`, `DE_slope_WGS84.tif`, `output_degrees.tif`, `output_projected.tif`, `DE_landcover_*`.
* Vectors: `NUTS_RG_20M_2024_4326.shp`, `merged_WDPA_polygons.shp`.

---


---

## How to cite

If you use this repository, please cite:

```
Munzer Osman, Maria Movsessian (2025). Geospatial and Uncertainty‐Based Quantification of Germany’s Onshore Wind-to-Hydrogen Technical Potential
```

---

---

## Project structure (final target - still under progress)

```
Project_2_Potential_Onshore_wind_to_H2/
├─ README.md
├─ environment.yml
├─ Makefile
├─ LICENSE
├─ CITATION.cff
├─ src/
│  ├─ data/                # download/prepare
│  ├─ pipeline/            # suitability + conversion
│  ├─ econ/                # LCOH
│  ├─ experiments/         # MC
│  └─ reporting/           # figures, tables
├─ config/
├─ notebooks/
├─ data/
│  ├─ raw/
│  ├─ interim/
│  └─ processed/
├─ outputs/
│  ├─ figures/
│  └─ results/
└─ tests/
```

---

## Reproducibility notes

* Please pay attention to the unit conversions (e.g., power density → capacity factor assumptions).
* You may use any other solver to run the LP, however you may need to rewrite/reformulate the problem in a compatible syntax.
* It is recomended to set/use the same random seeds for MC where appropriate.

---

