Project proposal
================
XEM Team

turning survived into factor variable

``` r
titanicm <- titanic %>%
  mutate(Survived = factor(Survived)) %>%
  mutate(Pclass = factor(Pclass)) 
```

``` r
titanic_age_na <- titanicm %>%
  filter(!is.na(Age))
```

splitting data

``` r
set.seed(1116)
titanic_split <- initial_split(titanic_age_na, prop = 0.80)
train_data <- training(titanic_split)
test_data  <- testing(titanic_split)
```

build recipe

``` r
titanic_rec <- recipe(Survived ~ ., data = titanicm) %>%
  # PassengerId isn't predictor, but keep around to ID
  update_role(PassengerId, new_role = "ID") %>%
  # remove name and cabin
  step_rm(Name, Ticket, Cabin, SibSp, Parch, Fare, Embarked) %>%
  # remove NAs for step cut to work
  step_filter(!is.na(Age)) %>%
  # discretise age variable
  step_cut(Age, breaks = c(0, 6, 18, 54)) %>%
  # make dummy variables 
  step_dummy(all_nominal(), -all_outcomes()) %>%
  # remove zero variance predictors
  step_zv(all_predictors())
```

define model

``` r
titanic_mod <- logistic_reg() %>% 
  set_engine("glm")
```

define workflow

``` r
titanic_wflow <- workflow() %>% 
add_model(titanic_mod) %>% 
add_recipe(titanic_rec)
```

fit model to training data

``` r
titanic_fit <- titanic_wflow %>% 
  fit(data = train_data)
tidy(titanic_fit)
```

    ## # A tibble: 7 × 5
    ##   term         estimate std.error statistic  p.value
    ##   <chr>           <dbl>     <dbl>     <dbl>    <dbl>
    ## 1 (Intercept)      4.51     0.549      8.22 2.08e-16
    ## 2 Pclass_X2       -1.40     0.311     -4.52 6.27e- 6
    ## 3 Pclass_X3       -2.53     0.298     -8.52 1.67e-17
    ## 4 Sex_male        -2.69     0.245    -11.0  4.88e-28
    ## 5 Age_X.6.18.     -1.80     0.530     -3.39 6.88e- 4
    ## 6 Age_X.18.54.    -1.71     0.451     -3.78 1.54e- 4
    ## 7 Age_X.54.80.    -2.80     0.647     -4.33 1.52e- 5

-   **Slope - volume:** *All else held constant*, for each additional
    cubic centimetre books are larger in volume, we would expect the
    weight to be higher, on average, by 0.718 grams.
    -   in this case it makes sense for this to change independently
        change but sometimes this assumption isn’t reasonable (e.g. rain
        and humidity)
-   **Slope - cover:** *All else held constant*, paperback books are
    weigh, on average, by 184 grams less than hardcover books.
-   **Intercept:** Hardcover books with 0 volume are expected to weigh
    198 grams, on average
    -   (Doesn’t make sense in context.)

    For each unit increase in x, y is expected on average to be
    higher/lower by a factor of e^{b_1}“.

predict test data

``` r
 titanic_pred <-predict(titanic_fit, test_data, type = "prob") %>%
  bind_cols(test_data %>% select(Survived, PassengerId)) 
titanic_pred
```

    ## # A tibble: 143 × 4
    ##    .pred_0 .pred_1 Survived PassengerId
    ##      <dbl>   <dbl> <fct>          <int>
    ##  1   0.473  0.527  0                  7
    ##  2   0.919  0.0812 0                 13
    ##  3   0.424  0.576  1                 16
    ##  4   0.434  0.566  1                 26
    ##  5   0.473  0.527  0                 36
    ##  6   0.198  0.802  0                 42
    ##  7   0.456  0.544  0                 50
    ##  8   0.198  0.802  1                 54
    ##  9   0.919  0.0812 0                 58
    ## 10   0.785  0.215  0                 71
    ## # … with 133 more rows

roc curve

``` r
titanic_pred %>%
  roc_curve(truth = Survived, .pred_1, event_level = "second") %>%
  autoplot()
```

![](modelfitting_files/figure-gfm/titanic-roc-curve-1.png)<!-- -->

cutoff probability

``` r
cutoff_prob <- 0.6
titanic_pred %>%
  mutate(
    Survived      = if_else(Survived == 1, "Survived", "Not survived"),
    titanic_pred  = if_else(.pred_1 > cutoff_prob, "Predicted to survive", "predicted not to survive")
    ) %>%
  count(titanic_pred, Survived) %>%
  pivot_wider(names_from = Survived, values_from = n) %>%
  kable(col.names = c("", "Not Survived", "Survived"))
```

|                          | Not Survived | Survived |
|:-------------------------|-------------:|---------:|
| predicted not to survive |           89 |       20 |
| Predicted to survive     |            5 |       29 |

r squared and rmse

``` r
rsq(titanic_pred, truth = as.numeric(Survived), estimate = .pred_1)
```

    ## # A tibble: 1 × 3
    ##   .metric .estimator .estimate
    ##   <chr>   <chr>          <dbl>
    ## 1 rsq     standard       0.333

``` r
rmse(titanic_pred, truth = as.numeric(Survived), estimate = .pred_1)
```

    ## # A tibble: 1 × 3
    ##   .metric .estimator .estimate
    ##   <chr>   <chr>          <dbl>
    ## 1 rmse    standard        1.03
