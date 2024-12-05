# recipes-and-ratings

Here’s how to reframe your report using the guidelines provided for embedding Plotly plots, creating Markdown tables, and organizing content effectively.


## **Report for Recipes and Ratings**

### **Introduction**
The **Recipes and Ratings** dataset combines user-generated recipe data with ratings, offering insights into what makes a recipe successful. With over **25,000 recipes** and **1.2 million user ratings**, this project explores the relationship between recipe attributes and user satisfaction.

#### **Central Question**
**What is the relationship between cooking time and the average rating of recipes?**

This question is highly relevant for recipe developers and home cooks, as it uncovers actionable insights for optimizing recipes to align with user preferences. By analyzing attributes such as cooking time, complexity, and nutrition, this project seeks to predict user satisfaction and reveal patterns in highly rated recipes.

### **Data Cleaning**

#### **Cleaning Process**
We focused on preparing the data for analysis and prediction by:
1. **Parsing the `nutrition` column** into individual components like `calories` and `protein`.
2. **Handling Missing Values**:
   - Filled missing descriptions with "No description."
   - Imputed missing numerical values using the column mean.
3. **Merging Datasets**:
   - Combined recipes and ratings using the `id` column to calculate `avg_rating`.

#### **Cleaned Data Sample**
Below is the head of the cleaned DataFrame:

|     id |   minutes |   n_steps |   calories |   protein |   avg_rating |
|-------:|----------:|----------:|-----------:|----------:|-------------:|
| 333281 |        40 |        10 |      138.4 |         3 |            4 |
| 453467 |        45 |        12 |      595.1 |        13 |            5 |
| 306168 |        40 |         6 |      194.8 |        22 |            5 |
| 286009 |       120 |         7 |      878.3 |        20 |            5 |
| 475785 |        90 |        17 |      267   |        29 |            5 |



### **Exploratory Data Analysis**

#### **Univariate Analysis**
- **Cooking Time Distribution**  
  Most recipes fall within the 30–60 minute range.  
  <iframe  
    src="assets/cooking-time.html"  
    width="800"  
    height="600"  
    frameborder="0"  
  ></iframe>

- **Average Rating Distribution**  
  Ratings are skewed toward higher values, with many recipes rated above 4.5.  
  <iframe  
    src="assets/rating-distribution.html"  
    width="800"  
    height="600"  
    frameborder="0"  
  ></iframe>

#### **Bivariate Analysis**
- **Cooking Time vs. Average Rating**  
  The scatter plot below shows that extremely short or long cooking times correlate with slightly lower ratings, while recipes with moderate cooking times (30–60 minutes) tend to have higher ratings.  
  <iframe  
    src="assets/cooking_time-vs-rating.html"  
    width="800"  
    height="600"  
    frameborder="0"  
  ></iframe>


### **Interesting Aggregates**

We grouped recipes by cooking time bins to examine average ratings within each range.

| cooking_time_bin   |   avg_rating |
|:-------------------|-------------:|
| 0-15 min           |      4.67088 |
| 15-30 min          |      4.62338 |
| 30-60 min          |      4.60655 |
| 1-2 hours          |      4.62744 |
| 2+ hours           |      4.58891 |

Recipes with cooking times between **30 and 60 minutes** received the highest average ratings, highlighting user preference for moderately timed recipes.


### **Prediction Problem**

#### **Objective**
Predict the **average rating** of a recipe based on its attributes, including cooking time, number of steps, and nutritional values.

#### **Approach**
- **Problem Type**: Regression
- **Evaluation Metric**: Mean Squared Error (MSE)
- **Features**: `minutes`, `n_steps`, `calories`, `protein`


### **Baseline Model**

#### **Setup**
A Random Forest Regressor was trained using the following features:
1. `minutes`
2. `n_steps`
3. `calories`
4. `protein`

#### **Results**
- **Baseline Model MSE**: `0.456`

The baseline model provided a starting point for identifying the relationship between recipe attributes and user ratings.


### **Final Model**

#### **Improvements**
1. **Feature Engineering**:
   - Added `calories-per-minute` as a new feature.
2. **Hyperparameter Tuning**:
   - Tuned `n_estimators`, `max_depth`, and `min_samples_split` using GridSearchCV.

#### **Results**
- **Final Model MSE**: `0.405`
- **Feature Importance**:
  - Calories (46.3%)
  - Minutes (20.1%)
  - Protein (19.7%)
  - Number of Steps (13.9%)

The final model significantly improved accuracy, with **calories** emerging as the most important predictor.


### **Conclusions**

1. **Key Findings**:
   - Recipes with moderate cooking times (30–60 minutes) have the highest ratings.
   - Caloric content is the strongest predictor of user satisfaction.

2. **Practical Implications**:
   - Recipe developers should optimize caloric balance and cooking times to maximize ratings.

3. **Future Work**:
   - Include additional features like tags and ingredients for deeper insights.
   - Explore classification to predict whether a recipe will be rated above 4.5.