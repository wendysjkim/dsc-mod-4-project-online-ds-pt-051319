
# Module 4 Final Project 
# ARIMA Modeling: Real Estate Investment Recommendations

For this project, I tried to answer a seemingly simple question:

> what are the top 5 best zipcodes for us to invest in?

I analyzed zipcode-level median housing sales data from Zillowe [Zillow Research Page](https://www.zillow.com/research/data/). found in the file `zillow_data.csv`. I used ARIMA modeling to identify 5 zipcodes with highest predicted rate of returns in the three biggest cities (by number of zipcodes). The work is found in the file `analysis.ipynb`.

There were 14,723 zipcodes in total. I focused on top 3 cities with the most number of zipcodes. The top 3 cities with the most number of zipcodes are New York, Los Angeles and Houston. The data is consistent with [the population size](http://worldpopulationreview.com/us-cities/) of each city.

> Largest Cities in the US by Population (2019)
> * New York City, NY (Population: 8,601,186)
> * Los Angeles, CA (Population: 4,057,841)
> * Chicago, IL (Population: 2,679,044)
> * Houston, TX (Population: 2,359,480)

<img src='https://github.com/wendysjkim/dsc-mod-4-project-online-ds-pt-051319/blob/work/images/0.%20Zipcode%20Count.png'>
<img src='https://github.com/wendysjkim/dsc-mod-4-project-online-ds-pt-051319/blob/work/images/1.%20Top%203%20Bubble.png'>
  
I further filtered down the samples sizes to 100 zipcodes, by looking at the growth rates in the past 3 years.

## EDA and Visualizations
There were 4 outliers in NYC whose prices were significantly higher than the rest of the regions. More interestingly, these zipcodes didn’t seem to be affected by the 2008 financial crisis. Rather, their median housing sales value increased.
<img src='https://github.com/wendysjkim/dsc-mod-4-project-online-ds-pt-051319/blob/work/images/2.%20Top%203%20Price.png'>

These 4 outliers are located in Manhattan:

| Zipcode | Neighborhood  |
|--------|--------------  |
| 10128 | Upper East Side | 
| 10021 | Upper East Side   |
| 10011 | Chelsea and Clinton | 
| 10014 | Greenwich Village and Soho| 

<img src='https://github.com/wendysjkim/dsc-mod-4-project-online-ds-pt-051319/blob/work/images/3.%20NY%20Outliers.png'>

After excluding these outliers, the housing price trend seemed much more reasonable.

<img src='https://github.com/wendysjkim/dsc-mod-4-project-online-ds-pt-051319/blob/work/images/4.%20Top%203%20Excl.%20Outliers.png'>

## ARIMA Grid Search
I ran ARIMA grid search for each zipcode, to identify optimal ARIMA parameters. I selected ARIMA paramters as those with the smallest mean squared errors.

I used SARIMAX with no seasonal order and constant specified. If the constant is specified (i.e. trends=’c’), the results should be the same as using the statsmodels ARIMA funciton. I decided to use SARIMAX without the constant specified after few tries. SARIMAX also has a great method (results.plot_diagnostics) to visaulize the model output. For example,

<img src='https://github.com/wendysjkim/dsc-mod-4-project-online-ds-pt-051319/blob/work/images/5.%2011356%20plot%20diagnostics.png'>
<img src='https://github.com/wendysjkim/dsc-mod-4-project-online-ds-pt-051319/blob/work/images/6.%2011356%20projection.png'>

For the 100 fast-growing zip codes I chose, I ran ARIMA grid search with p values of [0, 1, 2, 4, 6, 8, 10] and d & q values of [0, 1, 2]. Since the data is available monthly, I iterated through more p values (up to 10) to capture any sort of seaonality with the AR component of the ARIMA model.

The ARIMA grid search results are saved in the file `ARIMA_results_range0_2.csv`.

## Results
Among the top 100 fast-growing zipcodes, I identified 30 zipcodes (10 zipcodes in each city) with the best performing models (i.e. smallest MSEs). Then, I calculated predicted growth rate in 1-year, 3-year and 5-year time periods.

Then, I identified top 5 zipcodes with highest 1-year return on investment. I decided to rely on 1-year ROI, as the uncertainty grows as we go further in the future.

> * Top 5 ROI 1 year RegionID : {95984, 61779, 61780, 61781, 62107}
> * Top 5 ROI 3 years RegionID: {96040, 95983, 95984, 61779, 96028}
> * Top 5 ROI 5 years RegionID: {96040, 95983, 95984, 61779, 96028}

The 5 chosen zipcodes are as follows. Among the 5 zipcodes, 4 are in NY and 1 is in LA. The 1-year ROI is expected to range 10-16%.
 > * 10303
 > * 11421 
 > * 10305
 > * 90003
 > * 10304
 
<img src='https://github.com/wendysjkim/dsc-mod-4-project-online-ds-pt-051319/blob/work/images/7.%20Results%20Pred%20Conf.png'>
<img src='https://github.com/wendysjkim/dsc-mod-4-project-online-ds-pt-051319/blob/work/images/8.%20Results%20Pred.png'>
<img src='https://github.com/wendysjkim/dsc-mod-4-project-online-ds-pt-051319/blob/work/images/9.%20Results%20map.png'>
