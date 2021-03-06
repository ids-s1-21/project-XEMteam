Survival on the Titanic
================
by XEM Team

## Summary

Titanic, said to be “unsinkable”, sank in 1912, killing 1502 out of 2224
passengers. One of the first big accidents with data about it which was
also been used to improve maritime safety by passing new policies,
better procedures and construction to avoid similar catastrophes.

We analysed original data from the accident and explored how different
attributes of passengers are related to their survival. It must be noted
that we have data on 891 passengers out of 2224 people on board and we
don’t know how representative our sample is out of the whole data. These
attributes include gender, age and wealth. We used logistic regression
model to try and predict survival based on these attributes. Lastly, we
created a mosaic plot on the basis of a historic plot, short time
following the accident, to display the relationship between different
variables and survival.

#### Age and survival

From the dataset, age is given in years. We expect to see a relationship
between age and survival based on different age groups rather than the
numerical age itself, so we decided to convert ages into categorical
groups ( children, teenagers, young , middle aged and elderly adults).
This allowed us to create a barplot to show how the age of passengers
and their survival are related. We found that children have higher
survival rate than the rest age groups and survival rate generally
decreases for higher age groups. This can be explained by the fact that
it’s in human nature to give priority to saving infants and children,
this was indeed reported as order from the captain.

#### Gender and survival

During these times, in the spirit of chivalry, women were saved first.
This was historically recorded as orders from the captain of the
Titanic. Based on this, we wanted to verify if gender affected the
survival rate. Since this is historical data, gender is only recorded as
male and female, thus we used binary data for our research. A barplot
was created, using gender as the predictor variable and percentage of
survival displayed in terms of colours showing survivals or not as the
outcome variable. The graph showed that the percentage of survival of
female and male was 74.2% and 18.9%, respectively. We concluded that
female had higher survival rate than male.

#### Wealth and survival

Ticket Price and Passengers’ class are major indicators of
socio-economic status of passengers. By displaying ticket price
distribution by class, using boxplots, shows that 1st Class has the
highest median ticket price for both survivals and non. Following that,
by finding the percentage of survival by class, using barplot, it is
noted that the percentage of survival for 1st class is more than double
than the percentage of the 3rd. Port of Embarkation has been also
considered as an indication of passenger’s wealth. By finding survival
rate by class along with median ticket price, per port, it is shown that
there is an association between port of embarkation, ticket price,
passengers’ class and survival rate.

#### Model

We fitted a linear regression model to our data, trying to use the
different variables available to us from the dataset and compared the
predicted properties. We focused on making our model parsimonious and
saw that the explanatory variables, which allow for the model with
better predicted performance are indeed gender, age and passenger class.
We then used and introduced different statistical tests (not in the
syllabus), as well as a ROC course to display and evaluate the model’s
strength on the test data. The model predicts that young, 1st class,
female passengers are more likely predicted to survive compared to the
counterparts of each of the attributes. It’s worthwhile noting that the
purpose of a predictive model when it refers to historic data like this
is rather than predict per se, provide an overall better understanding
of the data and what it refers to.

#### Recration of historic plot

<img src="https://www.researchgate.net/profile/Michael-Friendly/publication/330916468/figure/fig1/AS:723679168196613@1549549967751/GBrons-chart-of-The-Loss-of-the-Titanic-from-The-Sphere-4-May-1912-Each-subgroup.png" width="30%" style="float:right; padding:10px" />
One of the first visualizations that had ever being created with data
from the Titanic is a graph by graphic artist G. Bron, published on
Sphere (British newspaper) one week after the accident. His work is an
early innovation in data display where each subgroup shown by a bar with
area proportional to the numbers of cases which today can be seen as an
early mosaic plot. We decided to work on our own mosaic plot to display
how all different factors analysed are related to survival in a single
visualization.

<img src="README_files/figure-gfm/mosaic-plot-readme-1.png" width="60%" style="display: block; margin: auto;" />

## Presentation

Our presentation video can be found
[here](https://media.ed.ac.uk/media/1_7bgp9ovd).

## Data

CKAN training (2014). Titanic \[Dataset\], viewed 22 October 2021,
<https://data.wu.ac.at/schema/datahub_io/MWViYjhmYTctMzE2NS00OWEwLThmZDgtMTUwZjI4MThiYTJl>.

## References

\[1\] Source: Friendly, M., Symanzik, J., & Onder, O. (2019, February
6). Royal Statistical Society Publications. Royal Statistical Society.
Retrieved November 19, 2021, from
<https://rss.onlinelibrary.wiley.com/doi/full/10.1111/j.1740-9713.2019.01229.x>.

<https://rkabacoff.github.io/datavis/Models.html#Mosaic>

<https://www.houstoninjurylawyer.com/titanic-changed-maritime-law/>

<https://www.datavis.ca/papers/titanic/>

\[2\] <https://github.com/awesomedata/awesome-public-datasets>

\[3\] <https://www.kaggle.com/c/titanic/data>
