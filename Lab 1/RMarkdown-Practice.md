---
title: "RMarkdown Practice"
author: "Madeline Frey"
date: "1/5/2021"
output: 
  html_document: 
    keep_md: yes
---



# This is my first RMarkdown
# _This is my first RMarkdown_

[Marine Science Club Webpage](https://marinesciclubdavis.weebly.com/)

```r
2*3
```

```
## [1] 6
```

```r
1+1+1
```

```
## [1] 3
```

```r
9/3
```

```
## [1] 3
```


```r
#install.packages("tidyverse")
library("tidyverse")
```


```r
ggplot(mtcars, aes(x = factor(cyl))) +
    geom_bar()
```

![](RMarkdown-Practice_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

