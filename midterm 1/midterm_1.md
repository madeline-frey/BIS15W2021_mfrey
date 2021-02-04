---
title: "Midterm 1"
author: "Madeline Frey"
date: "2021-02-02"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your code should be organized, clean, and run free from errors. Be sure to **add your name** to the author header above. You may use any resources to answer these questions (including each other), but you may not post questions to Open Stacks or external help sites. There are 12 total questions.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

This exam is due by **12:00p on Thursday, January 28**.  

## Load the tidyverse
If you plan to use any other libraries to complete this assignment then you should load them here.

```r
library(tidyverse)
library(janitor)
```

## Questions
**1. (2 points) Briefly explain how R, RStudio, and GitHub work together to make work flows in data science transparent and repeatable. What is the advantage of using RMarkdown in this context?**  
### R is the programming language used by us in the console of RStudio. We use Github to keep track of versions and work collaboratively with others on coding projects. RMarkdowns are useful in producing easy-to-follow documents that look nice and present necessary information. 

<style>
div.blue { background-color:#e6f0ff; border-radius: 5px; padding: 20px;}
</style>
<div class = "blue">

**2. (2 points) What are the three types of `data structures` that we have discussed? Why are we using data frames for BIS 15L?**
### vectors, tables, and data frames. Data frames are very useful to manage many variables and observations. 
</div>

In the midterm 1 folder there is a second folder called `data`. Inside the `data` folder, there is a .csv file called `ElephantsMF`. These data are from Phyllis Lee, Stirling University, and are related to Lee, P., et al. (2013), "Enduring consequences of early experiences: 40-year effects on survival and success among African elephants (Loxodonta africana)," Biology Letters, 9: 20130011. [kaggle](https://www.kaggle.com/mostafaelseidy/elephantsmf).  

**3. (2 points) Please load these data as a new object called `elephants`. Use the function(s) of your choice to get an idea of the structure of the data. Be sure to show the class of each variable.**

```r
elephants <- readr::read_csv("data/ElephantsMF.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Age = col_double(),
##   Height = col_double(),
##   Sex = col_character()
## )
```

```r
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ Age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17...
## $ Height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204....
## $ Sex    <chr> "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", "M", ...
```

**4. (2 points) Change the names of the variables to lower case and change the class of the variable `sex` to a factor.**

```r
elephants <- select_all(elephants, tolower)
elephants$sex <- as.factor(elephants$sex)
glimpse(elephants)
```

```
## Rows: 288
## Columns: 3
## $ age    <dbl> 1.40, 17.50, 12.75, 11.17, 12.67, 12.67, 12.25, 12.17, 28.17...
## $ height <dbl> 120.00, 227.00, 235.00, 210.00, 220.00, 189.00, 225.00, 204....
## $ sex    <fct> M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, M, ...
```

**5. (2 points) How many male and female elephants are represented in the data?**

```r
elephants %>% 
  tabyl(sex)
```

```
##  sex   n   percent
##    F 150 0.5208333
##    M 138 0.4791667
```

**6. (2 points) What is the average age all elephants in the data?**

```r
elephants %>% 
  summarize(mean_age=mean(age))
```

```
## # A tibble: 1 x 1
##   mean_age
##      <dbl>
## 1     11.0
```


**7. (2 points) How does the average age and height of elephants compare by sex?**

```r
elephants %>% 
  group_by(sex) %>% 
  summarize(mean_age=mean(age), mean_height=mean(height))
```

```
## # A tibble: 2 x 3
##   sex   mean_age mean_height
## * <fct>    <dbl>       <dbl>
## 1 F        12.8         190.
## 2 M         8.95        185.
```


**8. (2 points) How does the average height of elephants compare by sex for individuals over 25 years old. Include the min and max height as well as the number of individuals in the sample as part of your analysis.**

```r
elephants %>% 
  filter(age>25) %>% 
  group_by(sex) %>% 
  summarize(mean_height=mean(height), max_height=max(height), min_height=min(height), n=n())
```

```
## # A tibble: 2 x 5
##   sex   mean_height max_height min_height     n
## * <fct>       <dbl>      <dbl>      <dbl> <int>
## 1 F            233.       278.       206.    25
## 2 M            273.       304.       237.     8
```

For the next series of questions, we will use data from a study on vertebrate community composition and impacts from defaunation in [Gabon, Africa](https://en.wikipedia.org/wiki/Gabon). One thing to notice is that the data include 24 separate transects. Each transect represents a path through different forest management areas.  

Reference: Koerner SE, Poulsen JR, Blanchard EJ, Okouyi J, Clark CJ. Vertebrate community composition and diversity declines along a defaunation gradient radiating from rural villages in Gabon. _Journal of Applied Ecology_. 2016. This paper, along with a description of the variables is included inside the midterm 1 folder.  

**9. (2 points) Load `IvindoData_DryadVersion.csv` and use the function(s) of your choice to get an idea of the overall structure. Change the variables `HuntCat` and `LandUse` to factors.**

```r
gabon <- readr::read_csv("data/IvindoData_DryadVersion.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_double(),
##   HuntCat = col_character(),
##   LandUse = col_character()
## )
## i Use `spec()` for the full column specifications.
```

```r
gabon <- select_all(gabon, tolower)
gabon$huntcat <- as.factor(gabon$huntcat)
gabon$landuse <- as.factor(gabon$landuse)
glimpse(gabon)
```

```
## Rows: 24
## Columns: 26
## $ transectid              <dbl> 1, 2, 2, 3, 4, 5, 6, 7, 8, 9, 13, 14, 15, 1...
## $ distance                <dbl> 7.14, 17.31, 18.32, 20.85, 15.95, 17.47, 24...
## $ huntcat                 <fct> Moderate, None, None, None, None, None, Non...
## $ numhouseholds           <dbl> 54, 54, 29, 29, 29, 29, 29, 54, 25, 73, 46,...
## $ landuse                 <fct> Park, Park, Park, Logging, Park, Park, Park...
## $ veg_rich                <dbl> 16.67, 15.75, 16.88, 12.44, 17.13, 16.50, 1...
## $ veg_stems               <dbl> 31.20, 37.44, 32.33, 29.39, 36.00, 29.22, 3...
## $ veg_liana               <dbl> 5.78, 13.25, 4.75, 9.78, 13.25, 12.88, 8.38...
## $ veg_dbh                 <dbl> 49.57, 34.59, 42.82, 36.62, 41.52, 44.07, 5...
## $ veg_canopy              <dbl> 3.78, 3.75, 3.43, 3.75, 3.88, 2.50, 4.00, 4...
## $ veg_understory          <dbl> 2.89, 3.88, 3.00, 2.75, 3.25, 3.00, 2.38, 2...
## $ ra_apes                 <dbl> 1.87, 0.00, 4.49, 12.93, 0.00, 2.48, 3.78, ...
## $ ra_birds                <dbl> 52.66, 52.17, 37.44, 59.29, 52.62, 38.64, 4...
## $ ra_elephant             <dbl> 0.00, 0.86, 1.33, 0.56, 1.00, 0.00, 1.11, 0...
## $ ra_monkeys              <dbl> 38.59, 28.53, 41.82, 19.85, 41.34, 43.29, 4...
## $ ra_rodent               <dbl> 4.22, 6.04, 1.06, 3.66, 2.52, 1.83, 3.10, 1...
## $ ra_ungulate             <dbl> 2.66, 12.41, 13.86, 3.71, 2.53, 13.75, 3.10...
## $ rich_allspecies         <dbl> 22, 20, 22, 19, 20, 22, 23, 19, 19, 19, 21,...
## $ evenness_allspecies     <dbl> 0.793, 0.773, 0.740, 0.681, 0.811, 0.786, 0...
## $ diversity_allspecies    <dbl> 2.452, 2.314, 2.288, 2.006, 2.431, 2.429, 2...
## $ rich_birdspecies        <dbl> 11, 10, 11, 8, 8, 10, 11, 11, 11, 9, 11, 11...
## $ evenness_birdspecies    <dbl> 0.732, 0.704, 0.688, 0.559, 0.799, 0.771, 0...
## $ diversity_birdspecies   <dbl> 1.756, 1.620, 1.649, 1.162, 1.660, 1.775, 1...
## $ rich_mammalspecies      <dbl> 11, 10, 11, 11, 12, 12, 12, 8, 8, 10, 10, 1...
## $ evenness_mammalspecies  <dbl> 0.736, 0.705, 0.650, 0.619, 0.736, 0.694, 0...
## $ diversity_mammalspecies <dbl> 1.764, 1.624, 1.558, 1.484, 1.829, 1.725, 1...
```


**10. (4 points) For the transects with high and moderate hunting intensity, how does the average diversity of birds and mammals compare?**

```r
gabon %>% 
  select(huntcat, diversity_birdspecies, diversity_mammalspecies) %>% 
  group_by(huntcat) %>% 
  summarize(mean_bird_div=mean(diversity_birdspecies), mean_mammal_div=mean(diversity_mammalspecies))
```

```
## # A tibble: 3 x 3
##   huntcat  mean_bird_div mean_mammal_div
## * <fct>            <dbl>           <dbl>
## 1 High              1.66            1.74
## 2 Moderate          1.62            1.68
## 3 None              1.70            1.68
```

**11. (4 points) One of the conclusions in the study is that the relative abundance of animals drops off the closer you get to a village. Let's try to reconstruct this (without the statistics). How does the relative abundance (RA) of apes, birds, elephants, monkeys, rodents, and ungulates compare between sites that are less than 5km from a village to sites that are greater than 20km from a village? The variable `Distance` measures the distance of the transect from the nearest village. Hint: try using the `across` operator.**  

```r
gabon %>% 
  filter(distance<5 | distance>20) %>% 
  group_by(distance<5) %>% 
  summarize(across(c(ra_apes, ra_birds, ra_elephant, ra_monkeys, ra_rodent, ra_ungulate),mean))
```

```
## # A tibble: 2 x 7
##   `distance < 5` ra_apes ra_birds ra_elephant ra_monkeys ra_rodent ra_ungulate
## * <lgl>            <dbl>    <dbl>       <dbl>      <dbl>     <dbl>       <dbl>
## 1 FALSE             7.21     44.5      0.557        40.1      2.68        4.98
## 2 TRUE              0.08     70.4      0.0967       24.1      3.66        1.59
```


**12. (4 points) Based on your interest, do one exploratory analysis on the `gabon` data of your choice. This analysis needs to include a minimum of two functions in `dplyr.`**

```r
#question: does the number of households (proxy for the amount of people) dictate hunting severity?
gabon %>% 
  select(huntcat, numhouseholds) %>% 
  group_by(huntcat) %>% 
  summarize(mean_household=mean(numhouseholds), deviation=sd(numhouseholds),n=n())
```

```
## # A tibble: 3 x 4
##   huntcat  mean_household deviation     n
## * <fct>             <dbl>     <dbl> <int>
## 1 High               30.4      21.8     7
## 2 Moderate           49.6      14.6     8
## 3 None               33.2      12.4     9
```

```r
#answer: not necessarily. 
```

