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
as well as the standard deviation and excludes outliers

``` r
titanic %>%
  filter(Survived == "1") %>%
  mutate(Age_Range = case_when(Age >= 0  & Age <= 15 ~ "0-15",
                                             Age >= 16  & Age <= 30 ~ "16-30",
                                             Age >= 31  & Age <= 45 ~ "31-45",
                               Age >= 46 & Age<= 60 ~ "46-60",
                               Age >= 61 & Age<=85 ~ "61-85" ) )
```

    ##     PassengerId Survived Pclass
    ## 1             2        1      1
    ## 2             3        1      3
    ## 3             4        1      1
    ## 4             9        1      3
    ## 5            10        1      2
    ## 6            11        1      3
    ## 7            12        1      1
    ## 8            16        1      2
    ## 9            18        1      2
    ## 10           20        1      3
    ## 11           22        1      2
    ## 12           23        1      3
    ## 13           24        1      1
    ## 14           26        1      3
    ## 15           29        1      3
    ## 16           32        1      1
    ## 17           33        1      3
    ## 18           37        1      3
    ## 19           40        1      3
    ## 20           44        1      2
    ## 21           45        1      3
    ## 22           48        1      3
    ## 23           53        1      1
    ## 24           54        1      2
    ## 25           56        1      1
    ## 26           57        1      2
    ## 27           59        1      2
    ## 28           62        1      1
    ## 29           66        1      3
    ## 30           67        1      2
    ## 31           69        1      3
    ## 32           75        1      3
    ## 33           79        1      2
    ## 34           80        1      3
    ## 35           82        1      3
    ## 36           83        1      3
    ## 37           85        1      2
    ## 38           86        1      3
    ## 39           89        1      1
    ## 40           98        1      1
    ## 41           99        1      2
    ## 42          107        1      3
    ## 43          108        1      3
    ## 44          110        1      3
    ## 45          124        1      2
    ## 46          126        1      3
    ## 47          128        1      3
    ## 48          129        1      3
    ## 49          134        1      2
    ## 50          137        1      1
    ## 51          142        1      3
    ## 52          143        1      3
    ## 53          147        1      3
    ## 54          152        1      1
    ## 55          157        1      3
    ## 56          162        1      2
    ## 57          166        1      3
    ## 58          167        1      1
    ## 59          173        1      3
    ## 60          184        1      2
    ## 61          185        1      3
    ## 62          187        1      3
    ## 63          188        1      1
    ## 64          191        1      2
    ## 65          193        1      3
    ## 66          194        1      2
    ## 67          195        1      1
    ## 68          196        1      1
    ## 69          199        1      3
    ## 70          205        1      3
    ## 71          208        1      3
    ## 72          209        1      3
    ## 73          210        1      1
    ## 74          212        1      2
    ## 75          216        1      1
    ## 76          217        1      3
    ## 77          219        1      1
    ## 78          221        1      3
    ## 79          225        1      1
    ## 80          227        1      2
    ## 81          231        1      1
    ## 82          234        1      3
    ## 83          238        1      2
    ## 84          242        1      3
    ## 85          248        1      2
    ## 86          249        1      1
    ## 87          256        1      3
    ## 88          257        1      1
    ## 89          258        1      1
    ## 90          259        1      1
    ## 91          260        1      2
    ## 92          262        1      3
    ## 93          268        1      3
    ## 94          269        1      1
    ## 95          270        1      1
    ## 96          272        1      3
    ## 97          273        1      2
    ## 98          275        1      3
    ## 99          276        1      1
    ## 100         280        1      3
    ## 101         284        1      3
    ## 102         287        1      3
    ## 103         289        1      2
    ## 104         290        1      3
    ## 105         291        1      1
    ## 106         292        1      1
    ## 107         299        1      1
    ## 108         300        1      1
    ## 109         301        1      3
    ## 110         302        1      3
    ## 111         304        1      2
    ## 112         306        1      1
    ## 113         307        1      1
    ## 114         308        1      1
    ## 115         310        1      1
    ## 116         311        1      1
    ## 117         312        1      1
    ## 118         316        1      3
    ## 119         317        1      2
    ## 120         319        1      1
    ## 121         320        1      1
    ## 122         323        1      2
    ## 123         324        1      2
    ## 124         326        1      1
    ## 125         328        1      2
    ## 126         329        1      3
    ## 127         330        1      1
    ## 128         331        1      3
    ## 129         335        1      1
    ## 130         338        1      1
    ## 131         339        1      3
    ## 132         341        1      2
    ## 133         342        1      1
    ## 134         346        1      2
    ## 135         347        1      2
    ## 136         348        1      3
    ## 137         349        1      3
    ## 138         357        1      1
    ## 139         359        1      3
    ## 140         360        1      3
    ## 141         367        1      1
    ## 142         368        1      3
    ## 143         369        1      3
    ## 144         370        1      1
    ## 145         371        1      1
    ## 146         376        1      1
    ## 147         377        1      3
    ## 148         381        1      1
    ## 149         382        1      3
    ## 150         384        1      1
    ## 151         388        1      2
    ## 152         390        1      2
    ## 153         391        1      1
    ## 154         392        1      3
    ## 155         394        1      1
    ## 156         395        1      3
    ## 157         400        1      2
    ## 158         401        1      3
    ## 159         408        1      2
    ## 160         413        1      1
    ## 161         415        1      3
    ## 162         417        1      2
    ## 163         418        1      2
    ## 164         427        1      2
    ## 165         428        1      2
    ## 166         430        1      3
    ## 167         431        1      1
    ## 168         432        1      3
    ## 169         433        1      2
    ## 170         436        1      1
    ## 171         438        1      2
    ## 172         441        1      2
    ## 173         444        1      2
    ## 174         445        1      3
    ## 175         446        1      1
    ## 176         447        1      2
    ## 177         448        1      1
    ## 178         449        1      3
    ## 179         450        1      1
    ## 180         454        1      1
    ## 181         456        1      3
    ## 182         458        1      1
    ## 183         459        1      2
    ## 184         461        1      1
    ## 185         470        1      3
    ## 186         473        1      2
    ## 187         474        1      2
    ## 188         480        1      3
    ## 189         484        1      3
    ## 190         485        1      1
    ## 191         487        1      1
    ## 192         490        1      3
    ## 193         497        1      1
    ## 194         505        1      1
    ## 195         507        1      2
    ## 196         508        1      1
    ## 197         510        1      3
    ## 198         511        1      3
    ## 199         513        1      1
    ## 200         514        1      1
    ## 201         517        1      2
    ## 202         519        1      2
    ## 203         521        1      1
    ## 204         524        1      1
    ## 205         527        1      2
    ## 206         531        1      2
    ## 207         534        1      3
    ## 208         536        1      2
    ## 209         538        1      1
    ## 210         540        1      1
    ## 211         541        1      1
    ## 212         544        1      2
    ## 213         547        1      2
    ## 214         548        1      2
    ## 215         550        1      2
    ## 216         551        1      1
    ## 217         554        1      3
    ## 218         555        1      3
    ## 219         557        1      1
    ## 220         559        1      1
    ## 221         560        1      3
    ## 222         570        1      3
    ## 223         571        1      2
    ## 224         572        1      1
    ## 225         573        1      1
    ## 226         574        1      3
    ## 227         577        1      2
    ## 228         578        1      1
    ## 229         580        1      3
    ## 230         581        1      2
    ## 231         582        1      1
    ## 232         586        1      1
    ## 233         588        1      1
    ## 234         592        1      1
    ## 235         597        1      2
    ## 236         600        1      1
    ## 237         601        1      2
    ## 238         605        1      1
    ## 239         608        1      1
    ## 240         609        1      2
    ## 241         610        1      1
    ## 242         613        1      3
    ## 243         616        1      2
    ## 244         619        1      2
    ## 245         622        1      1
    ## 246         623        1      3
    ## 247         628        1      1
    ## 248         631        1      1
    ## 249         633        1      1
    ## 250         636        1      2
    ## 251         642        1      1
    ## 252         644        1      3
    ## 253         645        1      3
    ## 254         646        1      1
    ## 255         648        1      1
    ## 256         650        1      3
    ## 257         652        1      2
    ## 258         654        1      3
    ## 259         661        1      1
    ## 260         665        1      3
    ## 261         670        1      1
    ## 262         671        1      2
    ## 263         674        1      2
    ## 264         678        1      3
    ## 265         680        1      1
    ## 266         682        1      1
    ## 267         690        1      1
    ## 268         691        1      1
    ## 269         692        1      3
    ## 270         693        1      3
    ## 271         698        1      3
    ## 272         701        1      1
    ## 273         702        1      1
    ## 274         707        1      2
    ## 275         708        1      1
    ## 276         709        1      1
    ## 277         710        1      3
    ## 278         711        1      1
    ## 279         713        1      1
    ## 280         717        1      1
    ## 281         718        1      2
    ## 282         721        1      2
    ## 283         725        1      1
    ## 284         727        1      2
    ## 285         728        1      3
    ## 286         731        1      1
    ## 287         738        1      1
    ## 288         741        1      1
    ## 289         743        1      1
    ## 290         745        1      3
    ## 291         748        1      2
    ## 292         751        1      2
    ## 293         752        1      3
    ## 294         755        1      2
    ## 295         756        1      2
    ## 296         760        1      1
    ## 297         763        1      3
    ## 298         764        1      1
    ## 299         766        1      1
    ## 300         775        1      2
    ## 301         778        1      3
    ## 302         780        1      1
    ## 303         781        1      3
    ## 304         782        1      1
    ## 305         787        1      3
    ## 306         789        1      3
    ## 307         797        1      1
    ## 308         798        1      3
    ## 309         802        1      2
    ## 310         803        1      1
    ## 311         804        1      3
    ## 312         805        1      3
    ## 313         810        1      1
    ## 314         821        1      1
    ## 315         822        1      3
    ## 316         824        1      3
    ## 317         828        1      2
    ## 318         829        1      3
    ## 319         830        1      1
    ## 320         831        1      3
    ## 321         832        1      2
    ## 322         836        1      1
    ## 323         839        1      3
    ## 324         840        1      1
    ## 325         843        1      1
    ## 326         850        1      1
    ## 327         854        1      1
    ## 328         856        1      3
    ## 329         857        1      1
    ## 330         858        1      1
    ## 331         859        1      3
    ## 332         863        1      1
    ## 333         866        1      2
    ## 334         867        1      2
    ## 335         870        1      3
    ## 336         872        1      1
    ## 337         875        1      2
    ## 338         876        1      3
    ## 339         880        1      1
    ## 340         881        1      2
    ## 341         888        1      1
    ## 342         890        1      1
    ##                                                                                   Name
    ## 1                                  Cumings, Mrs. John Bradley (Florence Briggs Thayer)
    ## 2                                                               Heikkinen, Miss. Laina
    ## 3                                         Futrelle, Mrs. Jacques Heath (Lily May Peel)
    ## 4                                    Johnson, Mrs. Oscar W (Elisabeth Vilhelmina Berg)
    ## 5                                                  Nasser, Mrs. Nicholas (Adele Achem)
    ## 6                                                      Sandstrom, Miss. Marguerite Rut
    ## 7                                                             Bonnell, Miss. Elizabeth
    ## 8                                                     Hewlett, Mrs. (Mary D Kingcome) 
    ## 9                                                         Williams, Mr. Charles Eugene
    ## 10                                                             Masselmani, Mrs. Fatima
    ## 11                                                               Beesley, Mr. Lawrence
    ## 12                                                         McGowan, Miss. Anna "Annie"
    ## 13                                                        Sloper, Mr. William Thompson
    ## 14                           Asplund, Mrs. Carl Oscar (Selma Augusta Emilia Johansson)
    ## 15                                                       O'Dwyer, Miss. Ellen "Nellie"
    ## 16                                      Spencer, Mrs. William Augustus (Marie Eugenie)
    ## 17                                                            Glynn, Miss. Mary Agatha
    ## 18                                                                    Mamee, Mr. Hanna
    ## 19                                                         Nicola-Yarred, Miss. Jamila
    ## 20                                            Laroche, Miss. Simonne Marie Anne Andree
    ## 21                                                       Devaney, Miss. Margaret Delia
    ## 22                                                           O'Driscoll, Miss. Bridget
    ## 23                                            Harper, Mrs. Henry Sleeper (Myna Haxtun)
    ## 24                                  Faunthorpe, Mrs. Lizzie (Elizabeth Anne Wilkinson)
    ## 25                                                                   Woolner, Mr. Hugh
    ## 26                                                                   Rugg, Miss. Emily
    ## 27                                                        West, Miss. Constance Mirium
    ## 28                                                                 Icard, Miss. Amelie
    ## 29                                                            Moubarek, Master. Gerios
    ## 30                                                        Nye, Mrs. (Elizabeth Ramell)
    ## 31                                                     Andersson, Miss. Erna Alexandra
    ## 32                                                                       Bing, Mr. Lee
    ## 33                                                       Caldwell, Master. Alden Gates
    ## 34                                                            Dowdell, Miss. Elizabeth
    ## 35                                                         Sheerlinck, Mr. Jan Baptist
    ## 36                                                      McDermott, Miss. Brigdet Delia
    ## 37                                                                 Ilett, Miss. Bertha
    ## 38                             Backstrom, Mrs. Karl Alfred (Maria Mathilda Gustafsson)
    ## 39                                                          Fortune, Miss. Mabel Helen
    ## 40                                                     Greenfield, Mr. William Bertram
    ## 41                                                Doling, Mrs. John T (Ada Julia Bone)
    ## 42                                                    Salkjelsvik, Miss. Anna Kristine
    ## 43                                                              Moss, Mr. Albert Johan
    ## 44                                                                 Moran, Miss. Bertha
    ## 45                                                                 Webber, Miss. Susan
    ## 46                                                        Nicola-Yarred, Master. Elias
    ## 47                                                           Madsen, Mr. Fridtjof Arne
    ## 48                                                                   Peter, Miss. Anna
    ## 49                                       Weisz, Mrs. Leopold (Mathilde Francoise Pede)
    ## 50                                                        Newsom, Miss. Helen Monypeny
    ## 51                                                            Nysten, Miss. Anna Sofia
    ## 52                                Hakkarainen, Mrs. Pekka Pietari (Elin Matilda Dolck)
    ## 53                                        Andersson, Mr. August Edvard ("Wennerstrom")
    ## 54                                                   Pears, Mrs. Thomas (Edith Wearne)
    ## 55                                                    Gilnagh, Miss. Katherine "Katie"
    ## 56                                  Watt, Mrs. James (Elizabeth "Bessie" Inglis Milne)
    ## 57                                     Goldsmith, Master. Frank John William "Frankie"
    ## 58                                              Chibnall, Mrs. (Edith Martha Bowerman)
    ## 59                                                        Johnson, Miss. Eleanor Ileen
    ## 60                                                           Becker, Master. Richard F
    ## 61                                                 Kink-Heilmann, Miss. Luise Gretchen
    ## 62                                     O'Brien, Mrs. Thomas (Johanna "Hannah" Godfrey)
    ## 63                                       Romaine, Mr. Charles Hallace ("Mr C Rolmane")
    ## 64                                                                 Pinsky, Mrs. (Rosa)
    ## 65                                     Andersen-Jensen, Miss. Carla Christine Nielsine
    ## 66                                                          Navratil, Master. Michel M
    ## 67                                           Brown, Mrs. James Joseph (Margaret Tobin)
    ## 68                                                                Lurette, Miss. Elise
    ## 69                                                    Madigan, Miss. Margaret "Maggie"
    ## 70                                                            Cohen, Mr. Gurshon "Gus"
    ## 71                                                         Albimona, Mr. Nassef Cassem
    ## 72                                                           Carr, Miss. Helen "Ellen"
    ## 73                                                                    Blank, Mr. Henry
    ## 74                                                          Cameron, Miss. Clear Annie
    ## 75                                                             Newell, Miss. Madeleine
    ## 76                                                              Honkanen, Miss. Eliina
    ## 77                                                               Bazzani, Miss. Albina
    ## 78                                                      Sunderland, Mr. Victor Francis
    ## 79                                                        Hoyt, Mr. Frederick Maxfield
    ## 80                                                           Mellors, Mr. William John
    ## 81                                        Harris, Mrs. Henry Birkhardt (Irene Wallach)
    ## 82                                                      Asplund, Miss. Lillian Gertrud
    ## 83                                                    Collyer, Miss. Marjorie "Lottie"
    ## 84                                                      Murphy, Miss. Katherine "Kate"
    ## 85                                                     Hamalainen, Mrs. William (Anna)
    ## 86                                                       Beckwith, Mr. Richard Leonard
    ## 87                                             Touma, Mrs. Darwis (Hanne Youssef Razi)
    ## 88                                                      Thorne, Mrs. Gertrude Maybelle
    ## 89                                                                Cherry, Miss. Gladys
    ## 90                                                                    Ward, Miss. Anna
    ## 91                                                         Parrish, Mrs. (Lutie Davis)
    ## 92                                                   Asplund, Master. Edvin Rojj Felix
    ## 93                                                            Persson, Mr. Ernst Ulrik
    ## 94                                       Graham, Mrs. William Thompson (Edith Junkins)
    ## 95                                                              Bissette, Miss. Amelia
    ## 96                                                        Tornquist, Mr. William Henry
    ## 97                                           Mellinger, Mrs. (Elizabeth Anne Maidment)
    ## 98                                                          Healy, Miss. Hanora "Nora"
    ## 99                                                   Andrews, Miss. Kornelia Theodosia
    ## 100                                                   Abbott, Mrs. Stanton (Rosa Hunt)
    ## 101                                                         Dorking, Mr. Edward Arthur
    ## 102                                                            de Mulder, Mr. Theodore
    ## 103                                                               Hosono, Mr. Masabumi
    ## 104                                                               Connolly, Miss. Kate
    ## 105                                                       Barber, Miss. Ellen "Nellie"
    ## 106                                            Bishop, Mrs. Dickinson H (Helen Walton)
    ## 107                                                              Saalfeld, Mr. Adolphe
    ## 108                                    Baxter, Mrs. James (Helene DeLaudeniere Chaput)
    ## 109                                           Kelly, Miss. Anna Katherine "Annie Kate"
    ## 110                                                                 McCoy, Mr. Bernard
    ## 111                                                                Keane, Miss. Nora A
    ## 112                                                     Allison, Master. Hudson Trevor
    ## 113                                                            Fleming, Miss. Margaret
    ## 114 Penasco y Castellana, Mrs. Victor de Satode (Maria Josefa Perez de Soto y Vallejo)
    ## 115                                                     Francatelli, Miss. Laura Mabel
    ## 116                                                     Hays, Miss. Margaret Bechstein
    ## 117                                                         Ryerson, Miss. Emily Borie
    ## 118                                                    Nilsson, Miss. Helmina Josefina
    ## 119                                                Kantor, Mrs. Sinai (Miriam Sternin)
    ## 120                                                           Wick, Miss. Mary Natalie
    ## 121                           Spedden, Mrs. Frederic Oakley (Margaretta Corning Stone)
    ## 122                                                          Slayter, Miss. Hilda Mary
    ## 123                                Caldwell, Mrs. Albert Francis (Sylvia Mae Harbaugh)
    ## 124                                                           Young, Miss. Marie Grice
    ## 125                                                            Ball, Mrs. (Ada E Hall)
    ## 126                                     Goldsmith, Mrs. Frank John (Emily Alice Brown)
    ## 127                                                       Hippach, Miss. Jean Gertrude
    ## 128                                                                 McCoy, Miss. Agnes
    ## 129                                 Frauenthal, Mrs. Henry William (Clara Heinsheimer)
    ## 130                                                    Burns, Miss. Elizabeth Margaret
    ## 131                                                              Dahl, Mr. Karl Edwart
    ## 132                                                     Navratil, Master. Edmond Roger
    ## 133                                                     Fortune, Miss. Alice Elizabeth
    ## 134                                                      Brown, Miss. Amelia "Mildred"
    ## 135                                                          Smith, Miss. Marion Elsie
    ## 136                                          Davison, Mrs. Thomas Henry (Mary E Finck)
    ## 137                                             Coutts, Master. William Loch "William"
    ## 138                                                        Bowerman, Miss. Elsie Edith
    ## 139                                                               McGovern, Miss. Mary
    ## 140                                                  Mockler, Miss. Helen Mary "Ellie"
    ## 141                                   Warren, Mrs. Frank Manley (Anna Sophia Atkinson)
    ## 142                                                     Moussa, Mrs. (Mantoura Boulos)
    ## 143                                                                Jermyn, Miss. Annie
    ## 144                                                      Aubart, Mme. Leontine Pauline
    ## 145                                                        Harder, Mr. George Achilles
    ## 146                                              Meyer, Mrs. Edgar Joseph (Leila Saks)
    ## 147                                                    Landergren, Miss. Aurora Adelia
    ## 148                                                              Bidois, Miss. Rosalie
    ## 149                                                        Nakid, Miss. Maria ("Mary")
    ## 150                                Holverson, Mrs. Alexander Oskar (Mary Aline Towner)
    ## 151                                                                   Buss, Miss. Kate
    ## 152                                                              Lehmann, Miss. Bertha
    ## 153                                                         Carter, Mr. William Ernest
    ## 154                                                             Jansson, Mr. Carl Olof
    ## 155                                                             Newell, Miss. Marjorie
    ## 156                                Sandstrom, Mrs. Hjalmar (Agnes Charlotta Bengtsson)
    ## 157                                                   Trout, Mrs. William H (Jessie L)
    ## 158                                                                 Niskanen, Mr. Juha
    ## 159                                                     Richards, Master. William Rowe
    ## 160                                                             Minahan, Miss. Daisy E
    ## 161                                                          Sundman, Mr. Johan Julian
    ## 162                                    Drew, Mrs. James Vivian (Lulu Thorne Christian)
    ## 163                                                      Silven, Miss. Lyyli Karoliina
    ## 164                                        Clarke, Mrs. Charles V (Ada Maria Winfield)
    ## 165                Phillips, Miss. Kate Florence ("Mrs Kate Louise Phillips Marshall")
    ## 166                                                 Pickard, Mr. Berk (Berk Trembisky)
    ## 167                                          Bjornstrom-Steffansson, Mr. Mauritz Hakan
    ## 168                                  Thorneycroft, Mrs. Percival (Florence Kate White)
    ## 169                                Louch, Mrs. Charles Alexander (Alice Adelaide Slow)
    ## 170                                                          Carter, Miss. Lucile Polk
    ## 171                                              Richards, Mrs. Sidney (Emily Hocking)
    ## 172                                        Hart, Mrs. Benjamin (Esther Ada Bloomfield)
    ## 173                                                          Reynaldo, Ms. Encarnacion
    ## 174                                                  Johannesen-Bratthammer, Mr. Bernt
    ## 175                                                          Dodge, Master. Washington
    ## 176                                                  Mellinger, Miss. Madeleine Violet
    ## 177                                                        Seward, Mr. Frederic Kimber
    ## 178                                                     Baclini, Miss. Marie Catherine
    ## 179                                                     Peuchen, Major. Arthur Godfrey
    ## 180                                                           Goldenberg, Mr. Samuel L
    ## 181                                                                 Jalsevac, Mr. Ivan
    ## 182                                                  Kenyon, Mrs. Frederick R (Marion)
    ## 183                                                                Toomey, Miss. Ellen
    ## 184                                                                Anderson, Mr. Harry
    ## 185                                                      Baclini, Miss. Helene Barbara
    ## 186                                            West, Mrs. Edwy Arthur (Ada Mary Worth)
    ## 187                                       Jerwan, Mrs. Amin S (Marie Marthe Thuillard)
    ## 188                                                           Hirvonen, Miss. Hildur E
    ## 189                                                             Turkula, Mrs. (Hedwig)
    ## 190                                                            Bishop, Mr. Dickinson H
    ## 191                                    Hoyt, Mrs. Frederick Maxfield (Jane Anne Forby)
    ## 192                                              Coutts, Master. Eden Leslie "Neville"
    ## 193                                                     Eustis, Miss. Elizabeth Mussey
    ## 194                                                              Maioni, Miss. Roberta
    ## 195                                      Quick, Mrs. Frederick Charles (Jane Richards)
    ## 196                                      Bradley, Mr. George ("George Arthur Brayton")
    ## 197                                                                     Lang, Mr. Fang
    ## 198                                                           Daly, Mr. Eugene Patrick
    ## 199                                                          McGough, Mr. James Robert
    ## 200                                     Rothschild, Mrs. Martin (Elizabeth L. Barrett)
    ## 201                                                       Lemore, Mrs. (Amelia Milley)
    ## 202                               Angle, Mrs. William A (Florence "Mary" Agnes Hughes)
    ## 203                                                              Perreault, Miss. Anne
    ## 204                                    Hippach, Mrs. Louis Albert (Ida Sophia Fischer)
    ## 205                                                               Ridsdale, Miss. Lucy
    ## 206                                                           Quick, Miss. Phyllis May
    ## 207                                             Peter, Mrs. Catherine (Catherine Rizk)
    ## 208                                                             Hart, Miss. Eva Miriam
    ## 209                                                                LeRoy, Miss. Bertha
    ## 210                                                 Frolicher, Miss. Hedwig Margaritha
    ## 211                                                            Crosby, Miss. Harriet R
    ## 212                                                                  Beane, Mr. Edward
    ## 213                                                  Beane, Mrs. Edward (Ethel Clarke)
    ## 214                                                         Padro y Manent, Mr. Julian
    ## 215                                                     Davies, Master. John Morgan Jr
    ## 216                                                        Thayer, Mr. John Borland Jr
    ## 217                                                  Leeni, Mr. Fahim ("Philip Zenni")
    ## 218                                                                 Ohman, Miss. Velin
    ## 219                  Duff Gordon, Lady. (Lucille Christiana Sutherland) ("Mrs Morgan")
    ## 220                                             Taussig, Mrs. Emil (Tillie Mandelbaum)
    ## 221                                       de Messemaeker, Mrs. Guillaume Joseph (Emma)
    ## 222                                                                  Jonsson, Mr. Carl
    ## 223                                                                 Harris, Mr. George
    ## 224                                      Appleton, Mrs. Edward Dale (Charlotte Lamson)
    ## 225                                                   Flynn, Mr. John Irwin ("Irving")
    ## 226                                                                  Kelly, Miss. Mary
    ## 227                                                               Garside, Miss. Ethel
    ## 228                                          Silvey, Mrs. William Baird (Alice Munger)
    ## 229                                                                Jussila, Mr. Eiriik
    ## 230                                                        Christy, Miss. Julie Rachel
    ## 231                               Thayer, Mrs. John Borland (Marian Longstreth Morris)
    ## 232                                                                Taussig, Miss. Ruth
    ## 233                                                   Frolicher-Stehli, Mr. Maxmillian
    ## 234                                    Stephenson, Mrs. Walter Bertram (Martha Eustis)
    ## 235                                                         Leitch, Miss. Jessie Wills
    ## 236                                       Duff Gordon, Sir. Cosmo Edmund ("Mr Morgan")
    ## 237                                Jacobsohn, Mrs. Sidney Samuel (Amy Frances Christy)
    ## 238                                                    Homer, Mr. Harry ("Mr E Haven")
    ## 239                                                        Daniel, Mr. Robert Williams
    ## 240                              Laroche, Mrs. Joseph (Juliette Marie Louise Lafargue)
    ## 241                                                          Shutes, Miss. Elizabeth W
    ## 242                                                        Murphy, Miss. Margaret Jane
    ## 243                                                                Herman, Miss. Alice
    ## 244                                                        Becker, Miss. Marion Louise
    ## 245                                                       Kimball, Mr. Edwin Nelson Jr
    ## 246                                                                   Nakid, Mr. Sahid
    ## 247                                                      Longley, Miss. Gretchen Fiske
    ## 248                                               Barkworth, Mr. Algernon Henry Wilson
    ## 249                                                          Stahelin-Maeglin, Dr. Max
    ## 250                                                                  Davis, Miss. Mary
    ## 251                                                               Sagesser, Mlle. Emma
    ## 252                                                                    Foo, Mr. Choong
    ## 253                                                             Baclini, Miss. Eugenie
    ## 254                                                          Harper, Mr. Henry Sleeper
    ## 255                                                Simonius-Blumer, Col. Oberst Alfons
    ## 256                                                    Stanley, Miss. Amy Zillah Elsie
    ## 257                                                                Doling, Miss. Elsie
    ## 258                                                      O'Leary, Miss. Hanora "Norah"
    ## 259                                                      Frauenthal, Dr. Henry William
    ## 260                                                        Lindqvist, Mr. Eino William
    ## 261                                  Taylor, Mrs. Elmer Zebley (Juliet Cummins Wright)
    ## 262                      Brown, Mrs. Thomas William Solomon (Elizabeth Catherine Ford)
    ## 263                                                              Wilhelms, Mr. Charles
    ## 264                                                            Turja, Miss. Anna Sofia
    ## 265                                                 Cardeza, Mr. Thomas Drake Martinez
    ## 266                                                                 Hassab, Mr. Hammad
    ## 267                                                  Madill, Miss. Georgette Alexandra
    ## 268                                                            Dick, Mr. Albert Adrian
    ## 269                                                                 Karun, Miss. Manca
    ## 270                                                                       Lam, Mr. Ali
    ## 271                                                   Mullens, Miss. Katherine "Katie"
    ## 272                                  Astor, Mrs. John Jacob (Madeleine Talmadge Force)
    ## 273                                                   Silverthorne, Mr. Spencer Victor
    ## 274                                                      Kelly, Mrs. Florence "Fannie"
    ## 275                                                  Calderhead, Mr. Edward Pennington
    ## 276                                                               Cleaver, Miss. Alice
    ## 277                                  Moubarek, Master. Halim Gonios ("William George")
    ## 278                                   Mayne, Mlle. Berthe Antonine ("Mrs de Villiers")
    ## 279                                                           Taylor, Mr. Elmer Zebley
    ## 280                                                      Endres, Miss. Caroline Louise
    ## 281                                                Troutt, Miss. Edwina Celia "Winnie"
    ## 282                                                  Harper, Miss. Annie Jessie "Nina"
    ## 283                                                      Chambers, Mr. Norman Campbell
    ## 284                                        Renouf, Mrs. Peter Henry (Lillian Jefferys)
    ## 285                                                           Mannion, Miss. Margareth
    ## 286                                                      Allen, Miss. Elisabeth Walton
    ## 287                                                             Lesurer, Mr. Gustave J
    ## 288                                                        Hawksford, Mr. Walter James
    ## 289                                              Ryerson, Miss. Susan Parker "Suzette"
    ## 290                                                                 Stranden, Mr. Juho
    ## 291                                                              Sinkkonen, Miss. Anna
    ## 292                                                                  Wells, Miss. Joan
    ## 293                                                                Moor, Master. Meier
    ## 294                                                   Herman, Mrs. Samuel (Jane Laver)
    ## 295                                                          Hamalainen, Master. Viljo
    ## 296                           Rothes, the Countess. of (Lucy Noel Martha Dyer-Edwards)
    ## 297                                                              Barah, Mr. Hanna Assi
    ## 298                                          Carter, Mrs. William Ernest (Lucile Polk)
    ## 299                                               Hogeboom, Mrs. John C (Anna Andrews)
    ## 300                                              Hocking, Mrs. Elizabeth (Eliza Needs)
    ## 301                                                      Emanuel, Miss. Virginia Ethel
    ## 302                              Robert, Mrs. Edward Scott (Elisabeth Walton McMillan)
    ## 303                                                               Ayoub, Miss. Banoura
    ## 304                                          Dick, Mrs. Albert Adrian (Vera Gillespie)
    ## 305                                                          Sjoblom, Miss. Anna Sofia
    ## 306                                                         Dean, Master. Bertram Vere
    ## 307                                                        Leader, Dr. Alice (Farnham)
    ## 308                                                                   Osman, Mrs. Mara
    ## 309                                        Collyer, Mrs. Harvey (Charlotte Annie Tate)
    ## 310                                                Carter, Master. William Thornton II
    ## 311                                                    Thomas, Master. Assad Alexander
    ## 312                                                            Hedman, Mr. Oskar Arvid
    ## 313                                     Chambers, Mrs. Norman Campbell (Bertha Griggs)
    ## 314                                 Hays, Mrs. Charles Melville (Clara Jennings Gregg)
    ## 315                                                                  Lulic, Mr. Nikola
    ## 316                                                                 Moor, Mrs. (Beila)
    ## 317                                                              Mallet, Master. Andre
    ## 318                                                       McCormack, Mr. Thomas Joseph
    ## 319                                          Stone, Mrs. George Nelson (Martha Evelyn)
    ## 320                                            Yasbeck, Mrs. Antoni (Selini Alexander)
    ## 321                                                    Richards, Master. George Sibley
    ## 322                                                        Compton, Miss. Sara Rebecca
    ## 323                                                                    Chip, Mr. Chang
    ## 324                                                               Marechal, Mr. Pierre
    ## 325                                                            Serepeca, Miss. Augusta
    ## 326                                       Goldenberg, Mrs. Samuel L (Edwiga Grabowska)
    ## 327                                                          Lines, Miss. Mary Conover
    ## 328                                                         Aks, Mrs. Sam (Leah Rosen)
    ## 329                                         Wick, Mrs. George Dennick (Mary Hitchcock)
    ## 330                                                             Daly, Mr. Peter Denis 
    ## 331                                              Baclini, Mrs. Solomon (Latifa Qurban)
    ## 332                                Swift, Mrs. Frederick Joel (Margaret Welles Barron)
    ## 333                                                           Bystrom, Mrs. (Karolina)
    ## 334                                                       Duran y More, Miss. Asuncion
    ## 335                                                    Johnson, Master. Harold Theodor
    ## 336                                   Beckwith, Mrs. Richard Leonard (Sallie Monypeny)
    ## 337                                              Abelson, Mrs. Samuel (Hannah Wizosky)
    ## 338                                                   Najib, Miss. Adele Kiamie "Jane"
    ## 339                                      Potter, Mrs. Thomas Jr (Lily Alexenia Wilson)
    ## 340                                       Shelley, Mrs. William (Imanita Parrish Hall)
    ## 341                                                       Graham, Miss. Margaret Edith
    ## 342                                                              Behr, Mr. Karl Howell
    ##        Sex   Age SibSp Parch            Ticket     Fare           Cabin
    ## 1   female 38.00     1     0          PC 17599  71.2833             C85
    ## 2   female 26.00     0     0  STON/O2. 3101282   7.9250                
    ## 3   female 35.00     1     0            113803  53.1000            C123
    ## 4   female 27.00     0     2            347742  11.1333                
    ## 5   female 14.00     1     0            237736  30.0708                
    ## 6   female  4.00     1     1           PP 9549  16.7000              G6
    ## 7   female 58.00     0     0            113783  26.5500            C103
    ## 8   female 55.00     0     0            248706  16.0000                
    ## 9     male    NA     0     0            244373  13.0000                
    ## 10  female    NA     0     0              2649   7.2250                
    ## 11    male 34.00     0     0            248698  13.0000             D56
    ## 12  female 15.00     0     0            330923   8.0292                
    ## 13    male 28.00     0     0            113788  35.5000              A6
    ## 14  female 38.00     1     5            347077  31.3875                
    ## 15  female    NA     0     0            330959   7.8792                
    ## 16  female    NA     1     0          PC 17569 146.5208             B78
    ## 17  female    NA     0     0            335677   7.7500                
    ## 18    male    NA     0     0              2677   7.2292                
    ## 19  female 14.00     1     0              2651  11.2417                
    ## 20  female  3.00     1     2     SC/Paris 2123  41.5792                
    ## 21  female 19.00     0     0            330958   7.8792                
    ## 22  female    NA     0     0             14311   7.7500                
    ## 23  female 49.00     1     0          PC 17572  76.7292             D33
    ## 24  female 29.00     1     0              2926  26.0000                
    ## 25    male    NA     0     0             19947  35.5000             C52
    ## 26  female 21.00     0     0        C.A. 31026  10.5000                
    ## 27  female  5.00     1     2        C.A. 34651  27.7500                
    ## 28  female 38.00     0     0            113572  80.0000             B28
    ## 29    male    NA     1     1              2661  15.2458                
    ## 30  female 29.00     0     0        C.A. 29395  10.5000             F33
    ## 31  female 17.00     4     2           3101281   7.9250                
    ## 32    male 32.00     0     0              1601  56.4958                
    ## 33    male  0.83     0     2            248738  29.0000                
    ## 34  female 30.00     0     0            364516  12.4750                
    ## 35    male 29.00     0     0            345779   9.5000                
    ## 36  female    NA     0     0            330932   7.7875                
    ## 37  female 17.00     0     0        SO/C 14885  10.5000                
    ## 38  female 33.00     3     0           3101278  15.8500                
    ## 39  female 23.00     3     2             19950 263.0000     C23 C25 C27
    ## 40    male 23.00     0     1          PC 17759  63.3583         D10 D12
    ## 41  female 34.00     0     1            231919  23.0000                
    ## 42  female 21.00     0     0            343120   7.6500                
    ## 43    male    NA     0     0            312991   7.7750                
    ## 44  female    NA     1     0            371110  24.1500                
    ## 45  female 32.50     0     0             27267  13.0000            E101
    ## 46    male 12.00     1     0              2651  11.2417                
    ## 47    male 24.00     0     0           C 17369   7.1417                
    ## 48  female    NA     1     1              2668  22.3583           F E69
    ## 49  female 29.00     1     0            228414  26.0000                
    ## 50  female 19.00     0     2             11752  26.2833             D47
    ## 51  female 22.00     0     0            347081   7.7500                
    ## 52  female 24.00     1     0  STON/O2. 3101279  15.8500                
    ## 53    male 27.00     0     0            350043   7.7958                
    ## 54  female 22.00     1     0            113776  66.6000              C2
    ## 55  female 16.00     0     0             35851   7.7333                
    ## 56  female 40.00     0     0        C.A. 33595  15.7500                
    ## 57    male  9.00     0     2            363291  20.5250                
    ## 58  female    NA     0     1            113505  55.0000             E33
    ## 59  female  1.00     1     1            347742  11.1333                
    ## 60    male  1.00     2     1            230136  39.0000              F4
    ## 61  female  4.00     0     2            315153  22.0250                
    ## 62  female    NA     1     0            370365  15.5000                
    ## 63    male 45.00     0     0            111428  26.5500                
    ## 64  female 32.00     0     0            234604  13.0000                
    ## 65  female 19.00     1     0            350046   7.8542                
    ## 66    male  3.00     1     1            230080  26.0000              F2
    ## 67  female 44.00     0     0          PC 17610  27.7208              B4
    ## 68  female 58.00     0     0          PC 17569 146.5208             B80
    ## 69  female    NA     0     0            370370   7.7500                
    ## 70    male 18.00     0     0          A/5 3540   8.0500                
    ## 71    male 26.00     0     0              2699  18.7875                
    ## 72  female 16.00     0     0            367231   7.7500                
    ## 73    male 40.00     0     0            112277  31.0000             A31
    ## 74  female 35.00     0     0      F.C.C. 13528  21.0000                
    ## 75  female 31.00     1     0             35273 113.2750             D36
    ## 76  female 27.00     0     0  STON/O2. 3101283   7.9250                
    ## 77  female 32.00     0     0             11813  76.2917             D15
    ## 78    male 16.00     0     0   SOTON/OQ 392089   8.0500                
    ## 79    male 38.00     1     0             19943  90.0000             C93
    ## 80    male 19.00     0     0         SW/PP 751  10.5000                
    ## 81  female 35.00     1     0             36973  83.4750             C83
    ## 82  female  5.00     4     2            347077  31.3875                
    ## 83  female  8.00     0     2        C.A. 31921  26.2500                
    ## 84  female    NA     1     0            367230  15.5000                
    ## 85  female 24.00     0     2            250649  14.5000                
    ## 86    male 37.00     1     1             11751  52.5542             D35
    ## 87  female 29.00     0     2              2650  15.2458                
    ## 88  female    NA     0     0          PC 17585  79.2000                
    ## 89  female 30.00     0     0            110152  86.5000             B77
    ## 90  female 35.00     0     0          PC 17755 512.3292                
    ## 91  female 50.00     0     1            230433  26.0000                
    ## 92    male  3.00     4     2            347077  31.3875                
    ## 93    male 25.00     1     0            347083   7.7750                
    ## 94  female 58.00     0     1          PC 17582 153.4625            C125
    ## 95  female 35.00     0     0          PC 17760 135.6333             C99
    ## 96    male 25.00     0     0              LINE   0.0000                
    ## 97  female 41.00     0     1            250644  19.5000                
    ## 98  female    NA     0     0            370375   7.7500                
    ## 99  female 63.00     1     0             13502  77.9583              D7
    ## 100 female 35.00     1     1         C.A. 2673  20.2500                
    ## 101   male 19.00     0     0        A/5. 10482   8.0500                
    ## 102   male 30.00     0     0            345774   9.5000                
    ## 103   male 42.00     0     0            237798  13.0000                
    ## 104 female 22.00     0     0            370373   7.7500                
    ## 105 female 26.00     0     0             19877  78.8500                
    ## 106 female 19.00     1     0             11967  91.0792             B49
    ## 107   male    NA     0     0             19988  30.5000            C106
    ## 108 female 50.00     0     1          PC 17558 247.5208         B58 B60
    ## 109 female    NA     0     0              9234   7.7500                
    ## 110   male    NA     2     0            367226  23.2500                
    ## 111 female    NA     0     0            226593  12.3500            E101
    ## 112   male  0.92     1     2            113781 151.5500         C22 C26
    ## 113 female    NA     0     0             17421 110.8833                
    ## 114 female 17.00     1     0          PC 17758 108.9000             C65
    ## 115 female 30.00     0     0          PC 17485  56.9292             E36
    ## 116 female 24.00     0     0             11767  83.1583             C54
    ## 117 female 18.00     2     2          PC 17608 262.3750 B57 B59 B63 B66
    ## 118 female 26.00     0     0            347470   7.8542                
    ## 119 female 24.00     1     0            244367  26.0000                
    ## 120 female 31.00     0     2             36928 164.8667              C7
    ## 121 female 40.00     1     1             16966 134.5000             E34
    ## 122 female 30.00     0     0            234818  12.3500                
    ## 123 female 22.00     1     1            248738  29.0000                
    ## 124 female 36.00     0     0          PC 17760 135.6333             C32
    ## 125 female 36.00     0     0             28551  13.0000               D
    ## 126 female 31.00     1     1            363291  20.5250                
    ## 127 female 16.00     0     1            111361  57.9792             B18
    ## 128 female    NA     2     0            367226  23.2500                
    ## 129 female    NA     1     0          PC 17611 133.6500                
    ## 130 female 41.00     0     0             16966 134.5000             E40
    ## 131   male 45.00     0     0              7598   8.0500                
    ## 132   male  2.00     1     1            230080  26.0000              F2
    ## 133 female 24.00     3     2             19950 263.0000     C23 C25 C27
    ## 134 female 24.00     0     0            248733  13.0000             F33
    ## 135 female 40.00     0     0             31418  13.0000                
    ## 136 female    NA     1     0            386525  16.1000                
    ## 137   male  3.00     1     1        C.A. 37671  15.9000                
    ## 138 female 22.00     0     1            113505  55.0000             E33
    ## 139 female    NA     0     0            330931   7.8792                
    ## 140 female    NA     0     0            330980   7.8792                
    ## 141 female 60.00     1     0            110813  75.2500             D37
    ## 142 female    NA     0     0              2626   7.2292                
    ## 143 female    NA     0     0             14313   7.7500                
    ## 144 female 24.00     0     0          PC 17477  69.3000             B35
    ## 145   male 25.00     1     0             11765  55.4417             E50
    ## 146 female    NA     1     0          PC 17604  82.1708                
    ## 147 female 22.00     0     0            C 7077   7.2500                
    ## 148 female 42.00     0     0          PC 17757 227.5250                
    ## 149 female  1.00     0     2              2653  15.7417                
    ## 150 female 35.00     1     0            113789  52.0000                
    ## 151 female 36.00     0     0             27849  13.0000                
    ## 152 female 17.00     0     0           SC 1748  12.0000                
    ## 153   male 36.00     1     2            113760 120.0000         B96 B98
    ## 154   male 21.00     0     0            350034   7.7958                
    ## 155 female 23.00     1     0             35273 113.2750             D36
    ## 156 female 24.00     0     2           PP 9549  16.7000              G6
    ## 157 female 28.00     0     0            240929  12.6500                
    ## 158   male 39.00     0     0 STON/O 2. 3101289   7.9250                
    ## 159   male  3.00     1     1             29106  18.7500                
    ## 160 female 33.00     1     0             19928  90.0000             C78
    ## 161   male 44.00     0     0 STON/O 2. 3101269   7.9250                
    ## 162 female 34.00     1     1             28220  32.5000                
    ## 163 female 18.00     0     2            250652  13.0000                
    ## 164 female 28.00     1     0              2003  26.0000                
    ## 165 female 19.00     0     0            250655  26.0000                
    ## 166   male 32.00     0     0 SOTON/O.Q. 392078   8.0500             E10
    ## 167   male 28.00     0     0            110564  26.5500             C52
    ## 168 female    NA     1     0            376564  16.1000                
    ## 169 female 42.00     1     0        SC/AH 3085  26.0000                
    ## 170 female 14.00     1     2            113760 120.0000         B96 B98
    ## 171 female 24.00     2     3             29106  18.7500                
    ## 172 female 45.00     1     1      F.C.C. 13529  26.2500                
    ## 173 female 28.00     0     0            230434  13.0000                
    ## 174   male    NA     0     0             65306   8.1125                
    ## 175   male  4.00     0     2             33638  81.8583             A34
    ## 176 female 13.00     0     1            250644  19.5000                
    ## 177   male 34.00     0     0            113794  26.5500                
    ## 178 female  5.00     2     1              2666  19.2583                
    ## 179   male 52.00     0     0            113786  30.5000            C104
    ## 180   male 49.00     1     0             17453  89.1042             C92
    ## 181   male 29.00     0     0            349240   7.8958                
    ## 182 female    NA     1     0             17464  51.8625             D21
    ## 183 female 50.00     0     0      F.C.C. 13531  10.5000                
    ## 184   male 48.00     0     0             19952  26.5500             E12
    ## 185 female  0.75     2     1              2666  19.2583                
    ## 186 female 33.00     1     2        C.A. 34651  27.7500                
    ## 187 female 23.00     0     0   SC/AH Basle 541  13.7917               D
    ## 188 female  2.00     0     1           3101298  12.2875                
    ## 189 female 63.00     0     0              4134   9.5875                
    ## 190   male 25.00     1     0             11967  91.0792             B49
    ## 191 female 35.00     1     0             19943  90.0000             C93
    ## 192   male  9.00     1     1        C.A. 37671  15.9000                
    ## 193 female 54.00     1     0             36947  78.2667             D20
    ## 194 female 16.00     0     0            110152  86.5000             B79
    ## 195 female 33.00     0     2             26360  26.0000                
    ## 196   male    NA     0     0            111427  26.5500                
    ## 197   male 26.00     0     0              1601  56.4958                
    ## 198   male 29.00     0     0            382651   7.7500                
    ## 199   male 36.00     0     0          PC 17473  26.2875             E25
    ## 200 female 54.00     1     0          PC 17603  59.4000                
    ## 201 female 34.00     0     0        C.A. 34260  10.5000             F33
    ## 202 female 36.00     1     0            226875  26.0000                
    ## 203 female 30.00     0     0             12749  93.5000             B73
    ## 204 female 44.00     0     1            111361  57.9792             B18
    ## 205 female 50.00     0     0       W./C. 14258  10.5000                
    ## 206 female  2.00     1     1             26360  26.0000                
    ## 207 female    NA     0     2              2668  22.3583                
    ## 208 female  7.00     0     2      F.C.C. 13529  26.2500                
    ## 209 female 30.00     0     0          PC 17761 106.4250                
    ## 210 female 22.00     0     2             13568  49.5000             B39
    ## 211 female 36.00     0     2         WE/P 5735  71.0000             B22
    ## 212   male 32.00     1     0              2908  26.0000                
    ## 213 female 19.00     1     0              2908  26.0000                
    ## 214   male    NA     0     0     SC/PARIS 2146  13.8625                
    ## 215   male  8.00     1     1        C.A. 33112  36.7500                
    ## 216   male 17.00     0     2             17421 110.8833             C70
    ## 217   male 22.00     0     0              2620   7.2250                
    ## 218 female 22.00     0     0            347085   7.7750                
    ## 219 female 48.00     1     0             11755  39.6000             A16
    ## 220 female 39.00     1     1            110413  79.6500             E67
    ## 221 female 36.00     1     0            345572  17.4000                
    ## 222   male 32.00     0     0            350417   7.8542                
    ## 223   male 62.00     0     0       S.W./PP 752  10.5000                
    ## 224 female 53.00     2     0             11769  51.4792            C101
    ## 225   male 36.00     0     0          PC 17474  26.3875             E25
    ## 226 female    NA     0     0             14312   7.7500                
    ## 227 female 34.00     0     0            243880  13.0000                
    ## 228 female 39.00     1     0             13507  55.9000             E44
    ## 229   male 32.00     0     0 STON/O 2. 3101286   7.9250                
    ## 230 female 25.00     1     1            237789  30.0000                
    ## 231 female 39.00     1     1             17421 110.8833             C68
    ## 232 female 18.00     0     2            110413  79.6500             E68
    ## 233   male 60.00     1     1             13567  79.2000             B41
    ## 234 female 52.00     1     0             36947  78.2667             D20
    ## 235 female    NA     0     0            248727  33.0000                
    ## 236   male 49.00     1     0          PC 17485  56.9292             A20
    ## 237 female 24.00     2     1            243847  27.0000                
    ## 238   male 35.00     0     0            111426  26.5500                
    ## 239   male 27.00     0     0            113804  30.5000                
    ## 240 female 22.00     1     2     SC/Paris 2123  41.5792                
    ## 241 female 40.00     0     0          PC 17582 153.4625            C125
    ## 242 female    NA     1     0            367230  15.5000                
    ## 243 female 24.00     1     2            220845  65.0000                
    ## 244 female  4.00     2     1            230136  39.0000              F4
    ## 245   male 42.00     1     0             11753  52.5542             D19
    ## 246   male 20.00     1     1              2653  15.7417                
    ## 247 female 21.00     0     0             13502  77.9583              D9
    ## 248   male 80.00     0     0             27042  30.0000             A23
    ## 249   male 32.00     0     0             13214  30.5000             B50
    ## 250 female 28.00     0     0            237668  13.0000                
    ## 251 female 24.00     0     0          PC 17477  69.3000             B35
    ## 252   male    NA     0     0              1601  56.4958                
    ## 253 female  0.75     2     1              2666  19.2583                
    ## 254   male 48.00     1     0          PC 17572  76.7292             D33
    ## 255   male 56.00     0     0             13213  35.5000             A26
    ## 256 female 23.00     0     0          CA. 2314   7.5500                
    ## 257 female 18.00     0     1            231919  23.0000                
    ## 258 female    NA     0     0            330919   7.8292                
    ## 259   male 50.00     2     0          PC 17611 133.6500                
    ## 260   male 20.00     1     0 STON/O 2. 3101285   7.9250                
    ## 261 female    NA     1     0             19996  52.0000            C126
    ## 262 female 40.00     1     1             29750  39.0000                
    ## 263   male 31.00     0     0            244270  13.0000                
    ## 264 female 18.00     0     0              4138   9.8417                
    ## 265   male 36.00     0     1          PC 17755 512.3292     B51 B53 B55
    ## 266   male 27.00     0     0          PC 17572  76.7292             D49
    ## 267 female 15.00     0     1             24160 211.3375              B5
    ## 268   male 31.00     1     0             17474  57.0000             B20
    ## 269 female  4.00     0     1            349256  13.4167                
    ## 270   male    NA     0     0              1601  56.4958                
    ## 271 female    NA     0     0             35852   7.7333                
    ## 272 female 18.00     1     0          PC 17757 227.5250         C62 C64
    ## 273   male 35.00     0     0          PC 17475  26.2875             E24
    ## 274 female 45.00     0     0            223596  13.5000                
    ## 275   male 42.00     0     0          PC 17476  26.2875             E24
    ## 276 female 22.00     0     0            113781 151.5500                
    ## 277   male    NA     1     1              2661  15.2458                
    ## 278 female 24.00     0     0          PC 17482  49.5042             C90
    ## 279   male 48.00     1     0             19996  52.0000            C126
    ## 280 female 38.00     0     0          PC 17757 227.5250             C45
    ## 281 female 27.00     0     0             34218  10.5000            E101
    ## 282 female  6.00     0     1            248727  33.0000                
    ## 283   male 27.00     1     0            113806  53.1000              E8
    ## 284 female 30.00     3     0             31027  21.0000                
    ## 285 female    NA     0     0             36866   7.7375                
    ## 286 female 29.00     0     0             24160 211.3375              B5
    ## 287   male 35.00     0     0          PC 17755 512.3292            B101
    ## 288   male    NA     0     0             16988  30.0000             D45
    ## 289 female 21.00     2     2          PC 17608 262.3750 B57 B59 B63 B66
    ## 290   male 31.00     0     0 STON/O 2. 3101288   7.9250                
    ## 291 female 30.00     0     0            250648  13.0000                
    ## 292 female  4.00     1     1             29103  23.0000                
    ## 293   male  6.00     0     1            392096  12.4750            E121
    ## 294 female 48.00     1     2            220845  65.0000                
    ## 295   male  0.67     1     1            250649  14.5000                
    ## 296 female 33.00     0     0            110152  86.5000             B77
    ## 297   male 20.00     0     0              2663   7.2292                
    ## 298 female 36.00     1     2            113760 120.0000         B96 B98
    ## 299 female 51.00     1     0             13502  77.9583             D11
    ## 300 female 54.00     1     3             29105  23.0000                
    ## 301 female  5.00     0     0            364516  12.4750                
    ## 302 female 43.00     0     1             24160 211.3375              B3
    ## 303 female 13.00     0     0              2687   7.2292                
    ## 304 female 17.00     1     0             17474  57.0000             B20
    ## 305 female 18.00     0     0           3101265   7.4958                
    ## 306   male  1.00     1     2         C.A. 2315  20.5750                
    ## 307 female 49.00     0     0             17465  25.9292             D17
    ## 308 female 31.00     0     0            349244   8.6833                
    ## 309 female 31.00     1     1        C.A. 31921  26.2500                
    ## 310   male 11.00     1     2            113760 120.0000         B96 B98
    ## 311   male  0.42     0     1              2625   8.5167                
    ## 312   male 27.00     0     0            347089   6.9750                
    ## 313 female 33.00     1     0            113806  53.1000              E8
    ## 314 female 52.00     1     1             12749  93.5000             B69
    ## 315   male 27.00     0     0            315098   8.6625                
    ## 316 female 27.00     0     1            392096  12.4750            E121
    ## 317   male  1.00     0     2   S.C./PARIS 2079  37.0042                
    ## 318   male    NA     0     0            367228   7.7500                
    ## 319 female 62.00     0     0            113572  80.0000             B28
    ## 320 female 15.00     1     0              2659  14.4542                
    ## 321   male  0.83     1     1             29106  18.7500                
    ## 322 female 39.00     1     1          PC 17756  83.1583             E49
    ## 323   male 32.00     0     0              1601  56.4958                
    ## 324   male    NA     0     0             11774  29.7000             C47
    ## 325 female 30.00     0     0            113798  31.0000                
    ## 326 female    NA     1     0             17453  89.1042             C92
    ## 327 female 16.00     0     1          PC 17592  39.4000             D28
    ## 328 female 18.00     0     1            392091   9.3500                
    ## 329 female 45.00     1     1             36928 164.8667                
    ## 330   male 51.00     0     0            113055  26.5500             E17
    ## 331 female 24.00     0     3              2666  19.2583                
    ## 332 female 48.00     0     0             17466  25.9292             D17
    ## 333 female 42.00     0     0            236852  13.0000                
    ## 334 female 27.00     1     0     SC/PARIS 2149  13.8583                
    ## 335   male  4.00     1     1            347742  11.1333                
    ## 336 female 47.00     1     1             11751  52.5542             D35
    ## 337 female 28.00     1     0         P/PP 3381  24.0000                
    ## 338 female 15.00     0     0              2667   7.2250                
    ## 339 female 56.00     0     1             11767  83.1583             C50
    ## 340 female 25.00     0     1            230433  26.0000                
    ## 341 female 19.00     0     0            112053  30.0000             B42
    ## 342   male 26.00     0     0            111369  30.0000            C148
    ##     Embarked Age_Range
    ## 1          C     31-45
    ## 2          S     16-30
    ## 3          S     31-45
    ## 4          S     16-30
    ## 5          C      0-15
    ## 6          S      0-15
    ## 7          S     46-60
    ## 8          S     46-60
    ## 9          S      <NA>
    ## 10         C      <NA>
    ## 11         S     31-45
    ## 12         Q      0-15
    ## 13         S     16-30
    ## 14         S     31-45
    ## 15         Q      <NA>
    ## 16         C      <NA>
    ## 17         Q      <NA>
    ## 18         C      <NA>
    ## 19         C      0-15
    ## 20         C      0-15
    ## 21         Q     16-30
    ## 22         Q      <NA>
    ## 23         C     46-60
    ## 24         S     16-30
    ## 25         S      <NA>
    ## 26         S     16-30
    ## 27         S      0-15
    ## 28               31-45
    ## 29         C      <NA>
    ## 30         S     16-30
    ## 31         S     16-30
    ## 32         S     31-45
    ## 33         S      0-15
    ## 34         S     16-30
    ## 35         S     16-30
    ## 36         Q      <NA>
    ## 37         S     16-30
    ## 38         S     31-45
    ## 39         S     16-30
    ## 40         C     16-30
    ## 41         S     31-45
    ## 42         S     16-30
    ## 43         S      <NA>
    ## 44         Q      <NA>
    ## 45         S     31-45
    ## 46         C      0-15
    ## 47         S     16-30
    ## 48         C      <NA>
    ## 49         S     16-30
    ## 50         S     16-30
    ## 51         S     16-30
    ## 52         S     16-30
    ## 53         S     16-30
    ## 54         S     16-30
    ## 55         Q     16-30
    ## 56         S     31-45
    ## 57         S      0-15
    ## 58         S      <NA>
    ## 59         S      0-15
    ## 60         S      0-15
    ## 61         S      0-15
    ## 62         Q      <NA>
    ## 63         S     31-45
    ## 64         S     31-45
    ## 65         S     16-30
    ## 66         S      0-15
    ## 67         C     31-45
    ## 68         C     46-60
    ## 69         Q      <NA>
    ## 70         S     16-30
    ## 71         C     16-30
    ## 72         Q     16-30
    ## 73         C     31-45
    ## 74         S     31-45
    ## 75         C     31-45
    ## 76         S     16-30
    ## 77         C     31-45
    ## 78         S     16-30
    ## 79         S     31-45
    ## 80         S     16-30
    ## 81         S     31-45
    ## 82         S      0-15
    ## 83         S      0-15
    ## 84         Q      <NA>
    ## 85         S     16-30
    ## 86         S     31-45
    ## 87         C     16-30
    ## 88         C      <NA>
    ## 89         S     16-30
    ## 90         C     31-45
    ## 91         S     46-60
    ## 92         S      0-15
    ## 93         S     16-30
    ## 94         S     46-60
    ## 95         S     31-45
    ## 96         S     16-30
    ## 97         S     31-45
    ## 98         Q      <NA>
    ## 99         S     61-85
    ## 100        S     31-45
    ## 101        S     16-30
    ## 102        S     16-30
    ## 103        S     31-45
    ## 104        Q     16-30
    ## 105        S     16-30
    ## 106        C     16-30
    ## 107        S      <NA>
    ## 108        C     46-60
    ## 109        Q      <NA>
    ## 110        Q      <NA>
    ## 111        Q      <NA>
    ## 112        S      0-15
    ## 113        C      <NA>
    ## 114        C     16-30
    ## 115        C     16-30
    ## 116        C     16-30
    ## 117        C     16-30
    ## 118        S     16-30
    ## 119        S     16-30
    ## 120        S     31-45
    ## 121        C     31-45
    ## 122        Q     16-30
    ## 123        S     16-30
    ## 124        C     31-45
    ## 125        S     31-45
    ## 126        S     31-45
    ## 127        C     16-30
    ## 128        Q      <NA>
    ## 129        S      <NA>
    ## 130        C     31-45
    ## 131        S     31-45
    ## 132        S      0-15
    ## 133        S     16-30
    ## 134        S     16-30
    ## 135        S     31-45
    ## 136        S      <NA>
    ## 137        S      0-15
    ## 138        S     16-30
    ## 139        Q      <NA>
    ## 140        Q      <NA>
    ## 141        C     46-60
    ## 142        C      <NA>
    ## 143        Q      <NA>
    ## 144        C     16-30
    ## 145        C     16-30
    ## 146        C      <NA>
    ## 147        S     16-30
    ## 148        C     31-45
    ## 149        C      0-15
    ## 150        S     31-45
    ## 151        S     31-45
    ## 152        C     16-30
    ## 153        S     31-45
    ## 154        S     16-30
    ## 155        C     16-30
    ## 156        S     16-30
    ## 157        S     16-30
    ## 158        S     31-45
    ## 159        S      0-15
    ## 160        Q     31-45
    ## 161        S     31-45
    ## 162        S     31-45
    ## 163        S     16-30
    ## 164        S     16-30
    ## 165        S     16-30
    ## 166        S     31-45
    ## 167        S     16-30
    ## 168        S      <NA>
    ## 169        S     31-45
    ## 170        S      0-15
    ## 171        S     16-30
    ## 172        S     31-45
    ## 173        S     16-30
    ## 174        S      <NA>
    ## 175        S      0-15
    ## 176        S      0-15
    ## 177        S     31-45
    ## 178        C      0-15
    ## 179        S     46-60
    ## 180        C     46-60
    ## 181        C     16-30
    ## 182        S      <NA>
    ## 183        S     46-60
    ## 184        S     46-60
    ## 185        C      0-15
    ## 186        S     31-45
    ## 187        C     16-30
    ## 188        S      0-15
    ## 189        S     61-85
    ## 190        C     16-30
    ## 191        S     31-45
    ## 192        S      0-15
    ## 193        C     46-60
    ## 194        S     16-30
    ## 195        S     31-45
    ## 196        S      <NA>
    ## 197        S     16-30
    ## 198        Q     16-30
    ## 199        S     31-45
    ## 200        C     46-60
    ## 201        S     31-45
    ## 202        S     31-45
    ## 203        S     16-30
    ## 204        C     31-45
    ## 205        S     46-60
    ## 206        S      0-15
    ## 207        C      <NA>
    ## 208        S      0-15
    ## 209        C     16-30
    ## 210        C     16-30
    ## 211        S     31-45
    ## 212        S     31-45
    ## 213        S     16-30
    ## 214        C      <NA>
    ## 215        S      0-15
    ## 216        C     16-30
    ## 217        C     16-30
    ## 218        S     16-30
    ## 219        C     46-60
    ## 220        S     31-45
    ## 221        S     31-45
    ## 222        S     31-45
    ## 223        S     61-85
    ## 224        S     46-60
    ## 225        S     31-45
    ## 226        Q      <NA>
    ## 227        S     31-45
    ## 228        S     31-45
    ## 229        S     31-45
    ## 230        S     16-30
    ## 231        C     31-45
    ## 232        S     16-30
    ## 233        C     46-60
    ## 234        C     46-60
    ## 235        S      <NA>
    ## 236        C     46-60
    ## 237        S     16-30
    ## 238        C     31-45
    ## 239        S     16-30
    ## 240        C     16-30
    ## 241        S     31-45
    ## 242        Q      <NA>
    ## 243        S     16-30
    ## 244        S      0-15
    ## 245        S     31-45
    ## 246        C     16-30
    ## 247        S     16-30
    ## 248        S     61-85
    ## 249        C     31-45
    ## 250        S     16-30
    ## 251        C     16-30
    ## 252        S      <NA>
    ## 253        C      0-15
    ## 254        C     46-60
    ## 255        C     46-60
    ## 256        S     16-30
    ## 257        S     16-30
    ## 258        Q      <NA>
    ## 259        S     46-60
    ## 260        S     16-30
    ## 261        S      <NA>
    ## 262        S     31-45
    ## 263        S     31-45
    ## 264        S     16-30
    ## 265        C     31-45
    ## 266        C     16-30
    ## 267        S      0-15
    ## 268        S     31-45
    ## 269        C      0-15
    ## 270        S      <NA>
    ## 271        Q      <NA>
    ## 272        C     16-30
    ## 273        S     31-45
    ## 274        S     31-45
    ## 275        S     31-45
    ## 276        S     16-30
    ## 277        C      <NA>
    ## 278        C     16-30
    ## 279        S     46-60
    ## 280        C     31-45
    ## 281        S     16-30
    ## 282        S      0-15
    ## 283        S     16-30
    ## 284        S     16-30
    ## 285        Q      <NA>
    ## 286        S     16-30
    ## 287        C     31-45
    ## 288        S      <NA>
    ## 289        C     16-30
    ## 290        S     31-45
    ## 291        S     16-30
    ## 292        S      0-15
    ## 293        S      0-15
    ## 294        S     46-60
    ## 295        S      0-15
    ## 296        S     31-45
    ## 297        C     16-30
    ## 298        S     31-45
    ## 299        S     46-60
    ## 300        S     46-60
    ## 301        S      0-15
    ## 302        S     31-45
    ## 303        C      0-15
    ## 304        S     16-30
    ## 305        S     16-30
    ## 306        S      0-15
    ## 307        S     46-60
    ## 308        S     31-45
    ## 309        S     31-45
    ## 310        S      0-15
    ## 311        C      0-15
    ## 312        S     16-30
    ## 313        S     31-45
    ## 314        S     46-60
    ## 315        S     16-30
    ## 316        S     16-30
    ## 317        C      0-15
    ## 318        Q      <NA>
    ## 319              61-85
    ## 320        C      0-15
    ## 321        S      0-15
    ## 322        C     31-45
    ## 323        S     31-45
    ## 324        C      <NA>
    ## 325        C     16-30
    ## 326        C      <NA>
    ## 327        S     16-30
    ## 328        S     16-30
    ## 329        S     31-45
    ## 330        S     46-60
    ## 331        C     16-30
    ## 332        S     46-60
    ## 333        S     31-45
    ## 334        C     16-30
    ## 335        S      0-15
    ## 336        S     46-60
    ## 337        C     16-30
    ## 338        C      0-15
    ## 339        C     46-60
    ## 340        S     16-30
    ## 341        S     16-30
    ## 342        C     16-30

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

Part of passengers had cabin, while the other parts of passengers did
not have the cabin. In the group of passengers who had a cabin,
different passengers also had different kind of cabins according to the
prices. Bar plot will be used. One bar is going to show the survival
rate of the passengers who did not have a cabin, the other bars are used
to show different survival rates according to different kinds of cabins.
Mean will be useful in answering the question.

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
