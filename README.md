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
3. In the submitted column, I converted the string values to datetime format for later analysis. I also created a year column based on this column's content.
4. Since our central question focuses on the nutritional content of the recipes, I split the nutrition column into separate nutritional components, dropping the nutrition column to reduce redundancy. We can later use this for analyses.
5. I then created a new column, protein_grams, estimating grams of protein from the %DV (daily value) by dividing the %DV by 2 to get an approximate amount of usable protein in grams. I utilized grams instead of the default pdv as it's more intuitive to the day-to-day customer.
6. Using the protein_grams column from the previous step, I created a protein to calorie ratio column, protein_calorie_ratio. This will act as the determining factor for whether a recipe is suitable for a healthy fitness diet. We replace NaN values with 0 for recipes with 0 calories as th is makes the most sense.
7. I then utilize the protein_calorie_ratio column to create a binary column indicating whether a recipe is suitable for a healthy fitness diet where the threshold is 10% (1 gram of protein for every 10 calories). This will be useful for our hypotheses testing later.
The resulting dataframe is as follows:
<div style="overflow-x: auto;">
  '| name                             |     id |   minutes |   contributor_id | submitted           | tags                                                                                                                                                                                                                                                                                                                                       |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                                                                             |   n_ingredients |   avg_rating |   calories |   total_fat_pdv |   sugar_pdv |   sodium_pdv |   protein_pdv |   saturated_fat_pdv |   carbohydrates_pdv |   protein_grams |   protein_calorie_ratio |   is_cutting_diet |\n|:---------------------------------|-------:|----------:|-----------------:|:--------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------:|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-------------:|-----------:|----------------:|------------:|-------------:|--------------:|--------------------:|--------------------:|----------------:|------------------------:|------------------:|\n| brownies in the world best ever  | 333281 |        40 |           985201 | 2008-10-27 00:00:00 | ["\'60-minutes-or-less\'", "\'time-to-make\'", "\'course\'", "\'main-ingredient\'", "\'preparation\'", "\'for-large-groups\'", "\'desserts\'", "\'lunch\'", "\'snacks\'", "\'cookies-and-brownies\'", "\'chocolate\'", "\'bar-cookies\'", "\'brownies\'", "\'number-of-servings\'"]                                                                                    |        10 | [\'heat the oven to 350f and arrange the rack in the middle\', \'line an 8-by-8-inch glass baking dish with aluminum foil\', \'combine chocolate and butter in a medium saucepan and cook over medium-low heat , stirring frequently , until evenly melted\', \'remove from heat and let cool to room temperature\', \'combine eggs , sugar , cocoa powder , vanilla extract , espresso , and salt in a large bowl and briefly stir until just evenly incorporated\', \'add cooled chocolate and mix until uniform in color\', \'add flour and stir until just incorporated\', \'transfer batter to the prepared baking dish\', \'bake until a tester inserted in the center of the brownies comes out clean , about 25 to 30 minutes\', \'remove from the oven and cool completely before cutting\']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you\'ll ever make.....sereiously! there\'s no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they\'re pure heaven!                                                                                                              | [\'bittersweet chocolate\', \'unsalted butter\', \'eggs\', \'granulated sugar\', \'unsweetened cocoa powder\', \'vanilla extract\', \'brewed espresso\', \'kosher salt\', \'all-purpose flour\']                                                          |               9 |            4 |      138.4 |              10 |          50 |            3 |             3 |                  19 |                   6 |             1.5 |               0.0108382 |                 0 |\n| in canada chocolate chip cookies | 453467 |        45 |          1848091 | 2011-04-11 00:00:00 | ["\'60-minutes-or-less\'", "\'time-to-make\'", "\'cuisine\'", "\'preparation\'", "\'north-american\'", "\'for-large-groups\'", "\'canadian\'", "\'british-columbian\'", "\'number-of-servings\'"]                                                                                                                                                            |        12 | [\'pre-heat oven the 350 degrees f\', \'in a mixing bowl , sift together the flours and baking powder\', \'set aside\', \'in another mixing bowl , blend together the sugars , margarine , and salt until light and fluffy\', \'add the eggs , water , and vanilla to the margarine / sugar mixture and mix together until well combined\', \'add in the flour mixture to the wet ingredients and blend until combined\', \'scrape down the sides of the bowl and add the chocolate chips\', \'mix until combined\', \'scrape down the sides to the bowl again\', \'using an ice cream scoop , scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking\', \'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center\', \'serve hot and enjoy !\']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don\'t have margarine or don\'t like it, then just use butter (softened) instead.                                                                                                                                            | [\'white sugar\', \'brown sugar\', \'salt\', \'margarine\', \'eggs\', \'vanilla\', \'water\', \'all-purpose flour\', \'whole wheat flour\', \'baking soda\', \'chocolate chips\']                                                                             |              11 |            5 |      595.1 |              46 |         211 |           22 |            13 |                  51 |                  26 |             6.5 |               0.0109225 |                 0 |\n| broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ["\'60-minutes-or-less\'", "\'time-to-make\'", "\'course\'", "\'main-ingredient\'", "\'preparation\'", "\'side-dishes\'", "\'vegetables\'", "\'easy\'", "\'beginner-cook\'", "\'broccoli\'"]                                                                                                                                                                   |         6 | [\'preheat oven to 350 degrees\', \'spray a 2 quart baking dish with cooking spray , set aside\', \'in a large bowl mix together broccoli , soup , one cup of cheese , garlic powder , pepper , salt , milk , 1 cup of french onions , and soy sauce\', \'pour into baking dish , sprinkle remaining cheese over top\', \'bake for 25 minutes or until cheese is lightly browned\', \'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly , about 10 more minutes\']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don\'t think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell\'s soup. but i think mine is better since i don\'t like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | [\'frozen broccoli cuts\', \'cream of chicken soup\', \'sharp cheddar cheese\', \'garlic powder\', \'ground black pepper\', \'salt\', \'milk\', \'soy sauce\', \'french-fried onions\']                                                                   |               9 |            5 |      194.8 |              20 |           6 |           32 |            22 |                  36 |                   3 |            11   |               0.0564682 |                 0 |\n| pound cake                       | 286009 |       120 |           461724 | 2008-02-12 00:00:00 | ["\'time-to-make\'", "\'course\'", "\'cuisine\'", "\'preparation\'", "\'occasion\'", "\'north-american\'", "\'desserts\'", "\'american\'", "\'southern-united-states\'", "\'dinner-party\'", "\'holiday-event\'", "\'cakes\'", "\'dietary\'", "\'christmas\'", "\'thanksgiving\'", "\'low-sodium\'", "\'low-in-something\'", "\'taste-mood\'", "\'sweet\'", "\'4-hours-or-less\'"] |         7 | [\'freheat the oven to 300 degrees\', \'grease a 10-inch tube pan with butter , dust the bottom and sides with flour , and set aside\', \'in a large mixing bowl , cream the butter and sugar with an electric mixer and add the eggs one at a time , beating after each addition\', \'alternately add the flour and milk , stirring till the batter is smooth\', \'add the two extracts and stir till well blended\', \'scrape the batter into the prepared pan and bake till a cake tester or knife blade inserted in the center comes out clean , about 1 1 / 2 hours\', \'cool the cake in the pan on a rack for 5 minutes , then turn it out on the rack to cool completely\']                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | why a millionaire pound cake?  because it\'s super rich!  this scrumptious cake is the pride of an elderly belle from jackson, mississippi.  the recipe comes from "the glory of southern cooking" by james villas.                                                                                                                                                                | [\'butter\', \'sugar\', \'eggs\', \'all-purpose flour\', \'whole milk\', \'pure vanilla extract\', \'almond extract\']                                                                                                                                |               7 |            5 |      878.3 |              63 |         326 |           13 |            20 |                 123 |                  39 |            10   |               0.0113856 |                 0 |\n| meatloaf                         | 475785 |        90 |          2202916 | 2012-03-06 00:00:00 | ["\'time-to-make\'", "\'course\'", "\'main-ingredient\'", "\'preparation\'", "\'main-dish\'", "\'potatoes\'", "\'vegetables\'", "\'4-hours-or-less\'", "\'meatloaf\'", "\'simply-potatoes2\'"]                                                                                                                                                                 |        17 | [\'pan fry bacon , and set aside on a paper towel to absorb excess grease\', \'mince yellow onion , red bell pepper , and add to your mixing bowl\', \'chop garlic and set aside\', \'put 1tbsp olive oil into a saut pan , along with chopped garlic , teaspoons white pepper and a pinch of kosher salt\', \'bring to a medium heat to sweat your garlic\', \'preheat oven to 350f\', \'coarsely chop your baby spinach add to your heated pan , stir frequently for approximately 5 min to wilt\', \'add your spinach to the mixing bowl\', \'chop your now cooled bacon , and add it to the mixing bowl\', \'add your meatloaf mix to the bowl , with one egg and mix till thoroughly combined\', \'add your goat cheese , one egg , 1 / 8 tsp white pepper and 1 / 8 tsp of kosher salt and mix till thoroughly combined\', \'transfer to a 9x5 meatloaf pan , and cook for 60 min or until the internal temperature is at least 160f\', \'let stand for 5min\', \'melt 1tbsp unsalted butter into a frying pan , and cook up to three eggs at a time\', \'crack each egg into a separate dish , in order to prevent egg shells from reaching the pan , then add salt and pepper to taste\', \'wait until the egg whites are firm looking , but slightly runny on top before flipping your eggs\', \'after flipping , wait 10~20 seconds before removing each egg and placing it over your slices of meatloaf\'] | ready, set, cook! special edition contest entry: a mediterranean flavor inspired meatloaf dish. featuring: simply potatoes - shredded hash browns, egg, bacon, spinach, red bell pepper, and goat cheese.                                                                                                                                                                         | [\'meatloaf mixture\', \'unsmoked bacon\', \'goat cheese\', \'unsalted butter\', \'eggs\', \'baby spinach\', \'yellow onion\', \'red bell pepper\', \'simply potatoes shredded hash browns\', \'fresh garlic\', \'kosher salt\', \'white pepper\', \'olive oil\'] |              13 |            5 |      267   |              30 |          12 |           12 |            29 |                  48 |                   2 |            14.5 |               0.0543071 |                 0 |'
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

