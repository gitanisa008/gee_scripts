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

## Methodology

Emission estimates follow the IPCC Guidelines for National Greenhouse Gas Inventories and are calculated for each year within the selected range.

### 1. Forest Loss Detection
Deforested pixels are identified from the Hansen Global Forest Change dataset at 30 m resolution. Each pixel is classified into:
- **Primary forest loss** — overlapping with UMD Primary Humid Tropical Forests data
- **Secondary forest loss** — all other forest loss pixels

Each loss pixel is further classified by soil type:
- **Peatland** — overlapping with the Global Peatland Database
- **Mineral soil** — all non-peat areas

### 2. Biomass Emissions (CO2, CH4, N2O)
Emissions from aboveground biomass (AGB) are estimated using region-specific AGB values and IPCC emission factors.

**CO2:**
```
CO2 = Area (ha) × AGB (t/ha) × CF × 3.667
```
- `CF = 0.47` — carbon fraction of dry biomass (IPCC Table 4.3); 47% of dry biomass weight is carbon
- `3.667` — molecular weight ratio of CO2 to C (44/12); converts carbon mass to CO2 mass

**CH4:**
```
CH4 = Area (ha) × AGB (t/ha) × Cf × G_CH4 × GWP_CH4 / 1000
```
- `Cf = 0.45` — combustion factor (IPCC Table 2.6); only 45% of biomass actually burns during a fire
- `G_CH4 = 6.8 g/kg` — CH4 emission ratio (IPCC Table 2.5); grams of CH4 released per kg of biomass burned, resulting from incomplete combustion
- `GWP_CH4 = 25` — Global Warming Potential of CH4 over 100 years (IPCC AR4); converts CH4 mass to CO2-equivalent

**N2O:**
```
N2O = Area (ha) × AGB (t/ha) × Cf × G_N2O × GWP_N2O / 1000
```
- `G_N2O = 0.21 g/kg` — N2O emission ratio (IPCC Table 2.5); grams of N2O released per kg of biomass burned, originating from nitrogen in plant tissue
- `GWP_N2O = 298` — Global Warming Potential of N2O over 100 years (IPCC AR4)

**Region-specific AGB values:**

| Region | Primary Forest (t/ha) | Secondary Forest (t/ha) | Reference |
|--------|-----------------------|-------------------------|-----------|
| Kalimantan | 357 | 134 | Kronseder et al. (2012) |
| Sumatra | 300 | 125 | Laumonier et al. (2010) |
| Papua | 400 | 155 | Asner et al. (2014) |
| Sulawesi, Java, others | 175–225 | 90–110 | IPCC default |

### 3. Soil Organic Carbon (SOC)
SOC emissions are estimated separately for peatland and mineral soil.

**Peatland:**
```
SOC_peat = Peat area (ha) × 55.0 tCO2e/ha
```
- `55.0 tCO2e/ha` — annual SOC emission factor for drained tropical peatland (IPCC Wetlands Supplement 2014, Table 4.4); peatlands store carbon accumulated over thousands of years and release it rapidly when disturbed

**Mineral soil:**
```
SOC_mineral = Mineral area (ha) × SOC_REF × (1 − FLU) × 3.667
```
- `SOC_REF = 47.4 t C/ha` — reference SOC stock for tropical mineral soils at 0–30 cm depth (IPCC Table 2.3)
- `FLU = 0.48` — land use change factor (IPCC Table 5.5); mineral soils lose approximately 48% of their carbon stock after deforestation, so the fraction released is `(1 − 0.48) = 0.52`

### 4. Total Emissions
```
Total = CO2 + CH4 + N2O + SOC_peat + SOC_mineral
```
All values are reported in tonnes of CO2-equivalent (tCO2e) and aggregated per year over the selected time range.

### References
- IPCC (2006). *Guidelines for National Greenhouse Gas Inventories, Volume 4: Agriculture, Forestry and Other Land Use.*
- IPCC (2013). *Supplement to the 2006 IPCC Guidelines: Wetlands.*
- Kronseder, K. et al. (2012). Above ground biomass estimation across forest types at different degradation levels in Central Kalimantan. *Forest Ecology and Management.*
- Laumonier, Y. et al. (2010). Erratum to: Ecosystem-scale CO2 fluxes from forests of the Sumatran lowlands. *Forest Ecology and Management.*
- Asner, G.P. et al. (2014). Targeted carbon conservation at national scales with high-resolution monitoring. *PNAS.*

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

