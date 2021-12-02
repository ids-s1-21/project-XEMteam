Survival on the Titanic
================
by XEM Team

## Summary

The Titanic, said to be “unsinkable”, sank on April 1912, killing 1502
out of 2224 passengers and crew. This was one of the first big accidents
with data about it which was also used to improve maritime safety by
passing new policies, better procedures and construction to avoid
another catastrophe.

We analysed original data from the accident and explored how different
attributes of passengers are related to their survival. It must be noted
that we have data on 891 passengers out of 2224 people on board and we
don’t know how representative our sample is of the whole data. These
attributes include gender, age and socio-economic status (represented by
passenger class). We then moved on to build a logistic regression model
to try and predict survival based on these attributes. Lastly, we
created a Mosaic plot on the basis of an historic plot from the times of
the accident to display the relationship between the different variables
and survival.

#### Age and survival

From the data set, age is given in years. We expect to see a
relationship between age and survival based on different age groups
rather than the numerical age itself, so we decided to convert
continuous ages into categorical groups (to represent children,
teenagers, young , middle aged and elderly adults). This allowed us to
create a bar plot to show how the age of passengers and their survival
are related. We have found that children indeed have higher survival
rates than the other age groups and survival rates generally decrease
with age. This can be explained by the fact that it’s human nature to
give priority to saving infants and children first, and was indeed
reported as orders from the captain.

#### Gender and survival

During these times, in the spirit of chivalry, women were saved first.
This was historically recorded as orders from the captain of the
Titanic. Basing on this, we want to verify if gender affected the
survival rate. Since this is a historical data, gender is only recorded
as male and female, thus we will use binary data for the following
research. First, a bar plot is created, using gender as the predictor
variable and percentage of survival displayed in terms of colors showing
survivals or not as the outcome variable. The graph shows that the
percentage of survival of female is 74.2% yet the percentage of survival
of male is only 18.9%. We concluded that female have a higher survival
rate than male.

#### Wealth and survival

write about 150 words

#### Model

We fitted a linear regression model to our data trying to use the
different variables available to us from the data set and compared the
predicted properties. We focused on making our model parsimonious and
saw that the explanatory variables which allow for the model with better
predicted performance are indeed gender, age and passenger class. We
then used and introduced different statistical tests (not in the
syllabus), as well as a ROC course to display and evaluate the model’s
strength on the test data. In short, the model predicts that young, 1st
class, female passengers are more likely predicted to survive compared
to the counterparts of each of the attributes. It’s worthwhile noting
that the purpose of a predictive model when it refers to historic data
like this is rather than predict per se, provide an overall better
understanding of the data and what it refers to.

#### Recration of historic plot

<img src="https://www.researchgate.net/profile/Michael-Friendly/publication/330916468/figure/fig1/AS:723679168196613@1549549967751/GBrons-chart-of-The-Loss-of-the-Titanic-from-The-Sphere-4-May-1912-Each-subgroup.png" width="30%" style="float:right; padding:10px" />
One of the first visualizations (see right) to ever be created with data
from the Titanic is a graph by graphic artist G. Bron published on the
Sphere (British newspaper) one week after the accident. His work is an
early innovation in data display where each subgroup shown by a bar with
area proportional to the numbers of cases which today can be seen as an
early mosaic plot. We indeed decided to work on our own mosaic plot to
display how all the different factors analysed are related to survival
in a single visualisation (see below).

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