Based on the data-generating process, the column most likely to be NMAR (Not Missing At Random) is the avg_rating column. A recipe is missing an average rating only when no users rated it. The reason a recipe has no ratings may be directly tied to its true (unobserved) rating, the missingness depends on the missing value itself, which is also characteristic of NMAR. However,that missingness may also  be caused by properties of the recipe itself (perhaps very niche recipes, or extremely new recipes). 

### Missingness Dependency
A column that the missingness of the 'avg_rating' column does depend on may be the 'submitted' column. The intuition is that newer recipes haven’t had as much time to receive ratings so missing avg_rating should be more common for recent submissions. 
<iframe
  src="assets/nmar_permutation_2.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
To investigate this, we ran a permutation test to investigate whether the missingness of the 'avg_rating' column depends on the 'submitted' year of the recipes. The null hypothesis stated that there is no relationship between the year a recipe was submitted and whether its average rating is missing. We calculated the observed difference in mean submission year between recipes with missing and non-missing avg_rating values. 
<iframe
  src="assets/nmar_1.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
The observed difference in mean submission year between missing and non-missing recipes was ~0.73, indicating that recipes without ratings tend to be substantially newer. When we permuted the missingness indicator 10,000 times, none of the simulated mean differences were as extreme as the observed one, yielding a p-value of approximately 0. Since the p-value (~0) is less than 0.05, we reject the null hypothesis. This test provides statistical evidence that the missingness of avg_rating depends on the year the recipe was submitted, proving MAR.

