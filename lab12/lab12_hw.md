---
title: "Lab 12 Homework"
author: "Madeline Frey"
date: "2021-02-24"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the libraries

```r
library(tidyverse)
library(janitor)
library(here)
library(ggmap)
library(albersusa)
```

## Load the Data
We will use two separate data sets for this homework.  

1. The first [data set](https://rcweb.dartmouth.edu/~f002d69/workshops/index_rspatial.html) represent sightings of grizzly bears (Ursos arctos) in Alaska.  
2. The second data set is from Brandell, Ellen E (2021), Serological dataset and R code for: Patterns and processes of pathogen exposure in gray wolves across North America, Dryad, [Dataset](https://doi.org/10.5061/dryad.5hqbzkh51).  

1. Load the `grizzly` data and evaluate its structure. As part of this step, produce a summary that provides the range of latitude and longitude so you can build an appropriate bounding box.

```r
grizzly <- read_csv(here("lab12", "data", "bear-sightings.csv")) 
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   bear.id = col_double(),
##   longitude = col_double(),
##   latitude = col_double()
## )
```


```r
grizzly %>% 
  select(latitude, longitude) %>% 
  summary()
```

```
##     latitude       longitude     
##  Min.   :55.02   Min.   :-166.2  
##  1st Qu.:58.13   1st Qu.:-154.2  
##  Median :60.97   Median :-151.0  
##  Mean   :61.41   Mean   :-149.1  
##  3rd Qu.:64.13   3rd Qu.:-145.6  
##  Max.   :70.37   Max.   :-131.3
```

2. Use the range of the latitude and longitude to build an appropriate bounding box for your map.

```r
lat <- c(55.02, 70.37)
long <- c(-166.2, -131.3)# uses min and max lat and long
bbox <- make_bbox(long, lat, f = 0.05)
```

3. Load a map from `stamen` in a terrain style projection and display the map.

```r
bear_map1 <- get_map(bbox, maptype = "terrain", source = "stamen")
```

```
## Map tiles by Stamen Design, under CC BY 3.0. Data by OpenStreetMap, under ODbL.
```


```r
ggmap(bear_map1)
```

![](lab12_hw_files/figure-html/unnamed-chunk-6-1.png)<!-- -->

4. Build a final map that overlays the recorded observations of grizzly bears in Alaska.

```r
ggmap(bear_map1)+geom_point(data= grizzly, aes(longitude, latitude), size=.5)+
  labs(x="Longitude", y="Latitude", title= "Alaska Grizzly Bear Sightings")
```

![](lab12_hw_files/figure-html/unnamed-chunk-7-1.png)<!-- -->

5. Let's switch to the wolves data. Load the data and evaluate its structure.

```r
wolves <- read_csv(here("lab12", "data", "wolves.csv")) 
```

```
## 
## ── Column specification ────────────────────────────────────────────────────────
## cols(
##   .default = col_double(),
##   pop = col_character(),
##   age.cat = col_character(),
##   sex = col_character(),
##   color = col_character()
## )
## ℹ Use `spec()` for the full column specifications.
```

```r
wolves$pop <- as.factor(wolves$pop)
```

6. How many distinct wolf populations are included in this study? Mae a new object that restricts the data to the wolf populations in the lower 48 US states.

```r
wolves %>% 
  count(n_distinct(pop))
```

```
## # A tibble: 1 x 2
##   `n_distinct(pop)`     n
## *             <int> <int>
## 1                17  1986
```

```r
levels(wolves$pop)
```

```
##  [1] "AK.PEN"  "BAN.JAS" "BC"      "DENALI"  "ELLES"   "GTNP"    "INT.AK" 
##  [8] "MEXICAN" "MI"      "MT"      "N.NWT"   "ONT"     "SE.AK"   "SNF"    
## [15] "SS.NWT"  "YNP"     "YUCH"
```


```r
lower_wolves <- wolves %>% 
  filter(pop!="AK.PEN",pop!="DENALI", pop!="INT.AK", pop!="YUCH", pop!="SE.AK", pop!="BAN.JAS", pop!="ONT", pop!="BC", pop!="SS.NWT", pop!="N.NWT", pop!="ELLES")
lower_wolves
```

```
## # A tibble: 1,169 x 23
##    pop    year age.cat sex   color   lat  long habitat human pop.density
##    <fct> <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>       <dbl>
##  1 GTNP   2012 P       M     G      43.8 -111.  10375. 3924.        34.0
##  2 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0
##  3 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0
##  4 GTNP   2012 P       M     B      43.8 -111.  10375. 3924.        34.0
##  5 GTNP   2013 A       F     G      43.8 -111.  10375. 3924.        34.0
##  6 GTNP   2013 A       M     G      43.8 -111.  10375. 3924.        34.0
##  7 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0
##  8 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0
##  9 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0
## 10 GTNP   2013 P       F     G      43.8 -111.  10375. 3924.        34.0
## # … with 1,159 more rows, and 13 more variables: pack.size <dbl>,
## #   standard.habitat <dbl>, standard.human <dbl>, standard.pop <dbl>,
## #   standard.packsize <dbl>, standard.latitude <dbl>, standard.longitude <dbl>,
## #   cav.binary <dbl>, cdv.binary <dbl>, cpv.binary <dbl>, chv.binary <dbl>,
## #   neo.binary <dbl>, toxo.binary <dbl>
```

7. Use the `albersusa` package to make a base map of the lower 48 US states.

```r
us_comp <- usa_sf()
```


```r
lower_comp <- us_comp %>% 
  filter(name!="Alaska", name!="Hawaii")
```


```r
ggplot()+
  geom_sf(data = lower_comp, size = 0.125)
```

![](lab12_hw_files/figure-html/unnamed-chunk-15-1.png)<!-- -->

8. Use the relimited data to plot the distribution of wolf populations in the lower 48 US states.

```r
ggplot()+
  geom_sf(data = lower_comp, size = 0.125)+
  geom_point(data=lower_wolves, aes(long, lat))
```

![](lab12_hw_files/figure-html/unnamed-chunk-16-1.png)<!-- -->

9. What is the average pack size for the wolves in this study by region?

```r
wolves %>% #assuming you are asking for all in the study.
  group_by(pop) %>% 
  summarize(mean_pack=mean(pack.size))
```

```
## # A tibble: 17 x 2
##    pop     mean_pack
##  * <fct>       <dbl>
##  1 AK.PEN       8.78
##  2 BAN.JAS      9.56
##  3 BC           5.88
##  4 DENALI       6.45
##  5 ELLES        9.19
##  6 GTNP         8.1 
##  7 INT.AK       6.24
##  8 MEXICAN      4.04
##  9 MI           7.12
## 10 MT           5.62
## 11 N.NWT        4   
## 12 ONT          4.37
## 13 SE.AK        5   
## 14 SNF          4.81
## 15 SS.NWT       3.55
## 16 YNP          8.25
## 17 YUCH         6.37
```

10. Make a new map that shows the distribution of wolves in the lower 48 US states but which has the size of location markers adjusted by pack size.

```r
lower_wolves <- lower_wolves %>% 
   group_by(pop) %>% 
  mutate(mean_pack=mean(pack.size))
lower_wolves
```

```
## # A tibble: 1,169 x 24
## # Groups:   pop [6]
##    pop    year age.cat sex   color   lat  long habitat human pop.density
##    <fct> <dbl> <chr>   <chr> <chr> <dbl> <dbl>   <dbl> <dbl>       <dbl>
##  1 GTNP   2012 P       M     G      43.8 -111.  10375. 3924.        34.0
##  2 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0
##  3 GTNP   2012 P       F     G      43.8 -111.  10375. 3924.        34.0
##  4 GTNP   2012 P       M     B      43.8 -111.  10375. 3924.        34.0
##  5 GTNP   2013 A       F     G      43.8 -111.  10375. 3924.        34.0
##  6 GTNP   2013 A       M     G      43.8 -111.  10375. 3924.        34.0
##  7 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0
##  8 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0
##  9 GTNP   2013 P       M     G      43.8 -111.  10375. 3924.        34.0
## 10 GTNP   2013 P       F     G      43.8 -111.  10375. 3924.        34.0
## # … with 1,159 more rows, and 14 more variables: pack.size <dbl>,
## #   standard.habitat <dbl>, standard.human <dbl>, standard.pop <dbl>,
## #   standard.packsize <dbl>, standard.latitude <dbl>, standard.longitude <dbl>,
## #   cav.binary <dbl>, cdv.binary <dbl>, cpv.binary <dbl>, chv.binary <dbl>,
## #   neo.binary <dbl>, toxo.binary <dbl>, mean_pack <dbl>
```


```r
ggplot()+ 
  geom_sf(data = lower_comp, size = 0.125)+
  geom_point(data=lower_wolves, aes(long, lat, size=mean_pack))+
  labs(x="Longitude", y="Latitude", title= "Alaska Grizzly Bear Sightings")+
   theme_minimal()
```

![](lab12_hw_files/figure-html/unnamed-chunk-19-1.png)<!-- -->

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
