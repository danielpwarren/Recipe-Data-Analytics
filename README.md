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
I then merged the two dataframes of
## Assessment of Missingness
## Hypothesis Testing
## Framing a Prediction Problem
## Baseline Model
## Final Model
## Fairness Analysis