Next, we can examine whether the missingness of the 'avg_rating' column depends on the number of ingredients in a recipe. We suspect the missingness of the avg_rating column does not depend on the number of ingredients, but we will still investigate!
<iframe
  src="assets/nmar_permutation_3.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
The normalized histograms above illustrate the distribution of the number of ingredients in recipes with and without missing average ratings. The shapes of the two distributions appear quite similar, suggesting that the number of ingredients does not significantly influence whether a recipe has a missing average rating. However, we can perform a permutation test to formally assess this.

The null hypothesis is that there is no relationship between the number of ingredients in a recipe and whether its average rating is missing. The alternative hypothesis is that there is a relationship between the number of ingredients in a recipe and whether its average rating is missing. We binned n_ingredients into five categories and compared the distribution of ingredient bins among recipes with and without missing avg_rating using total variation distance (TVD) as the test statistic. I chose total variation distance (TVD) as the test statistic instead of a difference in means or medians because TVD compares the entire distribution of ingredient counts between recipes with missing and non-missing ratings. In contrast, the mean and median collapse the data into a single summary number, which can be problematic here because n_ingredients is discrete, skewed, and heavily concentrated around a few values. As a result, the median can only take on a small set of values (often 0 or 1 apart), and the mean becomes overly sensitive to the dataset’s size and long right tail.
<iframe
  src="assets/nmar_permutation_4.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
