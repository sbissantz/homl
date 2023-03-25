Hands-On Machine Learning with R
================
Steven Marcel Bißantz
2023-03-25

## Chapter 2 Modeling Process

``` r
# Helper packages
pkgs <- c("rsample", # Resampling procedures
          "caret",   # Resampling and model training 
          "h20"      # Resampling and model training
          ) 
# lapply(pkgs, library, character.only=TRUE) 

# h2o 
h2o::h2o.no_progress()  # turn off h2o progress bars
h2o::h2o.init()         # launch h2o
```

    ##  Connection successful!
    ## 
    ## R is connected to the H2O cluster: 
    ##     H2O cluster uptime:         36 minutes 48 seconds 
    ##     H2O cluster timezone:       Europe/Berlin 
    ##     H2O data parsing timezone:  UTC 
    ##     H2O cluster version:        3.40.0.1 
    ##     H2O cluster version age:    1 month and 16 days 
    ##     H2O cluster name:           H2O_started_from_R_steven_qdt481 
    ##     H2O cluster total nodes:    1 
    ##     H2O cluster total memory:   4.00 GB 
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

# Important: h2o can only deal with unordered factors

# Coerce ordered factors
unorder_if <- function(x) ifelse(is.ordered(x), factor(x, ordered = FALSE), x)
# Note: '[]' used to keep the data frame structure
churn[] <- lapply(churn, unorder_if)
```
