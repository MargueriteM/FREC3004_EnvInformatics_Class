Module 6: Terrestrial Carbon Flux
================
FREC 3004 Environmental Informatics

``` r
library(tidyverse)
library(lubridate)
library(modelr)
```

## Science question

How much carbon dioxide does a forest remove from the atmosphere?

## Environmental Learning objectives

Having completed the module students will be able to:

  - Import and analyze net carbon fluxes from the FLUX2015 database
    using R, tidyverse, and ggplot.
  - Describe how carbon flux varies from minutes to annual time scales.
  - Compare how the rate of carbon uptake by ecosystems varies among
    locations.
  - Separate a net flux (NEP) into components (GPP and RE).
  - Create informative figures in ggplot that display temporal variation
    in carbon fluxes.

## Data Science Learning objectives

  - Create simple functions in R
  - Modify and create strings

## Assumed R knowledge

  - mutate()
  - group\_by()
  - ggplot()
  - geom\_line()
  - geom\_point()
  - select()
  - summarize()
  - mean()
  - filter()
  - rename()
  - read\_csv()
  - as.numeric()
  - lm()
  - rbind()
  - geom\_abline()
  - geom\_smooth()
  - geom\_histogram()
  - head()
  - read\_delim()
  - rename()
  - slice()
  - as.numeric()
  - paste0()
  - for loop
  - if-else statement
  - ifelse()
  - vector()
  - geom\_hline()
  - joins

## R knowledge covered in module

  - str\_sub()
  - add\_predictions()
  - add\_residuals()
  - make\_datetime()
  - creating simple functions

## Background, and why you should care:

Carbon is the currency of ecosystem production, comprising half of the
dry weight of plant tissues produced when atmospheric CO2 is fixed via
photosynthesis and then used to build biomass. The rate at which new
plant biomass accumulates or, put another way, the “flux” of carbon from
the atmosphere to plant biomass is net primary production (NPP). A
closely related term, net ecosystem production (NEP), is the balance
between gross primary production (GPP, whole-canopy carbon fixation) and
ecosystem respiration (Re, respiration from all organisms). Often, NEP
is equated with “carbon sequestration”, a general term used to describe
the quantity of carbon dioxide removed from the atmosphere, because it
is a measure of the net balance between CO2 moving into the forest from
photosynthetic carbon fixation and that which is released back to the
atmosphere from respiration. This balance between whole- ecosystem
carbon uptake (i.e., GPP) and loss (i.e., Re) changes over time as
leaves grow and later fall, as shifting climate alters the balance
between these two opposing fluxes of carbon, and as disturbances modify
the number and physiology of primary producers. Spatially, variability
in climate and disturbance regime may explain why ecosystems in
different regions sequester carbon at different rates. Understanding how
much carbon ecosystems sequester and why is therefore a question of
fundamental importance to ecologists, and also to the management of
terrestrial ecosystems for carbon sequestration.

Facilitating the quest to answer these questions, researchers are
increasingly drawing from open research networks that provide high
temporal resolution (usually half-hourly to hourly) estimates of NEP,
GPP, and Re for different ecosystems all over the world. Among these
networks, the National Ecological Observatory Network (NEON,
<https://www.neonscience.org/>) and FLUXNET
(<http://fluxnet.fluxdata.org/>) provide freely available carbon flux
data. When combined with long-term environmental data from networks
collecting meteorological information such as the National Oceanic and
Atmospheric Administration (NOAA, <http://www.noaa.gov/>), it is
possible to assess the sensitivity of carbon fluxes to ongoing and
predicted climate change. This lab challenges participants to use open,
long-term data from a variety of networks to ask:

1)  How variable are carbon fluxes among sites and over time?;
2)  Do temporal patterns of carbon flux differ at different timescales
    of minutes, days, and years?; and
3)  Given historical climate trends, how will changes over time, should
    they continue, affect the future trajectory of carbon fluxes?

## Part 1: Site selection and hypothesis development

Step 1: Select a unique NEON eco-region to investigate (Figure 1). You
can read about the different NEON eco-regions and how they were selected
here: <https://www.neonscience.org/field-sites>.

