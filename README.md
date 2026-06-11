# Political Advertising & Socioeconomic Characteristics
 
**An Analysis of Google Political Ad Spending Across U.S. States, 2020 to 2024**
 
ISM4930 | Team Advertising & Commerce
Quan Tran, Ha Tran, Quang Nguyen, Kimsing Lor, Yusuke Yoshizawa
University of South Florida, Spring 2026
 
---
 
## Overview
 
This project looks at whether Google political advertising spending is connected to the socioeconomic characteristics of U.S. states, and how those patterns shifted between the 2020 and 2024 presidential election cycles.
 
**Two core research questions:**
 
1. Does political advertising spending on Google vary across U.S. states, and how did those patterns change between 2020 and 2024?
2. Are states with lower median household income or higher educational attainment targeted with more political ad spending?
---
 
## Key Findings
 
- **Swing states drive most of the spending.** Battleground states, especially Pennsylvania, Michigan, and Wisconsin, saw much higher ad spend per capita in 2024 compared to 2020.
- **Pennsylvania** received over **$37.7M** in average Google political ad spend in 2024, nearly 7x its 2020 levels.
- **Median income does not predict spending.** High-income swing states still attracted massive budgets because of their electoral importance, not their demographics.
- **Education shows a weak pattern.** States with higher bachelor's degree attainment lean slightly toward Democratic spending, but swing state status matters far more than education levels.
- **National totals stayed stable; swing state variance grew.** Total political ad spend was similar across both cycles, but the gap between battleground and safe states widened significantly in 2024.
---
 
## Data Sources
 
### Google Ads Transparency Center (`Ads_final.csv`)
Source: [adstransparency.google.com](https://adstransparency.google.com/)
 
Google publishes political advertising data as part of its transparency efforts. This dataset includes every political ad run on Google platforms in the U.S., broken down by advertiser, state, date range, spend range, and targeting parameters (age, gender). We downloaded ad records covering 2020 and 2024 election periods and cleaned state codes, filled broad targeting nulls, and created a composite join key from state and year.
 
### U.S. Census Bureau ACS (`master_socioeconomic_panel.csv`)
Source: [data.census.gov](https://data.census.gov/)
 
The American Community Survey (ACS) is an annual Census Bureau survey that collects detailed social and economic data for every U.S. state. We pulled four tables for each year from 2020 to 2024:
 
- **S1901** (Income) -- median household income by state
- **S1701** (Poverty) -- percent of population below the poverty line
- **S1501** (Education) -- percent of adults 25+ with a bachelor's degree or higher
- **DP05** (Demographics) -- total population and voting-age population (18+)
These four tables were cleaned separately using automated Python functions and then merged into a single panel by state and year.
 
### Wikipedia (`political_leaning_by_election.csv`)
Source: Wikipedia (U.S. presidential and Senate election results pages)
 
Used to label each state as Democratic or Republican for the 2020 and 2024 election cycles. This allowed the Tableau dashboards to color-code spending by party outcome and identify which states flipped between cycles.
 
---
 
## Repository Structure
 
```
political-ads-project/
|
|-- data/
|   |-- README.md               # Data schema documentation
|
|-- notebooks/
|   |-- 01_data_cleaning.ipynb  # Census data cleaning (income, poverty, education, population)
|   |-- 02_data_merge.ipynb     # State_Year_Key bridge merge and audit
|   |-- 03_master_pipeline.ipynb# Final panel assembly and abbreviation mapping
|
|-- visualizations/
|   |-- Final_Project.twbx      # Tableau workbook (four interactive dashboards)
|
|-- report/
|   |-- Final_Report.docx       # Full written report
|   |-- Final_Presentation.pdf  # Slide deck
|
|-- index.html                  # Project portfolio page (GitHub Pages)
|-- README.md
```
 
---
 
## Methodology
 
### Data Pipeline
 
```
Census ACS Files (S1901, S1701, S1501, DP05)
        |
  Python cleaning (per-indicator functions)
        |
  income_panel_2020_2024.csv
  poverty_panel_2020_2024.csv
  education_panel_2020_2024.csv
  population_panel_2020_2024.csv
        |
  master_socioeconomic_panel.csv  +  Ads_final.csv
        |
  State_Year_Key bridge merge (e.g. "FL2024")
        |
  Tableau dashboards + analysis
```
 
### Key Engineering Decisions
 
- **State_Year_Key**: A composite key (`state_code + year`, e.g. `FL2024`) was created to join the Google Ads dataset to the Census panel reliably across five years. A null audit confirmed no unmatched rows.
- **DC normalization**: The Census Bureau stores DC under an extremely long official name. This was replaced with `"DC"` before any merging.
- **Per capita normalization**: Ad spend was divided by voter-age population (18+) to make fair comparisons across states of different sizes.
- **Broad Targeting fill**: Null values in `age_targeting` and `gender_targeting` were filled with `"Broad Targeting"` to keep all spend rows in the dataset.
---
 
## Dashboards (Tableau)
 
The Tableau workbook (`Final_Project.twbx`) has four dashboards:
 
| Dashboard | Description |
|---|---|
| **Overall Snapshot** | U.S. choropleth of ad spend per capita with a political swing bar chart for 2020 vs. 2024 |
| **Swing States Deep Dive** | Absolute vs. relative spending side by side for battleground states |
| **Median Income vs. Ad Spend** | Scatter plots by election year for all states and swing states only |
| **Educational Attainment vs. Ad Spend** | Percent with bachelor's degree or higher vs. spend per capita |
 
---
 
## Tech Stack
 
- **Python** (pandas, glob, re) -- data cleaning and pipeline
- **Tableau** -- interactive dashboards
- **Google Colab** -- collaborative notebook environment
- **U.S. Census Bureau ACS** -- socioeconomic panel data
- **Google Ads Transparency Center** -- political ad spend data
---
 
## Team
 
| Name | Role |
|---|---|
| Ha Tran | Data pipeline, Census cleaning, Tableau dashboards |
| Quan Tran | Data pipeline, Census cleaning, Tableau dashboards|
| Quang Nguyen | Data analysis, Tableau dashboards, presentation |
| Kimsing Lor | Data analysis, Tableau dashboards, presentation |
| Yusuke Yoshizawa | Tableau dashboards|
 
---
 
*University of South Florida | ISM4930 | Spring 2026*
