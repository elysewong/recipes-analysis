# Healthy Recipe Analysis

## **Introduction**
My dataset explores a collection of recipes along with user interactions and ratings. It includes different recipe attributes such as ingredients, cooking time, and nutritional information, as well as user ratings and reviews. 

> **The central question of my project is: "What types of recipes work for a cutting diet (high protein, low calories)?"** 

Readers should care about this dataset and question because many individuals are interested in maintaining a healthy diet while achieving specific fitness goals. Understanding which recipes are effective for a cutting diet can help readers make informed dietary choices that align with their health and fitness objectives. This dataset contains 234429 entries (rows). The relevant columns to my question include: nutrition and name. These columns provide insights into the nutritional content (calories, protein, carbs, fats) and the names of the recipes, which are crucial for identifying suitable options for a cutting diet.

## **Data Cleaning and Exploratory Data Analysis**
### Data Cleaning
I approached data cleaning as follows:
1. Using df.info(), I took note of columns that include null values ('name', 'description', 'avg_rating'). Within the name column, I noticed there were quite a few empty strings, I replaced these with NaN values. We will keep the null values in the name and description column for now, until we work on our analyses. We also take note of the missing values in the average rating column, that we will work on to later fill in the next step: Assessment of Missingness. 
2. In the name column, I removed the leading number from the 'name' column, for cleanliness and readability when using this for later analysis.
3. In the submitted column, I converted the string values to datetime format for later analysis.
4. Since our central question focuses on the nutritional content of the recipes, I split the nutrition column into separate nutritional components, dropping the nutrition column to reduce redundancy. We can later use this for analyses.
5. I then created a new column, protein_grams, estimating grams of protein from the %DV (daily value) by dividing the %DV by 2 to get an approximate amount of usable protein in grams. I utilized grams instead of the default pdv as it's more intuitive to the day-to-day customer.
6. Using the protein_grams column from the previous step, I created a protein to calorie ratio column, protein_calorie_ratio. This will act as the determining factor for whether a recipe is suitable for a healthy fitness diet. We replace NaN values with 0 for recipes with 0 calories as th is makes the most sense.
7. I then utilize the protein_calorie_ratio column to create a binary column indicating whether a recipe is suitable for a healthy fitness diet where the threshold is 10% (1 gram of protein for every 10 calories). This will be useful for our hypotheses testing later.
The resulting dataframe is as follows:
<div style="overflow-x: auto;">
  <div>
  <style scoped>
      .dataframe tbody tr th:only-of-type {
          vertical-align: middle;
      }

      .dataframe tbody tr th {
          vertical-align: top;
      }

      .dataframe thead th {
          text-align: right;
      }
  </style>
  <table border="1" class="dataframe">
    <thead>
      <tr style="text-align: right;">
        <th></th>
        <th>name</th>
        <th>id</th>
        <th>minutes</th>
        <th>contributor_id</th>
        <th>...</th>
        <th>carbohydrates_pdv</th>
        <th>protein_grams</th>
        <th>protein_calorie_ratio</th>
        <th>is_cutting_diet</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <th>0</th>
        <td>brownies in the world best ever</td>
        <td>333281</td>
        <td>40</td>
        <td>985201</td>
        <td>...</td>
        <td>6.0</td>
        <td>1.5</td>
        <td>0.01</td>
        <td>0</td>
      </tr>
      <tr>
        <th>1</th>
        <td>in canada chocolate chip cookies</td>
        <td>453467</td>
        <td>45</td>
        <td>1848091</td>
        <td>...</td>
        <td>26.0</td>
        <td>6.5</td>
        <td>0.01</td>
        <td>0</td>
      </tr>
      <tr>
        <th>2</th>
        <td>broccoli casserole</td>
        <td>306168</td>
        <td>40</td>
        <td>50969</td>
        <td>...</td>
        <td>3.0</td>
        <td>11.0</td>
        <td>0.06</td>
        <td>0</td>
      </tr>
      <tr>
        <th>3</th>
        <td>pound cake</td>
        <td>286009</td>
        <td>120</td>
        <td>461724</td>
        <td>...</td>
        <td>39.0</td>
        <td>10.0</td>
        <td>0.01</td>
        <td>0</td>
      </tr>
      <tr>
        <th>4</th>
        <td>meatloaf</td>
        <td>475785</td>
        <td>90</td>
        <td>2202916</td>
        <td>...</td>
        <td>2.0</td>
        <td>14.5</td>
        <td>0.05</td>
        <td>0</td>
      </tr>
    </tbody>
  </table>
  <p>5 rows × 22 columns</p>
  </div>
