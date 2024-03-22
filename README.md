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

This scatter plot reveals a bias towards higher ratings for recipes with lower steps.

### Interesting Aggregates

The following table is a pivot comparing the mean median min and max number of steps for recipes with different number of ingredients.

|   n_ingredients |   ('mean', 'n_steps') |   ('median', 'n_steps') |   ('min', 'n_steps') |   ('max', 'n_steps') |
|----------------:|----------------------:|------------------------:|---------------------:|---------------------:|
|               1 |               6.75    |                       7 |                    2 |                   20 |
|               2 |               5.96993 |                       5 |                    1 |                   55 |
|               3 |               5.53977 |                       5 |                    1 |                   69 |
|               4 |               6.49383 |                       5 |                    1 |                   55 |
|               5 |               7.32981 |                       6 |                    1 |                   80 |
|               6 |               7.7409  |                       7 |                    1 |                   86 |
|               7 |               8.29748 |                       8 |                    1 |                   52 |
|               8 |               9.05289 |                       8 |                    1 |                   67 |
|               9 |               9.75998 |                       9 |                    1 |                   87 |
|              10 |              10.6983  |                      10 |                    1 |                   57 |
|              11 |              11.1874  |                      10 |                    1 |                   65 |
|              12 |              11.7822  |                      11 |                    1 |                   57 |
|              13 |              12.5491  |                      11 |                    1 |                   68 |
|              14 |              13.5407  |                      12 |                    1 |                   88 |
|              15 |              14.3564  |                      13 |                    2 |                   62 |
|              16 |              15.1429  |                      14 |                    2 |                   77 |
|              17 |              15.6569  |                      15 |                    1 |                   55 |
|              18 |              16.4107  |                      15 |                    3 |                   65 |
|              19 |              16.6759  |                      16 |                    3 |                   59 |
|              20 |              17.2266  |                      17 |                    2 |                   76 |
|              21 |              18.7336  |                      17 |                    3 |                   48 |
|              22 |              21.7214  |                      27 |                    4 |                   59 |
|              23 |              17.7537  |                      17 |                    5 |                   59 |
|              24 |              17.6732  |                      17 |                    4 |                   44 |
|              25 |              19.14    |                      18 |                    6 |                   45 |
|              26 |              18.9485  |                      21 |                    5 |                   49 |
|              27 |              18.0882  |                      19 |                    7 |                   34 |
|              28 |              23.9429  |                      23 |                    5 |                   76 |
|              29 |              18.5517  |                      17 |                   15 |                   33 |
|              30 |              27.9062  |                      23 |                    6 |                   62 |
|              31 |              18.9167  |                      20 |                   10 |                   29 |
|              32 |              37       |                      40 |                   28 |                   40 |
|              33 |               6       |                       6 |                    6 |                    6 |

Generally, recipes become more complex as the number of ingredients increases, but there are surprising outliers with very few or many steps for their ingredient count. This suggests that recipe complexity isn't solely determined by the number of ingredients.

## Assessment of Missingness

### NMAR Analysis
We will consider the `review` column. This column is the most likely to have NMAR missing data, since recipes with medium reviews could be correlated with missing review text. This could be due the reviewers beliving that if a recipe is straightforward enough, then it does not merit a text review.
We could attempt to make this column MAR by adding in 

### Missingness Dependency
We'll perform a dependancy test on the columns `rating` and `n_ingredients`

<iframe
  src="assets/simulated-missing.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

We can see that, with a p-value of 0.0 the `rating` column is dependant on the `n_ingredients` column.

## Hypothesis Testing
The hypotheses I plan to test are:

$H_0$: The number of steps and the expected time to complete a task have no effect on the task's rating. Any observed differences in ratings due to changes in the number of steps and expected time are purely due to chance.

$H_1$: The number of steps and the expected time to complete a task both negatively affect the task's rating. Tasks that have a higher number of steps and longer expected times to complete will have lower ratings, not due to chance.

To do this I perform a hypothesis test on both the interactions between `n_steps` and `avg_rating`, as well as `minutes` and `avg_rating`

<iframe
  src="assets/simulated-correlations-n_steps.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
<iframe
  src="assets/simulated-correlations-minutes.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

the p-values were 0.366 and 0.241, which is not enough to reject $H_0$.

## Framing a Prediction Problem

I'll predict the `calorie` count of each recipe, using only the `total fat (PDV)` and `protein (PDV)` columns. This is a regression problem. I chose to predict calories using these features since I wanted to see how much information could be squeezed out of the minimum amount of data. I'll be using both MSE and $R^2$ to evaluate my models.

## Baseline Model

The baseline model is just a simple linear regression model trained only on the `total fat (PDV)` and `protein (PDV)`. The performance of the model was less than ideal as there wasn't much information to work with. The MSE and $R^2$ of the base model were approximately 156606.06, 0.6583 respectively.

## Final Model

I added two features to the model, first a degree 2 polynomial given the `total fat (PDV)` as this was the more useful of the two parameters and boosting it gave a performance increase. I also added a binarizer for the `total fat (PDV)` with a threshold of 33.6 to mark data with relatively high fat content.

The modeling algorithm I settled on was Lasso, which is a modified LinearRegression algorithm that uses L1 regularization. The hyperparameters chosen for this model were alpha=0.1, max_iter=1000, tol=0.01, fit_intercept=False. These were chosen after performing a gridsearch across different predefined hyperparameters. The model shows a small performance boost over the baseline with the test mse lowered by 628.94 and the $R^2$ increased by 0.00137.

## Fairness Analysis

For the fairness evalutation of my model I choose to split the reserved test data into two groups, one of high protien and the other of low protein. The evaluation metric used was the difference in RMSE

$H_0$: The model is fair, its mean RMSE for values lower than the mean protein in the dataset is close the the RMSE of values greater than the mean protein

$H_1$: The model is not fair and there is a bias towards either lower or higher protein values

The final p-value generated from this test was 0.0933, which is not strong enough to reject the null, so its likely that our model is fair for this group.