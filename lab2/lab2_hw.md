---
title: "Lab 2 Homework"
author: "Hannah Kempf"
date: "2021-01-09"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?

A vector in R is a data structure. Basically, it is one way that R can organize data. A vector is often depicted as a list of numbers, variables or words.

2. What is a data matrix in R?  

A data matrix is a different type of R data structure. It consists of multiple vectors, similar to a data table.

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.

First, create an object `all_data` that combines all the vectors...

```r
all_data <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)

#print this.
all_data
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```
Next, transform this into a matrix, and then print to check.


```r
spring_matrix <- matrix(all_data, nrow = 8, byrow = T)
spring_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```
Yay!

5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.


```r
scientists <- c("Jill", "Steve", "Susan")
scientists
```

```
## [1] "Jill"  "Steve" "Susan"
```

```r
#Name the columns with the "scientist" vector
colnames(spring_matrix) <- scientists

#print matrix
spring_matrix
```

```
##       Jill Steve Susan
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```
Next, name the rows.

I.e, make a vector for the names, then use that vector w/ the `rownames()` command.

```r
spring_names <- c("Bluebell Spring", "Opal Spring", "Riverside Spring", "Too Hot Spring", "Mystery Spring", "Emerald Spring", "Black Spring", "Pearl Spring")

#name the rows using this vector!

rownames(spring_matrix) <- spring_names

#print the matrix
spring_matrix
```

```
##                   Jill Steve Susan
## Bluebell Spring  36.25 35.40 35.30
## Opal Spring      35.15 35.35 33.35
## Riverside Spring 30.70 29.65 29.20
## Too Hot Spring   39.70 40.05 38.65
## Mystery Spring   31.85 31.40 29.30
## Emerald Spring   30.20 30.65 29.75
## Black Spring     32.90 32.50 32.80
## Pearl Spring     36.80 36.45 33.15
```

6. Calculate the mean temperature of all eight* springs.


```r
spring1_avg <- mean(spring_matrix[1,])
spring2_avg <- mean(spring_matrix[2,])
spring3_avg <- mean(spring_matrix[3,])
spring4_avg <- mean(spring_matrix[4,])
spring5_avg <- mean(spring_matrix[5,])
spring6_avg <- mean(spring_matrix[6,])
spring7_avg <- mean(spring_matrix[7,])
spring8_avg <- mean(spring_matrix[8,])
  
#Make new object for the averages
Average <- c(spring1_avg, spring2_avg, spring3_avg, spring4_avg, spring5_avg, spring6_avg, spring7_avg, spring8_avg)

#Print this out
Average
```

```
## [1] 35.65000 34.61667 29.85000 39.46667 30.85000 30.20000 32.73333 35.46667
```

```r
#Seems right.
```
7. Add this as a new column in the data matrix.

Reminder:
`cbind()` adds columns.
`rbind()` adds rows

I.e., make a new matrix using the older one + `cbind()`...


```r
spring_matrix_update <- cbind(spring_matrix, Average)

spring_matrix_update
```

```
##                   Jill Steve Susan  Average
## Bluebell Spring  36.25 35.40 35.30 35.65000
## Opal Spring      35.15 35.35 33.35 34.61667
## Riverside Spring 30.70 29.65 29.20 29.85000
## Too Hot Spring   39.70 40.05 38.65 39.46667
## Mystery Spring   31.85 31.40 29.30 30.85000
## Emerald Spring   30.20 30.65 29.75 30.20000
## Black Spring     32.90 32.50 32.80 32.73333
## Pearl Spring     36.80 36.45 33.15 35.46667
```

8. Show Susan's value for Opal Spring only.

```r
spring_matrix_update[2,3]
```

```
## [1] 33.35
```

9. Calculate the mean for Jill's column only.  


```r
mean(spring_matrix_update[,1])
```

```
## [1] 34.19375
```

10. Use the data matrix to perform one calculation or operation of your interest.

The average of all of Steve's data minus the average of Susan's data.

```r
mean(spring_matrix_update[,2]) - mean(spring_matrix_update[,3])
```

```
## [1] 1.24375
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
