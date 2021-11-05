hw4
================
Mingxi Xu
11/5/2021

The learning objectives are to conduct data scraping and perform text
mining.

# HPC

## Problem 1: Make sure your code is nice

Rewrite the following R functions to make them faster. It is OK (and
recommended) to take a look at Stackoverflow and Google

``` r
# Total row sums
library(microbenchmark)

fun1 <- function(mat) {
  n <- nrow(mat)
  ans <- double(n) 
  for (i in 1:n) {
    ans[i] <- sum(mat[i, ])
  }
  ans
}

fun1alt <- function(mat) {
  # YOUR CODE HERE
  ans <- rowSums(mat)
  ans
}

#  Cumulative sum by row
fun2 <- function(mat) {
  n <- nrow(mat)
  k <- ncol(mat)
  ans <- mat
  for (i in 1:n) {
    for (j in 2:k) {
      ans[i,j] <- mat[i, j] + ans[i, j - 1]
    }
  }
  ans
}

fun2alt <- function(mat) {
  # YOUR CODE HERE
  n <- nrow(mat)
  ans <- mat
  for (i in 1:n){
  ans[i, ] = cumsum(mat[i, ])
  }
  ans
}
```

``` r
# Use the data with this code

set.seed(2315)
dat <- matrix(rnorm(200 * 100), nrow = 200)

# Test for the first
microbenchmark::microbenchmark(
  fun1(dat),
  fun1alt(dat), unit = "relative", check = "equivalent"
)
```

    ## Unit: relative
    ##          expr      min       lq     mean   median       uq       max neval cld
    ##     fun1(dat) 10.81883 10.97519 8.387718 10.91998 10.84065 0.6777964   100   b
    ##  fun1alt(dat)  1.00000  1.00000 1.000000  1.00000  1.00000 1.0000000   100  a

``` r
# Test for the second
microbenchmark::microbenchmark(
  fun2(dat),
  fun2alt(dat), unit = "relative", check = "equivalent"
)
```

    ## Unit: relative
    ##          expr      min       lq     mean  median       uq       max neval cld
    ##     fun2(dat) 5.270298 4.231922 2.999981 3.34848 3.396273 0.2895433   100   b
    ##  fun2alt(dat) 1.000000 1.000000 1.000000 1.00000 1.000000 1.0000000   100  a

The last argument, check = “equivalent”, is included to make sure that
the functions return the same result.

## Problem 2: Make things run faster with parallel computing

The following function allows simulating PI

``` r
sim_pi <- function(n = 1000, i = NULL) {
  p <- matrix(runif(n*2), ncol = 2)
  mean(rowSums(p^2) < 1) * 4
}

# Here is an example of the run
set.seed(156)
sim_pi(1000) # 3.132
```

    ## [1] 3.132

In order to get accurate estimates, we can run this function multiple
times, with the following code:

``` r
# This runs the simulation a 4,000 times, each with 10,000 points
set.seed(1231)
system.time({
  ans <- unlist(lapply(1:4000, sim_pi, n = 10000))
  print(mean(ans))
})
```

    ## [1] 3.14124

    ##    user  system elapsed 
    ##   3.622   0.916   4.554

Rewrite the previous code using parLapply() to make it run faster. Make
sure you set the seed using clusterSetRNGStream():

``` r
library(parallel)
# YOUR CODE HERE
system.time({
  # YOUR CODE HERE
  cl = makePSOCKcluster(2L)
  clusterSetRNGStream(cl, 1231)
  ans <- unlist(parLapply(cl, 1:4000, sim_pi, n=10000))
  print(mean(ans))
  stopCluster(cl)
  # YOUR CODE HERE
})
```

    ## [1] 3.141577

    ##    user  system elapsed 
    ##   0.009   0.005   2.701

# SQL

Setup a temporary database by running the following chunk

``` r
# install.packages(c("RSQLite", "DBI"))

library(RSQLite)
library(DBI)

# Initialize a temporary in memory database
con <- dbConnect(SQLite(), ":memory:")

# Download tables
film <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/film.csv")
film_category <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/film_category.csv")
category <- read.csv("https://raw.githubusercontent.com/ivanceras/sakila/master/csv-sakila-db/category.csv")

# Copy data.frames to database
dbWriteTable(con, "film", film)
dbWriteTable(con, "film_category", film_category)
dbWriteTable(con, "category", category)
```

When you write a new chunk, remember to replace the r with sql,
connection=con. Some of these questions will reqruire you to use an
inner join. Read more about them here
<https://www.w3schools.com/sql/sql_join_inner.asp>

## Question 1

How many many movies is there avaliable in each rating catagory.

``` sql
SELECT COUNT(rating), rating
FROM film
GROUP BY rating
```

<div class="knitsql-table">

| COUNT(rating) | rating |
|--------------:|:-------|
|           180 | G      |
|           210 | NC-17  |
|           194 | PG     |
|           223 | PG-13  |
|           195 | R      |

5 records

</div>

## Question 2

What is the average replacement cost and rental rate for each rating
category.

``` sql
SELECT rating, AVG(replacement_cost), AVG(rental_rate)
FROM film
GROUP BY rating
```

<div class="knitsql-table">

| rating | AVG(replacement\_cost) | AVG(rental\_rate) |
|:-------|-----------------------:|------------------:|
| G      |               20.12333 |          2.912222 |
| NC-17  |               20.13762 |          2.970952 |
| PG     |               18.95907 |          3.051856 |
| PG-13  |               20.40256 |          3.034843 |
| R      |               20.23103 |          2.938718 |

5 records

</div>

## Question 3

Use table film\_category together with film to find the how many films
there are witth each category ID

``` sql
SELECT t2.category_id, COUNT(t1.film_id)
FROM film AS t1
  INNER JOIN film_category AS t2 ON t1.film_id = t2.film_id
GROUP BY t2.category_id
```

<div class="knitsql-table">

| category\_id | COUNT(t1.film\_id) |
|:-------------|-------------------:|
| 1            |                 64 |
| 2            |                 66 |
| 3            |                 60 |
| 4            |                 57 |
| 5            |                 58 |
| 6            |                 68 |
| 7            |                 62 |
| 8            |                 69 |
| 9            |                 73 |
| 10           |                 61 |

Displaying records 1 - 10

</div>

## Question 4

Incorporate table category into the answer to the previous question to
find the name of the most popular category.

``` sql
SELECT t3.name, COUNT(t2.film_id) AS 'film_count'
FROM film_category AS t1
  LEFT JOIN film AS t2 ON t1.film_id = t2.film_id
  LEFT JOIN category AS t3 ON t1.category_id = t3.category_id
GROUP BY t1.category_id
ORDER BY film_count DESC
```

<div class="knitsql-table">

| name        | film\_count |
|:------------|------------:|
| Sports      |          74 |
| Foreign     |          73 |
| Family      |          69 |
| Documentary |          68 |
| Animation   |          66 |
| Action      |          64 |
| New         |          63 |
| Drama       |          62 |
| Sci-Fi      |          61 |
| Games       |          61 |

Displaying records 1 - 10

</div>

The most popular category is Sports.

``` r
dbDisconnect(con)
```
