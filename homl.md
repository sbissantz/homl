Notes: ‘Hands-On Machine Learning with R’
================
Steven Marcel Bißantz
2023-03-25

## Chapter 2 Modeling Process

``` r
# Helper packages
pkgs <- c("rsample", # Resampling procedures
          "caret",   # Resampling and model training 
          "h20")     # Resampling and model training
           
# lapply(pkgs, library, character.only=TRUE) 

# h2o 
h2o::h2o.no_progress() # Avoid the progress bars in the output
h2o::h2o.init()
```

    ##  Connection successful!
    ## 
    ## R is connected to the H2O cluster: 
    ##     H2O cluster uptime:         1 days 22 hours 
    ##     H2O cluster timezone:       Europe/Berlin 
    ##     H2O data parsing timezone:  UTC 
    ##     H2O cluster version:        3.40.0.1 
    ##     H2O cluster version age:    1 month and 18 days 
    ##     H2O cluster name:           H2O_started_from_R_steven_qdt481 
    ##     H2O cluster total nodes:    1 
    ##     H2O cluster total memory:   3.99 GB 
    ##     H2O cluster total cores:    8 
    ##     H2O cluster allowed cores:  8 
    ##     H2O cluster healthy:        TRUE 
    ##     H2O Connection ip:          localhost 
    ##     H2O Connection port:        54321 
    ##     H2O Connection proxy:       NA 
    ##     H2O Internal Security:      FALSE 
    ##     R Version:                  R version 4.2.3 (2023-03-15)

``` r
?AmesHousing::ames_raw

# Ames housing data
ames <- AmesHousing::make_ames()

# Coerce to a h2o frame
ames_h2o <- h2o::as.h2o(ames)

# Job attrition data
# Note: the data are now in the modeldata package
churn <- modeldata::attrition
```

Important: h2o cannot handle ordered factors so we need to coerce them
to unordered factors before the analysis.

``` r
# Coerce ordered factors
unorder_if <- function(x) ifelse(is.ordered(x), factor(x, ordered = FALSE), x)
# Trick: Use '[]' to keep the data frame structure
churn[] <- lapply(churn, unorder_if)
```

### Data splitting

Simple random sampling and stratified sampling

#### Simple random sampling

Use `base` and no additional packages

``` r
# Reproduceable results
set.seed(123)
# Number of rows
n_rows <- nrow(ames)
# Row index
row_index <- seq(n_rows)
# Index for the training cases
train_index <- sample(row_index, size = round(n_rows) * 0.7, replace = FALSE)
# Training set
train <- ames[train_index,] ; cat("Training set: ", dim(train), "\n")
```

    ## Training set:  2051 81

``` r
# Testing set
test <- ames[-train_index,] ; cat("Testing set: ", dim(test), "\n")
```

    ## Testing set:  879 81

``` r
# Plausibility check
cat("Split: ", dim(train)[1] / n_rows * 100, "/", dim(test)[1] / n_rows * 100)
```

    ## Split:  70 / 30

Use the `caret` package

``` r
set.seed(123)
```

Use the `rsample` package

``` r
set.seed(123)
```
