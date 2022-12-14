Mini Data Analysis Milestone 2
================

*To complete this milestone, you can edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to your second (and last) milestone in your mini data analysis project!

In Milestone 1, you explored your data, came up with research questions,
and obtained some results by making summary tables and graphs. This
time, we will first explore more in depth the concept of *tidy data.*
Then, you’ll be sharpening some of the results you obtained from your
previous milestone by:

-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 55 points (compared to the 45 points
of the Milestone 1): 45 for your analysis, and 10 for your entire
mini-analysis GitHub repository. Details follow.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

-   Understand what *tidy* data is, and how to create it using `tidyr`.
-   Generate a reproducible and clear report using R Markdown.
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
library(lubridate)
library(broom)
library(here)
library(scales)
```

# Task 1: Tidy your data (15 points)

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

*Tidy’ing* data is sometimes necessary because it can simplify
computation. Other times it can be nice to organize data so that it can
be easier to understand when read manually.

### 2.1 (2.5 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

``` r
answer2.1 <- vancouver_trees %>%
    select(tree_id, genus_name, street_side_name, neighbourhood_name, height_range_id, diameter, curb, date_planted)
head(answer2.1)
```

    ## # A tibble: 6 × 8
    ##   tree_id genus_name street_side_name neighbo…¹ heigh…² diame…³ curb  date_pla…⁴
    ##     <dbl> <chr>      <chr>            <chr>       <dbl>   <dbl> <chr> <date>    
    ## 1  149556 ULMUS      EVEN             MARPOLE         2      10 N     1999-01-13
    ## 2  149563 ZELKOVA    EVEN             MARPOLE         4      10 N     1996-05-31
    ## 3  149579 STYRAX     EVEN             KENSINGT…       3       4 Y     1993-11-22
    ## 4  149590 FRAXINUS   EVEN             KENSINGT…       4      18 Y     1996-04-29
    ## 5  149604 ACER       EVEN             KENSINGT…       2       9 Y     1993-12-17
    ## 6  149616 PYRUS      ODD              MARPOLE         2       5 Y     NA        
    ## # … with abbreviated variable names ¹​neighbourhood_name, ²​height_range_id,
    ## #   ³​diameter, ⁴​date_planted

If we only focus on the dataset **itself**, this dataset is tidy. This
is because:

1.  There is no cell with multiple values
2.  Each column represents a meaningful variable of trees
3.  Each row is an observation

But if we focus on a **specific research questions**, for example my
third research question (Is there a pattern where curbs occur?), this
data is untidy. The reason is because in this case having(Y) or not
having(N) a curb should be **two** different variables (represented in
different columns) if we want to see the distribution of curb occurrence
over street_side_name, but now it’s concluded by only **one**
categorical value called ‘curb’.
<!----------------------------------------------------------------------------->

### 2.2 (5 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

For the research question: ‘Is there a pattern where curbs occur?’, we
want to investigate if there’s a relationship between having curb or not
and street_side_name. Here’s what we do:

1.  Compute the sum and percentage of curb occurrence over
    street_side_name
2.  As the ‘curb’ is a <chr> var, we then create a factor
    ‘curb_occurrence’ (noCurb, hasCurb), in which noCurb represents N
    and hasCurb represents Y.

``` r
# Compute the sum and percentage of curb occurrence over street_side_name
# The result should be untidy for this research question as having(Y) or not having(N) a curb should be **two** different variable 
curb_location_untidy <- vancouver_trees %>%
    mutate(curb_occurrence = factor(case_when(curb == "Y" ~ "hasCurb",
                            curb == "N" ~ "noCurb"),
                        levels = c('hasCurb', 'noCurb'))) %>%
    group_by(street_side_name, .drop=FALSE) %>%
    count(curb_occurrence, street_side_name) %>% 
    mutate(percentage = prop.table(n) * 100) # Percentage of trees with(out) curbs in a type of street_side_name