Since the p-value (0.1270) is greater than 0.05, we fail to reject the null hypothesis. We do not find sufficient evidence to suggest that the missingness of avg_rating depends on the number of ingredients in a recipe.

We do not impute missing values in the name, description, or avg_rating columns. We do not fill in missing values for name or description because these fields are not relevant to our prediction task and there is no sensible way to impute them since these columns contain free-form string text. For the avg_rating column, filling in missing values would skew the logic behind our data as the reason these values are null are because they have not yet been rated. Any imputation of ratings would bias the training process as we are falsifying this information.

## **Hypothesis Testing**
*Null Hypothesis*: Healthy-fitness-diet recipes have the same or higher average rating than other recipes.

*Alternative Hypothesis*: Healthy-fitness-diet recipes have a lower average rating than other recipes.


*Test Statistic*: Difference of means (sample mean rating of healthy recipes - sample mean rating of other recipes) 
This is a good choice because our research question is specifically about whether one group tends to be rated lower on average, and ratings are numerical, so comparing group means is very interpretable.

<iframe
  src="assets/hypothesis_test.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
I used a significance level of 𝛼 = 0.05, which is a very standard threshold in statistical analysis. The observed p-value from the permutation test was 0.0517. Since 𝑝 = 0.0517 > 0.05, we fail to reject the null hypothesis as we do not have enough evidence to find that healthy-fitness-diet recipes are associated with a lower average rating.

## Framing a Prediction Problem
In this project, my prediction problem is to predict the number of steps (n_steps) in a recipe. This is a regression problem. At prediction time, a recipe platform or user would know nutritional information, calories, cooking time, and ingredient count from metadata, but not the full written steps. Predicting n_steps helps estimate recipe complexity before opening the full details.

To evaluate my model, I use Root Mean Squared Error (RMSE). RMSE is the most appropriate metric for this regression problem as it penalizes large errors more heavily than MAE, which matters more as dramatically over- or under-predicting a recipe’s complexity is more harmful than being slightly off. Classification metrics like accuracy or F1-score are not suitable because this is not a classification task. By framing the problem as regression and evaluating with RMSE, the model aligns with both the structure of the data and the goal of predicting continuous user ratings.

## Baseline Model

For my baseline model, I attempt to use a linear regression model to predict the target variable, n_steps. The model utilizes three quantitative features: minutes, n_ingredients, and avg_rating. I utilize a Pipeline and ColumnTransformer to handle different preprocessing needs. Specifically, the avg_rating feature contains missing values, so I transformed using a Pipeline that first applies mean imputation (SimpleImputer) and then scales the result (StandardScaler). The remaining numerical features, minutes and n_ingredients, are also transformed via StandardScaler. Although standard scaling does not inherently improve the prediction accuracy of a LinearRegression model, applying it to all input features is beneficial to ensures all features are on a comparable scale. Finally, the preprocessed features are fed into the LinearRegression model, and the model's performance on the unseen test data is evaluated using Root Mean Squared Error (RMSE) and the R-squared score, which measures the proportion of the target's variance explained by the features