Step 2: Within your selected NEON eco-region, choose a site to
investigate

1)  Go to <http://fluxnet.fluxdata.org>
2)  Go to the “Site” tab, click on “Site List”, and choose “Map View”.
3)  Click on sites in your eco-region and click on different sites to
    find one that has “Data Start” and “Data End” dates that span 5 or
    more years
4)  Take note of the name of the site (i.e., US-Ha1)

**Question 1:** What site did you pick? What ecoregion is it in?

**Answer 1:**

Step 3: Develop hypotheses for your site. Specifically, describe what
you think temporal patterns of NEP, GPP, and RE at the following
temporal scales will be:

**Question 2:**

Hourly within a day

**Answer 2:**

  - NEP:
  - GPP:
  - RE:

**Question 3:**

Monthly to month

**Answer 3:**

  - NEP:
  - GPP:
  - RE:

**Question 4:**

Year to year

**Answer 4:**

  - NEP:
  - GPP:
  - RE:

## Part 2: Test your hypotheses using real data

Step 1: Download carbon flux data for your site.

1)  Go to <http://fluxnet.fluxdata.org>
2)  Click “Sign-In” and create an account using your Virginia Tech email
3)  Sign in to the site
4)  Click “Data” tab and chose the “FLUXNET2015 Dataset”
5)  Click “Download FLUXNET2015 Dataset”
6)  Chose “CC-BY-4.0 Data”
7)  Click “FULLSET data product (includes SUBSET)”
8)  In the list of Site IDs, chose your Site and move to the “Your
    Selections”
9)  Chose “Education” for the “Intended Use”
10) In the description, write “For analysis in Virginia Tech FREC 3004
    Environmental Informatics course”
11) Open the data policy (see link below box) and summarize how it
    applies to use in this course

**Question 5:** How are we meeting the data policy requirements?

**Answer 5:**

12) If we are meeting the data policy requirements, click “I
    acknowledge..”
13) Click “Download All Files”
14) Click on the link with your Site ID (next to README)
15) Move the files to your module data subfolder

Step 2: Import the hourly time-step data (i.e., file with
“FLUXNET2015\_FULLSET\_HH” or “FLUXNET2015\_FULLSET\_HR”). Orient and
familiarize yourself with the variable headers:
<http://fluxnet.fluxdata.org/data/fluxnet2015-dataset/fullset-data-product/>.

``` r
#INSERT CODE HERE
```

Step 3: Tidy up the timedate variable. The timestep variable is a
non-standard format that we need to clean up using a new set of
functions. First, notice the format of `TIMESTAMP_START`. It will vary
depending on your site but in general it will look like this:
199101010000. The first 4 numbers are the year, next 2 are the month,
next 2 are the day, next 2 are the hour, and next 2 are the minute. We
can use this information along with the function `str_sub()` in the
`stringr` package that comes with `tidyverse` to grab the correct set of
numbers for the corresponding unit of time. Here is an example: The
numbers in the str\_sub() below are the beginning and ending character
to grab.

``` r
str_sub("EI_IS_GREAT", 1, 2)
str_sub("EI_IS_GREAT", 7, 11)
```

Since it is a character it needs to be converted to a number using
`as.numeric`. Then you can use the `make_datetime()` function that is in
the `lubridate` package to combine.

``` r
flux_data <- flux_data %>% 
  mutate(YEAR = as.numeric(str_sub(TIMESTAMP_START, 1, 4)),
         MONTH = as.numeric(str_sub(TIMESTAMP_START, 5, 6)),
         DAY = as.numeric(str_sub(TIMESTAMP_START, 7, 8)),
         HR = as.numeric(str_sub(TIMESTAMP_START, 9, 10)),
         MIN = as.numeric(str_sub(TIMESTAMP_START, 11, 12)),
         TIMESTAMP_START = make_datetime(YEAR, MONTH, DAY, HR, MIN, tz = "UTC")) %>% 
  select(TIMESTAMP_START, YEAR, MONTH, DAY, HR, MIN, NIGHT, NEE_VUT_REF, TA_F)
```

