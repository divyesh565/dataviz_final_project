---
title: "FIFA World Cup Matches Data Visualization"
author: "Divyesh Desai - `ddesai7656@floridapoly.edu`"
output: 
  html_document:
    keep_md: true
    toc: true
    toc_float: true
---
***

> Importing the required library.


``` r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

``` r
library(ggplot2)
```


> Loading the data from the URL and reading it through the imported library.


``` r
# Load and read the Wold Cup match data

url <- "https://raw.githubusercontent.com/reisanar/datasets/master/WorldCupMatches.csv"
world_cup_data <- read.csv(url, stringsAsFactors = FALSE)
```

> Removing spaces from column names for future use.


``` r
# Remove spaces from column names
colnames(world_cup_data) <- gsub(" ", "", colnames(world_cup_data))
```

> Printing the structure of the data.


``` r
# Explore the structure of the data

str(world_cup_data)
```

```
## 'data.frame':	4572 obs. of  20 variables:
##  $ Year                : int  1930 1930 1930 1930 1930 1930 1930 1930 1930 1930 ...
##  $ Datetime            : chr  "13 Jul 1930 - 15:00 " "13 Jul 1930 - 15:00 " "14 Jul 1930 - 12:45 " "14 Jul 1930 - 14:50 " ...
##  $ Stage               : chr  "Group 1" "Group 4" "Group 2" "Group 3" ...
##  $ Stadium             : chr  "Pocitos" "Parque Central" "Parque Central" "Pocitos" ...
##  $ City                : chr  "Montevideo " "Montevideo " "Montevideo " "Montevideo " ...
##  $ Home.Team.Name      : chr  "France" "USA" "Yugoslavia" "Romania" ...
##  $ Home.Team.Goals     : int  4 3 2 3 1 3 4 3 1 1 ...
##  $ Away.Team.Goals     : int  1 0 1 1 0 0 0 0 0 0 ...
##  $ Away.Team.Name      : chr  "Mexico" "Belgium" "Brazil" "Peru" ...
##  $ Win.conditions      : chr  " " " " " " " " ...
##  $ Attendance          : int  4444 18346 24059 2549 23409 9249 18306 18306 57735 2000 ...
##  $ Half.time.Home.Goals: int  3 2 2 1 0 1 0 2 0 0 ...
##  $ Half.time.Away.Goals: int  0 0 0 0 0 0 0 0 0 0 ...
##  $ Referee             : chr  "LOMBARDI Domingo (URU)" "MACIAS Jose (ARG)" "TEJADA Anibal (URU)" "WARNKEN Alberto (CHI)" ...
##  $ Assistant.1         : chr  "CRISTOPHE Henry (BEL)" "MATEUCCI Francisco (URU)" "VALLARINO Ricardo (URU)" "LANGENUS Jean (BEL)" ...
##  $ Assistant.2         : chr  "REGO Gilberto (BRA)" "WARNKEN Alberto (CHI)" "BALWAY Thomas (FRA)" "MATEUCCI Francisco (URU)" ...
##  $ RoundID             : int  201 201 201 201 201 201 201 201 201 201 ...
##  $ MatchID             : int  1096 1090 1093 1098 1085 1095 1092 1097 1099 1094 ...
##  $ Home.Team.Initials  : chr  "FRA" "USA" "YUG" "ROU" ...
##  $ Away.Team.Initials  : chr  "MEX" "BEL" "BRA" "PER" ...
```

> Printing the first few rows of the data.


``` r
# Explore the first few rows of the data

