Project proposal
================
XEM Team

``` r
library(tidyverse)
library(broom)
library(here)
library(dplyr)
library(ggplot2)
library(ggridges)
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

We assume that men were helping women while the tragic of Titanic was
taking place, since this is a habitual behavior of men to give priority
to women. In order to confirm if this is true we will create a bar plot
using gender as the predictor variable and frequency displayed in terms
of colors showing survivals or not. Moreover, we will calculate the
survival rate by gender. Since the variables that we will be using are
categorical , an appropriate statistical method to verify our hypothesis
is the Chi-Square test as it will show whether gender and survival are
independent or not of one another.

``` r
titanic %>%
  filter ( Survived == "1") %>%
  count(Sex) %>%
  mutate(n/sum(n))
```

    ##      Sex   n  n/sum(n)
    ## 1 female 233 0.6812865
    ## 2   male 109 0.3187135

#### Hypothesis 2: The younger survivals are more than the older survivals .

The age of a passenger can be considered to be high priority. We take it
for granted that people give priority to saving infants and young
children by giving priority when it comes to providing life saving
equipment.Beyond that the survival of passengers is highly related to
physical abilities which means that the younger the passenger is the
higher possibility to survive. We will have to create a new variable
called “Age\_Range” which groups the ages by scale of 15, that is 0-15,
16-30 etc. Then a possible way to visualize the data it is to use a
histogram using “Age Range” as the predictor variable(X) and “Frequency”
as the outcome variable(Y) displayed in terms of colors showing
survivals or not. This type of graph will point out the the modal class
of Age Ranges. Moreover we will calculate the survival rate by age range
which will allow us to find out the ages with the highest possibility to
survive. A statistical method that is more useful to answer our
hypothesis is to create a box plot for all ages so that we can conclude
which ages have highest possibility to survive as it displays the median
as well as the standard deviation and excludes outliers, extreme values.

``` r
titanic %>%
  filter(Survived == "1") %>%
  mutate(Age_Range = case_when(Age >= 0  & Age <= 15 ~ "0-15",
                                             Age >= 16  & Age <= 30 ~ "16-30",
                                             Age >= 31  & Age <= 45 ~ "31-45",
                               Age >= 46 & Age<= 60 ~ "46-60",
                               Age >= 61 & Age<=85 ~ "61-85" ) ) %>%
  ggplot(mapping=aes())
```

![](proposal_files/figure-gfm/age-survival-1.png)<!-- -->

#### Hypothesis 3: The higher the class of the passenger, the higher survival rate.

The class of the passenger can be considered to be a measure of their
socio-economic status: based on our knowledge on the Titanic disaster we
expect to find that the higher the class of the passenger, the higher
their survival rate. Moreover, we expect the ticket price (`fare`
variable) to follow a similar relationship as it is very likely that the
ticket price and the class will have a linear relationship.See the
summary statistics below: filtering by the passengers who didn’t survive
(`filter(Survived == "0")`), we can see that the proportion of
passengers who didn’t survive is as high as 0.68 in 3rd class passengers
and much lower for first and second class (approximately 0.15 and 0.18
respectively). Based on this , we can predict that the ticket price will
have a positive linear relationship with the frequency survivals. A
possible graph to display this relationship is the scatter plot having
the ticket price to be the predictor variable and the survivals to be
the outcome variable . A statistical method to use in order to support
our hypothesis is to display the regression line on the scatter plot and
find the regression constant in order to figure out if there is a strong
correlation between ticket price and survivals.

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

Cabins were on higher decks of Titanic which means that the passengers
living in cabins had easier and faster access to the life saving boats.
Based on this fact, we can conclude that passengers living in cabins had
higher possibility to survive. We can visualize the data into a
histogram as whether a passenger had or did not have a cabin as the
predictor variable and frequency displayed in terms of colors showing
survivals or not. Moreover, we will calculate the survival rate by
having cabin or not. A Chi-square test will be carried out to show
whether having a cabin or not affects the possibility of passenger to
survive.

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
