# data512_common_analysis

## NOTE

**The reflection can be found in 'Part 1 -- Common Analysis Reflection.pdf'. There was no way to submit two links so I submitted the github repo.**

## Summary

This repository is the code, data, and analysis for the analysis of wildfire data. This project is interested in the years of 1963 to present day and fires up to 1250 miles from the city of Pullman. From the acres burned and distance from pullman, this project seeks to estimate the smoke impact on the city of Pullman.

## AQI

This analysis uses the Air Quality System API which can be accessed [here](https://aqs.epa.gov/aqsweb/documents/data_api.html).

To access this API, it is necessary to create an account and generate API key. The way to do this can be seen in airquality.ipynb.

## Files

The code for gathering the fire data for analysis and smoke impact estimates can be found in load.ipynb. The files for the actual smoke estimate can be found in estimate.ipynb. airquality.ipynb is for gathering the AQI information for Pullman and it generates a file called aqi_10_worst_avg_2002-2020.json which contains the sum of the 10 worst AQI days for particulate matter 10 for each year. Finally, visualization.ipynb contains the code for visualizing some of the key information about wildfire in a 1250 mile radius around pullman.

Visualizations of acres burned by year, histogram of number of fires every 50 miles, AQI and smoke impact by year, and smoke impact predictions can be found in the figures folder.

## Data

The data for the wildfires since the 1800's can be found at the USGS website [here](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81). It is the 'GeoJSON Files.zip' file. We use the 'USGS_Wildland_Fire_Combined_Dataset.json' file for analysis. This file is parsed to evaluate if each fire is in the time period of interest and if it is in the region of interest. If it is, the id, year, acres, hectares and fire type are saved in fires.json.

The airquality.ipynb generates aqi_10_worst_avg_2002-2020.json which is the sum of the 10 worst days of air quality for particulate matter 10 over the period of time where Pullman was routinely capturing these values. I chose to only look at particulate matter 10 because this is what is generated primarily from wildfires and things like ozone don't really impact it. I found this information [here](https://ww2.arb.ca.gov/resources/inhalable-particulate-matter-and-health).

The visualizations and estimates are calculated from fires.json and aqi_10_worst_avg_2002-2020.json.

## Considerations

Going through the wildfire data to find the fires near your target city can take upwards of an hour. Keep this in mind when running load.ipynb.

When running airquality.ipynb, make sure to uncomment the code to get an access token for your email. This only needs done once.

## Installation

To utilize this repository, you must first clone it:

```bash
git clone https://github.com/Dreamweaver2k/data512_common_analysis.git
cd ./data512_common_analysis
```

Now, it is necessary to download the proper environment. Please make sure you have [conda](https://conda.io/projects/conda/en/latest/index.html) installed on your system for the next step.

```bash
conda env create -n <your-environment-name> -f environment.yml
```

## Usage

First, download the [data]() and run load.ipynb. Then run airquality.ipynb. Then, either visualization.ipynb or estimate.ipynb can be run.

## License

MIT License