</div>

### Univariate Analysis
Here we look at the distributions of relevant columns separately by using DataFrame operations and drawing some relevant plots.

<iframe
  src="assets/univariate_2.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
The plot shows how most recipes cluster at very low protein-to-calorie ratios, with the distribution dropping off quickly as the ratio increases. Only a small fraction of recipes reach ratios above 0.1, showing that high-protein, low-calorie options are relatively rare in this dataset.

<iframe
  src="assets/univariate_1.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
The plot shows that most recipes do not meet the protein-to-calorie threshold for a cutting diet, with more than 79,000 falling into the “Not Cutting” category. Only about 4,500 recipes qualify, showing that high-protein, lower-calorie options make up a very small share of the dataset.


### Bivariate Analysis
Here we look at the statistics of pairs of columns to identify possible associations.

<iframe
  src="assets/bivariate_2.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
The plot shows recipes labeled as suitable for a cutting diet tend to receive the same higher average ratings. The distributions overlap heavily, though, there is a slightly lower bottom whisker for cutting diet recipes.This isn't a dramatic difference, and may also be due to the small size of the cutting diet recipe group.

<iframe
  src="assets/bivariate_1.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
The plot shows most recipes cluster at low protein and low calorie values, showing that typical dishes don’t pack large amounts of protein. A few extreme outliers with unusually high protein and calorie numbers stretch the scale, indicating either very large recipes or noisy nutrition data.

<style type="text/css">
#T_00bda caption {
  font-size: 16px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 10px;
}
#T_00bda_row0_col0, #T_00bda_row1_col0 {
  border: 1px solid #ccc;
  padding: 6px;
}
</style>
<table id="T_00bda">
  <caption>Average Rating by Cutting Diet</caption>
  <thead>
    <tr>
      <th class="blank level0" >&nbsp;</th>
      <th id="T_00bda_level0_col0" class="col_heading level0 col0" >avg_rating</th>
    </tr>
    <tr>
      <th class="index_name level0" >is_cutting_diet</th>
      <th class="blank col0" >&nbsp;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_00bda_level0_row0" class="row_heading level0 row0" >0</th>
      <td id="T_00bda_row0_col0" class="data row0 col0" >4.63</td>
    </tr>
    <tr>
      <th id="T_00bda_level0_row1" class="row_heading level0 row1" >1</th>
      <td id="T_00bda_row1_col0" class="data row1 col0" >4.61</td>
    </tr>
  </tbody>
</table>
This grouped table summarizes the average rating for cutting-diet and non–cutting-diet recipes, showing that both categories receive nearly identical ratings (4.67 vs. 4.68). This suggests that healthier, high-protein recipes are not inherently rated higher or lower by users, despite being nutritionally distinct.

