Mini-Hackathon 3: Tidy Scripts
================
Mariken van der Velden & Kasper Welbers

Mini-Hackathons are performed and submitted in pairs of two. You must
hand in your assignment on Canvas the next week before **Tuesday
Midnight**.

Use this RMarkdown template on the canvas page for this mini-hackathon
to complete your hackathon. When you are finished, knit the file into a
pdf with the knit button in the toolbar (or using Ctrl+Shift+K). For
this you need to have the `knitr` and `printr` packages installed, and
all your code needs to work (see the R course companion for more
instructions). If you cannot knit the `.Rmd` file, there is probably an
error in your R code, therefore add `eval=FALSE` to the code chunk: `{r,
eval = FALSE}`, so you are still able to knit and upload the file.

# This Mini-Hackathon

This mini-hackathon builds upon the tutorials on
[transforming](https://github.com/ccs-amsterdam/r-course-material/blob/master/tutorials/R-tidy-5-transformation.md),
[summarizing](https://github.com/ccs-amsterdam/r-course-material/blob/master/tutorials/R-tidy-5b-groupby.md),
[reshaping](https://github.com/ccs-amsterdam/r-course-material/blob/master/tutorials/r-tidy-12-reshaping.md),
and
[visualizing](https://github.com/ccs-amsterdam/r-course-material/blob/master/tutorials/r-tidy-3_7-visualization.md)
data. For this hackathon, you can (and are strongly recommended to) use
code from this week’s tutorial as well as provided here. If you aim to
conduct additional analyses in R, we of course encourage this.
Nevertheless, it is important to not do additional analysis just for the
sake of running more code chunks. For that reason, please provide a
justification for these additional analyses.

Also, we recommend to use parameters for RMarkdown codeblocks, in
particular the `cache = TRUE` parameter for codeblocks that take long to
compute (e.g., downloading data from AmCAT). A brief explanation of some
usefull parameters to make a `.Rmd` file pretty is given in the [the
tutorial of two weeks
ago](https://github.com/MarikenvdVelden/Replication-Hackathons/blob/main/Intro-to-rmd-and-data-retrieval.md).
Additionally, you can use [this cheat
sheet](https://rstudio.com/wp-content/uploads/2015/02/rmarkdown-cheatsheet.pdf).

Important to take into account is that this week we will build upon
codes of last weeks.

### Hackathon Challenges

#### Challenge 1

Choose a data set. You can either work with the AmCAT data set with
newspaper articles on the US presidential candidates in 2020 or the
Guardian data set with newspaper articles in politics, economy, society
and international sections from 2012 to 2016. To get the data, one can
run the code to download the data from `quanteda.corpora` or download
the `.csv` file with AmCAT data from Canvas. Take a look at respectively
[the first
tutorial](https://github.com/MarikenvdVelden/Replication-Hackathons/blob/main/Intro-to-rmd-and-data-retrieval.md)
or the first mini-hackathon (see
[here](https://github.com/MarikenvdVelden/Replication-Hackathons/blob/main/Mini-Hackathon1.md)
and
[here](https://github.com/MarikenvdVelden/Replication-Hackathons/blob/main/Mini-Hackathon1-Guardian.md))
to see how to import data. Use tidyverse to transform the data in the
following ways:

  - Apply a dictionary from `quanteda.dictionaries` to the corpus of
    your textual data and create a meaningful variable (e.g.
    [subjectivity
    score](https://github.com/ccs-amsterdam/r-course-material/blob/master/tutorials/sentiment_analysis.md))
    using `mutate`.
  - Subset the data set by (a) filtering out values that are not of
    interest (e.g. neutral category in sentiment analyses) using
    `filter`; and (b) selecting the variables that you need for
    computing new variables (e.g. mean values of a variable), using
    `select`.
  - Aggregate (or summarize) the data to another group level, for
    example to another time level (e.g. monthly level) or based on
    another variable of your choice (e.g. news outlet) using `group_by`,
    and `summarize` or `mutate`.

If you want to use the following code to query the AmCAT data set based
on the `.csv` file:

``` r
library(tidyverse)
library(amcatr)
library(quanteda)

d <- read_csv("../amcat_data.csv")
corp <- corpus(d, docid_field = 'id', text_field = 'text')
q  <- grepl('econom', texts(corp), ignore.case = T)
d <- corpus_subset(corp, q) %>%
  convert(corp, to = "data.frame")
```

Querying the Guardian data is described in
[Mini-Hackathon 2](https://github.com/MarikenvdVelden/Replication-Hackathons/blob/main/Mini-Hackathon2.md).

#### Challenge 2

Develop an infographic to visualize a meaningful relationship based on
your data using `ggplot2`. Try to make the infographic as informative
and appealing as possible. For this, you can use the [*ggplot2 cheat
sheet*](https://rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf),
the [*R Graphics Cookbook*](http://www.cookbook-r.com/Graphs/), [Kieran
Healy’s *Data Visualization* book](https://socviz.co/), and/ or the
[*BBC Visual and Data Journalism
Cookbook*](https://bbc.github.io/rcookbook/). If you want to make more
than one graph, have a look at the `facet_wrap` function: `?facet_wrap`.

#### Challenge 3

What does your infographic demonstrate? Explain your answer as if you
would inform an Economist reader.

#### Challenge 4

What if you would write for a tabloid newspaper such as the Sun, would
you assess, based on the conclusions in the study of [Prior
(2014)](https://vu.on.worldcat.org/oclc/8272642139), the visual literacy
of the readership to change? If so, what would you adjust in the
infographic?
