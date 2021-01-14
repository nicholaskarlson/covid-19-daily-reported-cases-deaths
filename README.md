# Sourcing daily COVID-19 cases and deaths

In December 2020, the [ECDC](https://www.ecdc.europa.eu/en/publications-data/download-todays-data-geographic-distribution-covid-19-cases-worldwide) decided to stop publishing daily new cases and deaths per country. Since many of our trends depend on this kind of data, we needed replacement. The [Johns Hopkins University's Center for Systems Science and Engineering (CSSE)](https://github.com/CSSEGISandData/COVID-19) publishes data every day, however they use the cumulative number of cases and deaths by country.

We wrote a script that converts the cumulative case numbers back to daily case numbers. 

This dataset of daily case numbers is then appended to last continuous one published by the ECDC containing daily reported cases and deaths by country reaching back to the beginning of 2020.

## Contact

If you are interested in this dataset, please contact us at data-team[at]dw.com so we can find a way to provide you with this. 


## Disclaimer

We do this to the best of our knowledge and abilities, which sure doens't mean it's perfect and there cannot be any mistakes. If you notice a (potential) mistake, please reach out to us.

DW does not make any (and hereby disclaims all) liability, responsibility and warranties regarding the accuracy, completeness, fitness for a particular purpose, quality, merchantability and/or non-infringement of any information provided in these datasets. 


## Data processing

We convert the JHU-CSSE's cumulative case numbers back to daily numbers by getting today’s data and yesterday’s data and calculating the difference and saving it to a new dataset.

If you are interested in how we are processing the JHU data, please refer to the [Scraper Script](Scraper_JHU_CSSE.py).

All countries are processed the same way, with four exceptions:

* for `Denmark` and `France`: we only *include* rows where the column `Province_State` is `NaN` provided as these entries seem to match the national numbers reported

* for `Germany` and the `Netherlands`: we *exclude* rows where `Province_State` is `Unknown` since we could not reproduce where the numbers in these rows can be attributed to


## Output and dataset structures

The scraper saves three different datasets. You can check out sample datasets in the [data folder](/data).

### Dataset: `jhu-csse-newly-reported-cases_2020-12-14.csv`

Contains the daily new figures for cases and deaths on the date given in the filename. It does not include the data of previous dates.


|  column name |  data type |  description |
|--------------|-----------------|--------------------------------|
| cases |  integer | number of newly reported cases on the given date |
| deaths |  integer | number of newly reported deaths on the given date |
| countriesAndTerritories | string | full length country name |
| dateRep| date | date in `yyyy-mm-dd` format; will be the same for all entries |
| countryterritoryCode | string | country identifier ISO alpha 3 code |



### Dataset: `ecdc_base-plus_update.csv`

Contains the daily reported figures for cases and deaths -- back to the beginning of reporting (beginning of 2020 for most countries).

|  column name |  data type |  description |
|--------------|-----------------|--------------------------------|
| cases |  integer | number of newly reported cases on the given date |
| deaths |  integer | number of newly reported deaths on the given date |
| countriesAndTerritories | string | full length country name |
| dateRep| date | date in `yyyy-mm-dd` format; will range from Dec 2019 for single countries up until latest available data|
| countryterritoryCode | string | country identifier ISO alpha 3 code |


### Dataset: `PROCESSED_daily_cases_ecdc_jhu_2020-12-14.csv`

Contains the same as the previous dataset, just with two additional columns - population data (of 2019, sourced from the [World Bank](https://data.worldbank.org/indicator/SP.POP.TOTL)) and the continent (sourced from the [UN](https://unstats.un.org/unsd/methodology/m49/)) added to it.

|  column name |  data type |  description |
|--------------|-----------------|--------------------------------|
| cases |  integer | number of newly reported cases on the given date |
| deaths |  integer | number of newly reported deaths on the given date |
| countriesAndTerritories | string | full length country name |
| dateRep| date | date in `yyyy-mm-dd` format; will range from Dec 2019 for single countries up until latest available data|
| countryterritoryCode | string | country identifier ISO alpha 3 code |
| population | integer | population as of 2019, according to World Bank |
| continent | string | Geographic continent assignment, based on the UN |



## FAQ

### If I want to use this data, how do I get it?

Please contact us at data-team[at]dw[dot]com so we can find a way to provide you with this. 

### If I use your data, what is the source I should give?

The source credit for the data should be `ECDC, JHU-CSSE` as the dataset consists of ECDC data up until mid December and the JHU data from then on.

### I noticed a mismatch between your dataset and national data

Yes, we did too. For the first two weeks of December, we compared the daily data by ECDC with the reverse-engineered daily data by JHU-CSSE. During that time, we too noticed a mismatch, with the JHU data being higher than the daily cases the ECDC listed. We could not find a fix for this problem, so the only thing we can do is make you aware of it.

In our reporting, we decided to check additional national sources if the accuracy of a certain number is essential to our story.
 


