# Data Dictionary

This document describes variables in the input datasets and the merged dataset.

## Input Datasets

### CDC COPD Prevalence Dataset

**File**: Data/input_data/County_COPD_prevalence.csv (original) or Data/cleaned/copd_clean.csv (cleaned)

**Records**: 3,147 counties (original), varies after cleaning

**All Original Variables**:
- **FullGeoName** (String): Combined geographic label (state abbreviation + county name)
- **LocationID** (String): Unique numeric code representing the county
- **Public_Health_Jurisdiction** (String): State or jurisdiction responsible for the record
- **StateDesc** (String): Full name of the state
- **County** (String): Full name of the county
- **Percent_COPD** (Numeric): Percentage of adults diagnosed with COPD in the county (Range: 3.3% to 13.3%)
- **95% Confidence Interval** (String): Range of values reflecting statistical uncertainty around estimated COPD prevalence
- **Quartile** (String): Quartile classification of COPD prevalence

**Variables Used in Merged Dataset**:
- State (mapped from StateDesc)
- County (standardized, no " County" suffix)
- Percent_COPD
- Quartile

**Citation**: Centers for Disease Control and Prevention. (2021). County-Level Estimates of COPD Prevalence. Retrieved from https://www.cdc.gov/copd/php/case-reporting/county-level-estimates-in-copd.html

### EPA Air Quality Dataset

**File**: Data/input_data/annual_aqi_by_county_2021.csv (original) or Data/cleaned/air_clean.csv (cleaned)

**Records**: 1,006 counties (original), varies after cleaning

**All Original Variables**:
- **State** (String): Full name of the U.S. state
- **County** (String): Full name of the county
- **Year** (Integer): Year of data collection (2021)
- **Days with AQI** (Integer): Number of days with AQI data
- **Good Days** (Integer): Number of days with Good AQI (0-50)
- **Moderate Days** (Integer): Number of days with Moderate AQI (51-100)
- **Unhealthy for Sensitive Groups Days** (Integer): Number of days with Unhealthy for Sensitive Groups AQI (101-150)
- **Unhealthy Days** (Integer): Number of days with Unhealthy AQI (151-200)
- **Very Unhealthy Days** (Integer): Number of days with Very Unhealthy AQI (201-300)
- **Hazardous Days** (Integer): Number of days with Hazardous AQI (301-500)
- **Max AQI** (Integer): Maximum AQI value for the year
- **90th Percentile AQI** (Integer): 90th percentile AQI value
- **Median AQI** (Integer): Median Air Quality Index value (Range: 3 to 122)
- **Days CO** (Integer): Number of days with elevated carbon monoxide levels
- **Days NO2** (Integer): Number of days with elevated nitrogen dioxide levels (Range: 0 to 364)
- **Days Ozone** (Integer): Number of days with elevated ozone levels (Range: 0 to 365)
- **Days PM2.5** (Integer): Number of days with elevated PM2.5 levels (Range: 0 to 365)
- **Days PM10** (Integer): Number of days with elevated PM10 levels (Range: 0 to 365)

**Variables Used in Merged Dataset**:
- State
- County (standardized, no " County" suffix)
- Year
- Median AQI
- Days Ozone
- Days PM2.5
- Days NO2
- Days PM10

**Citation**: Environmental Protection Agency. (2021). Annual Air Quality Index by County. Retrieved from https://aqs.epa.gov/aqsweb/airdata/download_files.html#Annual

## Merged Dataset

### Combined Dataset

**File**: Data/merge_data/merged_dataset.csv

**Records**: 923 counties (after inner join of both datasets)

**Variables** (10 total):

**Geographic Identifiers**:
- **State** (String): Full name of the U.S. state
- **County** (String): County name (standardized, no " County" suffix)

**Health Outcome** (from CDC):
- **Percent_COPD** (Numeric): Percentage of adults diagnosed with COPD in the county (Range: 3.3% to 13.3%)
- **Quartile** (String): Quartile classification of COPD prevalence

**Temporal** (from EPA):
- **Year** (Integer): Year of data collection (2021)

**Air Quality Variables** (from EPA):
- **Median AQI** (Integer): Median Air Quality Index value (Range: 3 to 122)
- **Days Ozone** (Integer): Number of days with elevated ozone levels (Range: 0 to 365)
- **Days PM2.5** (Integer): Number of days with elevated PM2.5 levels (Range: 0 to 365)
- **Days NO2** (Integer): Number of days with elevated nitrogen dioxide levels (Range: 0 to 364)
- **Days PM10** (Integer): Number of days with elevated PM10 levels (Range: 0 to 365)

**Merge Method**: Inner join on State and County columns
