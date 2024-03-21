# Recipe Data Analytics
**Author**: Daniel Warren
## Introduction
On the website [food.com](https://www.food.com/) any given recipe has a number of elements that may impact its overall rating.
In this project **I'd like to examine how the number of steps and expected time in a recipe contribute to its rating**.
The dataset I chose to use for this analysis is a subset of recipes and ratings scraped from food.com, originally scraped by the authors of [this paper](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf).
This question would be useful to any food.com recipe creator as it could be used to optimize their recipes for higher average reviews.
Our dataset is initially split into two parts, the `RAW_recipes.csv` which contains 83,782 rows and 12 columns, and the `RAW_interactions.csv` which contains 731,927 rows and 5 columns. 
Both of these files are available for download from this github repository in the `data` folder.

The columns present in the `RAW_recipes.csv` file are as follows:

| Column | Description |
|------|-----------|
|`name` | Recipe name |
|`id` | Recipe ID | 
|`minutes` | Minutes to prepare recipe |
|`contributor_id` | User ID who submitted this recipe |
|`submitted` | Date recipe was submitted | 
|`tags` | Food.com tags for recipe |
|`nutrition` | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
|`n_steps` | Number of steps in recipe |
|`steps` | Text for recipe steps, in order |
|`description` | User-provided description |

The columns present in the `RAW_interactions.csv` file are as follows:

| Column | Description |
|---|---|
| `user_id` | User ID |
| `recipe_id` | Recipe ID |
| `date` | Date of interaction |
| `rating` | Rating given |
| `review` | Review text |

The columns relevent to our specific hypothesis are `minutes`, `n_steps`, and `rating`.

## Data Cleaning and Exploratory Data Analysis
### Data Cleaning

To clean the data I start by replacing the 0 values for `rating` in the `raw_interactions` with `np.NaN`, as there is no option to rate a recipe 0 on food.com. This is exemplified in the website itself and also in the text of some reviews present in the dataset, where users claim they "would rate 0 starts if it was possible".
Next, since some of the values in `recipes_raw` are strings that resemble python lists I use `.apply(ast.literal_eval)` to process them from strings into true python lists. This modification was done to the `tags`, `steps`, `ingredients`, and `nutrition` columns.
I then merged the two dataframes together, leaving us one dataframe containing all the relevant information.
I then collected the average rating of each individual recipe and saved it to each recipe.
Finally, I unrolled the nutrition information into individual columns as this would make our prediction step easier.

Here is a preview of some of the more important columns after the data cleaning process.
Note that the rows that appear to be duplicates contain different information from each review.

| name                                 |   rating |   avg_rating |   n_steps |   minutes |
|:-------------------------------------|---------:|-------------:|----------:|----------:|
| 1 brownies in the world    best ever |        4 |            4 |        10 |        40 |
| 1 in canada chocolate chip cookies   |        5 |            5 |        12 |        45 |
| 412 broccoli casserole               |        5 |            5 |         6 |        40 |
| 412 broccoli casserole               |        5 |            5 |         6 |        40 |
| 412 broccoli casserole               |        5 |            5 |         6 |        40 |

### Univariate Analysis
Here is a bar chart showing the distribution of the number of steps across all the recipes.
<iframe
  src="assets/step-distribution.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that the graph is roughly normally distributed, with its center at 7. There are extreme outliers in this distribution with 75 recipes having a count of 40 steps.

### Bivariate Analysis

Here is a scatter plot of `avg_rating` and `n_steps` columns

<iframe
  src="assets/rating-steps-scatter.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

## Assessment of Missingness
## Hypothesis Testing
## Framing a Prediction Problem
## Baseline Model
## Final Model
## Fairness Analysis