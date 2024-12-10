# Exploring the Causes of Power Outages 🔋🔌
Made with ❤️ by Ethan Lam (lamethan204@gmail.com)
<br />
---
<br />

## Introduction 
### Why explore power outages? 🤔💭
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

**"What are the most notable causes and characteristics of an power outage?"**

I will explore power outages using a dataset that describes
the major outages witnessed in the continental United States during January 2000
to July 2016. As defined by the Department of Energy, the major outages refer to
those that impacted atleast 50,000 customers or caused an unplanned firm load
loss of atleast 300 MW. 

This data can be found from Purdue University's Laboratory for Advancing 
Sustainable Critical Infrastrcutre, at

https://engineering.purdue.edu/LASCI/research-data/outages

---

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

---
<br />

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning 🧹🧼

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
4. The month column are in numbers, let's change these to abbreviated month names
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

### Univariate Analysis 📊

Now that our data is cleaned up, let's explore single variables that may correlate with
power outages.

This density heatmap shows the amount of reported outages in each state. We can
see that California has the greatest amount of reported outages, but we also
notice that states with higher population have more reported outages.
(coincidence or not?)
<iframe src="assets/power_outages_choropleth.html" width="100%" height="600"></iframe><br />

This simple bar chart displays the number of outages per count. We can see that
severe weather dominates the distribution of causes. Weather and or
climate may be a strong factor of a cause for a power outage.
<iframe src="assets/causes_count.html" width="100%" height="600"></iframe><br />

This line chart shows the number of outages per year. We can see a spike in 2011
which can be explained by the numerous extreme weather events such as Hurricane
Irene, the Texas Winter Storm, and a tornado outbreak.
<iframe src="assets/outages_yearly_count.html" width="100%" height="600"></iframe><br />

### Bivariate Analysis 📈

It seems like we have variables that correlate with one another, let's explore
these.

First, I expected that the use of lots of electricity shoud have a positive
relationship with the duration of power outages. (i.e. more electricity sold =
more electricity used = more severe/longer power outages?) We do see a very weak
positive correlation here, meaning that these variables may not be as related as we thought.
<iframe src="assets/duration_vs_sales.html" width="100%" height="600"></iframe><br />

Next, we examine the relationship between the cause of a power outage and its
duration. We use duration because it can be a measure of severity. (longer
duration = higher severity). The plot shows that severe weather and fuel supply
emergency, on average, have higher durations than other causes. This might be
because weather storms last days to weeks at a time and power plants that supply
electricity may take long periods of time to repair.
<iframe src="assets/duration_vs_cause.html" width="100%" height="600"></iframe><br />

### Interesting Aggregates 📋

We know that climate region definitely has some correlation with the causes of a
power outage. This table below shows a count of power outages for each cause
category for each climate region. We notice that the Northeast experiences the
most severe weather, which could correlate to extreme Winter storms.
Interestingly, we also see that the Northeast experiences the most intentional
attacks by a large margin. Again, nan values mean there were no reports of that
respective cause in that region.

<div style="overflow-x:auto; width: 100%;">
  <table>
    <thead>
      <tr>
        <th>CLIMATE.REGION</th>
        <th>equipment failure</th>
        <th>fuel supply emergency</th>
        <th>intentional attack</th>
        <th>islanding</th>
        <th>public appeal</th>
        <th>severe weather</th>
        <th>system operability disruption</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Central</td>
        <td>7</td>
        <td>4</td>
        <td>38</td>
        <td>3</td>
        <td>2</td>
        <td>135</td>
        <td>11</td>
      </tr>
      <tr>
        <td>East North Central</td>
        <td>3</td>
        <td>5</td>
        <td>20</td>
        <td>1</td>
        <td>2</td>
        <td>104</td>
        <td>3</td>
      </tr>
      <tr>
        <td>Northeast</td>
        <td>5</td>
        <td>14</td>
        <td>135</td>
        <td>1</td>
        <td>4</td>
        <td>176</td>
        <td>15</td>
      </tr>
      <tr>
        <td>Northwest</td>
        <td>2</td>
        <td>1</td>
        <td>89</td>
        <td>5</td>
        <td>2</td>
        <td>29</td>
        <td>4</td>
      </tr>
      <tr>
        <td>South</td>
        <td>10</td>
        <td>7</td>
        <td>28</td>
        <td>2</td>
        <td>42</td>
        <td>113</td>
        <td>27</td>
      </tr>
      <tr>
        <td>Southeast</td>
        <td>5</td>
        <td>nan</td>
        <td>9</td>
        <td>nan</td>
        <td>5</td>
        <td>118</td>
        <td>16</td>
      </tr>
      <tr>
        <td>Southwest</td>
        <td>5</td>
        <td>2</td>
        <td>64</td>
        <td>1</td>
        <td>1</td>
        <td>10</td>
        <td>9</td>
      </tr>
      <tr>
        <td>West</td>
        <td>21</td>
        <td>17</td>
        <td>31</td>
        <td>28</td>
        <td>9</td>
        <td>70</td>
        <td>41</td>
      </tr>
      <tr>
        <td>West North Central</td>
        <td>1</td>
        <td>1</td>
        <td>4</td>
        <td>5</td>
        <td>2</td>
        <td>4</td>
        <td>nan</td>
      </tr>
    </tbody>
  </table>
