Survival on the Titanic
================
by XEM Team

## Summary

The Titanic, said to be “unsinkable”, collided with an iceberg and sank
on April 1942, killing 1502 out of 2224 passengers and crew. This was
one of the first accidents of this scale with important data about it
which was used to then improve maritime safety by passing new policies,
better safety procedures and construction to avoid another catastrophe.

We have decided to analyse original data from the accident and explore
how different attributes of passengers are related to their survival in
the catastrophe. These attributes include gender, age and socio-economic
status (represente by passenger class). We also looked at a range of
other variables including ticket price and port of embarkation. We then
moved on to build a logistic regression model to try and predict
survival based on these attributes. Lastly, we created a Mosaic plot on
the basis of an historic plot from the times of the accident to display
the relationship between the explanatory variables and survival.

#### Age and survival

#### Gender and survival

#### Wealth and survival

#### Logistic regression model

We focused on making our model parsimonious and saw that the explanatory
variables which allow for the model with better predicted performance
are indeed gender, age and passenger class.

#### Recration of historic plot

<img src="https://www.researchgate.net/profile/Michael-Friendly/publication/330916468/figure/fig1/AS:723679168196613@1549549967751/GBrons-chart-of-The-Loss-of-the-Titanic-from-The-Sphere-4-May-1912-Each-subgroup.png" width="30%" style="float:right; padding:10px" />
One of the first visualizations (see right) to ever be created with data
from the Titanic is a graph by graphic artist G. Bron published on the
Sphere (British newspaper) one week after the accident. His work is an
early innovation in data display where each subgroup shown by a bar with
area proportional to the numbers of cases which today can be seen as an
early mosaic plot. We indeed decided to work on our own mosaic plot to
display the different factors related to survival in a single plot.

<img src="README_files/figure-gfm/mosaic-plot-readme-1.png" width="60%" style="display: block; margin: auto;" />

## Presentation

Our presentation can be found [here](presentation/presentation.html).

## Data

CKAN training (2014). Titanic \[Dataset\], viewed 22 October 2021,
<https://data.wu.ac.at/schema/datahub_io/MWViYjhmYTctMzE2NS00OWEwLThmZDgtMTUwZjI4MThiYTJl>.

## References

\[1\] Source: Friendly, M., Symanzik, J., & Onder, O. (2019, February
6). Royal Statistical Society Publications. Royal Statistical Society.
Retrieved November 19, 2021, from
<https://rss.onlinelibrary.wiley.com/doi/full/10.1111/j.1740-9713.2019.01229.x>.

<https://rkabacoff.github.io/datavis/Models.html#Mosaic>

<https://www.datavis.ca/papers/titanic/>

<https://www.datavis.ca/papers/titanic/>

\[2\] <https://github.com/awesomedata/awesome-public-datasets>

\[3\] <https://www.kaggle.com/c/titanic/data>