# BEFORE: This is the original data before tidy'ing
curb_location_untidy
```

    ## # A tibble: 12 × 4
    ## # Groups:   street_side_name [6]
    ##    street_side_name curb_occurrence     n percentage
    ##    <chr>            <fct>           <int>      <dbl>
    ##  1 BIKE MED         hasCurb            38    100    
    ##  2 BIKE MED         noCurb              0      0    
    ##  3 EVEN             hasCurb         65746     91.6  
    ##  4 EVEN             noCurb           6007      8.37 
    ##  5 GREENWAY         hasCurb             0      0    
    ##  6 GREENWAY         noCurb              5    100    
    ##  7 MED              hasCurb          2752     83.5  
    ##  8 MED              noCurb            545     16.5  
    ##  9 ODD              hasCurb         65128     91.2  
    ## 10 ODD              noCurb           6246      8.75 
    ## 11 PARK             hasCurb           143     99.3  
    ## 12 PARK             noCurb              1      0.694

This curb_location_untidy is not tidy because (not) having a curb are
two scenarios of which we wants to see the distribution. Thus we use
**pivot_wider()** to make them two separate columns in the new data

``` r
# Tidy the data
curb_location_tidy <- curb_location_untidy %>%
    pivot_wider(
      id_cols = street_side_name,
      names_from = curb_occurrence,
      values_from = c(n, percentage))

# AFTER: This is the data after tidy'ing
curb_location_tidy
```

    ## # A tibble: 6 × 5
    ## # Groups:   street_side_name [6]
    ##   street_side_name n_hasCurb n_noCurb percentage_hasCurb percentage_noCurb
    ##   <chr>                <int>    <int>              <dbl>             <dbl>
    ## 1 BIKE MED                38        0              100               0    
    ## 2 EVEN                 65746     6007               91.6             8.37 
    ## 3 GREENWAY                 0        5                0             100    
    ## 4 MED                   2752      545               83.5            16.5  
    ## 5 ODD                  65128     6246               91.2             8.75 
    ## 6 PARK                   143        1               99.3             0.694

Use **pivot_longer()** again to untidy it.

``` r
# Untidy it to be the same as 'curb_location_untidy'
curb_location_untidy_again <- curb_location_tidy %>%
    pivot_longer(
        cols = -street_side_name,
        names_to = c(".value", "curb_existence"),
        names_sep = "_")
curb_location_untidy_again
```

    ## # A tibble: 12 × 4
    ## # Groups:   street_side_name [6]
    ##    street_side_name curb_existence     n percentage
    ##    <chr>            <chr>          <int>      <dbl>
    ##  1 BIKE MED         hasCurb           38    100    
    ##  2 BIKE MED         noCurb             0      0    
    ##  3 EVEN             hasCurb        65746     91.6  
    ##  4 EVEN             noCurb          6007      8.37 
    ##  5 GREENWAY         hasCurb            0      0    
    ##  6 GREENWAY         noCurb             5    100    
    ##  7 MED              hasCurb         2752     83.5  
    ##  8 MED              noCurb           545     16.5  
    ##  9 ODD              hasCurb        65128     91.2  
    ## 10 ODD              noCurb          6246      8.75 
    ## 11 PARK             hasCurb          143     99.3  
    ## 12 PARK             noCurb             1      0.694

<!----------------------------------------------------------------------------->

### 2.3 (7.5 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the next four tasks:

<!-------------------------- Start your work below ---------------------------->

1.  *Is there a pattern where curbs occur?*
2.  *Are trees planted year round?*

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

Reason:

1.  I think the first one is an open-ended question instead of a simple
    yes-or-no one. Thus it has a lot to be discovered.

2.  The second question involves a time variable ‘date_planted’ which
    others does not have.
    <!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

<!--------------------------- Start your work below --------------------------->

``` r
version1.0 <- vancouver_trees %>%
    mutate(curb_occurrence = factor(case_when(curb == "Y" ~ "hasCurb",
                        curb == "N" ~ "noCurb"),
                    levels = c('hasCurb', 'noCurb'))) %>%
    drop_na(date_planted) %>%
    select(-c(tree_id, on_street_block, curb, civic_number))
