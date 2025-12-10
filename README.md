# Technical Documentation

This document provides technical documentation for the Air Quality and COPD Prevalence Analysis project.

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

The project uses Snakemake for workflow automation.

**Rules**:
1. `check_integrity`: Verifies input data files
2. `clean_data`: Cleans both datasets
3. `integrate_datasets`: Merges cleaned datasets
4. `all`: Default target producing final merged dataset

**Execution**:
```bash
# Run complete workflow
snakemake --cores 1

# Run specific rule
snakemake clean_data --cores 1
```

## Data Pipeline

1. **Data Acquisition**: Download from CDC and EPA websites, store in `Data/input_data/`
2. **Data Cleaning**: Run `scripts/data_clean.py` to clean both datasets
3. **Data Integration**: Run `scripts/data_integration.py` to merge datasets
4. **Analysis**: Use `workflow.ipynb` for statistical analysis and visualization

## Usage

### Quick Start

1. Download data from Box link (see README.md) to `Data/input_data/`
2. Set up environment: `pip install -r requirements.txt`
3. Run workflow: `snakemake --cores 1`
4. Open notebook: `jupyter notebook workflow.ipynb`

### Running Scripts Directly

```bash
python scripts/data_clean.py
python scripts/data_integration.py
python scripts/check_integrity.py
```

## Troubleshooting

- **FileNotFoundError**: Ensure input data files are in `Data/input_data/`
- **ModuleNotFoundError**: Install packages with `pip install -r requirements.txt`
- **Snakemake "Nothing to be done"**: Files are up to date. Use `--forceall` to re-run
- **Jupyter errors**: Ensure merged dataset exists and kernel uses correct environment

For more details, see README.md and USER_GUIDE.md.
