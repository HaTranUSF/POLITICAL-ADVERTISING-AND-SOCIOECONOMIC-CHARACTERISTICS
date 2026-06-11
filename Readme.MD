# Data Documentation

This directory is where the project datasets live. Due to file size, raw source files are not committed to the repository. See below for how to obtain each dataset and what schema to expect.

---

## Datasets

### `Ads_final.csv`
**Source:** [Google Ads Transparency Center](https://adstransparency.google.com/)  
**Description:** Google political advertising spend, disaggregated by advertiser, state, date range, and targeting parameters.

Key columns:
| Column | Description |
|---|---|
| `State_Year_Key` | Bridge key: `state_code + year` (e.g., `FL2024`) |
| `state_code` | 2-letter U.S. state abbreviation (uppercased) |
| `date_range_start` | Ad run start date |
| `date_range_end` | Ad run end date |
| `spend_usd` | Total ad spend in USD |
| `age_targeting` | Age targeting group (nulls filled as `Broad Targeting`) |
| `gender_targeting` | Gender targeting (nulls filled as `Broad Targeting`) |

---

### `master_socioeconomic_panel.csv`
**Source:** [U.S. Census Bureau — American Community Survey (ACS 5-Year)](https://data.census.gov/)  
**Description:** State-level socioeconomic indicators, 2020–2024.

Key columns:
| Column | Description |
|---|---|
| `State_Year_Key` | Bridge key: `state_code + year` |
| `State` | 2-letter state abbreviation |
| `Year` | Census year (2020–2024) |
| `Median_Income` | Median household income (dollars) — from ACS S1901 |
| `Poverty_Rate_Pct` | % of population below poverty line — from ACS S1701 |
| `Pct_Bachelors_Higher` | % of population 25+ with bachelor's degree or higher — from ACS S1501 |
| `Total_Population` | Total state population — from ACS DP05 |
| `Voter_Age_Population` | Population 18 years and over — from ACS DP05 |

---

### `political_leaning_by_election.csv`
**Source:** Wikipedia (U.S. Presidential and Senate election results)  
**Description:** State-level political outcome (D/R) for the 2020 and 2024 election cycles.

Key columns:
| Column | Description |
|---|---|
| `State` | 2-letter state abbreviation |
| `Year` | Election year |
| `Party` | Winning party (`D` or `R`) |
| `Type` | `Presidential` or `Senate` |
| `Battleground` | Boolean — whether state is considered a swing state |

---

## How to Reproduce the Panel

1. Download ACS 5-year files from [data.census.gov](https://data.census.gov/) for each year:
   - Income: table `S1901`
   - Poverty: table `S1701`
   - Education: table `S1501`
   - Population: table `DP05`

2. Run the cleaning notebooks in order:
   ```
   notebooks/01_data_cleaning.ipynb   # per-indicator Census cleaning
   notebooks/02_data_merge.ipynb      # merge audit + bridge key
   notebooks/03_master_pipeline.ipynb # final panel assembly
   ```

3. The final `master_socioeconomic_panel.csv` and `Ads_final.csv` are the inputs to the Tableau workbook.
