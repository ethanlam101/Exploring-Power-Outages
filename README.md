# Exploring the Causes of Power Outages 🔋🔌
Made with ❤️ by Ethan Lam (lamethan204@gmail.com)

# Introduction 
## Why explore power outages? 🤔💭
Don't you hate it when your power goes out in the middle of a really important
task? Just in a matter of seconds, it seems like life has grinded to a halt--no
lights, no heat, and essentially, no connection to the outside world. 

Power and electricity has been the backbone of our world and its processes ever since its
invention. When this backbone suddenly dissapears, it doesn't just incovenience
us, it disrupts entire communiites, stalls busuinesses, and even puts lives at
risk. 

Thus, it is important to understand why and how power outages are caused
to avoid them in the future!

This project will be centered around one question,

#### "What are the most notable causes and characteristics of an power outage?"

For these reasons, I will explore power outages using a dataset that describes
the major outages witnessed in the continental United States during January 2000
to July 2016. As defined by the Department of Energy, the major outages refer to
those that impacted atleast 50,000 customers or caused an unplanned firm load
loss of atleast 300 MW. This data can be found from Purdue University's
Laboratory for Advancing Sustainable Critical Infrastrcutre, at
https://engineering.purdue.edu/LASCI/research-data/outages

In the raw Dataframe, there are 1,534 rows and 57 columns. This translates to
1,534 reported outages which meet the critiea mentioned earlier and a mix of
qualitative and quantitative potentionally correlated variables.

Here are the columns we will particuarly focus upon.

|Column                |Description|
|---                |---        |
|`'YEAR'`                |Year an outage occurred|
|`'MONTH'`                |Month an outage occurred|
|`'U.S._STATE'`                |State the outage occurred in|
|`'NERC.REGION'`                |North American Electric Reliability Corporation (NERC) regions involved in the outage event|
|`'CLIMATE.REGION'`                |U.S. Climate regions as specified by National Centers for Environmental Information (9 Regions)|
|`'ANOMALY.LEVEL'`                |Oceanic El Niño/La Niña (ONI) index referring to the cold and warm episodes by season|
|`'OUTAGE.START.DATE'`                |Day of the year when the outage event started|
|`'OUTAGE.START.TIME'`                |Time of the day when the outage event started|
|`'OUTAGE.RESTORATION.DATE'`                |Day of the year when power was restored to all the customers|
|`'OUTAGE.RESTORATION.TIME'`                |Time of the day when power was restored to all the customers|
|`'CAUSE.CATEGORY'`                |Categories of all the events causing the major power outages|
|`'OUTAGE.DURATION'`                |Duration of outage events (in minutes)|
|`'DEMAND.LOSS.MW'`                |Amount of peak demand lost during an outage event (in Megawatt) [but in many cases, total demand is reported]|
|`'CUSTOMERS.AFFECTED'`                |Number of customers affected by the power outage event|
|`'RES.SALES'`                |Electricity consumption in the residental sector (megawatt per hour)|
|`'COM.SALES'`                |Electricity consumption in the commercial sector (megawatt per hour)|
|`'IND.SALES'`                |Electricity consumption in the industrial sector (megawatt per hour)|
|`'TOTAL.PRICE'`                |Average monthly electricity price in the U.S. state (cents/kilowatt-hour)|
|`'TOTAL.SALES'`                |Total electricity consumption in the U.S. state (megawatt-hour)|
|`'TOTAL.CUSTOMERS'`                |Annual number of total customers served in the U.S. state|
|`'POPPCT_URBAN'`                |Percentage of the total population of the U.S. state represented by the urban population (in %)|
|`'POPDEN_URBAN'`                |Population density of the urban areas (persons per square mile)|
|`'AREAPCT_URBAN'`                |Percentage of the land area of the U.S. state represented by the land area of the urban areas (in %)|

# Data Cleaning and Exploratory Data Analysis

## Data Cleaning 🧹🧼

1. Let's start by dropping the irrelevant columns and only keeping features we
   are interested in (in other words, columns that have correlation to these
   outages) These are: `YEAR`, `MONTH`, `U.S._STATE`, `NERC.REGION`,
   `CLIMATE.REGION`, `ANOMALY.LEVEL`,`OUTAGE.START.DATE`, `OUTAGE.START.TIME`,
   `OUTAGE.RESTORATION.DATE`, `OUTAGE.RESTORATION.TIME`, `CAUSE.CATEGORY`,
   `OUTAGE.DURATION`, `DEMAND.LOSS.MW`, `CUSTOMERS.AFFECTED`,
   `TOTAL.PRICE`, `TOTAL.SALES`, `TOTAL.CUSTOMERS`, `POPPCT_URBAN`,
   `POPDEN_URBAN`, `AREAPCT_URBAN`.
2. Next, we see that the power outage start date and time is given by
   `OUTAGE.START.DATE` and `OUTAGE.START.TIME`, but it would be preferable if
   these two columns were combined into one `pd.Timestamp` column. This goes for
   the same with `OUTAGE.RESTORATION.DATE` and `OUTAGE.RESTORATION.TIME`.
3. Following that, we also notice that some columns contain values of 0, which
   likely mean missing values. (i.e. `OUTAGE.DURATION` of 0 is impossible) We
   should impute these missing values, but remember that methods of imputation
   may lead to skewed or inaccurate results. For now let's replace these values
   with `np.nan`.
4. The month column are in numbers, lets change these to abbreviated month names
   for reading ease in future digrams.
5. Finally, because cities are so different (ranging in population, land area,
   amount of industrial/commercial sector) let's standardize `RES.SALES`,
   `COM.SALES`, and `IND.SALES`. This will give us insight into how
   "industrial" a city is (maybe more industrial = more power = more outages?)

Here is what a few columns of our dataframe looks like.

| MONTH   | U.S._STATE   | CAUSE.CATEGORY     | OUTAGE.START                       |   IND.SALES |
|:--------|:-------------|:-------------------|:-----------------------------------|------------:|
| Jul     | Minnesota    | severe weather     | Friday, July 01, 2011 17:00:00     |   -0.308476 |
| May     | Minnesota    | intentional attack | Sunday, May 11, 2014 18:38:00      |   -0.410267 |
| Oct     | Minnesota    | severe weather     | Tuesday, October 26, 2010 20:00:00 |   -0.381646 |
| Jun     | Minnesota    | severe weather     | Tuesday, June 19, 2012 04:30:00    |   -0.362797 |
| Jul     | Minnesota    | severe weather     | Saturday, July 18, 2015 02:00:00   |   -0.459947 |

## Univariate Analysis 📊

Now that our data is cleaned up, let's explore single variables that may correlate with
power outages.

<iframe src="assets/folium_map.html" width="100%" height="600"></iframe>








