# Technical Documentation

This document provides technical documentation for the Air Quality and COPD Prevalence Analysis project based on the workflow.ipynb.

## Getting Started (For TAs)

### Step 1: Data Acquisition

There are two ways to obtain the input data files:

**Option 1: Download from Box (Recommended)**
- Download from: https://uofi.box.com/s/q60tyfy78aamuya8340uzchizcm6p2gl
- Place files in Data/input_data/:
  - annual_aqi_by_county_2021.csv
  - County_COPD_prevalence.csv

**Option 2: Download from Source Websites**
- EPA AQI data: https://aqs.epa.gov/aqsweb/airdata/download_files.html#Annual
  - Download annual_aqi_by_county_2021.zip
  - Extract the annual_aqi_by_county_2021.csv
  - Save to Data/input_data/annual_aqi_by_county_2021.csv
- CDC COPD data: https://www.cdc.gov/copd/php/case-reporting/county-level-estimates-in-copd.html
  - Download CSV
  - Save to Data/input_data/County_COPD_prevalence.csv

### Step 2: Set Up Environment

pip install -r requirements.txt

### Step 3: Open Jupyter Notebook: workflow.ipynb

### Step 4: Run Snakemake Commands in Notebook

The notebook contains two Snakemake commands that must be run in order:

**First Snakemake Command:**

!snakemake --cores 1

**What this does:**
- Cleans both datasets (EPA and CDC) using scripts/data_clean.py
- Integrates the cleaned datasets using scripts/data_integration.py
- Creates Data/cleaned/air_clean.csv
- Creates Data/cleaned/copd_clean.csv
- Creates Data/merge_data/merged_dataset.csv

**Second Snakemake Command:**

!snakemake --cores 1 --forceall check_integrity

**What this does:**
- Verifies data integrity using SHA-256 checksums
- Checks input data files and merged dataset
- Prints three SHA-256 checksums to the console

**Expected Output:**
When you run this command, you should see the following checksums:

8c4d462e9db5166f2e56e59fe2a3de1c56d6b93b278bbb9280a667e5e9d811d5
2fdc551ec38d346d3f3d9609f50710c85eedb9daea0e6d079a8249a12837920f
c2024080939ff712552681fcd3eb5e549b84a9aa2d51046bf84cef371154c96a

These correspond to:
1. annual_aqi_by_county_2021.csv
2. County_COPD_prevalence.csv
3. merged_dataset.csv

If they match, it means the data files haven't been damaged.

### Step 5: Run Analysis Cells in Notebook

After running the Snakemake commands, run the remaining cells in the notebook to:

1. **Load and explore data**: Check dataset structure, missing values, data types
2. **Calculate summary statistics**: Get descriptive statistics for all variables
3. **Perform correlation analysis**: Calculate correlation matrix
4. **Generate visualizations**: Run the visualization cells to create the following plots:
   - **Graph 1: Correlation Heatmap - Relationships Between COPD and Air Quality Variables**
     - Run the cell to create correlation heatmap
   - **Graph 2: Data Distributions**
     - Run the cell to create distribution histograms
   - **Graph 3: COPD vs Each Pollutant**
     - Run the cell to create scatter plots for each pollutant vs COPD
   - **Graph 4: Direct Comparison - Which Pollutant is Most Closely Linked to COPD? (Answering RQ2)**
     - Run the cell to create bar chart comparing correlations
5. **Run multicollinearity diagnostics**: Calculate VIF values
6. **Perform OLS regression**: Run multiple regression analysis




## Script Documentation

### scripts/data_clean.py

Cleans and standardizes raw input data from CDC and EPA sources.

**Functions**:
- clean_copd(input_path, output_path): Cleans CDC COPD prevalence dataset
  - Removes " County" suffix from county names
  - Converts Percent_COPD to numeric
  - Removes missing values
  - Outputs: State, County, Percent_COPD, Quartile

- clean_air(input_path, output_path): Cleans EPA air quality dataset
  - Removes " County" suffix from county names
  - Converts numeric columns
  - Removes missing values
  - Outputs: State, County, Year, Median AQI, Days Ozone, Days PM2.5, Days NO2, Days PM10

### scripts/data_integration.py

Integrates cleaned COPD and air quality datasets.

**Functions**:
- integrate_datasets(copd_path, air_path, merged_path): Merges datasets on State and County
  - Performs inner join
  - Outputs merged dataset with 923 counties

### scripts/check_integrity.py

Verifies data integrity using SHA-256 checksums.

**Functions**:
- sha256(file): Calculates SHA-256 hash of a file
