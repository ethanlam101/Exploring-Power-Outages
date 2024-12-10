# Exploring the Causes of Power Outages 🔋🔌
Author: Ethan Lam

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

#### ***"What are the most notable causes and characteristics of an power outage?"***

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
|`'TOTAL.PRICE'`                |Average monthly electricity price in the U.S. state (cents/kilowatt-hour)|
|`'TOTAL.SALES'`                |Total electricity consumption in the U.S. state (megawatt-hour)|
|`'TOTAL.CUSTOMERS'`                |Annual number of total customers served in the U.S. state|
|`'POPPCT_URBAN'`                |Percentage of the total population of the U.S. state represented by the urban population (in %)|
|`'POPDEN_URBAN'`                |Population density of the urban areas (persons per square mile)|
|`'AREAPCT_URBAN'`                |Percentage of the land area of the U.S. state represented by the land area of the urban areas (in %)|





