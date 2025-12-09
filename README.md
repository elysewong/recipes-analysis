# Healthy Recipe Analysis

## **Introduction**
My dataset explores a collection of recipes along with user interactions and ratings. It includes different recipe attributes such as ingredients, cooking time, and nutritional information, as well as user ratings and reviews. 

> **The central question of my project is: "What types of recipes work for a cutting diet (high protein, low calories)?"** 

Readers should care about this dataset and question because many individuals are interested in maintaining a healthy diet while achieving specific fitness goals. Understanding which recipes are effective for a cutting diet can help readers make informed dietary choices that align with their health and fitness objectives. This dataset contains 234429 entries (rows). The relevant columns to my question include: nutrition and name. These columns provide insights into the nutritional content (calories, protein, carbs, fats) and the names of the recipes, which are crucial for identifying suitable options for a cutting diet.

## **Data Cleaning and Exploratory Data Analysis**
### Data Cleaning
I approached data cleaning as follows:
1. Using df.info(), I took note of columns that include null values ('name', 'description', 'avg_rating'). We will keep the null values in the name and description column for now, until we work on our analyses. We also take note of the missing values in the average rating column, that we will work on to later fill in the next step: Assessment of Missingness. 
2. In the name column, I removed the leading number from the 'name' column, for cleanliness and readability when using this for later analysis.
3. Since our central question focuses on the nutritional content of the recipes, I split the 'nutrition' column into separate nutritional components. We can later use this for analyses.
4. I then created a new column, protein_grams, estimating grams of protein from the %DV (daily value) by dividing the %DV by 2 to get an approximate amount of usable protein in grams. I utilized grams instead of the default pdv as it's more intuitive to the day-to-day customer.
5. Using the protein_grams column from the previous step, I created a protein to calorie ratio column, protein_calorie_ratio. This will act as the determining factor for whether a recipe is suitable for a healthy fitness diet. We replace NaN values with 0 for recipes with 0 calories as th is makes the most sense.
6. I then utilize the protein_calorie_ratio column to create a binary column indicating whether a recipe is suitable for a healthy fitness diet where the threshold is 10% (1 gram of protein for every 10 calories). This will be useful for our hypotheses testing later.
The resulting dataframe is as follows:
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
      <td>in the world best ever</td>
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
      <td>canada chocolate chip cookies</td>
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
      <td>casserole</td>
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
      <td>cake</td>
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
      <td></td>
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
<p>5 rows × 23 columns</p>
</div>

### Univariate Analysis

<iframe
  src="assets/univariate_1.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>
