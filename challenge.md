Las Vegas Restaurants
================
Your Name
2018-

``` r
# Libraries
library(tidyverse)

# Files
file_yelp <- ""
```

Every year, Yelp releases a public dataset of local businesses for
students to explore and use in innovative research. The data includes
information on businesses, ratings, and reviews. More information on
Yelp’s data and student challenge can be found
[here](https://www.yelp.com/dataset/challenge). Since the data changes
each year, please download the 2017 dataset on
[Box](https://stanford.box.com/s/eijybzf2mhbfrahlxlfauenxb4rgiwu0).

For this challenge you will explore businesses in Las Vegas, focusing on
restaurants and food-related businesses (e.g. bakeries, grocery stores,
etc.). You will do some preliminary EDA, then finish by creating
restaurant maps for Las Vegas (by cuisine type) and ultimately
automating this map-making process.

## Detailed data

### Download raw data

Download the raw data from
[Box](https://stanford.box.com/s/eijybzf2mhbfrahlxlfauenxb4rgiwu0). Read
in the csv file as a data frame, then use `glimpse` or `summary` to
preview the data.

### Clean data for analysis

**q1.0** Is this data exclusively for businesses in the U.S., or does it
include other countries? Create a dataframe `q1` that only contains
businesses in the U.S. (Hint: use variables `state` and `postal_code`.)

**q1.1** Are the states well-represented in this dataset, or is the data
concentrated in certain states? To answer this, print out the top 5
states with the most businesses in this dataset.

**q1.2** From Q1.1, we saw that Arizona and Nevada have the most
businesses. We will focus on businesses in Nevada - specifically,
businesses in Las Vegas. Create a new dataset, `q1.2`, that only
includes businesses in Las Vegas.

## Create restaurant data

**q2.0** We are interested in creating a restaurant map, so we want to
filter out our data further to only include food-related businesses.

From `q1.2`, create a new dataframe `q2` that only contains businesses
whose category includes the string “Restaurants” or “Food”.

**q2.1** You may have noticed that each restaurant’s `categories`
include multiple values. For example, a restaurant’s category might be
“Restaurants, American, Burgers, Fast Food”. We want to have only one
label per restaurant.

This process is a bit subjective, but for the purposes of this challenge
we will do the following. Using `q2`, create a dataframe `q2.1` using
the following steps:

  - Use the function `separate_rows()` to create a new observation for
    each label in `categories` for each restaurant. You may want to
    define the `sep =` parameter.
  - Since we have multiple observations now for the same business,
    create a new variable called `row_num` that sequentially numbers the
    rows for each individual business. `row_num` should start over at 1
    for each new business. We will use this variable later to ensure we
    end up with one observation per restaurant.
  - All food-related businesses have generic values in their
    `categories`, such as “Food” or “Restaurants”. We do **not** want
    these generic labels as our ultimate category; instead, we are
    interested in *specific* `categories` labels, such as “Mexican” or
    “Bakeries”. To solve this, remove any observations whose
    `categories` value is in the `generic_food_labels` vector defined
    below.
  - Now, to get one observation per business, keep only the minimum
    row\_num for each business (`row_num == min(row_num)`).

You can do all of this with one pipe. Don’t create multiple intermediate
datasets.

``` r
generic_food_labels <- 
  c(
    "Restaurants", 
    "Food", 
    "Food Trucks", 
    "Food Delivery Services",
    "Food Stands", 
    "Event Planning & Services", 
    "Food Court", 
    "Caterers",
    "Shopping",
    "Arts & Entertainment",
    "Car Wash",
    "Gas Stations",
    "Automotive",
    "Local Flavor",
    "Bookstores",
    "Nightlife",
    "Venues & Event Spaces",
    "Business Consulting",
    "Hotels & Travel",
    "Social Clubs",
    "Party & Event Planning",
    "Food Tours"
  )
```

## Restaurant EDA

Now we have a clean dataset of Las Vegas restaurants (q2.1) that we can
explore.

**q3.0** Is a restaurant’s rating related to its number of reviews?
Create a visualization that answers this question and interpret.

**q3.1** Create a vector called `top_15_categ` that includes the top 15
most common restaurant types in Las Vegas. What are the top 5 most
common types of restaurants in Las Vegas?

**q3.2** Using dataframe q2.1 and top\_15\_categ, create a visualization
that explores the rating distributions for each of the top 15
categories. You can either recreate [this
chart](https://imgur.com/a/ttpcdNh) or create a better one of your own\!

**q3.3** Create a visualization of your own to uncover a new insight
about restaurants in Las Vegas.

## Creating a Restaurant Map

Our Yelp data uses latitude and longitude to identify a business’s
location. We will use two R libraries, `ggmap` and `mapproj`, to create
our maps, as they use latitude and longitude data. You may want to move
the library calls below to the top of the R markdown file for style
purposes.

``` r
library(ggmap)
library(mapproj)
```

    ## Loading required package: maps

    ## 
    ## Attaching package: 'maps'

    ## The following object is masked from 'package:purrr':
    ## 
    ##     map

**q4.0** `ggmap` has pre-created maps that we can load and use. Create a
variable `lv_map` that is a map of Las Vegas (Hint: use `get_map()`,
specifying `location` as Las Vegas and `zoom` to get a good view). Use
`ggmap(lv_map)` to view the map you have created.

**q4.1** We can plot our restaurant data directly on the map of Las
Vegas by using `ggmap(lv_map) + geom_point(...)`. Create a dataframe
`q4.1` that only consists of Las Vegas “Burgers” restaurants. Plot the
points of `q4.1` over the map of Las Vegas, `lv_map`, to recreate [this
burger map](https://imgur.com/a/M8ua0cD).

## Automating the Restaurant Map

We were able to create a map for Burgers, but suppose we want to create
this same restaurant map for each of the 15 most popular restaurant
types that we identified in `top_15_categ`.

**q5.0** First, create a function called `create_food_map` that inputs a
particular cuisine type (string) and returns the map we created above
but for this specific cuisine type (`print(image)` may be helpful to
return the image). Remember to update the map’s subtitle so that it is
customized for whatever cuisine type you are entering in (e.g.
“Category: Pizza”, “Category: Mexican”).

**q5.1** Using the function you just created and `top_15_categ`, write
one line of code that will create a food map for each of the cuisine
types in `top_15_categ`. When you run this code, the output should be a
total of 15 restaurant maps, each customized for a specific cuisine
type.