version1.0
```

    ## # A tibble: 70,063 × 17
    ##    std_street    genus…¹ speci…² culti…³ commo…⁴ assig…⁵ root_…⁶ plant…⁷ on_st…⁸
    ##    <chr>         <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1 W 58TH AV     ULMUS   AMERIC… BRANDON BRANDO… N       N       N       W 58TH…
    ##  2 W 58TH AV     ZELKOVA SERRATA <NA>    JAPANE… N       N       N       W 58TH…
    ##  3 WINDSOR ST    STYRAX  JAPONI… <NA>    JAPANE… N       N       4       WINDSO…
    ##  4 E 39TH AV     FRAXIN… AMERIC… AUTUMN… AUTUMN… Y       N       4       E 39TH…
    ##  5 WINDSOR ST    ACER    CAMPES… <NA>    HEDGE … N       N       4       WINDSO…
    ##  6 SHERBROOKE ST ACER    PLATAN… COLUMN… COLUMN… N       N       6       SHERBR…
    ##  7 SHERBROOKE ST ACER    PLATAN… COLUMN… COLUMN… N       N       6       SHERBR…
    ##  8 SHERBROOKE ST ACER    PLATAN… COLUMN… COLUMN… N       N       3       SHERBR…
    ##  9 E 39TH AV     FRAXIN… AMERIC… AUTUMN… AUTUMN… N       N       3       E 39TH…
    ## 10 E 39TH AV     TILIA   EUCHLO… <NA>    CRIMEA… N       N       5       E 39TH…
    ## # … with 70,053 more rows, 8 more variables: neighbourhood_name <chr>,
    ## #   street_side_name <chr>, height_range_id <dbl>, diameter <dbl>,
    ## #   date_planted <date>, longitude <dbl>, latitude <dbl>,
    ## #   curb_occurrence <fct>, and abbreviated variable names ¹​genus_name,
    ## #   ²​species_name, ³​cultivar_name, ⁴​common_name, ⁵​assigned, ⁶​root_barrier,
    ## #   ⁷​plant_area, ⁸​on_street

<!----------------------------------------------------------------------------->

# Task 2: Special Data Types (10)

For this exercise, you’ll be choosing two of the three tasks below –
both tasks that you choose are worth 5 points each.

But first, tasks 1 and 2 below ask you to modify a plot you made in a
previous milestone. The plot you choose should involve plotting across
at least three groups (whether by facetting, or using an aesthetic like
colour). Place this plot below (you’re allowed to modify the plot if
you’d like). If you don’t have such a plot, you’ll need to make one.
Place the code for your plot below.

<!-------------------------- Start your work below ---------------------------->

``` r
ggplot(version1.0, aes(x = curb_occurrence, fill = street_side_name)) + 
    geom_bar(aes(), position = "dodge") +
    # Y axis in logarithmic
    scale_y_log10(breaks = trans_breaks("log10", function(x) 10^x), # Set scales to be 10^n
              labels = trans_format("log10", math_format(10^.x))) # Labels
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->
<!----------------------------------------------------------------------------->

Now, choose two of the following tasks.

1.  Produce a new plot that reorders a factor in your original plot,
    using the `forcats` package (3 points). Then, in a sentence or two,
    briefly explain why you chose this ordering (1 point here for
    demonstrating understanding of the reordering, and 1 point for
    demonstrating some justification for the reordering, which could be
    subtle or speculative.)

2.  Produce a new plot that groups some factor levels together into an
    “other” category (or something similar), using the `forcats` package
    (3 points). Then, in a sentence or two, briefly explain why you
    chose this grouping (1 point here for demonstrating understanding of
    the grouping, and 1 point for demonstrating some justification for
    the grouping, which could be subtle or speculative.)

