Activity 4
================
Viktoria Basso

## Data and packages

Most of the functions that we will use this semester are from the
`{tidyverse}` (which is a package of many packages - a super package).
While we will focus on `{dplyr}`, it will be nice to be able to produce
graphical displays. Therefore, instead of loading both `{dplyr}` and
`{ggplot2}`, in the code chunk below we load all of the `{tidyverse}`.

``` r
library(tidyverse)
```

There is some other useful items to point out from this code chunk:

1.  It has been named (i.e., `load_packages`). Notice that the name
    doesn’t contain any spaces (I used `_` to separate words). If you
    put spaces in your R code chunk names, you might encounter errors
    later when you Knit your document. This is helpful because if you
    create a plot within a code chunk, the image will have that name!
    Another helpful thing by naming code chunks is that it helps with
    document navigation. In the upper right-hand corner of this `.Rmd`
    file, you can see the document outline
    <img src="README-img/document-outline-icon.png" alt="document outline" width = "20"/>
    icon. Clicking on this provides you with a table of contents type
    view. Another way to see this information by clicking on the “\#
    Data and packages” at the bottom left-hand area of this `.Rmd` pane.
2.  There is a code chunk option of `message = FALSE`…

### Messages

Knit your report with the `message = FALSE` R code chunk option and pay
attention to how this particular code chunk and output for loading the
`{tidyverse}` looks.

Now, switch this option to `message = TRUE` and knit your report again.
Discuss what is different when the `message` option is turned on
(`TRUE`) and turned off (`FALSE`) for the previous code chunk.

**Response**: When it is FALSE, you do not see the output from running
the code. When it is TRUE, then you can see the output.

Turn the `message` option off and continue in this activity.

### Data

