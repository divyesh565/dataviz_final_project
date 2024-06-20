---
title: "Data Visualization for Exploratory Data Analysis"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
---

# Data Visualization Project 03


In this exercise you will explore methods to create different types of data visualizations (such as plotting text data, or exploring the distributions of continuous variables).


## PART 1: Density Plots

Using the dataset obtained from FSU's [Florida Climate Center](https://climatecenter.fsu.edu/climate-data-access-tools/downloadable-data), for a station at Tampa International Airport (TPA) for 2022, attempt to recreate the charts shown below which were generated using data from 2016. You can read the 2022 dataset using the code below: 


``` r
library(readr)
library(dplyr)
library(ggplot2)
library(lubridate)
library(viridis)
library(ggridges)


weather_data_url <- "https://raw.githubusercontent.com/reisanar/datasets/master/tpa_weather_2022.csv"
weather_tpa <- read_csv(weather_data_url)
# random sample 
sample_n(weather_tpa, 4)
```

```
## # A tibble: 4 × 7
##    year month   day precipitation max_temp min_temp ave_temp
##   <dbl> <dbl> <dbl>         <dbl>    <dbl>    <dbl>    <dbl>
## 1  2022    12    24       0             45       31     38  
## 2  2022     3    10       0.00001       84       72     78  
## 3  2022     8    23       0             92       80     86  
## 4  2022     8     4       1.24          95       76     85.5
```

See https://www.reisanar.com/slides/relationships-models#10 for a reminder on how to use this type of dataset with the `lubridate` package for dates and times (example included in the slides uses data from 2016).

Using the 2022 data: 

(a) Create a plot like the one below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_facet.png" width="80%" style="display: block; margin: auto;" /><img src="lastname_project_03_files/figure-html/unnamed-chunk-2-2.png" width="80%" style="display: block; margin: auto;" />

Hint: the option `binwidth = 3` was used with the `geom_histogram()` function.

(b) Create a plot like the one below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_density.png" width="80%" style="display: block; margin: auto;" /><img src="lastname_project_03_files/figure-html/unnamed-chunk-3-2.png" width="80%" style="display: block; margin: auto;" />

Hint: check the `kernel` parameter of the `geom_density()` function, and use `bw = 0.5`.

(c) Create a plot like the one below:

<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_density_facet.png" width="80%" style="display: block; margin: auto;" /><img src="lastname_project_03_files/figure-html/unnamed-chunk-4-2.png" width="80%" style="display: block; margin: auto;" />

Hint: default options for `geom_density()` were used. 

(d) Generate a plot like the chart below:


<img src="https://github.com/reisanar/figs/raw/master/tpa_max_temps_ridges_plasma.png" width="80%" style="display: block; margin: auto;" />

```
## Warning: The dot-dot notation (`..x..`) was deprecated in ggplot2 3.4.0.
## ℹ Please use `after_stat(x)` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
## Picking joint bandwidth of 1.93
```

<img src="lastname_project_03_files/figure-html/unnamed-chunk-5-2.png" width="80%" style="display: block; margin: auto;" />

Hint: use the`{ggridges}` package, and the `geom_density_ridges()` function paying close attention to the `quantile_lines` and `quantiles` parameters. The plot above uses the `plasma` option (color scale) for the _viridis_ palette.


(e) Create a plot of your choice that uses the attribute for precipitation _(values of -99.9 for temperature or -99.99 for precipitation represent missing data)_.


``` r
ggplot(data_2022, aes(x = precipitation)) +
  geom_histogram(binwidth = 1, fill = "#532D8E", color = "black", alpha = 0.7) +
  labs(title = "Distribution of Daily Precipitation in 2022",
       x = "Precipitation (mm)",
       y = "Number of Days") +
  theme_minimal() +
  theme(
    axis.text.x = element_text( hjust = 1)
  )
```

![](lastname_project_03_files/figure-html/unnamed-chunk-6-1.png)<!-- -->


## PART 2 

> **You can choose to work on either Option (A) or Option (B)**. Remove from this template the option you decided not to work on. 


### Option (A): Visualizing Text Data

Review the set of slides (and additional resources linked in it) for visualizing text data: https://www.reisanar.com/slides/text-viz#1

Choose any dataset with text data, and create at least one visualization with it. For example, you can create a frequency count of most used bigrams, a sentiment analysis of the text data, a network visualization of terms commonly used together, and/or a visualization of a topic modeling approach to the problem of identifying words/documents associated to different topics in the text data you decide to use. 

Make sure to include a copy of the dataset in the `data/` folder, and reference your sources if different from the ones listed below:

