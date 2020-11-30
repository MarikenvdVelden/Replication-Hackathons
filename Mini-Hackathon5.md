Mini-Hackathon 5: Times-Series (Part II)
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

This mini-hackathon builds upon **all** tutorials of this course. For
this hackathon, you’re therefore strongly required to revisit the
tutorials of the last five weeks. If you aim to conduct additional
analyses in R, we of course encourage this. Nevertheless, it is
important to not do additional analysis just for the sake of running
more code chunks. For that reason, please provide a justification for
these additional analyses.

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

Based on the literature in week 1 – 6 of this course, investigate
initial support for a “simple” research question that could be answered
using times-series data. This question should have the structure of does
*variable X* lead to more/ less in *variable Y*. To answer your
question, you can choose any data set we worked with in the tutorials:

  - AmCAT data
  - Guardian data
  - Google trends data
  - Stock market data
  - Polling data

If this is not sufficient to answer your question, you can search the
[fivethirtyeight database](https://data.fivethirtyeight.com/) for more
data [here](https://github.com/fivethirtyeight/data).

Please describe the research question and elaborate on the data,
including the data structure you need (e.g. which level of aggregation)
in order to answer the question.

#### Challenge 2

Create a data set using a **tidy script** with your X, Y, and date
variable.

#### Challenge 3

Develop an infographic to visualize a meaningful relationship between
your X and Y variable based on your data using `ggplot2`. Try to make
the infographic as informative and appealing as possible. For this, you
can use the [*ggplot2 cheat
sheet*](https://rstudio.com/wp-content/uploads/2015/03/ggplot2-cheatsheet.pdf),
the [*R Graphics Cookbook*](http://www.cookbook-r.com/Graphs/), [Kieran
Healy’s *Data Visualization* book](https://socviz.co/), and/ or the
[*BBC Visual and Data Journalism
Cookbook*](https://bbc.github.io/rcookbook/). If you want to make more
than one graph, have a look at the `facet_wrap` function: `?facet_wrap`.

#### Challenge 4

What does your infographic demonstrate? Interpret the infographic in
light of your research question.

#### Challenge 5

To answer your question, run a correlation and interpret the value.
Reflect on your research design and describe the limitations.