head(world_cup_data)
```

```
##   Year             Datetime   Stage        Stadium        City Home.Team.Name
## 1 1930 13 Jul 1930 - 15:00  Group 1        Pocitos Montevideo          France
## 2 1930 13 Jul 1930 - 15:00  Group 4 Parque Central Montevideo             USA
## 3 1930 14 Jul 1930 - 12:45  Group 2 Parque Central Montevideo      Yugoslavia
## 4 1930 14 Jul 1930 - 14:50  Group 3        Pocitos Montevideo         Romania
## 5 1930 15 Jul 1930 - 16:00  Group 1 Parque Central Montevideo       Argentina
## 6 1930 16 Jul 1930 - 14:45  Group 1 Parque Central Montevideo           Chile
##   Home.Team.Goals Away.Team.Goals Away.Team.Name Win.conditions Attendance
## 1               4               1         Mexico                      4444
## 2               3               0        Belgium                     18346
## 3               2               1         Brazil                     24059
## 4               3               1           Peru                      2549
## 5               1               0         France                     23409
## 6               3               0         Mexico                      9249
##   Half.time.Home.Goals Half.time.Away.Goals                Referee
## 1                    3                    0 LOMBARDI Domingo (URU)
## 2                    2                    0      MACIAS Jose (ARG)
## 3                    2                    0    TEJADA Anibal (URU)
## 4                    1                    0  WARNKEN Alberto (CHI)
## 5                    0                    0    REGO Gilberto (BRA)
## 6                    1                    0  CRISTOPHE Henry (BEL)
##                Assistant.1                Assistant.2 RoundID MatchID
## 1    CRISTOPHE Henry (BEL)        REGO Gilberto (BRA)     201    1096
## 2 MATEUCCI Francisco (URU)      WARNKEN Alberto (CHI)     201    1090
## 3  VALLARINO Ricardo (URU)        BALWAY Thomas (FRA)     201    1093
## 4      LANGENUS Jean (BEL)   MATEUCCI Francisco (URU)     201    1098
## 5     SAUCEDO Ulises (BOL) RADULESCU Constantin (ROU)     201    1085
## 6  APHESTEGUY Martin (URU)        LANGENUS Jean (BEL)     201    1095
##   Home.Team.Initials Away.Team.Initials
## 1                FRA                MEX
## 2                USA                BEL
## 3                YUG                BRA
## 4                ROU                PER
## 5                ARG                FRA
## 6                CHI                MEX
```

> After viewing the data, realizing datetime values are in string format, so converting the datetime field to a Date object.


``` r
# Convert date column to Date type

world_cup_data$Datetime <- as.POSIXct(world_cup_data$Datetime, format = "%d %b %Y - %H:%M")
head(world_cup_data)
```

```
##   Year            Datetime   Stage        Stadium        City Home.Team.Name
## 1 1930 1930-07-13 15:00:00 Group 1        Pocitos Montevideo          France
## 2 1930 1930-07-13 15:00:00 Group 4 Parque Central Montevideo             USA
## 3 1930 1930-07-14 12:45:00 Group 2 Parque Central Montevideo      Yugoslavia
## 4 1930 1930-07-14 14:50:00 Group 3        Pocitos Montevideo         Romania
## 5 1930 1930-07-15 16:00:00 Group 1 Parque Central Montevideo       Argentina
## 6 1930 1930-07-16 14:45:00 Group 1 Parque Central Montevideo           Chile
##   Home.Team.Goals Away.Team.Goals Away.Team.Name Win.conditions Attendance
## 1               4               1         Mexico                      4444
## 2               3               0        Belgium                     18346
## 3               2               1         Brazil                     24059
## 4               3               1           Peru                      2549
## 5               1               0         France                     23409
## 6               3               0         Mexico                      9249
##   Half.time.Home.Goals Half.time.Away.Goals                Referee
## 1                    3                    0 LOMBARDI Domingo (URU)
## 2                    2                    0      MACIAS Jose (ARG)
## 3                    2                    0    TEJADA Anibal (URU)
## 4                    1                    0  WARNKEN Alberto (CHI)
## 5                    0                    0    REGO Gilberto (BRA)
## 6                    1                    0  CRISTOPHE Henry (BEL)
##                Assistant.1                Assistant.2 RoundID MatchID
## 1    CRISTOPHE Henry (BEL)        REGO Gilberto (BRA)     201    1096
## 2 MATEUCCI Francisco (URU)      WARNKEN Alberto (CHI)     201    1090
## 3  VALLARINO Ricardo (URU)        BALWAY Thomas (FRA)     201    1093
## 4      LANGENUS Jean (BEL)   MATEUCCI Francisco (URU)     201    1098
## 5     SAUCEDO Ulises (BOL) RADULESCU Constantin (ROU)     201    1085
## 6  APHESTEGUY Martin (URU)        LANGENUS Jean (BEL)     201    1095
##   Home.Team.Initials Away.Team.Initials
## 1                FRA                MEX
## 2                USA                BEL
## 3                YUG                BRA
## 4                ROU                PER
## 5                ARG                FRA
## 6                CHI                MEX
```


> Extracting the year from the date and storing it in the Year column.


``` r
# Extract year from date

