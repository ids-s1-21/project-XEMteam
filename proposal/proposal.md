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

<<<<<<< HEAD
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
=======
<<<<<<< HEAD
## 3. Data analysis plan

Hypothesis 1: Wemen have a higher survival rate than men. For question 1
we will create a new variable called “survival rate”, using the
“survived” column divided by the total number of different gender, as
the response variable. We will also use “gender” as predictor variable.

Hypothesis 2: The relationship between survival rate and age.

=======
The titanic.csv file contains data for 891 of the real Titanic
passengers. Each row represents one person. The are 12 different columns
which describe different attributes about the person including whether
they survived, their age, their passenger-class, their sex and the fare
they paid.
<https://web.stanford.edu/class/archive/cs/cs109/cs109.1166/problem12.html>
>>>>>>> 0f37d7e931fad3b324746a9fa970888a4656559a

<https://www.kaggle.com/c/titanic/data>

## 3. Data analysis plan

>>>>>>> fceee4c3e8a0116afa5c2c44d02eb89b838dcec8
## References
