# data512_common_analysis

## Summary

This repository is the code, data, and analysis for the analysis of wildfire data. From the acres burned and distance from pullman, this project seeks to estimate the smoke impact on the city of Pullman

## AQI

This analysis uses the Air Quality System API which can be accessed [here](https://aqs.epa.gov/aqsweb/documents/data_api.html).

To access this API, it is necessary to create an account and generate API key. The way to do this can be seen in airquality.ipynb.

## Files

The code for gathering the fire data for analysis and smoke impact estimates can be found in load.ipynb. The files for the actual smoke estimate can be found in estimate.ipynb. airquality.ipynb is for gathering the AQI information for Pullman and it generates a file called aqi_10_worst_avg_2002-2020.json which contains the sum of the 10 worst AQI days for particulate matter 10 for each year. Finally, visualization.ipynb contains the code for visualizing some of the key information about wildfire in a 1250 mile radius around pullman.

## Data

The data for the wildfires since the 1800's can be found at the USGS website [here](https://www.sciencebase.gov/catalog/item/61aa537dd34eb622f699df81). It is the 'GeoJSON Files.zip' file. We use the 'USGS_Wildland_Fire_Combined_Dataset.json' file for analysis.

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

The wikipedia.ipynb file utilizes the Wikimedia API to collect information for files the U.S. cities csv. The analysis.ipynb loads the data generated from the previous file and creates tables for analysis.

## Research Implications

I was surprised by a number of the results found in analysis.ipynb, specifically in the representation in both per capita articles and per capita quality articles for small states like Wyoming and Alaska. I also expected a bias towards the east and west coasts, but the highest per capita region was the midwest which surprised me.

> **1. What biases did you expect to find in the data (before you started working with it), and why?**

I expected a few things going into this project. First, I expected that the original 13 colonies and some of the older states on the east coast to have both more articles and more quality articles. This is because during my time in New Jersey I noticed that there are many small burrows which I thought could drive up total numbers, but I am also aware that the 13 colonies have the longest history which could lead to there being more to write about which in turn leads to a higher quality prediciton. Additionally, I expected that western states would have more high quality articles since there are many tech people on the west coast who I deemed more likely to write for wikipedia.

> **2. What (potential) sources of bias did you discover in the course of your data processing and analysis?**

I am not sure that the tables I created revealed anything revolutionary about bias, but when I did an empircal dive into the data, I did see some possible evidence of bias. I randomly looked through some articles and it was fairly common that cities I had never heard of and that did not have subjectively good wikipedia pages were given a good grade by the ML model. This leads me to believe that the model has a bias in rating articles higher. This would mean that states or regions with more articles would probably have more "quality" articles too due to false positives.

> **3. How might a researcher supplement or transform this dataset to potentially correct for the limitations/biases you observed?**

I think one way a researcher could supplement this data is by crawling the individual pages talk section and pulling the human annotations where possible. Additionally, one could add in metrics like word count or encode common sections to capture the actual contents of the articles.

## License

MIT License
