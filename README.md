# Air Quality Across Indian Cities, 2015-2020: A Data Visualization Study

An end-to-end visual analysis of daily air-quality readings from 26 Indian cities, built with Pandas, Matplotlib, Seaborn and Plotly. The project asks 22 specific questions about pollution levels, seasonality, data quality and the 2020 COVID-19 lockdown. Every chart answers one stated question and is followed by a short written insight.

## Dataset
- **Source:** Kaggle, "Air Quality Data in India (2015-2020)" (`city_day.csv`), compiled from Central Pollution Control Board (CPCB) data.
- **Size:** 29,531 daily rows, 26 cities, 1 Jan 2015 to 1 Jul 2020, 16 columns (PM2.5, PM10, NO, NO2, NOx, NH3, CO, SO2, O3, Benzene, Toluene, Xylene, AQI, AQI_Bucket).
- The raw file is not committed to this repository. Download it from Kaggle and place it in `data/raw/`.

## Key findings
1. **Seasonality is strong.** Median PM2.5 across the 8 core cities is 81 µg/m³ in winter, 76 post-monsoon, 53 in summer and 35 in monsoon. In Patna, Lucknow and Delhi the winter median is 196, 171 and 169 µg/m³, against 53 in Chennai.
2. **The north exceeds the national standard most days.** PM2.5 was above 60 µg/m³ (the 24-hour NAAQS) on 72.5% of measured days in Delhi, 68.3% in Patna and 66.7% in Lucknow, against 10.0% in Bengaluru.
3. **AQI category mix.** The share of days rated Poor or worse was 65.1% in Delhi, 53.5% in Patna and 49.4% in Lucknow, against 2.1% to 6.7% in Bengaluru, Hyderabad, Mumbai and Chennai.
4. **Delhi's seasonal pattern grows with the level.** A multiplicative decomposition fits better than an additive one (residual-trend correlation 0.04 vs 0.20). The monthly seasonal peak falls in November, not in December or January.
5. **Lockdown window (25 Mar to 30 Jun 2020).** Mean PM2.5 was 31% to 56% lower than on the same dates in 2016-19 in the 7 cities with enough baseline data (Delhi 48%). Against 2019 alone the drops are smaller (25% to 54%; Delhi 40%), because pollution was already falling before 2020. These are observed differences, not proof of cause, since weather also differs between years.
6. **Pollutants.** PM2.5 and PM10 correlate at 0.86 (Spearman). The PM2.5-NO2 correlation varies a lot by city, from 0.09 in Patna to 0.79 in Mumbai, so a single pooled figure would mislead.

## Data quality decisions
| Issue found | Action |
|---|---|
| Xylene 61% missing | Dropped the column |
| Other pollutants 7% to 38% missing | Interpolated only gaps of 3 days or fewer, within each city; longer gaps left empty |
| 18 of 26 cities have short or patchy records | Kept 8 "core" cities with at least 90% date coverage for all cross-city comparisons |
| CO identical to NO in 1,322 rows (mostly Ahmedabad) | Set the copied CO values to missing |
| Ahmedabad AQI tracks NO (rho 0.88) rather than PM2.5 (0.50); Delhi's tracks PM2.5 (0.92) | Excluded Ahmedabad's AQI from all AQI charts; kept its PM2.5, PM10 and NO2 |
| PM10 capped at exactly 1000; AQI up to 2049 | Flagged, not deleted |
| Months with fewer than 20 valid days | Shown as gaps, not averaged |

## Techniques demonstrated
Data preparation (dtype conversion, derived columns, groupby, crosstab, pivot_table, exported clean CSV); histograms, KDE, ECDF, skewness, box and violin plots; multivariate scatter (colour, marker, size, transparency); correlation heatmap; faceting; resampling, rolling averages and seasonal decomposition; grouped, stacked and normalized bar charts and subplots; Plotly interactivity (hover, dropdown, range slider, legend toggling) and a bubble map.

## Chart critique
The raw-scale PM2.5 histogram hides the shape of the data behind a long right tail. Clipping at the 99th percentile (disclosed) and a log scale fix this. A stacked bar of absolute AQI counts mixes pollution level with the number of measured days, so a normalized version with sample sizes replaces it.

## Limitations
- Coverage is uneven. Mumbai has PM2.5 readings on only 39% of days, and Ahmedabad on 70%.
- Gap-filling by interpolation is an estimate, and it is limited to 3 days.
- The lockdown comparison uses 1 to 4 baseline years per city and cannot separate lockdown effects from weather.
- A few NO2 readings below 1 µg/m³ look like instrument errors and were not removed.
- Bubble-map coordinates are entered by hand (city centres). The data is city-level, so there is no state choropleth.
- The cause of Ahmedabad's AQI problem is a hypothesis (the copied CO column), not a confirmed finding.

## Repository structure
```
india-air-quality/
├── data/raw/          # city_day.csv (download from Kaggle)
├── data/processed/    # city_day_clean.csv
├── notebooks/         # 01_air_quality_analysis.ipynb
├── outputs/figures/   # saved charts (PNG)
├── outputs/           # interactive HTML charts
├── requirements.txt
└── README.md
```

## Setup
```
pip install -r requirements.txt
jupyter notebook notebooks/01_air_quality_analysis.ipynb
```
Run the notebook from top to bottom. Figures are saved to `outputs/figures/` and interactive charts to `outputs/`.

## Screenshots
![PM2.5 distribution](outputs/figures/01_pm25_distribution.png)
![Seasonal medians by city](outputs/figures/11_pm25_grouped_city_season.png)
![AQI share by city](outputs/figures/13_aqi_stacked_normalized.png)
![Lockdown change](outputs/figures/10_lockdown_change.png)
![PM2.5 vs NO2 by city](outputs/figures/18_pm25_no2_facets.png)

## Credits
Data: Central Pollution Control Board, India, as compiled on Kaggle.