</div>

---
<br />

## Assessment of Missingness

### NMAR Analysis ❓❓

The missing mechanism, Not Missing At Random, describes the missing mechanism in
which the missing data is likely missing because of the data itself. For
example, participants with severe depression are more likely to refuse to
complete a depression survey.

In our dataset, we see that missing values in `CUSTOMERS.AFFECTED` likely is
NMAR data because these values are collected by self reporting companies. These
companies likely did not report the values, hence the missing data. 

Some information we could collect that would make the missingness of
`CUSTOMERS.AFFECTED` missing at random (dependent on observed data), would be 
the size of the reporting companies. Larger companies might have stricter 
reporting policies, while smaller ones may not.

### Missingness Dependency ⁉️

Now let's test whether or not our missingness in `CAUSE.CATEGORY.DETAIL` is
dependent on other columns. We will test this against the columns
`CAUSE.CATEGORY` and `U.S._STATE`.

#### <u> Cause Category </u>

To see if `CAUSE.CATEGORY.DETAIL`'s missingness is dependent on
`CAUSE.CATEGORY`, we will examine the distribution of when
`CAUSE.CATEGORY.DETAIL` is missing vs not missing.

**Null Hypothesis:** The distribution of `CAUSE.CATEGORY` is the same when
`CAUSE.CATEGORY.DETAIL` is missing vs not missing.

**Alternate Hypothesis:** The distribution of `CAUSE.CATEGORY` is different when
`CAUSE.CATEGORY.DETAIL` is missing vs not missing.

**Test Statistic:** Total Variation Distance because we are comaring two
categorical distributions

**Significane Level:** 0.05

Just from our observed data, we find an observed TVD of `0.411`.

<iframe src="assets/cause_category_detail_missingness.png" width="100%" height="600"></iframe><br />

Shown below is the empirical distribution of our tested TVDs. We see that none 
of the tested TVDs are greater than our observed TVD. This gives us a p-value 
of 0.0, thus we can safely reject the null hypothesis in favour of our 
alternative hypothesis. <br />

The missingness of `CAUSE.CATEGORY.DETAIL` is very likely MAR dependent on `CAUSE.CATEGORY`.

<iframe src="assets/empirical_dist_cause_category_detail.html" width="100%" height="600"></iframe><br />

### <u> Total Sales </u>

Next we will test if `CAUSE.CATEGORY`'s missingness is dependent on
`TOTAL.SALES`. Just like last problem, we will examine the distributions of
missingness.

**Null Hypothesis:** The distribution of `CAUSE.CATEGORY` is the same when
`TOTAL.SALES` is missing vs not missing.

**Alternate Hypothesis:** The distribution of `CAUSE.CATEGORY` is different when
`TOTAL.SALES` is missing vs not missing.

**Test Statistic:** Total Variation Distance because we are comaring two
categorical distributions

**Significane Level:** 0.05

From our observed data, we see an observed TVD of `0.26`.

<iframe src="assets/total_sales_missingness.png" width="100%" height="600"></iframe><br />

Shown below is the empirical distribution of our tested TVDs. We see that a good
amount of tested TVDs are greater than our observed TVD. This gives us a p-value 
of 0.087, thus we fail to reject the null hypothesis. <br />

The missingness of `TOTAL.SALES` is very likely NOT MAR dependent on `CAUSE.CATEGORY`.

<iframe src="assets/empirical_dist_total_sales.html" width="100%" height="600"></iframe><br />

---
<br />

## Hypothesis Testing 🧪🔬

From our density heatmap earlier, we noticed a general trend, that is, the more
populated a state, the more reports of severe outages. Let's test whether or not
living in a largely populated state will increase your chances experiencing a
severe outage.

For our tests, we will define largely populated states as 
`[California, Texas, Florida]`. Our durations will be dervived from the
`OUTAGE.DURATION` column.

**Null Hypothesis:** On average, the duration of power outages in largely
populated states is the same as the duration of power outages in every other
state.

**Alternate Hypothesis:** On average, the duration of power outages in largely
populated states is greater than duration of power outages in every other
state.

**Test Statistic:** Difference in means.

**Significance Value:** 0.05

We will be using a permutation test because we are trying to see if these two
distributions come from the same population. 

From our data, we find an observed value of `-461.15` which means that every
other state, on average, has a higher duration of power outages.

After computing our permutation tests with 10,000 simulations, our empirical
distributions of difference is means is shown below. We derive a p-value of
0.939 which means we greatly fail to reject our null hypothesis.

It seems like lessly populated states tend to have longer power outages!

<iframe src="assets/empirical_dist_diff_means.html" width="100%" height="600"></iframe><br />

---
<br />

## Framing a Prediction Problem 🧠💡