<style type="text/css">
#T_16e04 caption {
  font-size: 16px;
  font-weight: bold;
  text-align: center;
  margin-bottom: 10px;
}
#T_16e04_row0_col0, #T_16e04_row0_col1, #T_16e04_row0_col2, #T_16e04_row0_col3, #T_16e04_row0_col4, #T_16e04_row1_col0, #T_16e04_row1_col1, #T_16e04_row1_col2, #T_16e04_row1_col3, #T_16e04_row1_col4, #T_16e04_row2_col0, #T_16e04_row2_col1, #T_16e04_row2_col2, #T_16e04_row2_col3, #T_16e04_row2_col4, #T_16e04_row3_col0, #T_16e04_row3_col1, #T_16e04_row3_col2, #T_16e04_row3_col3, #T_16e04_row3_col4, #T_16e04_row4_col0, #T_16e04_row4_col1, #T_16e04_row4_col2, #T_16e04_row4_col3, #T_16e04_row4_col4, #T_16e04_row5_col0, #T_16e04_row5_col1, #T_16e04_row5_col2, #T_16e04_row5_col3, #T_16e04_row5_col4 {
  border: 1px solid #ccc;
  padding: 6px;
}
</style>
<table id="T_16e04">
  <caption>Average Rating by Calorie Bins and Ingredient Count Bins</caption>
  <thead>
    <tr>
      <th class="index_name level0" >ingredient_bin</th>
      <th id="T_16e04_level0_col0" class="col_heading level0 col0" >1–5</th>
      <th id="T_16e04_level0_col1" class="col_heading level0 col1" >6–10</th>
      <th id="T_16e04_level0_col2" class="col_heading level0 col2" >11–15</th>
      <th id="T_16e04_level0_col3" class="col_heading level0 col3" >16–20</th>
      <th id="T_16e04_level0_col4" class="col_heading level0 col4" >21+</th>
    </tr>
    <tr>
      <th class="index_name level0" >calorie_bin</th>
      <th class="blank col0" >&nbsp;</th>
      <th class="blank col1" >&nbsp;</th>
      <th class="blank col2" >&nbsp;</th>
      <th class="blank col3" >&nbsp;</th>
      <th class="blank col4" >&nbsp;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th id="T_16e04_level0_row0" class="row_heading level0 row0" >0–250</th>
      <td id="T_16e04_row0_col0" class="data row0 col0" >4.67</td>
      <td id="T_16e04_row0_col1" class="data row0 col1" >4.62</td>
      <td id="T_16e04_row0_col2" class="data row0 col2" >4.62</td>
      <td id="T_16e04_row0_col3" class="data row0 col3" >4.59</td>
      <td id="T_16e04_row0_col4" class="data row0 col4" >4.73</td>
    </tr>
    <tr>
      <th id="T_16e04_level0_row1" class="row_heading level0 row1" >250–500</th>
      <td id="T_16e04_row1_col0" class="data row1 col0" >4.62</td>
      <td id="T_16e04_row1_col1" class="data row1 col1" >4.61</td>
      <td id="T_16e04_row1_col2" class="data row1 col2" >4.62</td>
      <td id="T_16e04_row1_col3" class="data row1 col3" >4.64</td>
      <td id="T_16e04_row1_col4" class="data row1 col4" >4.70</td>
    </tr>
    <tr>
      <th id="T_16e04_level0_row2" class="row_heading level0 row2" >500–750</th>
      <td id="T_16e04_row2_col0" class="data row2 col0" >4.65</td>
      <td id="T_16e04_row2_col1" class="data row2 col1" >4.61</td>
      <td id="T_16e04_row2_col2" class="data row2 col2" >4.62</td>
      <td id="T_16e04_row2_col3" class="data row2 col3" >4.65</td>
      <td id="T_16e04_row2_col4" class="data row2 col4" >4.75</td>
    </tr>
    <tr>
      <th id="T_16e04_level0_row3" class="row_heading level0 row3" >750–1000</th>
      <td id="T_16e04_row3_col0" class="data row3 col0" >4.58</td>
      <td id="T_16e04_row3_col1" class="data row3 col1" >4.62</td>
      <td id="T_16e04_row3_col2" class="data row3 col2" >4.64</td>
      <td id="T_16e04_row3_col3" class="data row3 col3" >4.66</td>
      <td id="T_16e04_row3_col4" class="data row3 col4" >4.72</td>
    </tr>
    <tr>
      <th id="T_16e04_level0_row4" class="row_heading level0 row4" >1000–1500</th>
      <td id="T_16e04_row4_col0" class="data row4 col0" >4.61</td>
      <td id="T_16e04_row4_col1" class="data row4 col1" >4.62</td>
      <td id="T_16e04_row4_col2" class="data row4 col2" >4.59</td>
      <td id="T_16e04_row4_col3" class="data row4 col3" >4.66</td>
      <td id="T_16e04_row4_col4" class="data row4 col4" >4.57</td>
    </tr>
    <tr>
      <th id="T_16e04_level0_row5" class="row_heading level0 row5" >1500–3000</th>
      <td id="T_16e04_row5_col0" class="data row5 col0" >4.62</td>
      <td id="T_16e04_row5_col1" class="data row5 col1" >4.62</td>
      <td id="T_16e04_row5_col2" class="data row5 col2" >4.64</td>
      <td id="T_16e04_row5_col3" class="data row5 col3" >4.66</td>
      <td id="T_16e04_row5_col4" class="data row5 col4" >4.66</td>
    </tr>
  </tbody>
