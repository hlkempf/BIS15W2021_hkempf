---
title: "RMarkdown Practice"
output: 
  html_document: 
    keep_md: yes
---




# This is my first **Markdown** file!

## This is my first _Markdown_ file!

### This is my first Markdown file!


```r
4-1
```

```
## [1] 3
```

```r
4*2
```

```
## [1] 8
```

```r
4+1
```

```
## [1] 5
```

```r
4/2
```

```
## [1] 2
```
## This is my email [email](mailto:hlkempf@ucdavis.edu)

## This is Google [Google](http://www.google.com)

## From lab1_2 step 3 

```r
#install.packages("tidyverse")
library("tidyverse")
```


```r
ggplot(mtcars, aes(x = factor(cyl))) +
    geom_bar()
```

![](RMarkdown-Practice_files/figure-html/unnamed-chunk-3-1.png)<!-- -->