The model achieved an RMSE of approximately 5.7564 and an R² of 0.1828. This indicates that the model captures a moderate amount of variance (~20%) but cannot represent the nonlinear relationships between features and recipe complexity. While the model captures a minor amount of variance in step count, I would not consider it “good” yet, because recipe complexity often depends on nonlinear interactions. For example, high-calorie recipes with many ingredients tend to have more steps. A linear model is restricted to modeling straight-line relationships, so it struggles to capture these patterns. This sets the stage for an improved model, a Random Forest, that can account for nonlinear structure in the data.

## Final Model
The final model for predicting recipe step count (n_steps) was constructed using a Random Forest Regressor, which, as we learned, is an ensemble algorithm. Model selection was performed via GridSearchCV using 3-fold cross-validation, which systematically searched a hyperparameter space for the optimal settings that minimized the prediction error, scored by the Negative Root Mean Squared Error (RMSE) metric. The best-performing configuration was found to be n_estimators=300, max_depth=10, and min_samples_split=5.

This final pipeline was significantly enhanced through feature engineering, which aimed to increase the complexity of the recipes. When picking these features, I thought about how the number of steps is determined likely not just by time, but by the recipe's complexity and also its culinary category. The new features added were: 
1. all nutritional information (e.g., calories, sugar_pdv), which I believe gives us context on recipe type. 
2. Recipe Age, calculated from the submitted date, to account for potential changes in recipe conventions over time
3. Top 10 Tags, which capture the specific items and processes involved. For instance, recipes containing the tag "dessert" can likely imply more complex steps that are not linearly related to preparation time (minutes). These features could provide helpful non-linear signals to the Random Forest.

The best parameters were {'regressor__max_depth': 20, 'regressor__min_samples_split': 10, 'regressor__n_estimators': 500}.

This engineered approach led to a performance improvement: the Final Model achieved an R-squared of approximately 0.3684 and an RMSE of approximately 5.0607. This is a big improvement over our Baseline Linear Regression model, which achieved an RMSE of approximately 5.7564 and an R² of 0.1828. This shows how the baseline model was not complex enough to capture the underlying structure of the data. There is still much to improve with this model

## Fairness Analysis
The fairness analysis was conducted using a Permutation Test to assess if the RandomForestRegressor model performs differently across distinct user demographics, specifically defined by the recipes' sugar content. Group X (High-Sugar) consisted of recipes with a Percent Daily Value for Sugar (sugar_pdv) ≥ 12.0 (the data median), serving as a proxy for "bakers" or "sugar lovers," while Group Y (Low-Sugar) consisted of recipes with sugar_pdv < 12.0. The chosen evaluation metric was Root Mean Squared Error (RMSE), and the Test Statistic was the difference, High-Sugar Group RMSE − Low-Sugar Group RMSE.

Our Null Hypothesis was that the model is fair, meaning the error difference is zero. The Alternative Hypothesis was that the model is unfair, with a higher error for the High-Sugar group. The test yielded an Observed Test Statistic of 0.3449 (with the High-Sugar Group RMSE = 5.1640 and Low-Sugar Group RMSE = 4.8191). 

<iframe
  src="assets/fairness.html"
  width="800"
  height="450"
  frameborder="0"
></iframe> 
Running 1,000 permutations resulted in a P-value of 0.0240. Since this p-value is greater than the significance level (α = 0.05), we reject the Null Hypothesis. There is statistically significant evidence that the model seems to perform significantly worse for the High-Sugar group (Group X) This means both "bakers" and "savory lovers" can't rely on the model equally, showing an unequal user experience since the model shows bias against a specific dietary subpopulation.