3.  If your data has some sort of time-based column like a date (but
    something more granular than just a year): mei

    1.  Make a new column that uses a function from the `lubridate` or
        `tsibble` package to modify your original time-based column. (3
        points)

        -   Note that you might first have to *make* a time-based column
            using a function like `ymd()`, but this doesn’t count.
        -   Examples of something you might do here: extract the day of
            the year from a date, or extract the weekday, or let 24
            hours elapse on your dates.

    2.  Then, in a sentence or two, explain how your new column might be
        useful in exploring a research question. (1 point for
        demonstrating understanding of the function you used, and 1
        point for your justification, which could be subtle or
        speculative).

        -   For example, you could say something like “Investigating the
            day of the week might be insightful because penguins don’t
            work on weekends, and so may respond differently”.

<!-------------------------- Start your work below ---------------------------->

**Task Number**: 2

``` r
answer <- version1.0 %>%
    mutate(street_side = fct_infreq(as.factor(street_side_name)))
answer
```

    ## # A tibble: 70,063 × 18
    ##    std_street    genus…¹ speci…² culti…³ commo…⁴ assig…⁵ root_…⁶ plant…⁷ on_st…⁸
    ##    <chr>         <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1 W 58TH AV     ULMUS   AMERIC… BRANDON BRANDO… N       N       N       W 58TH…
    ##  2 W 58TH AV     ZELKOVA SERRATA <NA>    JAPANE… N       N       N       W 58TH…
    ##  3 WINDSOR ST    STYRAX  JAPONI… <NA>    JAPANE… N       N       4       WINDSO…
    ##  4 E 39TH AV     FRAXIN… AMERIC… AUTUMN… AUTUMN… Y       N       4       E 39TH…
    ##  5 WINDSOR ST    ACER    CAMPES… <NA>    HEDGE … N       N       4       WINDSO…
    ##  6 SHERBROOKE ST ACER    PLATAN… COLUMN… COLUMN… N       N       6       SHERBR…
    ##  7 SHERBROOKE ST ACER    PLATAN… COLUMN… COLUMN… N       N       6       SHERBR…
    ##  8 SHERBROOKE ST ACER    PLATAN… COLUMN… COLUMN… N       N       3       SHERBR…
    ##  9 E 39TH AV     FRAXIN… AMERIC… AUTUMN… AUTUMN… N       N       3       E 39TH…
    ## 10 E 39TH AV     TILIA   EUCHLO… <NA>    CRIMEA… N       N       5       E 39TH…
    ## # … with 70,053 more rows, 9 more variables: neighbourhood_name <chr>,
    ## #   street_side_name <chr>, height_range_id <dbl>, diameter <dbl>,
    ## #   date_planted <date>, longitude <dbl>, latitude <dbl>,
    ## #   curb_occurrence <fct>, street_side <fct>, and abbreviated variable names
    ## #   ¹​genus_name, ²​species_name, ³​cultivar_name, ⁴​common_name, ⁵​assigned,
    ## #   ⁶​root_barrier, ⁷​plant_area, ⁸​on_street

Explanation1: fct_infreq() is used here to order street_side factor by
the frequency of its occurrence in a group hasCurb/noCurb, from the
least to the most.

``` r
ggplot(answer, aes(x = curb_occurrence, fill = street_side)) + 
    geom_bar(aes(), position = "dodge") +
    # Y axis in logarithmic
    scale_y_log10(breaks = trans_breaks("log10", function(x) 10^x), # Set scales to be 10^n
              labels = trans_format("log10", math_format(10^.x))) # Labels
```