</table>
This pivot table summarizes how recipe ratings vary with calorie range and number of ingredients. We see a clear pattern: simpler, lower-calorie recipes tend to receive higher ratings, while more complex or calorie-dense dishes show slightly lower average ratings. 

## **Assessment of Missingness**
In our recipes dataset, we find only three columns with missing entries: 'name', 'description', and 'avg_rating'. 

The 'name' and 'description' columns have missing values likely because some recipes may not have been provided with a name or description when they were added to the dataset. For example, user-generated content may sometimes be incomplete, such as when users submit recipes without filling out all the details. It's quite likely that a user forgot to include a description when submitting a recipe. These columns appear to be missing because some contributors did not fill them out, or because of inconsistencies in how recipes were submitted or stored in the database. In other words, the probability that name or description is missing depends on who submitted the recipe, the format of the upload, or other observed variables, not on the missing text itself. 

Another possibility is that the 'description' column is missing by design. Many recipe platforms have an optional description field and therefore, the missing descriptions occur because the dataset itself allows an empty description and users decide not to input. This is a classic example of missing by design!

Based on the data-generating process, the **column most likely to be NMAR** (Not Missing At Random) is the **avg_rating** column. A recipe is missing an average rating only when no users rated it, and that missingness is likely caused by properties of the recipe itself (perhaps very niche recipes, or extremely new recipes). Because the reason a recipe has no ratings may be directly tied to its true (unobserved) rating, the missingness depends on the missing value itself, which is also characteristic of NMAR. 

### Missingness Dependency
A column that the missingness of the 'avg_rating' column does depend on may be the 'submitted' column. The intuition is that newer recipes haven’t had as much time to receive ratings so missing avg_rating should be more common for recent submissions. 
<iframe
  src="assets/nmar_permutation_2.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
To investigate this, we ran a permutation test to investigate whether the missingness of the 'avg_rating' column depends on the 'submitted' year of the recipes. The null hypothesis stated that there is no relationship between the year a recipe was submitted and whether its average rating is missing. We calculated the observed difference in mean submission year between recipes with missing and non-missing avg_rating values. The observed difference in mean submission year between missing and non-missing recipes was ~0.73, indicating that recipes without ratings tend to be substantially newer. When we permuted the missingness indicator 10,000 times, none of the simulated mean differences were as extreme as the observed one, yielding a p-value of approximately 0. This provides strong evidence that missingness in avg_rating depends on submission year.


## **Hypothesis Testing**
**Null Hypothesis**: Healthy-fitness-diet recipes have the same or higher average rating than other recipes.
**Alternative Hypothesis**: Healthy-fitness-diet recipes have a lower average rating than other recipes.
**Test Statistic**: Difference of means (sample mean rating of healthy recipes - sample mean rating of other recipes) 
This is a good choice because our research question is specifically about whether one group tends to be rated lower on average, and ratings are numerical, so comparing group means is very interpretable.
I chose the standard **significance level** of 0.05 to define statistical significance.
Since the **p-value (0.0480)** is less than 0.05, we **reject the null hypothesis**.
In these trials, we find that healthy-fitness-diet recipes have a lower average rating than other recipes.

I used a **significance level** of **𝛼 = 0.05**, which is a very standard threshold in statistical analysis. The observed p-value from the permutation test was 0.0480. Since 
**𝑝 = 0.0480** < 0.05, we **reject the null hypothesis** and conclude that, in this sample, healthy-fitness-diet recipes tend to have a lower average rating than other recipes. While the difference in means is small in magnitude, this test provides statistical evidence that these recipes are not rated as highly on average.