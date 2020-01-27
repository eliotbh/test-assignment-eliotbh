Test statistical assignment
================
Eliot Barrett-Holman
25 January 2020

## Introduction

Please change the author and date fields above as appropriate. Do not
change the output format. Once you have completed the assignment you
want to knit your document into a markdown document in the
“github\_document” format and then commit both the .Rmd and .md files
(and all the associated files with graphs) to your private assignment
repository on Github.

## Reading data (40 points)

First, we need to read the data into R. For this assignment, I ask you
to use data from the youth self-completion questionnaire (completed by
children between 10 and 15 years old) from Wave 9 of the Understanding
Society. It is one of the files you have downloaded as part of SN6614
from the UK Data Service. To help you find and understand this file you
will need the following documents:

1)  The Understanding Society Waves 1-9 User Guide:
    <https://www.understandingsociety.ac.uk/sites/default/files/downloads/documentation/mainstage/user-guides/mainstage-user-guide.pdf>
2)  The youth self-completion questionnaire from Wave 9:
    <https://www.understandingsociety.ac.uk/sites/default/files/downloads/documentation/mainstage/questionnaire/wave-9/w9-gb-youth-self-completion-questionnaire.pdf>
3)  The codebook for the file:
    <https://www.understandingsociety.ac.uk/documentation/mainstage/dataset-documentation/datafile/youth/wave/9>

<!-- end list -->

``` r
library(tidyverse)
```

    ## Warning: package 'tidyverse' was built under R version 3.5.3

    ## -- Attaching packages --------------------------------------------------------------------------------------------------- tidyverse 1.3.0 --

    ## v ggplot2 3.2.1     v purrr   0.3.3
    ## v tibble  2.1.3     v dplyr   0.8.3
    ## v tidyr   1.0.0     v stringr 1.4.0
    ## v readr   1.3.1     v forcats 0.4.0

    ## Warning: package 'ggplot2' was built under R version 3.5.3

    ## Warning: package 'tibble' was built under R version 3.5.3

    ## Warning: package 'tidyr' was built under R version 3.5.3

    ## Warning: package 'readr' was built under R version 3.5.3

    ## Warning: package 'purrr' was built under R version 3.5.3

    ## Warning: package 'dplyr' was built under R version 3.5.3

    ## Warning: package 'stringr' was built under R version 3.5.3

    ## Warning: package 'forcats' was built under R version 3.5.3

    ## -- Conflicts ------------------------------------------------------------------------------------------------------ tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

``` r
# This attaches the tidyverse package. If you get an error here you need to install the package first. 
Data <- read_tsv("i_youth.tab")
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_double()
    ## )

    ## See spec(...) for full column specifications.

``` r
# You need to add between the quotation marks a full path to the required file on your computer.
```

## Tabulate variables (10 points)

In the survey children were asked the following question: “Do you have a
social media profile or account on any sites or apps?”. In this
assignment we want to explore how the probability of having an account
on social media depends on children’s age and gender.

Tabulate three variables: children’s gender, age (please use derived
variables) and having an account on social media.

``` r
# add your code here
table(Data$i_ypsex)
```

    ## 
    ##    1    2 
    ## 1411 1410

``` r
# 1411 males, 1410 females

table(Data$i_age_dv)
```

    ## 
    ##   9  10  11  12  13  14  15  16 
    ##   1 460 496 467 463 491 434   9

``` r
table(Data$i_ypsocweb)
```

    ## 
    ##   -9    1    2 
    ##   14 2277  530

## Recode variables (10 points)

We want to create a new binary variable for having an account on social
media so that 1 means “yes”, 0 means “no”, and all missing values are
coded as NA. We also want to recode gender into a new variable with the
values “male” and “female” (this can be a character vector or a factor).

``` r
# add your code here

Data$binaryaccount <- NA
Data$binaryaccount[Data$i_ypsocweb == 1] <- 1
Data$binaryaccount[Data$i_ypsocweb == 2] <- 0
Data$binaryaccount[Data$i_ypsocweb == -9] <- NA
table(is.na(Data$binaryaccount))
```

    ## 
    ## FALSE  TRUE 
    ##  2807    14

``` r
Data$gender <- NA
Data$gender[Data$i_ypsex == 1] <- "male"
Data$gender[Data$i_ypsex == 2 ] <- "female"
table(Data$gender)
```

    ## 
    ## female   male 
    ##   1410   1411

## Calculate means (10 points)

Produce code that calculates probabilities of having an account on
social media (i.e. the mean of your new binary variable produced in the
previous problem) by age and gender.

``` r
# add your code here

tapply(Data$binaryaccount, Data$gender, mean, na.rm=TRUE)
```

    ##    female      male 
    ## 0.8404558 0.7818959

``` r
tapply(Data$binaryaccount, Data$i_age_dv, mean, na.rm=TRUE)
```

    ##         9        10        11        12        13        14        15        16 
    ## 0.0000000 0.4868996 0.6989899 0.8655098 0.9152174 0.9468303 0.9585253 1.0000000

## Write short interpretation (10 points)

Write two or three sentences interpreting your findings above.

Here we find that female youths are more likely to have a social media
account than their male counterparts with an 84% probability compared to
a male’s 78% probability Moroever, we find that the probability of
having a social media account increases with age, with each of the nine
16 year-olds having an account. The 0% probability of a nine year old
having a social media account can be largely ignored here as we only
have one in the sample The gaps between ages 10-11 and 11-12 therefore
are the most significant with largest increases.

## Visualise results (20 points)

Create a statistical graph (only one, but it can be faceted)
illustrating your results (i.e. showing how the probability of having an
account on social media changes with age and gender). Which type of
statistical graph would be most appropriate for this?

``` r
# add your code here

ggplot(data = Data, aes(i_age_dv, y = mean(binaryaccount, na.rm = TRUE), fill = binaryaccount )) + geom_bar(position = "fill", stat = "identity" ) +facet_wrap(~gender) + labs(title = "Social Media usage by age and gender") + ylab("Proporiton") + xlab("Age")
```

![](testAssignment_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->
Couldn’t figure out how to get the proportions to work know you could
just plot(tapply(Data\(binaryaccount, Data\)i\_age\_dv, mean,
na.rm=TRUE)) but then you wouldn’t have it faceted for gender

## Conclusion

This is a test formative assignment and the mark will not count towards
your final mark. If you cannot answer any of the questions above this is
fine – we are just starting this module\! However, please do submit this
assignment in any case to make sure that you understand the procedure,
that it works correctly and you do not have any problems with summative
assignments later.
