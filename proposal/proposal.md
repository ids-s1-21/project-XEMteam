Project proposal
================
XEM Team

``` r
library(tidyverse)
library(broom)
library(here)
```

## 1. Introduction

Touted as the ultimate in transatlantic travel and said to be
“unsinkable”, the Titanic collided with an iceberg on 14 April 1912 on
her maiden voyage and sank shortly thereafter on 15 April, killing 1502
out of 2224 passengers and crew.
<https://rss.onlinelibrary.wiley.com/doi/full/10.1111/j.1740-9713.2019.01229.x>
We want to see **if and how the chance of survival of Titanic passengers
is related to different attributes of the passengers including sex, age,
socio-economic status etc.** – this is the general research question of
our project.

The data set we have used have comes from the Awesome Public Data Sets
on GitHub (<https://github.com/awesomedata/awesome-public-datasets>) and
there is no information on where the data originated or how it was
collected. However, we have found an article on a similar data set so we
have reason to believe that *“the primary sources of data on the Titanic
derive from official inquiries launched in Britain and the USA. Shortly
after the disaster, the British Parliament authorised the British Board
of Trade Inquiry with Lord Mersey as chair. The committee interviewed
over 100 witnesses over 36 days of hearings. Their report, issued on 30
July 1912, contained extensive tables of passengers and crew, broken
down by age group, gender, class and survival”*
<https://rss.onlinelibrary.wiley.com/doi/full/10.1111/j.1740-9713.2019.01229.x>

The titanic.csv file contains data for 887 of the real Titanic
passengers (2208 total). Each row represents on passenger and there are
12 different columns which describe different attributes about the
person including whether they survived, their age, their
passenger-class, their sex and the fare they paid.
<https://web.stanford.edu/class/archive/cs/cs109/cs109.1166/problem12.html>

## 2. Data

``` r
titanic <- read.csv(here::here("data/titanic.csv"))

glimpse(titanic)
```

    ## Rows: 891
    ## Columns: 12
    ## $ PassengerId <int> 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17,…
    ## $ Survived    <int> 0, 1, 1, 1, 0, 0, 0, 0, 1, 1, 1, 1, 0, 0, 0, 1, 0, 1, 0, 1…
    ## $ Pclass      <int> 3, 1, 3, 1, 3, 3, 1, 3, 3, 2, 3, 1, 3, 3, 3, 2, 3, 2, 3, 3…
    ## $ Name        <chr> "Braund, Mr. Owen Harris", "Cumings, Mrs. John Bradley (Fl…
    ## $ Sex         <chr> "male", "female", "female", "female", "male", "male", "mal…
    ## $ Age         <dbl> 22, 38, 26, 35, 35, NA, 54, 2, 27, 14, 4, 58, 20, 39, 14, …
    ## $ SibSp       <int> 1, 1, 0, 1, 0, 0, 0, 3, 0, 1, 1, 0, 0, 1, 0, 0, 4, 0, 1, 0…
    ## $ Parch       <int> 0, 0, 0, 0, 0, 0, 0, 1, 2, 0, 1, 0, 0, 5, 0, 0, 1, 0, 0, 0…
    ## $ Ticket      <chr> "A/5 21171", "PC 17599", "STON/O2. 3101282", "113803", "37…
    ## $ Fare        <dbl> 7.2500, 71.2833, 7.9250, 53.1000, 8.0500, 8.4583, 51.8625,…
    ## $ Cabin       <chr> "", "C85", "", "C123", "", "", "E46", "", "", "", "G6", "C…
    ## $ Embarked    <chr> "S", "C", "S", "S", "S", "Q", "S", "S", "S", "C", "S", "S"…

Note that we have added the dimensions and codebook for the dataset is
in the `README` in the `\data` folder.

The 12 variables in the data set are:

-   `PassangerId`: ID of passanger (from 1 to 891)
-   `Survived`: If passenger survived (0 = No, 1 = Yes)
-   `Pclass`: Passenger class (1 = 1st, 2 = 2nd, 3 = 3rd)
    -   **note**: this is a proxy for socio-economic status
-   `Name`: Name and Surname of passanger, if available
-   `Sex`: Gender of passanger (male or female)
    -   **note**: this is historical data and the gender of passengers
        is defined as binary
-   `Age`: Age in years (fractional if less than 1, if it’s estimated is
    it in the form of xx.5)
-   `SibSp`: Number of siblings/spouses aboard the Titanic
    -   Sibling = brother, sister, stepbrother, stepsister
    -   Spouse = husband, wife (mistresses and fiancés were ignored)
-   `Parch`: Number of parents/children aboard the Titanic
    -   **note**: Parent = mother, father
    -   **note**: Child = daughter, son, stepdaughter, stepson (some
        children travelled only with a nanny, therefore parch=0 for
        them)
-   `Ticket`: Ticket number
-   `Fare`: Passenger fare (i.e. cost of ticket in USD)
-   `Cabin`: Cabin number
-   `Embarked`: Port of embarkation (C = Cherbourg, Q = Queenstown, S =
    Southampton)

<https://www.kaggle.com/c/titanic/data>

## 3. Data analysis plan

#### Hypothesis 1: Women have a higher survival rate than men.

For question 1 we will create a new variable called
“survival\_rate\_gender”, using the “survived” column divided by the
total number of different gender, as the response variable. We will also
use “gender” as predictor variable.

#### Hypothesis 2: The younger survivals are more than the older survivals .

The age of a passenger can be considered to be high priority when it
comes to using life saving equipment from both their parents as well as
the crew of Titanic. We will have to create a new variable called
“age\_range” which groups the ages in to different ranges such 0-15 ,
16-30 etc. Since many passengers have unknown ages we will have to
ignore them for the purpose of this hypothesis. In order to display the
survival rate we will have to create a new variable called
“survival\_rate\_age\_range” using the “survived” column divided by the
frequency of each age range. Then a possible way to visualize the data
it is to use a bar plot using the “age\_range” as the predictor
variable(X) and “survival\_rate\_age\_range” as the outcome variable(Y).
This type of graph will point the

#### Hypothesis 3: The higher the class of the passenger, the higher survival rate.

The class of the passenger can be considered to be a measure of their
socio-economic status: based on our knowledge on the Titanic disaster we
expect to find that the higher the class of the passenger, the higher
their survival rate. Moreover, we expect the ticket price (`fare`
variable) to follow a similar relationship as it is very likely that the
ticket price and the class will have a linear relationship. See the
summary statistics below: filtering by the passengers who didn’t survive
(`filter(Survived == "0")`), we can see that the proportion of
passengers who didn’t survive is as high as 0.68 in 3rd class passengers
and much lower for first and second class (approximately 0.15 and 0.18
respectively).

``` r
titanic %>%
  filter(Survived == "0") %>%
  count(Pclass) %>%
  mutate(prop_death = n / sum(n)) 
```

    ##   Pclass   n prop_death
    ## 1      1  80  0.1457195
    ## 2      2  97  0.1766849
    ## 3      3 372  0.6775956

#### Hypothesis 4: If a passenger had a cabin, the higher the possibility to survive.

#### Visualising the data together – Recreating a G.Bron’s historic chart

Within our project, we would also like to recreate our version of
G.Bron’s chart of “The Loss of the ‘Titanic’”, from The Sphere, 4 May
1912 – first know data visualization on the topic. The plot clearly
shows the affect of all the different variables we have mentioned before
on the survival rate in a single data visualization. We could explore
unseen plot types and see which one can recreate similar results, as
well as a different visualization we believe better conveys the
information. One possible option is a mosaic plot, but we will need to
do more research to reach any conclusions.
