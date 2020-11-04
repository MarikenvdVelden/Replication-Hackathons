Mini-Hackathon 1: Sentiment Analysis - Guardian Data
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

This mini-hackathon builds upon the tutorial on [sentiment
analysis](https://github.com/ccs-amsterdam/r-course-material/blob/master/tutorials/sentiment_analysis.md).
For this hackathon, you can (and are strongly recommended to) use code
from this week’s tutorial as well as provided here. If you aim to
conduct additional analyses in R, we of course encourage this.
Nevertheless, it is important to not do additional analysis just for the
sake of running more code chunks. For that reason, please provide a
justification for these additional analyses.

Also, we recommend to use parameters for RMarkdown codeblocks, in
particular the `cache = TRUE` parameter for codeblocks that take long to
compute (e.g., downloading data from AmCAT). A brief explanation of some
usefull parameters is given in the [the tutorial of last
week](https://github.com/MarikenvdVelden/Replication-Hackathons/blob/main/Intro-to-rmd-and-data-retrieval.md).

Important to take into account is that this week you laptop will be
doing the heavy lifting. Accordingly, in the assignment, you should be
carefull not to choose a concept that has too many articles.

### Hackathon Challenges

#### Challenge 1

Choose a political issue and perform a sentiment analysis. To do so, we
will work with the Guardian data set. This contains Guardian newspaper
articles in politics, economy, society and international sections from
2012 to 2016. To get the data, one can run the following code to
download the data from `quanteda.corpora` for the political issue you
aim to study. For example, if one wants to know what the Guardian
reports about the economy, you can run the code chunk below. If you are
interested in another political issue, you can adapt the query.

``` r
devtools::install_github("quanteda/quanteda.corpora")
library(quanteda.corpora)

corp <- download('data_corpus_guardian')
```

``` r
library(quanteda.corpora)
corp <- download('data_corpus_guardian')
```

``` r
library(quanteda)
q  <- grepl('econom', texts(corp), ignore.case = T)
econ_news <- corpus_subset(corp, q)
```

To conduct the sentiment analysis using a dictionary, we have to create
a document-term matrix (DTM), for more information about DTM, [see the
tutorial on basic text analysis with
quanteda](https://github.com/ccs-amsterdam/r-course-material/blob/master/tutorials/R_text_3_quanteda.md).

We will work with the dictionaries implemented in an additional package
of quanteda. To be able to use this extension, make sure to install the
development version, because it inlcudes features that are not yet on
the released CRAN version. Please run:

``` r
devtools::install_github("kbenoit/quanteda.dictionaries") 
```

Thereafter, load the dictionary and inspect which dictionaries are
included in this package:

``` r
library(quanteda.dictionaries)
?`quanteda.dictionaries-package`
```

Choose a dictionary from `quanteda.dictionaries` and run this over the
news articles collected from AmCAT.

``` r
AFINN_dict <- dictionary(data_dictionary_AFINN)

result_AFINN <- dtm %>% 
  dfm_lookup(AFINN_dict) %>% 
  convert(to = "data.frame") %>% 
  as_tibble

head(result_AFINN)
```

| document   | negative | positive |
| :--------- | -------: | -------: |
| text136585 |       37 |       72 |
| text107174 |       24 |       28 |
| text31104  |        5 |       10 |
| text163885 |       21 |       49 |
| text81309  |       15 |       17 |
| text173244 |        3 |       26 |

Subsequently normalize the length of documents and compute a sort of
overall sentiment score as explained in the [sentiment
tutorial](https://github.com/ccs-amsterdam/r-course-material/blob/master/tutorials/sentiment_analysis.md).

Visualize the results of the sentiment analysis using the code below.
**NOTE**: the code chunck below assumes that you have created a
sentiment variable yourself, in the example I have called the variable
`sentiment1`. If you rename your sentiment variable differently, change
the code accordingly. My sentiment variable uses the following code from
the [sentiment
tutorial](https://github.com/ccs-amsterdam/r-course-material/blob/master/tutorials/sentiment_analysis.md):
`mutate(sentiment1=(positive - negative) / (positive + negative))`. You
can use a different way to calculate the sentiment, but be aware that
depending on the method that you use to calculate sentiment, the result
might differ from the results you see here in this file.

You can go wild with
[colors](http://www.stat.columbia.edu/~tzheng/files/Rcolor.pdf), if you
like. Use an aggregation level (days, weeks, months) that makes sense to
you, and interpret the results. You are recommended to use multiple
codeblocks, and to use `cache = T` where usefull.

``` r
# Add date variable to results
result_AFINN <- result_AFINN %>%
  mutate(date = as.Date(docvars(econ_news, "date")),
         id = "AFINN")

#Aggregate to the month level
library(tidyquant)
df <- result_AFINN %>%
    tq_transmute(select     = sentiment1,
                 mutate_fun = apply.monthly, #or apply.weekly for week level
                 FUN        = sum)
head(df)
```

| date       |  sentiment1 |
| :--------- | ----------: |
| 2012-01-28 | \-0.7668119 |
| 2012-02-25 |   0.1108273 |
| 2012-03-30 | \-1.0444823 |
| 2012-04-27 |   0.6739423 |
| 2012-05-28 |   2.8837905 |
| 2012-06-28 | \-0.5612669 |

``` r
library(tidyverse)
ggplot(df, mapping = aes(x = date, y = sentiment1)) +
  geom_line(color = "seagreen") +
  labs(x = "", y = "Sentiment Score", 
       title= "Tone of Guardian on Economy") +
  theme_minimal() +
  theme(plot.title = element_text(hjust = 0.5)) #to center the title of the plot
```

![](Mini-Hackathon1-Guardian_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

#### Challenge 2

If you use another dictionary, would you get the same results? [Recent
work by Wouter van Atteveldt, Mariken van der Velden, and Mark Boukes
shows that the results of sentiment analysis vastly differ between
dictionaries.](https://github.com/vanatteveldt/ecosent) Conduct a second
sentiment analysis using another dictionary from
`quanteda.dictionaries`. Check and interpret the correlation between the
sentiment analysis of the two different dictionaries. I have added the
`data_dictionary_geninqposneg` dictionary.

``` r
cor(result_AFINN$sentiment1, result_GENINQ$sentiment1)
```

    ## [1] 0.6039533

To understand the outcome of the correlation better, it can be useful to
visualize the over time trends of sentiment on the European Union
reported in the news on the presidential candidates for both
dictionaries.

``` r
df <- result_AFINN %>%
  add_row(result_GENINQ) %>%
  group_by(id) %>%
    tq_transmute(select     = c(sentiment1),
                 mutate_fun = apply.monthly, #or apply.weekly for week level
                 FUN        = sum)
#head(df)

ggplot(df, mapping = aes(x = date, y = sentiment1, group = id, colour = id)) +
  geom_line() +
  labs(x = "", y = "Sentiment Score", 
       title= "Tone of Guardian on Economy") +
  theme_minimal() +
  scale_color_manual(values=c("seagreen", "violet")) +
  theme(plot.title = element_text(hjust = 0.5),
        legend.title=element_blank(), #edit the legend
        legend.position = "bottom") #to center the title of the plot
```

![](Mini-Hackathon1-Guardian_files/figure-gfm/unnamed-chunk-13-1.png)<!-- -->

#### Challenge 3

Reflect on the validity of the current analysis, both at the level of
specific hits and at an aggregate level. For the validity of specific
hits, look at the `Key-Word-In-Context` listings for the 20 first hits,
as discussed in the [sentiment
tutorial](https://github.com/ccs-amsterdam/r-course-material/blob/master/tutorials/sentiment_analysis.md).
Do the sentiment words correctly relate to the sentiment with which your
concept is discussed (note: only focus on the words between angle
brackets)? If the specific hits do not accurately measure sentiment, do
you think the aggregate results are still usefull in your case?

#### Challenge 4

The method used in the tutorial is pretty crude. Even for dictionary
methods, there are ways to improve results. Can you think of any problem
in particular, and any ways to mend them? Are there problems which
cannot be solved, or would be very hard to solve?