The flux towers measure Net Ecosystem Exchange (NEE), which is negative
when the system takes up carbon from the atmosphere and positive when it
releases carbon. First, check to see what value is used to represent NA
values in the NEE\_VUT\_REF variable. Filter out NA values. Be sure that
your site still has 5-years of data after filtering out NA values. If
not, pick a new site and acquire that data. Second, we are going to
analyze Net Ecosystem Productivity (NEP), which is the negative of NEE.
NEE is NEE\_VUT\_REF in the data set. Create a new column called NEP
that is -NEE\_VUT\_REF.

A positive value for NEP means carbon is being absorbed from the
atmosphere because GPP \> RE and a negative value means that NEP is
being released by the ecosystem to the atmosphere because RE \> GPP.

Note that the units of NEP are umolCO2 /m2 /second.

``` r
#INSERT CODE
```

Step 4: Given your hypotheses, consider how to organize and then plot
your data. For examining how carbon fluxes change from month-to-month,
what is your x-axis? What about your y-axis? How do you organize your
data?

Plot the entire hourly time-series for NEP and describe the patterns. Be
sure to add a horizontal line at 0 so that it is easy to see when NEP is
positive or negative.

``` r
#INSERT CODE
```

**Question 6: ** Why is the plot with the hourly data hard to interpret?

**Answer 6: **

Plot the time-series for NEP at annual scale (calculate the annual
mean). Be sure to add a horizontal line at 0 so that it is easy to see
when NEP is positive or negative.

``` r
#INSERT CODE
```

Plot the time series for NEP at the monthly scale (calculate the mean
for each month of the year). Be sure to add a horizontal line at 0 so
that it is easy to see when NEP is positive or negative.

``` r
#INSERT CODE
```

Plot the time series for NEP at the hourly scale (calculate the mean for
each hour of the day). Be sure to add a horizontal line at 0 so that it
is easy to see when NEP is positive or negative.

``` r
#INSERT CODE
```

Step 5: Were your hypotheses for NEP generally supported?

**Question 7:** Hourly within a day

**Answer 7:**

**Question 8:** Monthly within a year

**Answer 8:**

**Question 9:** Year to year

**Answer 9:**

## Part 3: Modeling the loss of carbon through respiration (RE)

Recall from the introduction that NEP is the net of two processes:
Photosynthesis (GPP) and Respiration from plant and microbes (RE).  
To estimate GPP and RE we take advantage of the fact that we know GPP is
zero at night.

Step 1: Hypothesize how you expect air temperature to influence RE.

**Question 10:** Give your hypothesis for how RE varies with
temperature.

**Answer 10:**

Step 2: Examine the relationship between NEP and temperature (`TA_F`) at
night by using the NIGHT variable to select only the data that are
identified as occurring at NIGHT (see NIGHT column in the data) and
plotting TA\_F on the x-axis and NEP on the y-axis

``` r
#INSERT CODE
```

Step 3: To model the relationship between air temperature and night time
NEP (which is the same as night time RE) we will use a non-linear
exponential model that looks like this:

``` r
m <- nls(NEP ~ a * b^TA_F, night_only)
```

The object `m` is the output of the model. It is the same type of object
as the liner regression models (`fit <- lm(y ~ x)`) we used in previous
modules. The difference is that we can use a non-linear model when we
use the nls() function to find the model.

We can use the model object to make predictions and calculate residuals.
The `add_predictions` function takes the coefficients (or parameters)
from the model and creates predictions for each row. It creates a new
variable called `pred`. Similarly, `add_residuals()` calculates the
difference between the observed data and the model (similar to Module
3).

``` r
night_only <- night_only %>% 
  add_predictions(m) %>% 
  add_residuals(m)
```

Examine our model by adding the predicted nighttime NEP to the plot with
the data as a colored line. This will use geom\_points for plotting the
data and geom\_line for plotting the predictions from the model. Be sure
to look at the column names to get the created column that contain the
predictions from the model.

