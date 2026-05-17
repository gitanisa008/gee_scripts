# GEE Scripts

A collection of Google Earth Engine (GEE) scripts for various research projects

## Scripts

### luc_ghg
Estimates greenhouse gas (GHG) emissions from deforestation in Indonesia, broken down by province and year range. The script uses a GEE App interface where users can select a province and time range to calculate and visualize yearly emissions.

**Features:**
- Province-level filtering using administrative boundaries
- Distinguishes primary vs. secondary forest loss
- Separates peatland and mineral soil areas
- Calculates CO2, CH4, N2O, and soil organic carbon (SOC) emissions
- Interactive bar chart with yearly breakdown
- CSV export to Google Drive

**Datasets:**
- Administrative boundaries: `WM/geoLab/geoBoundaries/600/ADM1`
- Primary forest: `UMD/GLAD/PRIMARY_HUMID_TROPICAL_FORESTS/v1`
- Deforestation: `UMD/hansen/global_forest_change_2025_v1_13`
- Global peatland: `projects/sat-io/open-datasets/GLOBAL-PEATLAND-DATABASE`

**References:**
- AGB values: Kronseder et al. (2012) for Kalimantan, Laumonier et al. (2010) for Sumatra, Asner et al. (2014) for Papua, IPCC defaults for other regions
- Emission factors: IPCC Guidelines for National Greenhouse Gas Inventories

## Apps

| Script | App Link |
|--------|----------|
| luc_ghg | *(https://gitanisa.users.earthengine.app/view/greenhouse-gasses-emission-from-land-use-change)* |

## Notes
- All scripts are intended to be run in the [GEE Code Editor](https://code.earthengine.google.com/).
- Scripts may require a registered GEE account with access to the listed datasets.

