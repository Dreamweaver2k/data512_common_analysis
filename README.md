# Wildfire Analysis Pullman, WA

## Summary

This repository is the code, data, and analysis for wildfire data, specifically targeting the city of Pullman, WA. The analysis herein found that the impact of smoke is projected to increase over the coming years. However, there is insufficient evidence to show that an increase in smoke impact also leads to an increase in the cost of staffing ambulatory healthcare service workers in Whitman County, WA.

<!-- ## AQI

This analysis uses the Air Quality System API which can be accessed [here](https://aqs.epa.gov/aqsweb/documents/data_api.html).

To access this API, it is necessary to create an account and generate API key. The way to do this can be seen in airquality.ipynb. -->

## Files

The code for gathering the fire data for analysis and smoke impact estimates can be found in load.ipynb. The files for the actual smoke estimate can be found in estimate.ipynb. airquality.ipynb is for gathering the AQI information for Pullman and it generates a file called aqi_10_worst_avg_2002-2020.json which contains the sum of the 10 worst AQI days for particulate matter 10 for each year. Finally, visualization.ipynb contains the code for visualizing some of the key information about wildfire in a 1250 mile radius around pullman.

Visualizations of acres burned by year, histogram of number of fires every 50 miles, AQI and smoke impact by year, and smoke impact predictions can be found in the figures folder.

## Data

### Sources

- US Environmental Protection Agency (EPA) Air Quality Service (AQS)
  > EPA provided air quality data for validating smoke estimates from wildfires.
  - data/aqi_10_worse_avg_2002-2020.json
- Bureau of Labor Statistics (BLS)
  > BLS provided data for ambulatory health care service wages for the county of Whitman, WA which is where my target city of Pullman, WA resides. Additionally, it provided CPI values for inflation adjustment.
  - data/wages_timeseries.csv
  - data/cpi.csv
- Federal Reserve Economic Data (FRED)
  > The FRED provided population data for the target city of Pullman, WA.
  - data/population_timeseries.csv
- United States Geological Survey (USGS)
  > USGS provided files for fire data. These files contain data about each fire including year, acres burned, fire type, geo data, and more.
  - data/fires.json
  - data/smoke_impact.csv

### Files

- data/fires.json
  > This file contains fire information for all wildfires and prescribed fires from 1964 to 2020. This information includes the following fields:
  - "USGS_Assigned_ID" - The unique ID assigned to a particular fire
  - "Fire_Year" - The year the fire occured.
  - "Assigned_Fire_Type" - The type of fire typically prescribed fire or wildfire
  - "GIS_Acres" - The number of acres burned in the fire.
  - GIS_Hectares" - The number of hectares burned in the fire
  - "Distance" - The distance of the closest point of the fire from Pullman, WA in miles.
- data/wages_timeseries.csv

  > This file contains average wage information by year for ambulatory health care service workers in Whitman County, WA from 2001-2022. It contains the following fields:

  - "Wages" - Average wage of ambulatory health care service workers in dollars.
  - "Year" - The year of interest.

- data/employee_timeseries.csv

  > This file contains employment information for ambulatory health care service workers in Whitman County, WA from 1990-2023 for the months of the fire season. This information includes the following fields:

  - "Employees" - The number of employees in ambulatory health care service for Whitman county in thousands.
  - "Month" - The month of interest.
  - "Year" - The year of interest.

- data/cpi.csv

  > This file contains CPI information for the years of 2000-2023 during the months of the fire season. This information includes the following fields:

  - "cpi" - The consumer price index for the given month and year.
  - "Month" - The month of interest.
  - "Year" - The year of interest.

- data/population_timeseries.csv
  > This file contains yearly population estimates for Pullman, WA from 1970-2022.
  - "DATE" - The date the population is estimated for.
  - "WAWHIT5POP" - The population estimate in thousands.

The data for the wildfires since the 1800's can be found at the USGS website [here](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81). It is the 'GeoJSON Files.zip' file. We use the 'USGS_Wildland_Fire_Combined_Dataset.json' file for analysis. This file is parsed to evaluate if each fire is in the time period of interest and if it is in the region of interest. If it is, the id, year, acres, hectares and fire type are saved in fires.json.

The airquality.ipynb generates aqi_10_worst_avg_2002-2020.json which is the sum of the 10 worst days of air quality for particulate matter 10 over the period of time where Pullman was routinely capturing these values. I chose to only look at particulate matter 10 because this is what is generated primarily from wildfires and things like ozone don't really impact it. I found this information [here](https://ww2.arb.ca.gov/resources/inhalable-particulate-matter-and-health).

The visualizations and estimates are calculated from fires.json and aqi_10_worst_avg_2002-2020.json.

## Code

### Data Acquisition

#### Files

- data_acquisition/airquality.ipynb
  > This file contains the code for scraping AQI data for Pullman, WA from the EPS by utilizing the APS API. To run this file, you will need to register your email and get an access token which is detailed in the file. The code generates 'aqi_10_worse_avg_2002-2020.json' in the data folder which the average of the 10 worst days for AQI during the fire season in Pullman between 2002 and 2020.
- data_acquisition/bls.ipynb
  > This file retrieves ambulatory healthcare service employee and wage data from the BLS. It generates the 'employee_timeseries.csv' and 'wages_timeseries.csv' in the data folder.
- data_acquisition/load.ipynb
  > This file loads the wildfire dataset and processes it to calculate the distance of the fires from the target city. To run this code, you will need to download the USGS fire dataset and point the code to it. This code generates the 'fire.json' file in the data folder.

### Analysis

#### Files

- analysis/healthcare_analysis.ipynb
  > The code in this file creates a multiple linear regression to evaluate the correlation of smoke impact on yearly expense for staffing ambulatory healthcare workers in Whitman County, WA.
- analysis/smoke_estimate.ipynb
  > The code in this file creates a smoke impact estimate using the formula $e^{-d+a}*a$ where a and d are the acres burned and distance of each fire scaled to be between 0 and 1. The code generates the 'smoke_impact_timeseries' file in the data folder. In addition to this estimate, the code projects the future smoke impact on Pullman, WA by using both a linear and SARIMA model.
- analysis/visualization.ipynb
  > The code in this file generates some key visualizations for analyzing fires and their distribution around Pullman, WA.

## Usage

First, download the [data]() and run load.ipynb. Then run airquality.ipynb. Then, either visualization.ipynb or estimate.ipynb can be run.
To run this pipeline end to end, I suggest running the files in the following order:

1. Data Acquisition
   a. data_acquisition/load.ipynb
   b. data_acquisition/bls.ipynb
   c. data_acquisition/airquality.ipynb
2. Download population data from [FRED](www.linktofred.com)
3. Analysis
   a. analysis/smoke_estimate.ipynb
   b. analysis/visualization.ipynb
   c. analysis/healthcare_analysis.ipynb

## Considerations

Going through the wildfire data to find the fires near your target city can take upwards of an hour. Keep this in mind when running load.ipynb.

When running airquality.ipynb, make sure to uncomment the code to get an access token for your email. This only needs done once.

Although the BLS API for wages should work for the given code, sometimes it does not. If you are having issues retrieving the wage data using the API, access it in the [dashboard](https://www.bls.gov/data/). Go to State and County Employment and Wages field in the Pay & Benefits section. Click on one screen. Query your data from their.

The healthcare analysis portion of this repository considers Whitman County, the county within which Pullman is located, rather than Pullman itself.

## Installation

To utilize this repository, you must first clone it:

```bash
git clone https://github.com/chandlerault/data512_common_analysis.git
cd ./data512_common_analysis
```

Now, it is necessary to download the proper environment. Please make sure you have [conda](https://conda.io/projects/conda/en/latest/index.html) installed on your system for the next step.

```bash
conda env create -n <your-environment-name> -f environment.yml
```

## License

MIT License
