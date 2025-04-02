# 📁 Data Sources and Cleaning Overview  
**City of Dublin Capstone Project — Sprint 4 Final Deliverable**

This document provides an overview of the external datasets used in our analysis, including how each dataset was accessed, processed, and integrated into the final clustering analysis.

---

## 🔹 1. American Community Survey (ACS)  
- **Source**: U.S. Census Bureau — ACS 2022 5-Year Supplemental  
- **Access Method**: Pulled via `acs_data_to_sqlite.py` using the Census API  
- **Variables**: Initially 42; reduced to 19 high-quality variables based on coefficient of variation (CV < 15)  
- **Cleaning Steps**:
  - Constructed SQLite database with four tables: `census_data`, `place_dictionary`, `state_dictionary`, `variable_dictionary`
  - Filtered out Puerto Rico and places with populations below 10,000
  - Converted select variables to percentages for comparability
- **Output Files**:
  - `acsse_2022.db`
  - `CensusData_Clean.csv`
- **Links**:
  - [ACS Database](https://github.com/brutus-the-homeschooler/Capstone/blob/main/Database/acsse_2022.db)  
  - [Clean CSV](https://raw.githubusercontent.com/brutus-the-homeschooler/Capstone/refs/heads/main/Scripts/EDA/CensusData_Clean.csv)

---

## 🔹 2. Park and Trail Data  
- **Source**: Trust for Public Land (TPL) — ParkServe  
- **Access**: [https://www.tpl.org/park-data-downloads](https://www.tpl.org/park-data-downloads)  
- **Variables**: Park acreage, trail length, number of parks within proximity  
- **Cleaning & Processing**:
  - Created 8-mile buffers around the 564 ACS-validated places
  - Reprojected layers to NAD 1983 Albers Equal Area Conic (EPSG: 102003)
  - Performed spatial joins with TPL park/trail layers
  - Summarized unique parks and total park area per location
- **Output**: Shapefiles with joined spatial statistics

---

## 🔹 3. Climate Data  
- **Source**: Open-Meteo API ([https://open-meteo.com](https://open-meteo.com))  
- **Scope**: 91 cities (aligned with cluster cities)  
- **Variables**: Temperature, precipitation, wind speed (annual averages)  
- **Cleaning & Processing**:
  - Originally attempted NCEI nClimDiv, pivoted to Open-Meteo for coverage/consistency
  - Cleaned and standardized numerical values
  - Merged into broader cluster dataset
- **Output Files**:
  - `weather_data.csv`
  - `final_processed_data.csv`

---

## 🔹 4. Location Affordability Index (LAI)  
- **Source**: U.S. HUD LAI via ArcGIS Living Atlas  
- **Variables**: Income, housing/transportation costs by income class, rentership  
- **Cleaning & Processing**:
  - Removed highly correlated or low-reliability variables (e.g., Very Low Income and Working Individual groups)
  - Narrowed to 28 meaningful columns for analysis
  - Merged with ACS data using city-level identifiers
- **Output**:
  - `Dublin_LocationAffordability_Clean.csv`

---

## 🔹 5. Agricultural Production Data  
- **Source**: USDA Economic Research Services and NASS  
  - [https://www.ers.usda.gov](https://www.ers.usda.gov)  
  - [https://quickstats.nass.usda.gov](https://quickstats.nass.usda.gov)  
- **Variables**: Crop yield, production, and sales (county/state level)  
- **Cleaning & Processing**:
  - Filtered to U.S. data from 2010–2024 at annual frequency
  - Focused on relevant crops and economic indicators
  - Merged with master dataset for further analysis
- **Output**:
  - `merged_data.xlsx`

---

## 🧼 General Cleaning and Integration Notes  
- Standardized variables across all sources (e.g., percent of total population, z-scores)
- Merged datasets using FIPS codes, place names, and spatial joins
- Final cleaned datasets used in PCA and clustering analysis
- All outputs are reproducible via documented scripts in the `/code` folder

---

For questions or replication details, see the accompanying `README.md` or contact the project team.

