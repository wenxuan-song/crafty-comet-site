# Technical Documentation

This document provides technical documentation for the Air Quality and COPD Prevalence Analysis project based on the workflow.ipynb.

## Data Acquisition

There are two ways to obtain the input data files:

### Option 1: Download from Box (Recommended)
- Download from: https://uofi.box.com/s/q60tyfy78aamuya8340uzchizcm6p2gl
- Place files in Data/input_data/:
  - annual_aqi_by_county_2021.csv
  - County_COPD_prevalence.csv

### Option 2: Download from Source Websites
- EPA AQI data: https://aqs.epa.gov/aqsweb/airdata/download_files.html#Annual
  - Download annual_aqi_by_county_2021.zip
  - Extract the annual_aqi_by_county_2021.csv
  - Save to Data/input_data/annual_aqi_by_county_2021.csv
- CDC COPD data: https://www.cdc.gov/copd/php/case-reporting/county-level-estimates-in-copd.html
  - Download CSV
  - Save to Data/input_data/County_COPD_prevalence.csv

## Script Documentation

### scripts/data_clean.py

Cleans and standardizes raw input data from CDC and EPA sources.

**Functions**:
- `clean_copd(input_path, output_path)`: Cleans CDC COPD prevalence dataset
  - Removes " County" suffix from county names
  - Converts Percent_COPD to numeric
  - Removes missing values
  - Outputs: State, County, Percent_COPD, Quartile

- `clean_air(input_path, output_path)`: Cleans EPA air quality dataset
  - Removes " County" suffix from county names
  - Converts numeric columns
  - Removes missing values
  - Outputs: State, County, Year, Median AQI, Days Ozone, Days PM2.5, Days NO2, Days PM10

### scripts/data_integration.py

Integrates cleaned COPD and air quality datasets.

**Functions**:
- `integrate_datasets(copd_path, air_path, merged_path)`: Merges datasets on State and County
  - Performs inner join
  - Outputs merged dataset with 923 counties

### scripts/check_integrity.py

Verifies data integrity using SHA-256 checksums.

**Functions**:
- `sha256(file)`: Calculates SHA-256 hash of a file

## Workflow Documentation

### Snakemake Workflow

The project uses Snakemake for workflow automation. Execute the workflow using:

```bash
snakemake --cores 1
```

**Rules**:
1. `check_integrity`: Verifies input data files
2. `clean_data`: Cleans both datasets
3. `integrate_datasets`: Merges cleaned datasets
4. `all`: Default target producing final merged dataset

### Analysis Workflow (workflow.ipynb)

The Jupyter notebook workflow.ipynb contains the complete analysis:

1. **Data Loading and Exploration**: Load merged dataset and check structure
2. **Statistics**: Summary statistics for all variables
3. **Correlation Analysis**: Correlation matrix between COPD and air quality variables
4. **Visualizations**:
   - Correlation heatmap
   - Data distributions (histograms)
   - Scatter plots for each pollutant vs COPD
   - Direct comparison of pollutant correlations
5. **Multicollinearity Diagnostics**: VIF (Variance Inflation Factor) analysis
6. **OLS Regression**: Multiple regression model with all air quality variables

## Usage

### Quick Start

1. **Get data** (choose one option above)
2. **Set up environment**: `pip install -r requirements.txt`
3. **Run Snakemake workflow**: `snakemake --cores 1`
4. **Open notebook**: `jupyter notebook workflow.ipynb`
5. **Run all cells** in the notebook to execute the complete analysis

### Running Scripts Directly

```bash
python scripts/data_clean.py
python scripts/data_integration.py
python scripts/check_integrity.py
```

## Troubleshooting

- **FileNotFoundError**: Ensure input data files are in Data/input_data/
- **ModuleNotFoundError**: Install packages with `pip install -r requirements.txt`
- **Snakemake "Nothing to be done"**: Files are up to date. Use `--forceall` to re-run
- **Jupyter errors**: Ensure merged dataset exists and kernel uses correct environment

For more details, see README.md and USER_GUIDE.md.
