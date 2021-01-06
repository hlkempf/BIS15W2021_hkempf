---
title: "Lab 1 Homework"
author: "Hannah Kempf"
date: "2021-01-06"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
  pdf_document: default
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

**1. Please complete the [class survey](https://forms.gle/AHHXd3aobaAdkkFg9) if you have not already done so.**

Done!

**2. What is the difference between R and RStudio? What is GitHub and why is it useful to programmers?**  

R is a scripting language that is used to work with and manipulate data. RStudio is the GUI (graphical user interface) that allows us to use the R langauge. R studio is more user-friendly and allows you to personalize your R workspace and view plots/graphs etc. GitHub is largely used for version control, and as a means to upload your code and make it available for other folks. Most scientific papers that I read these days link to a github repository as a menas to make their work reproducible.

**3. Navigate to my [BIS15L](https://github.com/jmledford3115/BIS15LW2021_jledford) repository on GitHub. Notice that near the bottom there is a nice, clean description of the repository. You want this on your repository! It is built by making edits to the `README.md` file using the RMarkdown conventions. Edit your repository's `README.md` file so that you have a clean informative description, including your email address. Be creative! You don't need to just copy mine. There are lots of examples online.**  

Done! See [this link](https://github.com/hlkempf/BIS15W2021_hkempf)

**4. Calculate the following expressions. Be sure to include each one in a separate code chunk.**  

_HLK_: reminder that shortcut for inserting code chunk is `option+command+i`


```r
getwd()
```

```
## [1] "/Users/hannahkempf/Desktop/BIS15W2021_hkempf/lab1"
```


```r
 + 5 - 3 * 2  
```

```
## [1] -1
```

```r
  + 8 / 2 ** 2
```

```
## [1] 2
```
  
**5. Did any of the results in #4 surprise you? Write two programs that calculate each expression such that the result for the first example is 4 and the second example is 16.**  

These didn't surprise me, as order of operations (PEMDAS) would produce the results printed above. Using the following code would produce 4 and 16:


```r
(5-3) * 2
```

```
## [1] 4
```


```r
(8/2) ** 2
```

```
## [1] 16
```

**6. `Objects` in R are a way in which we can store data or operations. We will talk more about objects next week. For now, make a new object `pi` as 3.14159265359 by running the following code chunk. You should now see the object `pi` in the environment window in the top right.**  


```r
pi <- 3.14159265359
```

**7. Let's say we want to multiply `pi` by 2. Using the same arithmetic principles that we just learned, write a code chunk that performs this operation using the object we created.** 


```r
pi * 2
```

```
## [1] 6.283185
```

**8. In order to get help with any command in R, just type a `?` in front the command of interest. Practice this by running the following code chunk.**  

```r
?mean
```

**9. Let's calculate the mean for the numbers 2, 8, 6, 4, 9, 10. I have built an object `x` for you below so all you need to do is run the first code chunk and then create a second code chunk that shows the calculation. Give it a try!**  

```r
x <- c(2, 8, 6, 6, 7, 4, 9, 9, 9, 10)
```
Second code chunk:

```r
mean(x)
```

```
## [1] 7
```

**10. Repeat the procedure above, but this time calculate the median.**  

```r
median(x)
```

```
## [1] 7.5
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 

