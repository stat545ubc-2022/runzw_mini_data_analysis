Mini Data-Analysis Deliverable 1
================

# Welcome to your (maybe) first-ever data analysis project!

And hopefully the first of many. Let’s get started:

1.  Install the [`datateachr`](https://github.com/UBC-MDS/datateachr)
    package by typing the following into your **R terminal**:

<!-- -->

    install.packages("devtools")
    devtools::install_github("UBC-MDS/datateachr")

2.  Load the packages below.

``` r
library(datateachr)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6      ✔ purrr   0.3.4 
    ## ✔ tibble  3.1.8      ✔ dplyr   1.0.10
    ## ✔ tidyr   1.2.1      ✔ stringr 1.4.1 
    ## ✔ readr   2.1.2      ✔ forcats 0.5.2 
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(lubridate)
```

    ## 
    ## Attaching package: 'lubridate'
    ## 
    ## The following objects are masked from 'package:base':
    ## 
    ##     date, intersect, setdiff, union

``` r
library(scales)
```

    ## 
    ## Attaching package: 'scales'
    ## 
    ## The following object is masked from 'package:purrr':
    ## 
    ##     discard
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     col_factor

3.  Make a repository in the <https://github.com/stat545ubc-2022>
    Organization. You will be working with this repository for the
    entire data analysis project. You can either make it public, or make
    it private and add the TA’s and Lucy as collaborators. A link to
    help you create a private repository is available on the
    \#collaborative-project Slack channel.

# Instructions

## For Both Milestones

-   Each milestone is worth 45 points. The number of points allocated to
    each task will be annotated within each deliverable. Tasks that are
    more challenging will often be allocated more points.

-   10 points will be allocated to the reproducibility, cleanliness, and
    coherence of the overall analysis. While the two milestones will be
    submitted as independent deliverables, the analysis itself is a
    continuum - think of it as two chapters to a story. Each chapter, or
    in this case, portion of your analysis, should be easily followed
    through by someone unfamiliar with the content.
    [Here](https://swcarpentry.github.io/r-novice-inflammation/06-best-practices-R/)
    is a good resource for what constitutes “good code”. Learning good
    coding practices early in your career will save you hassle later on!

## For Milestone 1

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-1.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work below --->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to the mini-analysis GitHub repository you made
earlier, and tag a release on GitHub. Then, submit a link to your tagged
release on canvas.

**Points**: This milestone is worth 45 points: 43 for your analysis, 1
point for having your Milestone 1 document knit error-free, and 1 point
for tagging your release on Github.

# Learning Objectives

By the end of this milestone, you should:

-   Become familiar with your dataset of choosing
-   Select 4 questions that you would like to answer with your data
-   Generate a reproducible and clear report using R Markdown
-   Become familiar with manipulating and summarizing your data in
    tibbles using `dplyr`, with a research question in mind.

# Task 1: Choose your favorite dataset (10 points)

The `datateachr` package by Hayley Boyce and Jordan Bourak currently
composed of 7 semi-tidy datasets for educational purposes. Here is a
brief description of each dataset:

-   *apt_buildings*: Acquired courtesy of The City of Toronto’s Open
    Data Portal. It currently has 3455 rows and 37 columns.

-   *building_permits*: Acquired courtesy of The City of Vancouver’s
    Open Data Portal. It currently has 20680 rows and 14 columns.

-   *cancer_sample*: Acquired courtesy of UCI Machine Learning
    Repository. It currently has 569 rows and 32 columns.

-   *flow_sample*: Acquired courtesy of The Government of Canada’s
    Historical Hydrometric Database. It currently has 218 rows and 7
    columns.

-   *parking_meters*: Acquired courtesy of The City of Vancouver’s Open
    Data Portal. It currently has 10032 rows and 22 columns.

-   *steam_games*: Acquired courtesy of Kaggle. It currently has 40833
    rows and 21 columns.

-   *vancouver_trees*: Acquired courtesy of The City of Vancouver’s Open
    Data Portal. It currently has 146611 rows and 20 columns.

**Things to keep in mind**

-   We hope that this project will serve as practice for carrying our
    your own *independent* data analysis. Remember to comment your code,
    be explicit about what you are doing, and write notes in this
    markdown document when you feel that context is required. As you
    advance in the project, prompts and hints to do this will be
    diminished - it’ll be up to you!

-   Before choosing a dataset, you should always keep in mind **your
    goal**, or in other ways, *what you wish to achieve with this data*.
    This mini data-analysis project focuses on *data wrangling*,
    *tidying*, and *visualization*. In short, it’s a way for you to get
    your feet wet with exploring data on your own.

And that is exactly the first thing that you will do!

1.1 Out of the 7 datasets available in the `datateachr` package, choose
**4** that appeal to you based on their description. Write your choices
below:

**Note**: We encourage you to use the ones in the `datateachr` package,
but if you have a dataset that you’d really like to use, you can include
it here. But, please check with a member of the teaching team to see
whether the dataset is of appropriate complexity. Also, include a
**brief** description of the dataset here to help the teaching team
understand your data.

<!-------------------------- Start your work below ---------------------------->

1: apt_buildings 2: building_permits 3: steam_games 4: vancouver_trees

<!----------------------------------------------------------------------------->

1.2 One way to narrowing down your selection is to *explore* the
datasets. Use your knowledge of dplyr to find out at least *3*
attributes about each of these datasets (an attribute is something such
as number of rows, variables, class type…). The goal here is to have an
idea of *what the data looks like*.

*Hint:* This is one of those times when you should think about the
cleanliness of your analysis. I added a single code chunk for you below,
but do you want to use more than one? Would you like to write more
comments outside of the code chunk?

<!-------------------------- Start your work below ---------------------------->

``` r
### EXPLORE HERE ###
head(vancouver_trees)
```

    ## # A tibble: 6 × 20
    ##   tree_id civic_number std_str…¹ genus…² speci…³ culti…⁴ commo…⁵ assig…⁶ root_…⁷
    ##     <dbl>        <dbl> <chr>     <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ## 1  149556          494 W 58TH AV ULMUS   AMERIC… BRANDON BRANDO… N       N      
    ## 2  149563          450 W 58TH AV ZELKOVA SERRATA <NA>    JAPANE… N       N      
    ## 3  149579         4994 WINDSOR … STYRAX  JAPONI… <NA>    JAPANE… N       N      
    ## 4  149590          858 E 39TH AV FRAXIN… AMERIC… AUTUMN… AUTUMN… Y       N      
    ## 5  149604         5032 WINDSOR … ACER    CAMPES… <NA>    HEDGE … N       N      
    ## 6  149616          585 W 61ST AV PYRUS   CALLER… CHANTI… CHANTI… N       N      
    ## # … with 11 more variables: plant_area <chr>, on_street_block <dbl>,
    ## #   on_street <chr>, neighbourhood_name <chr>, street_side_name <chr>,
    ## #   height_range_id <dbl>, diameter <dbl>, curb <chr>, date_planted <date>,
    ## #   longitude <dbl>, latitude <dbl>, and abbreviated variable names
    ## #   ¹​std_street, ²​genus_name, ³​species_name, ⁴​cultivar_name, ⁵​common_name,
    ## #   ⁶​assigned, ⁷​root_barrier

``` r
head(steam_games)
```

    ## # A tibble: 6 × 21
    ##      id url          types name  desc_…¹ recen…² all_r…³ relea…⁴ devel…⁵ publi…⁶
    ##   <dbl> <chr>        <chr> <chr> <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ## 1     1 https://sto… app   DOOM  Now in… Very P… Very P… May 12… id Sof… Bethes…
    ## 2     2 https://sto… app   PLAY… PLAYER… Mixed,… Mixed,… Dec 21… PUBG C… PUBG C…
    ## 3     3 https://sto… app   BATT… Take c… Mixed,… Mostly… Apr 24… Harebr… Parado…
    ## 4     4 https://sto… app   DayZ  The po… Mixed,… Mixed,… Dec 13… Bohemi… Bohemi…
    ## 5     5 https://sto… app   EVE … EVE On… Mixed,… Mostly… May 6,… CCP     CCP,CCP
    ## 6     6 https://sto… bund… Gran… Grand … NaN     NaN     NaN     Rockst… Rockst…
    ## # … with 11 more variables: popular_tags <chr>, game_details <chr>,
    ## #   languages <chr>, achievements <dbl>, genre <chr>, game_description <chr>,
    ## #   mature_content <chr>, minimum_requirements <chr>,
    ## #   recommended_requirements <chr>, original_price <dbl>, discount_price <dbl>,
    ## #   and abbreviated variable names ¹​desc_snippet, ²​recent_reviews,
    ## #   ³​all_reviews, ⁴​release_date, ⁵​developer, ⁶​publisher

<!----------------------------------------------------------------------------->

1.3 Now that you’ve explored the 4 datasets that you were initially most
interested in, let’s narrow it down to 2. What lead you to choose these
2? Briefly explain your choices below, and feel free to include any code
in your explanation.

<!-------------------------- Start your work below ---------------------------->

**vancouver_trees** and **steam_games**

These two datasets have various types of data (numeric, categorical,
string), unlike cancer_sample which only has numeric one or apt_building
in which most of the data are boolean values.
<!----------------------------------------------------------------------------->

1.4 Time for the final decision! Going back to the beginning, it’s
important to have an *end goal* in mind. For example, if I had chosen
the `titanic` dataset for my project, I might’ve wanted to explore the
relationship between survival and other variables. Try to think of 1
research question that you would want to answer with each dataset. Note
them down below, and make your final choice based on what seems more
interesting to you!

<!-------------------------- Start your work below ---------------------------->

**vancouver_trees**
<!----------------------------------------------------------------------------->

# Important note

Read Tasks 2 and 3 *fully* before starting to complete either of them.
Probably also a good point to grab a coffee to get ready for the fun
part!

This project is semi-guided, but meant to be *independent*. For this
reason, you will complete tasks 2 and 3 below (under the **START HERE**
mark) as if you were writing your own exploratory data analysis report,
and this guidance never existed! Feel free to add a brief introduction
section to your project, format the document with markdown syntax as you
deem appropriate, and structure the analysis as you deem appropriate.
Remember, marks will be awarded for completion of the 4 tasks, but 10
points of the whole project are allocated to a reproducible and clean
analysis. If you feel lost, you can find a sample data analysis
[here](https://www.kaggle.com/headsortails/tidy-titarnic) to have a
better idea. However, bear in mind that it is **just an example** and
you will not be required to have that level of complexity in your
project.

# Task 2: Exploring your dataset (15 points)

If we rewind and go back to the learning objectives, you’ll see that by
the end of this deliverable, you should have formulated *4* research
questions about your data that you may want to answer during your
project. However, it may be handy to do some more exploration on your
dataset of choice before creating these questions - by looking at the
data, you may get more ideas. **Before you start this task, read all
instructions carefully until you reach START HERE under Task 3**.

2.1 Complete *4 out of the following 8 exercises* to dive deeper into
your data. All datasets are different and therefore, not all of these
tasks may make sense for your data - which is why you should only answer
*4*. Use *dplyr* and *ggplot*.

1.  Plot the distribution of a numeric variable.
2.  Create a new variable based on other variables in your data (only if
    it makes sense)
3.  Investigate how many missing values there are per variable. Can you
    find a way to plot this?
4.  Explore the relationship between 2 variables in a plot.
5.  Filter observations in your data according to your own criteria.
    Think of what you’d like to explore - again, if this was the
    `titanic` dataset, I may want to narrow my search down to passengers
    born in a particular year…
6.  Use a boxplot to look at the frequency of different observations
    within a single variable. You can do this for more than one variable
    if you wish!
7.  Make a new tibble with a subset of your data, with variables and
    observations that you are interested in exploring.
8.  Use a density plot to explore any of your variables (that are
    suitable for this type of plot).

2.2 For each of the 4 exercises that you complete, provide a *brief
explanation* of why you chose that exercise in relation to your data (in
other words, why does it make sense to do that?), and sufficient
comments for a reader to understand your reasoning and code.

<!-------------------------- Start your work below ---------------------------->

**1. Plot the distribution of a numeric variable.**

``` r
### Plot a bar chart to show the distribution of tree height range.
### This the most basic investigate height distribution, in the future more filter can be added to make a more specific one (height distribution in different neighborhood/genus/street)
height_range_distribution <- ggplot(vancouver_trees, aes(height_range_id)) + 
    geom_bar() +
    scale_x_continuous(breaks = seq(0, 10, 1))
height_range_distribution
```

![](mini_data_analysis_1_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

We can see that most trees are between height range \[1,4\]

**4. Explore the relationship between 2 variables in a plot.**

``` r
### Discover the relationship between root_barrier and diameter: do trees with large diameters more often require a root barrier? Assume we don't have any knowledge about trees, this is an example of exploring in what scenario we should install a root barrier.
diameter_root_barrier <- ggplot(vancouver_trees, aes(root_barrier, diameter)) +
    geom_boxplot(width = 0.1)
diameter_root_barrier
```

![](mini_data_analysis_1_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

From the graph, we can see that there’s no relationship between root
barrier and diameter when diameters are smaller, while with the increase
in diameter, installing a root barrier is more common.

**7. Make a new tibble with a subset of your data, with variables and
observations that you are interested in exploring.**

``` r
### The tibble shows trees that are located in the middle of the road with a curb
### This is an example of getting the characteristics of trees planted in the middle with a curb
trees_middle_curb <- vancouver_trees %>%
    filter(curb == "Y", street_side_name == "Med") %>%
    select(tree_id, civic_number, std_street, genus_name, species_name)
head(trees_middle_curb)
```

    ## # A tibble: 0 × 5
    ## # … with 5 variables: tree_id <dbl>, civic_number <dbl>, std_street <chr>,
    ## #   genus_name <chr>, species_name <chr>

An interesting finding is that all trees in the middle of a road do not
have a curb!

**8. Use a density plot to explore any of your variables (that are
suitable for this type of plot).**

``` r
### Use a ridge plot to plot the density of heights across different neighbourhood.
height_range_density <- ggplot(vancouver_trees, aes(height_range_id, neighbourhood_name)) +
    ggridges::geom_density_ridges() +
    scale_x_continuous(breaks = seq(0, 10, 1))
height_range_density
```

    ## Picking joint bandwidth of 0.214

![](mini_data_analysis_1_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->

Higher trees are most likely to be located in west point grey, kitslano,
and dunbar-southland. These areas are all parts of Vancouver west.
<!----------------------------------------------------------------------------->

# Task 3: Write your research questions (5 points)

So far, you have chosen a dataset and gotten familiar with it through
exploring the data. Now it’s time to figure out 4 research questions
that you would like to answer with your data! Write the 4 questions and
any additional comments at the end of this deliverable. These questions
are not necessarily set in stone - TAs will review them and give you
feedback; therefore, you may choose to pursue them as they are for the
rest of the project, or make modifications!

<!--- *****START HERE***** --->

1.  Are trees planted year round?
2.  How do heights of trees of the same genus vary in Vancouver?
3.  Is there a pattern where curbs occur?
4.  What is the distribution of diameters of trees
    <!---------------------------->

# Task 4: Process and summarize your data (13 points)

From Task 2, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (10 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Based on two categorical variables, calculate two summary statistics
    of your choosing.

**Graphing:**

5.  Create a graph out of summarized variables that has at least two
    geom layers.
6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.
8.  Create 3 histograms out of summarized variables, with each histogram
    having different sized bins. Pick the “best” one and explain why it
    is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

**Research question 1: Are trees planted year round?**

``` r
### Summarizing operation: No.2 
answer1 <- vancouver_trees %>% 
    drop_na(date_planted) %>%  # drop observations with N/A date of planted
    mutate(year_month_planted = floor_date(date_planted, unit="month")) %>% # remove date in date_planted
    mutate(month_planted = format(year_month_planted, "%m")) %>% # extract month from date_planted
    group_by(month_planted) %>%
    count(year_month_planted, name = "treesPerYearMonth") %>% # Count the number of trees planted in the same month in all years
    summarise(trees_per_month = floor(mean(treesPerYearMonth))) # Compute the average number of trees planted each month
answer1
```

    ## # A tibble: 12 × 2
    ##    month_planted trees_per_month
    ##    <chr>                   <dbl>
    ##  1 01                        404
    ##  2 02                        432
    ##  3 03                        409
    ##  4 04                        235
    ##  5 05                         57
    ##  6 06                         24
    ##  7 07                          8
    ##  8 08                          5
    ##  9 09                          6
    ## 10 10                         96
    ## 11 11                        406
    ## 12 12                        283

``` r
### Graphing operation: No.5
ggplot(answer1, aes(month_planted, trees_per_month)) +
    geom_col() +
    scale_y_continuous(breaks = seq(0, 450, 50)) +
    geom_text(aes(label = trees_per_month), vjust = -0.5)
```

![](mini_data_analysis_1_files/figure-gfm/unnamed-chunk-7-1.png)<!-- -->

**Research question 2: How do heights of trees of the same genus vary in
Vancouver?**

``` r
trees_count <- vancouver_trees %>%
    group_by(genus_name) %>%
    count(genus_name, name = "tree_count") %>% # Count statistics
    arrange(desc(tree_count))
trees_count
```

    ## # A tibble: 97 × 2
    ## # Groups:   genus_name [97]
    ##    genus_name tree_count
    ##    <chr>           <int>
    ##  1 ACER            36062
    ##  2 PRUNUS          30683
    ##  3 FRAXINUS         7381
    ##  4 TILIA            6773
    ##  5 QUERCUS          6119
    ##  6 CARPINUS         5806
    ##  7 FAGUS            4808
    ##  8 MALUS            4173
    ##  9 MAGNOLIA         3899
    ## 10 CRATAEGUS        3864
    ## # … with 87 more rows

``` r
# Choose the top 2 genus with the most trees (Make sure there are enought samples)
genuses <- c("ACER", "PRUNUS")

vancouver_trees_sample <- vancouver_trees %>%
    filter(genus_name %in% genuses)
vancouver_trees_sample
```

    ## # A tibble: 66,745 × 20
    ##    tree_id civic_number std_st…¹ genus…² speci…³ culti…⁴ commo…⁵ assig…⁶ root_…⁷
    ##      <dbl>        <dbl> <chr>    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1  149604         5032 WINDSOR… ACER    CAMPES… <NA>    HEDGE … N       N      
    ##  2  149617         4909 SHERBRO… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ##  3  149618         4925 SHERBRO… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ##  4  149619         4969 SHERBRO… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ##  5  149640         6968 SELKIRK… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ##  6  149680         5311 WINDSOR… ACER    CAMPES… <NA>    HEDGE … N       N      
    ##  7  149683         7011 SELKIRK… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ##  8  149684         1223 W 54TH … ACER    PLATAN… COLUMN… COLUMN… N       N      
    ##  9  149686         5007 WINDSOR… ACER    CAMPES… <NA>    HEDGE … N       N      
    ## 10  149694         1292 W 49TH … ACER    PLATAN… COLUMN… COLUMN… N       N      
    ## # … with 66,735 more rows, 11 more variables: plant_area <chr>,
    ## #   on_street_block <dbl>, on_street <chr>, neighbourhood_name <chr>,
    ## #   street_side_name <chr>, height_range_id <dbl>, diameter <dbl>, curb <chr>,
    ## #   date_planted <date>, longitude <dbl>, latitude <dbl>, and abbreviated
    ## #   variable names ¹​std_street, ²​genus_name, ³​species_name, ⁴​cultivar_name,
    ## #   ⁵​common_name, ⁶​assigned, ⁷​root_barrier

``` r
# Plot the density of heights in ACER and PRUNUS
ggplot(vancouver_trees_sample, aes(x = height_range_id)) +
    geom_density(aes(colour = genus_name, fill = genus_name), alpha = 0.1) # Alpha transparency is applied
```

![](mini_data_analysis_1_files/figure-gfm/unnamed-chunk-8-1.png)<!-- -->

``` r
vancouver_trees_summary_stat <- vancouver_trees_sample %>%
    group_by(genus_name) %>%
    summarise(average_height = mean(height_range_id, na.rm = TRUE), # Mean statistics
              min_height = min(height_range_id, na.rm = TRUE), max_height = max(height_range_id, na.rm = TRUE),# Range statistics
              standard_deviation = sd(height_range_id, na.rm = TRUE) # Standard deviation statistics
              ) 
vancouver_trees_summary_stat
```

    ## # A tibble: 2 × 5
    ##   genus_name average_height min_height max_height standard_deviation
    ##   <chr>               <dbl>      <dbl>      <dbl>              <dbl>
    ## 1 ACER                 2.73          0         10              1.61 
    ## 2 PRUNUS               2.23          0          9              0.842

**Research question 3: Is there a pattern where curbs occur?**

``` r
curb_location <- vancouver_trees %>%
    group_by(street_side_name) %>%
    count(curb, street_side_name) %>% 
    mutate(percentage = prop.table(n) * 100) # Percentage of trees with(out) curbs in a type of street_side_name
curb_location
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
ggplot(vancouver_trees, aes(x = curb, fill = street_side_name)) + 
    geom_bar(aes(), position = "dodge") +
    # Y axis in logarithmic
    scale_y_log10(breaks = trans_breaks("log10", function(x) 10^x), # Set scales to be 10^n
              labels = trans_format("log10", math_format(10^.x))) # Labels
```

![](mini_data_analysis_1_files/figure-gfm/unnamed-chunk-9-1.png)<!-- -->

**Research question 4: What is the distribution of diameters of trees?**

``` r
vancouver_trees_width <- vancouver_trees %>%
    # Create a categorical variable named width with 3 groups from the existing numerical variable diameter
    mutate(width=cut(diameter, breaks=c(0, 100, 200, 300), labels=c("thin","median","thick"))) 
vancouver_trees_width
```

    ## # A tibble: 146,611 × 21
    ##    tree_id civic_number std_st…¹ genus…² speci…³ culti…⁴ commo…⁵ assig…⁶ root_…⁷
    ##      <dbl>        <dbl> <chr>    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1  149556          494 W 58TH … ULMUS   AMERIC… BRANDON BRANDO… N       N      
    ##  2  149563          450 W 58TH … ZELKOVA SERRATA <NA>    JAPANE… N       N      
    ##  3  149579         4994 WINDSOR… STYRAX  JAPONI… <NA>    JAPANE… N       N      
    ##  4  149590          858 E 39TH … FRAXIN… AMERIC… AUTUMN… AUTUMN… Y       N      
    ##  5  149604         5032 WINDSOR… ACER    CAMPES… <NA>    HEDGE … N       N      
    ##  6  149616          585 W 61ST … PYRUS   CALLER… CHANTI… CHANTI… N       N      
    ##  7  149617         4909 SHERBRO… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ##  8  149618         4925 SHERBRO… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ##  9  149619         4969 SHERBRO… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ## 10  149625          720 E 39TH … FRAXIN… AMERIC… AUTUMN… AUTUMN… N       N      
    ## # … with 146,601 more rows, 12 more variables: plant_area <chr>,
    ## #   on_street_block <dbl>, on_street <chr>, neighbourhood_name <chr>,
    ## #   street_side_name <chr>, height_range_id <dbl>, diameter <dbl>, curb <chr>,
    ## #   date_planted <date>, longitude <dbl>, latitude <dbl>, width <fct>, and
    ## #   abbreviated variable names ¹​std_street, ²​genus_name, ³​species_name,
    ## #   ⁴​cultivar_name, ⁵​common_name, ⁶​assigned, ⁷​root_barrier

``` r
# Only use trees of which the width are less than 100 inch
vancouver_thin_trees <- vancouver_trees_width %>%
    filter(width == "thin")
vancouver_thin_trees
```

    ## # A tibble: 146,509 × 21
    ##    tree_id civic_number std_st…¹ genus…² speci…³ culti…⁴ commo…⁵ assig…⁶ root_…⁷
    ##      <dbl>        <dbl> <chr>    <chr>   <chr>   <chr>   <chr>   <chr>   <chr>  
    ##  1  149556          494 W 58TH … ULMUS   AMERIC… BRANDON BRANDO… N       N      
    ##  2  149563          450 W 58TH … ZELKOVA SERRATA <NA>    JAPANE… N       N      
    ##  3  149579         4994 WINDSOR… STYRAX  JAPONI… <NA>    JAPANE… N       N      
    ##  4  149590          858 E 39TH … FRAXIN… AMERIC… AUTUMN… AUTUMN… Y       N      
    ##  5  149604         5032 WINDSOR… ACER    CAMPES… <NA>    HEDGE … N       N      
    ##  6  149616          585 W 61ST … PYRUS   CALLER… CHANTI… CHANTI… N       N      
    ##  7  149617         4909 SHERBRO… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ##  8  149618         4925 SHERBRO… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ##  9  149619         4969 SHERBRO… ACER    PLATAN… COLUMN… COLUMN… N       N      
    ## 10  149625          720 E 39TH … FRAXIN… AMERIC… AUTUMN… AUTUMN… N       N      
    ## # … with 146,499 more rows, 12 more variables: plant_area <chr>,
    ## #   on_street_block <dbl>, on_street <chr>, neighbourhood_name <chr>,
    ## #   street_side_name <chr>, height_range_id <dbl>, diameter <dbl>, curb <chr>,
    ## #   date_planted <date>, longitude <dbl>, latitude <dbl>, width <fct>, and
    ## #   abbreviated variable names ¹​std_street, ²​genus_name, ³​species_name,
    ## #   ⁴​cultivar_name, ⁵​common_name, ⁶​assigned, ⁷​root_barrier

``` r
# Three histogram in different bindwidth
ggplot(vancouver_thin_trees, aes(diameter, y = ..density..)) +
   geom_histogram(binwidth = 5)
```

![](mini_data_analysis_1_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

``` r
ggplot(vancouver_thin_trees, aes(diameter, y = ..density..)) +
   geom_histogram(binwidth = 10)
```

![](mini_data_analysis_1_files/figure-gfm/unnamed-chunk-10-2.png)<!-- -->

``` r
ggplot(vancouver_thin_trees, aes(diameter, y = ..density..)) +
   geom_histogram(binwidth = 20)
```

![](mini_data_analysis_1_files/figure-gfm/unnamed-chunk-10-3.png)<!-- -->

Among three histograms, it shows the pattern when bindwidth is 5, where
you can clearly see a steady decrease in the occurrence when diameters
increase.
<!----------------------------------------------------------------------------->

### 1.2 (3 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!-------------------------- Start your work below ---------------------------->

**Question 1**: The graph shows an interesting fact that most trees are
planted during late fall to early spring (during November to next
March), while only very few are planted during summer season. This
research can further investigate if every genus of trees follow the same
pattern.

**Question 2**: The density graph shows that even if they are of the
same genus, trees may grow into different heights, and that the
deviation in height of PRUNUS is half as that of ACER.

**Question 3**: From the graph, we can see that trees without curb only
occur on greenway and never exist on bike way while those with curbs
appear anywhere except on greenway. Also, on streets (no matter which
side) and in parks, there are more trees with curbs than those without.
The research does not show the relationship between curbs and other
factors which may also affect whether or not a tree has a curb.

**Question 4**: Despite their genus, most trees have a diameter which is
less than 25 inches, while there is few trees of which the diameters are
above 75 inches.
<!----------------------------------------------------------------------------->

### Attribution

Thanks to Icíar Fernández Boyano for mostly putting this together, and
Vincenzo Coia for launching.