``` r
#INSERT CODE
```

**Question 11:** Was your hypothesis for how RE varies with temperature
supported by the data?

**Answer 11:**

Examine the residuals of the predicted NEP using a plot in the same way
that we examined residuals in Module 3.

**Question 12:** Show the plot. Are they normally distributed around 0?

**Answer 12:**

``` r
#INSERT CODE
```

Step 4: We can use this relationship between air temperature and NEP at
night to calculate RE as a function of air temperature throughout the
day. First, create a function to calculate RE at all time periods in the
full data set.  
Being able to make you own functions is a powerful way to develop code
that is reusable. For example, the following create a simple function
that adds two number together.

``` r
sum_two_numbers <- function(a, b){
  c <- a + b
  return(c)
}
```

Creating the function does not do anything other than defining the
function.  
You need to call the function to use it

``` r
sum_two_numbers(3, 1)
```

Now we can create a function that uses the output of the nls to
calculate RE for the full dataframe. A positive value of RE is a loss of
carbon from the ecosystem to the atmospheric through respiration.

``` r
#df: is the data frame that gets automatically passed from the pipe
#m: is the model object (from lm or nls)
calculate_Re <- function(df, m){ 
  df <- df %>% add_predictions(m) %>% 
    rename("RE" = pred) %>% 
    mutate(RE = -RE) 
  return(df)
}
```

Note that `df` is the data frame that gets automatically passed from the
pipe, and `m` is the model object from `lm` or `nls` function call. If
you are using the function without a pipe you need to use the command
`calculate_Re(df = night_only, m = m)` but if you are using the function
with a pipe then you can skip the df augument and use `calculate_Re(m =
m)`

Now apply the function to all the flux data (not the night only data).
Do not rerun the nls function - you need the nls function output (m)
from only the nighttime data because that is used to calcuate RE for all
the hours in the full data set.

``` r
#INSERT CODE
```

Plot RE at the monthly time scale (mean of each month within the year).

``` r
#INSERT CODE
```

**Question 13:** Do the patterns of RE at the monthly time-scale match
your expectations from Question 3?

**Answer 13:**

## Part 4: Modeling the gain of carbon through respiration (GPP)

Now that we have an estimate to for RE during the day we can use the
fact that NEP = GPP - RE (assuming, as we have done in our
calculate\_Re() function, that RE is a positive number if carbon is lost
from the ecosystem) to estimate GPP for each time period.

``` r
#INSERT CODE
```

Plot GPP at the hourly time scale (mean of each hour within the day)

``` r
#INSERT CODE
```

**Question 14:** Do the GPP patterns at the hourly time-scale match your
expectations from Question 2?

**Answer 14:**

**Question 15:** Create a plot with the **monthly** mean values of NEP,
GPP, and RE on the same plot (x-axis in month and y-axis is the flux
value). Do to this easily in ggplot I recommend using the
`pivot_longer()` function to create a variable that is the type of flux
(GPP, NEE, RE) and a column for the value

**Answer 15:**

``` r
#INSERT CODE
```

## Part 5: Analyze a new site

Pick a new site in a different eco-region and compare NEP values at the
monthly and yearly scales. Hint: you will need to use a join by month to
combine the monthly summarized data from the two sites.

**Question 16:** What site did you chose?

**Answer 16:**

**Question 17:** Use a plot to show how monthly mean NEP dynamics differ
between the two sites. Plot the monthly mean NEP from the two sites on
the same figure.

**Answer 17:**

``` r
#INSERT CODE
```

**Question 18:** Make a plot of the annual NEP vs. year with both sites
on the same figure.

**Answer 18:**

``` r
#INSERT CODE
```

**Question 19:** At the annual scale, which site removes more carbon
from the atmosphere?

**Answer 19:**

## References

Components of the module are from:

Gough, C. M., Giffen, C., Woodward, T. W. (2018). Environmental Drivers
of Ecosystem Carbon Fluxes from Minutes to Years. NEON Faculty Mentoring
Network, QUBES Educational Resources. <doi:10.25334/Q4VD7W>