- [Billboard Top 100 Lyrics](https://github.com/reisanar/datasets/blob/master/BB_top100_2015.csv)

- [RateMyProfessors comments](https://github.com/reisanar/datasets/blob/master/rmp_wit_comments.csv)

- [FL Poly News Articles](https://github.com/reisanar/datasets/blob/master/flpoly_news_SP23.csv)


(to get the "raw" data from any of the links listed above, simply click on the `raw` button of the GitHub page and copy the URL to be able to read it in your computer using the `read_csv()` function)


### Option (B): Data on Concrete Strength 

Concrete is the most important material in **civil engineering**. The concrete compressive strength is a highly nonlinear function of _age_ and _ingredients_. The dataset used here is from the [UCI Machine Learning Repository](https://archive.ics.uci.edu/ml/index.php), and it contains 1030 observations with 9 different attributes 9 (8 quantitative input variables, and 1 quantitative output variable). A data dictionary is included below: 


Variable                      |    Notes                
------------------------------|-------------------------------------------
Cement                        | kg in a $m^3$ mixture             
Blast Furnace Slag            | kg in a $m^3$ mixture  
Fly Ash                       | kg in a $m^3$ mixture             
Water                         | kg in a $m^3$ mixture              
Superplasticizer              | kg in a $m^3$ mixture
Coarse Aggregate              | kg in a $m^3$ mixture
Fine Aggregate                | kg in a $m^3$ mixture      
Age                           | in days                                             
Concrete compressive strength | MPa, megapascals


Below we read the `.csv` file using `readr::read_csv()` (the `readr` package is part of the `tidyverse`)


``` r
concrete <- read_csv("../data/concrete.csv", col_types = cols())
```


Let us create a new attribute for visualization purposes, `strength_range`: 


``` r
new_concrete <- concrete %>%
  mutate(strength_range = cut(Concrete_compressive_strength, 
                              breaks = quantile(Concrete_compressive_strength, 
                                                probs = seq(0, 1, 0.2))) )
colnames(new_concrete)
```

```
##  [1] "Cement"                        "Blast_Furnace_Slag"           
##  [3] "Fly_Ash"                       "Water"                        
##  [5] "Superplasticizer"              "Coarse_Aggregate"             
##  [7] "Fine_Aggregate"                "Age"                          
##  [9] "Concrete_compressive_strength" "strength_range"
```

``` r
# Plot the distribution of Cement
ggplot(new_concrete, aes(x = Cement)) +
  geom_density(fill = "blue", alpha = 0.5) +
  labs(title = "Distribution of Cement Content",
       x = "Cement (kg/m^3)",
       y = "Density") +
  theme_minimal()
```

![](lastname_project_03_files/figure-html/unnamed-chunk-9-1.png)<!-- -->

``` r
# Plot the distribution of Water
ggplot(new_concrete, aes(x = Water)) +
  geom_density(fill = "green", alpha = 0.5) +
  labs(title = "Distribution of Water Content",
       x = "Water (kg/m^3)",
       y = "Density") +
  theme_minimal()
```

![](lastname_project_03_files/figure-html/unnamed-chunk-9-2.png)<!-- -->



1. Explore the distribution of 2 of the continuous variables available in the dataset. Do ranges make sense? Comment on your findings.

2. Use a _temporal_ indicator such as the one available in the variable `Age` (measured in days). Generate a plot similar to the one shown below. Comment on your results.



<img src="https://github.com/reisanar/figs/raw/master/concrete_strength.png" width="80%" style="display: block; margin: auto;" />

```
## Warning: The following aesthetics were dropped during statistical transformation:
## colour.
## ℹ This can happen when ggplot fails to infer the correct grouping structure in
##   the data.
## ℹ Did you forget to specify a `group` aesthetic or to convert a numerical
##   variable into a factor?
```

```
## Warning: Removed 1 row containing missing values or values outside the scale range
## (`geom_point()`).
```

```
## Warning: Removed 12 rows containing missing values or values outside the scale range
## (`geom_point()`).
```

<img src="lastname_project_03_files/figure-html/unnamed-chunk-10-2.png" width="80%" style="display: block; margin: auto;" />


3. Create a scatter plot similar to the one shown below. Pay special attention to which variables are being mapped to specific aesthetics of the plot. Comment on your results. 

<img src="https://github.com/reisanar/figs/raw/master/cement_plot.png" width="80%" style="display: block; margin: auto;" /><img src="lastname_project_03_files/figure-html/unnamed-chunk-11-2.png" width="80%" style="display: block; margin: auto;" />




