---
title: "Lab 3 Homework"
author: "Hannah Kempf"
date: "2021-01-14"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Mammals Sleep
1. For this assignment, we are going to use built-in data on mammal sleep patterns. From which publication are these data taken from? Since the data are built-in you can use the help function in R.


```r
?msleep
```

The data is from: 

Savage and G. B. West. A quantitative, theoretical framework for understanding mammalian sleep. Proceedings of the National Academy of Sciences, 104 (3):1051-1056, 2007.

2. Store these data into a new data frame `sleep`.

```r
sleep <- msleep
```

3. What are the dimensions of this data frame (variables and observations)? How do you know? Please show the *code* that you used to determine this below.  


```r
dim(sleep)
```

```
## [1] 83 11
```
The dimensions are 83 x 11. 

4. Are there any NAs in the data? How did you determine this? Please show your code.  

```r
anyNA(sleep)
```

```
## [1] TRUE
```
Yes, there are NAs.

5. Show a list of the column names is this data frame.

```r
names(sleep)
```

```
##  [1] "name"         "genus"        "vore"         "order"        "conservation"
##  [6] "sleep_total"  "sleep_rem"    "sleep_cycle"  "awake"        "brainwt"     
## [11] "bodywt"
```

6. How many herbivores are represented in the data?  

```r
sleep$vore
```

```
##  [1] "carni"   "omni"    "herbi"   "omni"    "herbi"   "herbi"   "carni"  
##  [8] NA        "carni"   "herbi"   "herbi"   "herbi"   "omni"    "herbi"  
## [15] "omni"    "omni"    "omni"    "carni"   "herbi"   "omni"    "herbi"  
## [22] "insecti" "herbi"   "herbi"   "omni"    "omni"    "herbi"   "carni"  
## [29] "omni"    "herbi"   "carni"   "carni"   "herbi"   "omni"    "herbi"  
## [36] "herbi"   "carni"   "omni"    "herbi"   "herbi"   "herbi"   "herbi"  
## [43] "insecti" "herbi"   "carni"   "herbi"   "carni"   "herbi"   "herbi"  
## [50] "omni"    "carni"   "carni"   "carni"   "omni"    NA        "omni"   
## [57] NA        NA        "carni"   "carni"   "herbi"   "insecti" NA       
## [64] "herbi"   "omni"    "omni"    "insecti" "herbi"   NA        "herbi"  
## [71] "herbi"   "herbi"   NA        "omni"    "insecti" "herbi"   "herbi"  
## [78] "omni"    "omni"    "carni"   "carni"   "carni"   "carni"
```

```r
sleep$vore == "herbi"
```

```
##  [1] FALSE FALSE  TRUE FALSE  TRUE  TRUE FALSE    NA FALSE  TRUE  TRUE  TRUE
## [13] FALSE  TRUE FALSE FALSE FALSE FALSE  TRUE FALSE  TRUE FALSE  TRUE  TRUE
## [25] FALSE FALSE  TRUE FALSE FALSE  TRUE FALSE FALSE  TRUE FALSE  TRUE  TRUE
## [37] FALSE FALSE  TRUE  TRUE  TRUE  TRUE FALSE  TRUE FALSE  TRUE FALSE  TRUE
## [49]  TRUE FALSE FALSE FALSE FALSE FALSE    NA FALSE    NA    NA FALSE FALSE
## [61]  TRUE FALSE    NA  TRUE FALSE FALSE FALSE  TRUE    NA  TRUE  TRUE  TRUE
## [73]    NA FALSE FALSE  TRUE  TRUE FALSE FALSE FALSE FALSE FALSE FALSE
```

```r
str(sleep[sleep == "herbi"])
```

```
##  chr [1:32] "herbi" "herbi" "herbi" "herbi" "herbi" "herbi" "herbi" "herbi" ...
```
You can also just summarize that part of the table...


```r
table(sleep$vore)
```

```
## 
##   carni   herbi insecti    omni 
##      19      32       5      20
```

There are 32 herbivores.

7. We are interested in two groups; small and large mammals. Let's define small as less than or equal to 1kg body weight and large as greater than or equal to 200kg body weight. Make two new dataframes (large and small) based on these parameters.

_Here, I used `subset()` to define these groups from the main dataframe._


```r
#First the small animals.
small <- subset(sleep, bodywt <= 1)
small
```

```
## # A tibble: 36 x 11
##    name  genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##    <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
##  1 Owl … Aotus omni  Prim… <NA>                17         1.8      NA       7  
##  2 Grea… Blar… omni  Sori… lc                  14.9       2.3       0.133   9.1
##  3 Vesp… Calo… <NA>  Rode… <NA>                 7        NA        NA      17  
##  4 Guin… Cavis herbi Rode… domesticated         9.4       0.8       0.217  14.6
##  5 Chin… Chin… herbi Rode… domesticated        12.5       1.5       0.117  11.5
##  6 Star… Cond… omni  Sori… lc                  10.3       2.2      NA      13.7
##  7 Afri… Cric… omni  Rode… <NA>                 8.3       2        NA      15.7
##  8 Less… Cryp… omni  Sori… lc                   9.1       1.4       0.15   14.9
##  9 Big … Epte… inse… Chir… lc                  19.7       3.9       0.117   4.3
## 10 Euro… Erin… omni  Erin… lc                  10.1       3.5       0.283  13.9
## # … with 26 more rows, and 2 more variables: brainwt <dbl>, bodywt <dbl>
```

```r
#And then the large.
large <- subset(sleep, bodywt >= 200)
large
```

```
## # A tibble: 7 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Cow   Bos   herbi Arti… domesticated         4         0.7       0.667  20  
## 2 Asia… Elep… herbi Prob… en                   3.9      NA        NA      20.1
## 3 Horse Equus herbi Peri… domesticated         2.9       0.6       1      21.1
## 4 Gira… Gira… herbi Arti… cd                   1.9       0.4      NA      22.1
## 5 Pilo… Glob… carni Ceta… cd                   2.7       0.1      NA      21.4
## 6 Afri… Loxo… herbi Prob… vu                   3.3      NA        NA      20.7
## 7 Braz… Tapi… herbi Peri… vu                   4.4       1         0.9    19.6
## # … with 2 more variables: brainwt <dbl>, bodywt <dbl>
```
8. What is the mean weight for both the small and large mammals?

```r
mean(small$bodywt)
```

```
## [1] 0.2596667
```

```r
mean(large$bodywt)
```

```
## [1] 1747.071
```

9. Using a similar approach as above, do large or small animals sleep longer on average?  


```r
mean(small$sleep_total)
```

```
## [1] 12.65833
```

```r
mean(large$sleep_total)
```

```
## [1] 3.3
```
On average, the small animals sleep longer.

10. Which animal is the sleepiest among the entire dataframe?


```r
max(sleep$sleep_total)
```

```
## [1] 19.9
```

```r
subset(sleep, sleep_total == 19.9)
```

```
## # A tibble: 1 x 11
##   name  genus vore  order conservation sleep_total sleep_rem sleep_cycle awake
##   <chr> <chr> <chr> <chr> <chr>              <dbl>     <dbl>       <dbl> <dbl>
## 1 Litt… Myot… inse… Chir… <NA>                19.9         2         0.2   4.1
## # … with 2 more variables: brainwt <dbl>, bodywt <dbl>
```
The little brown bat is super sleepy. That's adorable. :')

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