In this activity, we will explore data on college majors and earnings
from the data behind the FiveThirtyEight story [The Economic Guide To
Picking A College
Major](https://fivethirtyeight.com/features/the-economic-guide-to-picking-a-college-major/).
These data originally come from the American Community Survey (ACS)
2010-2012 Public Use Microdata Series. If you are curious about how the
raw data from the ACS were cleaned and prepared (not a requirement), see
the FiveThirtyEight author’s
[code](https://github.com/fivethirtyeight/data/blob/master/college-majors/college-majors-rscript.R).

As we progress through this activity, remember that there are many
considerations that go into picking a major. Earning potential and
employment prospects are two (important) of these considerations, but
they do not tell the entire story. Keep this in mind as we analyze the
data.

These data are from an external (to R) file - a comma separated values
(`.csv`) file. We will use the `readr::read_csv` function to read in
these data. When you see something in this form, I am referring to the
function as well as what package it is from (i.e., `package::function`).
We will talk more about `::` later in the course.

I try to be consistent with notation so here are some other forms that
you might see in documents:

-   “`{package_name}`”; for example, `{readr}`.
-   “`function_name` function”; for example, `read_csv` function.
-   “*hint*”; for example, *arrange* the variable.
-   “**Specific Area**”; for example, **Environment** tab.

``` r
# Note that the :: method is redundant here
# because we have already loaded the {tidyverse}
# and {readr} is included in this.
# However, I am using this method to make you
# aware of the specific package the read_csv
# function comes from.

college_recent_grads <- readr::read_csv("data/recent-grads.csv")

# Typical style once {tidyverse} is loaded:
# college_recent_grads <- read_csv("data/recent-grads.csv")
```

Notice that this dataset is in a folder called `data`. We read it in
with the `read_csv` function, then saved the results as a new data frame
called `college_recent_grads`. You can view the entire dataset by
clicking on the name of the data frame in the **Environment** tab (make
sure you have run both the `load_packages` and `load_data` code chunks).

Explore and describe what the `message` option does in `load_data` code
chunk.

**Response**: The message option returns system information about the
import.

Turn the `message` option off and continue in this activity.

#### Aside

Base R has a function for reading in a csv file as well. First, knit
this report and look at the output for the `load_data` code chunk.

Now, edit the reading in data code in the `load_data` code chunk to
instead be:

    college_recent_grads <- read.csv("data/recent-grads.csv")

Then, knit this report and look at the output for the `load_data` code
chunk. What did you notice about the output from these different
functions? When creating a report, which function would be better to
use?

Now, edit the reading in data code in the `load_data` back to what is
was originally:

    college_recent_grads <- readr::read_csv("data/recent-grads.csv")

If you were to load the data using `read.csv` in an `.Rmd` file and try
to view it, it will print the ENTIRE dataset - this could make a short
report many hundreds of pages depending on the size of your dataset.
Historically, there were some other benefits of `readr::read_csv` over
`read.csv`, but these have mostly been updated to no longer be issues. I
strongly recommend that you use `readr::read_csv` instead of `read.csv`.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Data Codebook

Descriptions of the variables are provided below.

| Header                         | Description                                                                 |
|:-------------------------------|:----------------------------------------------------------------------------|
| `rank`                         | Rank by median earnings                                                     |
| `major_code`                   | Major code, FO1DP in ACS PUMS                                               |
| `major`                        | Major description                                                           |
| `major_category`               | Category of major from Carnevale et al                                      |
| `total`                        | Total number of people with major                                           |
| `sample_size`                  | Sample size (unweighted) of full-time, year-round ONLY (used for earnings)  |
| `men`                          | Male graduates                                                              |
| `women`                        | Female graduates                                                            |
| `sharewomen`                   | Women as share of total                                                     |
| `employed`                     | Number employed (ESR == 1 or 2)                                             |
| `employed_full_time`           | Employed 35 hours or more                                                   |
| `employed_part_time`           | Employed less than 35 hours                                                 |
| `employed_full_time_yearround` | Employed at least 50 weeks (WKW == 1) and at least 35 hours (WKHP &gt;= 35) |
| `unemployed`                   | Number unemployed (ESR == 3)                                                |
| `unemployment_rate`            | Unemployed / (Unemployed + Employed)                                        |
| `median`                       | Median earnings of full-time, year-round workers                            |
| `p25th`                        | 25th percentile of earnings                                                 |
| `p75th`                        | 75th percentile of earnings                                                 |
| `college_jobs`                 | Number with job requiring a college degree                                  |
| `non_college_jobs`             | Number with job not requiring a college degree                              |
| `low_wage_jobs`                | Number in low-wage service jobs                                             |

We can see that this data frame has a lot of useful information. In the
remaining sections we will answer the following questions (I’ll tell you
what to do, you don’t need to do anything yet):

-   Which major has the lowest unemployment rate?
-   Which major has the highest percentage of women?
-   Which STEM majors have incomes less than the median income of all
    majors?

Note that the ACS only asks [one
question](https://www.census.gov/acs/www/about/why-we-ask-each-question/sex/)
about a person’s sexual identity and we are therefore missing a lot of
important details about individuals.

## Analysis

### Which major has the lowest unemployment rate?

#### Describe your process

What information (variables) do we need to answer this question?
Describe how you would go about answering this question using a
different software (e.g., Excel, SAS, Python) that you are familiar with
or simply a general process. You do not need to code anything. As you
write this process, think of it as a series of steps and it might be
helpful to start at the goal and work backwards.

**Response**: I would sort the dataset by unemployment rate, then
observe the major at the top.

#### Using `{dplyr}`

In the R code chunk below, name it `rearrange_college`.

Take the `college_recent_grads` dataset, *then* `arrange` the dataset by
`unemployment_rate`.

``` r
arrange(college_recent_grads, unemployment_rate)
```

    ## # A tibble: 173 x 21
    ##     rank major_code major        major_category    total sample_size   men women
    ##    <dbl>      <dbl> <chr>        <chr>             <dbl>       <dbl> <dbl> <dbl>
    ##  1    53       4005 Mathematics… Computers & Math…   609           7   500   109
    ##  2    74       3801 Military Te… Industrial Arts …   124           4   124     0
    ##  3    84       3602 Botany       Biology & Life S…  1329           9   626   703
    ##  4   113       1106 Soil Science Agriculture & Na…   685           4   476   209
    ##  5   121       2301 Educational… Education           804           5   280   524
    ##  6    15       2409 Engineering… Engineering        4321          30  3526   795
    ##  7    20       3201 Court Repor… Law & Public Pol…  1148          14   877   271
    ##  8   120       2305 Mathematics… Education         14237         123  3872 10365
    ##  9     1       2419 Petroleum E… Engineering        2339          36  2057   282
    ## 10    65       1100 General Agr… Agriculture & Na… 10399         158  6053  4346
    ## # … with 163 more rows, and 13 more variables: sharewomen <dbl>,
    ## #   employed <dbl>, employed_fulltime <dbl>, employed_parttime <dbl>,
    ## #   employed_fulltime_yearround <dbl>, unemployed <dbl>,
    ## #   unemployment_rate <dbl>, p25th <dbl>, median <dbl>, p75th <dbl>,
    ## #   college_jobs <dbl>, non_college_jobs <dbl>, low_wage_jobs <dbl>

We have all of the information to answer our question (i.e., “Which
major has the lowest unemployment rate?”), but it is not in an effective
format. The major names barely fit on the page, we have many variables
are not that useful in answering our question (e.g., `major_code`,
`major_category`), and some variables that we might want
front-and-center are not easily viewed (i.e., `unemployment_rate`).
Also, notice that by default when you *arrange* by a variable, it
defaults to *ascending* order.

#### Removing unnecessary information

Use the pipeline that you made in the `rearrange_college` code chunk as
the start to this solution, **then** `select` only the variables `rank`,
`major`, and `unemployment_rate`. Name this code chunk
`lowest_unemploy`.

``` r
college_recent_grads %>%
arrange(unemployment_rate) %>% 
select(rank, major, unemployment_rate)
```

    ## # A tibble: 173 x 3
    ##     rank major                                      unemployment_rate
    ##    <dbl> <chr>                                                  <dbl>
    ##  1    53 Mathematics And Computer Science                     0      
    ##  2    74 Military Technologies                                0      
    ##  3    84 Botany                                               0      
    ##  4   113 Soil Science                                         0      
    ##  5   121 Educational Administration And Supervision           0      
    ##  6    15 Engineering Mechanics Physics And Science            0.00633
    ##  7    20 Court Reporting                                      0.0117 
    ##  8   120 Mathematics Teacher Education                        0.0162 
    ##  9     1 Petroleum Engineering                                0.0184 
    ## 10    65 General Agriculture                                  0.0196 
    ## # … with 163 more rows

**Response**: The lowest unemployment rate was Mathematics and Computer
Science, Military Technologies, Botany, Soil Science, and Educational
Administration.

<img src="README-img/noun_pause.png" alt="pause" width = "20"/>
<b>Planned Pause Point</b>: If you feel that you have a good
understanding of these commands, feel free to start working on your
project. The remainder of this activity will help to expand these
commands.

### Which major has the highest proportion of women?

Using the `college_recent_grads` dataset and functions from `{dplyr}`,
`arrange` the dataset by `sharewomen`, and `select` only `rank`,
`major`, and `sharewomen`. Name your code chunk `highest_prop_women`.

``` r
college_recent_grads %>%
  arrange(sharewomen) %>%
  select(rank, women, sharewomen, major)
```

    ## # A tibble: 173 x 4
    ##     rank women sharewomen major                                      
    ##    <dbl> <dbl>      <dbl> <chr>                                      
    ##  1    74     0     0      Military Technologies                      
    ##  2    67   371     0.0775 Mechanical Engineering Related Technologies
    ##  3    27  1678     0.0907 Construction Services                      
    ##  4     2    77     0.102  Mining And Mineral Engineering             
    ##  5     4   135     0.107  Naval Architecture And Marine Engineering  
    ##  6     9 10907     0.120  Mechanical Engineering                     
    ##  7     1   282     0.121  Petroleum Engineering                      
    ##  8   107  1893     0.125  Transportation Sciences And Technologies   
    ##  9   112   451     0.125  Forestry                                   
    ## 10    12  2105     0.140  Aerospace Engineering                      
    ## # … with 163 more rows

Discuss your output as it relates to the research question.

**Response**:Early Childhood Education and Communication Disorders
Science are the two with the highest purportion of women.

![](README-img/noun_pause.png) **Planned Pause Point**: If you have any
questions, contact your instructor. Otherwise feel free to continue on.

### Which STEM majors have incomes less than the median income of all majors?

One of the sections of the FiveThirtyEight story is “All STEM fields
aren’t the same”. We will see if this is true.

Below is a new vector called `stem_categories` that lists the *major
categories* (this is not the `majors` variable - look back at the Data
Codebook) that are considered STEM fields.

``` r
stem_categories <- c("Biology & Life Science",
                     "Computers & Mathematics",
                     "Engineering",
                     "Physical Sciences")
```

In our next activity we will see how to calculate summary statistics,
but for now believe me that the median salary for all majors is $36,000.

Using the `stem_categories` vector and knowledge that the median salary
for all majors is $36,000 to *keep* the rows in the
`college_recent_grads` data set that are in the STEM fields and earn
*less* than the `median` salary of all majors. Only display the
variables `major`, `p25th`, `median`, and `p75th` Name the code chunk
`stem_low_salaries`.

``` r
college_recent_grads %>%
  subset(major_category %in% stem_categories) %>%
  filter(median < 36000) %>%
  select(major, p25th, median, p75th)
```

    ## # A tibble: 10 x 4
    ##    major                                 p25th median p75th
    ##    <chr>                                 <dbl>  <dbl> <dbl>
    ##  1 Environmental Science                 25000  35600 40200
    ##  2 Multi-Disciplinary Or General Science 24000  35000 50000
    ##  3 Physiology                            20000  35000 50000
    ##  4 Communication Technologies            25000  35000 45000
    ##  5 Neuroscience                          30000  35000 44000
    ##  6 Atmospheric Sciences And Meteorology  28000  35000 50000
    ##  7 Miscellaneous Biology                 23000  33500 48000
    ##  8 Biology                               24000  33400 45000
    ##  9 Ecology                               23000  33000 42000
    ## 10 Zoology                               20000  26000 39000

Discuss your output as it relates to the research question.

**Response**:Ten STEM fields graduate with a salary below $36,000. I am
not surprised Environmental Science is one of them, but I am surprised
at Neuroscience.

![](README-img/noun_pause.png) **(Final) Planned Pause Point**: If you
have any questions, contact your instructor. Otherwise feel free to
continue on.

Knit, then stage everything listed in your **Git** pane, commit (with a
meaningful commit message), and push to your GitHub repo. Go to GitHub
and verify that your `activity04-data-pieplines.Rmd` file appears as you
intended it to.

You can now go back to the `README` file.

## Attribution

This activity is inspired by a lab from [Dr. Mine
Çetinkaya-Rundel](http://www2.stat.duke.edu/~mc301/)’s STA 199 course.