world_cup_data$Year <- as.integer(format(world_cup_data$Datetime, "%Y"))
world_cup_data <- world_cup_data %>% filter(!is.na(Year))
```


***

### Visualization 1 - Overall total match outcomes

> Summarizing match results based on goal counts and storing them in a separate column.


``` r
# Summarizing Data

world_cup_data <- world_cup_data %>%
  mutate(MatchResult = case_when(
    Home.Team.Goals > Away.Team.Goals ~ "Home Win",
    Home.Team.Goals < Away.Team.Goals ~ "Away Win",
    TRUE ~ "Draw"
  ))
```

> Each row represents a match result, so a new data set is created for the plot by counting the number of winning, losing, and drawn matches.


``` r
# Creating new data set for the plot

match_outcomes <- world_cup_data %>%
  group_by(MatchResult) %>%
  summarize(total_matches = n())
```

> Plotting a bar chart for the total number of matches versus the total number of match results in the data sets.


``` r
# manual bar colors
bar_colors <- c("#12436d","#28a197" ,"#801650")

ggplot(match_outcomes, aes(x = MatchResult, y = total_matches, fill = bar_colors)) +
  geom_bar(stat = "identity") +
  labs(title = "Overview of Match Results: Total Matches Played and Resulting Outcomes",
       x = "Match Outcomes",
       y = "Total Matches",
       fill = "Match Result") +
  scale_fill_identity() +
  theme_minimal() 
```

![](desai_project_01_files/figure-html/unnamed-chunk-10-1.png)<!-- -->

***

### Visualization 2 - Average Goals per Match Across World Cup Years

> Each row represents a match, thus a new dataset is created for the plot by calculating the average goals per match per World Cup year.


``` r
# Creating new data set for the plot

average_goals_per_match_data <- world_cup_data %>%
  group_by(Year) %>%
  summarize(
    total_matches = n(),
    total_goals = sum(Home.Team.Goals + Away.Team.Goals, na.rm = TRUE),
    average_goals = mean(Home.Team.Goals + Away.Team.Goals, na.rm = TRUE)
  )
```

> plotting the line chart for the Average Goals per Match per World Cup Year


``` r
ggplot(data = average_goals_per_match_data, aes(x = Year, y = average_goals, color = as.factor(Year))) +
  geom_point(size = 3) +
  geom_line(aes(group = 1), linewidth = 1.2) +
  labs(title = "Average Goals per Match per World Cup Year", x = "World Cup Years", y = "Average Goals") +
  scale_color_discrete(name = "World Cup Years") +
  theme_minimal() +
  theme(legend.position = "bottom", legend.box = "horizontal")
```

![](desai_project_01_files/figure-html/unnamed-chunk-12-1.png)<!-- -->
***

### Visualization 3 - Average Home Goals and Away Goals per Match Across World Cup Years

> Summarizing data to create a plot for Average Home Goals and Away Goals per Match Across World Cup Years.


``` r
average_home_away_goals_summary <- world_cup_data %>%
  group_by(Year) %>%
  summarize(
    average_home_goals = mean(Home.Team.Goals, na.rm = TRUE),
    average_away_goals = mean(Away.Team.Goals, na.rm = TRUE)
  )
```


> Plotting the bar chart for the Average Goals vs. Home Goals per Match per World Cup Year.


``` r
ggplot(data = average_home_away_goals_summary, aes(x = Year)) +
  geom_bar(aes(y = average_home_goals, fill = "Home Team Goals"), stat = "identity", position = "dodge") +
  geom_bar(aes(y = average_away_goals, fill = "Away Team Goals"), stat = "identity", position = "dodge") +
  labs(title = "Scoring Trends: Home vs. Away Goals in World Cup History", y = "Average Goals" , x = "World Cup Match Years") +
  scale_fill_manual(values = c("Home Team Goals" = "#ff7e0e", "Away Team Goals" = "#1f77b4"), name = "Goal Types") +
  theme_minimal() +
  theme(legend.position = "bottom", legend.text = element_text(size = 10), legend.title = element_text(size = 12)) +
  guides(fill = guide_legend(nrow = 2, byrow = TRUE))
```

![](desai_project_01_files/figure-html/unnamed-chunk-14-1.png)<!-- -->