![](mini-project-2_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

Explanation2: After we use fct_infreq(), now this diagram orders the
bars by their heights(i.e. count of trees), thus clearly shows the
comparison between total numbers of trees which occur on each type of
street side with and without a curb.
<!----------------------------------------------------------------------------->

<!-------------------------- Start your work below ---------------------------->

**Task Number**: 3

3.1:

``` r
answer3.1 <- version1.0 %>% 
    mutate(year_month_planted = floor_date(date_planted, unit="month")) %>% # remove date in date_planted
    mutate(month_planted = format(year_month_planted, "%m"))# extract month from date_planted
answer3.1
```

    ## # A tibble: 70,063 × 19
    ##    std_street    genus…¹ speci…² culti…³ commo…⁴ assig…⁵ root_…⁶ plant…⁷ on_st…⁸
    ##    <chr>         <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1 W 58TH AV     ULMUS   AMERIC… BRANDON BRANDO… N       N       N       W 58TH…
    ##  2 W 58TH AV     ZELKOVA SERRATA <NA>    JAPANE… N       N       N       W 58TH…
    ##  3 WINDSOR ST    STYRAX  JAPONI… <NA>    JAPANE… N       N       4       WINDSO…
    ##  4 E 39TH AV     FRAXIN… AMERIC… AUTUMN… AUTUMN… Y       N       4       E 39TH…
    ##  5 WINDSOR ST    ACER    CAMPES… <NA>    HEDGE … N       N       4       WINDSO…
    ##  6 SHERBROOKE ST ACER    PLATAN… COLUMN… COLUMN… N       N       6       SHERBR…
    ##  7 SHERBROOKE ST ACER    PLATAN… COLUMN… COLUMN… N       N       6       SHERBR…
    ##  8 SHERBROOKE ST ACER    PLATAN… COLUMN… COLUMN… N       N       3       SHERBR…
    ##  9 E 39TH AV     FRAXIN… AMERIC… AUTUMN… AUTUMN… N       N       3       E 39TH…
    ## 10 E 39TH AV     TILIA   EUCHLO… <NA>    CRIMEA… N       N       5       E 39TH…
    ## # … with 70,053 more rows, 10 more variables: neighbourhood_name <chr>,
    ## #   street_side_name <chr>, height_range_id <dbl>, diameter <dbl>,
    ## #   date_planted <date>, longitude <dbl>, latitude <dbl>,
    ## #   curb_occurrence <fct>, year_month_planted <date>, month_planted <chr>, and
    ## #   abbreviated variable names ¹​genus_name, ²​species_name, ³​cultivar_name,
    ## #   ⁴​common_name, ⁵​assigned, ⁶​root_barrier, ⁷​plant_area, ⁸​on_street

3.2

This is useful as we can further aggregate data by each **month** (or
**year** by simply changing the floor unit to be ‘year’) in every year,
so that we can see in general in which months trees are planted more
often regardless of the year.
<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 2.0 (no points)

Pick a research question, and pick a variable of interest (we’ll call it
“Y”) that’s relevant to the research question. Indicate these.

<!-------------------------- Start your work below ---------------------------->

**Because two of the research questions I picked in Task 1 both doesn’t
involve two numeric variable at the same time, I pick another one from
the four questions in milestone 1.**

**Research Question**: “What is the distribution of diameters of trees?”
(In this task, more specifically, what is the distribution of diameter
over height_range_id)?

**Variable of interest**: diameter

<!----------------------------------------------------------------------------->

## 2.1 (5 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

-   **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.

    -   You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
    -   You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
    -   You could use `lm()` to test for significance of regression.

<!-------------------------- Start your work below ---------------------------->

``` r
answer2.1 <- lm(diameter ~ height_range_id, vancouver_trees)
print(answer2.1)
```

    ## 
    ## Call:
    ## lm(formula = diameter ~ height_range_id, data = vancouver_trees)
    ## 
    ## Coefficients:
    ##     (Intercept)  height_range_id  
    ##         -0.3859           4.5208

<!----------------------------------------------------------------------------->

## 2.2 (5 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

-   Be sure to indicate in writing what you chose to produce.
-   Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
-   Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

Use augment to predict the diameters when height_range_id varies from
11-20, which doesn’t exist in the current dataset (maximum height_range:
10)

``` r
answer2.2 <- augment(answer2.1, newdata = tibble(height_range_id = 11:20))
print(answer2.2)
```

    ## # A tibble: 10 × 2
    ##    height_range_id .fitted
    ##              <int>   <dbl>
    ##  1              11    49.3
    ##  2              12    53.9
    ##  3              13    58.4
    ##  4              14    62.9
    ##  5              15    67.4
    ##  6              16    71.9
    ##  7              17    76.5
    ##  8              18    81.0
    ##  9              19    85.5
    ## 10              20    90.0

The .fitted column is the diamater prediction for
height_range_id\[11:20\].
<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 3.1 (5 points)

Take a summary table that you made from Milestone 1 (Task 4.2), and
write it as a csv file in your `output` folder. Use the `here::here()`
function.

-   **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
-   **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

``` r
vancouver_tree_curb <- vancouver_trees %>%
    group_by(street_side_name) %>%
    count(curb, street_side_name) %>% 
    mutate(percentage = prop.table(n) * 100) # Percentage of trees with(out) curbs in a type of street_side_name
vancouver_tree_curb
```

    ## # A tibble: 10 × 4
    ## # Groups:   street_side_name [6]
    ##    street_side_name curb      n percentage
    ##    <chr>            <chr> <int>      <dbl>
    ##  1 BIKE MED         Y        38    100    
    ##  2 EVEN             N      6007      8.37 
    ##  3 EVEN             Y     65746     91.6  
    ##  4 GREENWAY         N         5    100    
    ##  5 MED              N       545     16.5  
    ##  6 MED              Y      2752     83.5  
    ##  7 ODD              N      6246      8.75 
    ##  8 ODD              Y     65128     91.2  
    ##  9 PARK             N         1      0.694
    ## 10 PARK             Y       143     99.3

``` r
write_csv(vancouver_tree_curb, here("output", "vancouver_tree_curb.csv"))
```

<!----------------------------------------------------------------------------->

## 3.2 (5 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

-   The same robustness and reproducibility criteria as in 3.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->

Save the model.

``` r
saveRDS(vancouver_tree_curb, file = here("output", "vancouver_tree_curb.rds"))
```

load the model.

``` r
vancouver_tree_curb_loaded <- readRDS(here("output", "vancouver_tree_curb.rds"))
vancouver_tree_curb_loaded
```

    ## # A tibble: 10 × 4
    ## # Groups:   street_side_name [6]
    ##    street_side_name curb      n percentage
    ##    <chr>            <chr> <int>      <dbl>
    ##  1 BIKE MED         Y        38    100    
    ##  2 EVEN             N      6007      8.37 
    ##  3 EVEN             Y     65746     91.6  
    ##  4 GREENWAY         N         5    100    
    ##  5 MED              N       545     16.5  
    ##  6 MED              Y      2752     83.5  
    ##  7 ODD              N      6246      8.75 
    ##  8 ODD              Y     65128     91.2  
    ##  9 PARK             N         1      0.694
    ## 10 PARK             Y       143     99.3

<!----------------------------------------------------------------------------->

# Tidy Repository

Now that this is your last milestone, your entire project repository
should be organized. Here are the criteria we’re looking for.

## Main README (3 points)

There should be a file named `README.md` at the top level of your
repository. Its contents should automatically appear when you visit the
repository on GitHub.

Minimum contents of the README file:

-   In a sentence or two, explains what this repository is, so that
    future-you or someone else stumbling on your repository can be
    oriented to the repository.
-   In a sentence or two (or more??), briefly explains how to engage
    with the repository. You can assume the person reading knows the
    material from STAT 545A. Basically, if a visitor to your repository
    wants to explore your project, what should they know?

Once you get in the habit of making README files, and seeing more README
files in other projects, you’ll wonder how you ever got by without them!
They are tremendously helpful.

## File and Folder structure (3 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (2 points)

All output is recent and relevant:

-   All Rmd files have been `knit`ted to their output, and all data
    files saved from Task 4 above appear in the `output` folder.
-   All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
-   There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

PS: there’s a way where you can run all project code using a single
command, instead of clicking “knit” three times. More on this in STAT
545B!

## Error-free code (1 point)

This Milestone 1 document knits error-free, and the Milestone 2 document
knits error-free.

Plots failing to show up on Github in the .md counts as an error here.
So does the entire .md failing to show up on Github in the .md (“Sorry
about that, but we can’t show files that are this big right now”).

## Tagged release (1 point)

You’ve tagged a release for Milestone 1, and you’ve tagged a release for
Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
