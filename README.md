# used_car_analysis

## Link to the Jupyter notebook
[My Jupyter notebook](https://github.com/jeffhonghou/used_car_analysis/blob/main/prompt_II.ipynb)
### Business Understanding
From a business perspective, we are tasked with identifying key drivers for used car prices.  In the CRISP-DM overview, we are asked to convert this business framing to a data problem definition.  Using a few sentences, reframe the task as a data task with the appropriate technical vocabulary.

#### Reframed data task
- The objective of this task is to frame a supervised regression problem with the target variable being the price of used cars. Using the provided dataset, we aim to identify which vehicle features, such as year, mileage, manufacturer, condition, and title status, etc., have the most significant influence on pricing. This will be done through statistical analysis and by evaluating feature importance in predictive models. Additionally, we will train and compare several machine learning models, including Linear Regression, Ridge, and Lasso, to accurately predict used car prices based on these key attributes.
##### Research questions
- Which vehicle features (e.g., year, title status, condition, mileage) have the greatest impact on used car prices?
- How well can we predict used car prices using this dataset, and what is the model’s expected error?

### Data Understanding
After considering the business understanding, we want to get familiar with our data.  Write down some steps that you would take to get to know the dataset and identify any quality issues within.  Take time to get to know the dataset and explore what information it contains and how this could be used to inform your business understanding.

#### Step 1:
- Load the dataset and inspect the structure by viewing column names, data types, and non-null counts.
#### Step 2:
- Display summary statistics of the dataset to understand the distribution and central tendencies of numerical features.
#### Step 3:
- Display the first 5 rows.
#### Step 4:
- Display the total missing value percentages per column. 
##### Step 4.1
- The 'size' column will be dropped due to having more than 70% missing values, which makes it unreliable for analysis. Additionally, based on domain knowledge, the 'VIN' and 'id' columns are unique identifiers and are unlikely to have any meaningful impact on the car’s price. The 'region','model', and 'state' columns are also dropped to avoid large number of columns after applying the One-hot encoding.
#### Step 5
- Use plots to detect the potential relationships between each of the numeric features and the price.
#### Step 6
- Use plots to detect the potential relationships between each of the non-numeric features and the price.


### Data Preparation
After our initial exploration and fine-tuning of the business understanding, it is time to construct our final dataset prior to modeling.  Here, we want to make sure to handle any integrity issues and cleaning, the engineering of new features, any transformations that we believe should happen (scaling, logarithms, normalization, etc.), and general preparation for modeling with `sklearn`. 
##### Remove outliers
- Remove outliers from 'price' column
- Remove outliers from 'odometer' column
##### Other columns
- Based on domain knowledge, the 'manufacturer', 'model', 'condition','title_status', and 'transmission' will affect the price of used cars significantly. However, since there are too many unique values of 'model', here we only include the 'manufacturer','condition','title_status', and 'transmission' columns.


### Modeling
With your (almost?) final dataset in hand, it is now time to build some models.  Here, you should build a number of different regression models with the price as the target.  In building your models, you should explore different parameters and be sure to cross-validate your findings.
#### I created the following four models from which to choose the best one. GridSearchCV are used for both Ridge and Lasso regressions, and RandomizedSearchCV was used for Random Forest.

- Linear Regression
- Ridge Regression
- Lasso Regression
- Random Forest


### Evaluation
With some modeling accomplished, we aim to reflect on what we identify as a high-quality model and what we are able to learn from this.  We should review our business objective and explore how well we can provide meaningful insight into drivers of used car prices.  Your goal now is to distill your findings and determine whether the earlier phases need revisitation and adjustment or if you have information of value to bring back to your client.

##### Here is the comparison table of all above models:
- **Linear Regression**  
  - RMSE: $7,477.78  
  - Notes: Fast, but less accurate  

- **Ridge Regression**  
  - RMSE: $7,477.69  
  - Notes: Similar to Linear, slightly improved  

- **Lasso Regression**  
  - RMSE: $7,477.71  
  - Notes: Time-consuming, minimal improvement  

- **Random Forest**  
  - **RMSE: $4,364.67**  
  - **Notes: Best performer, highly accurate**

##### Business Objective Recap
- The goal was to **predict used car prices** and **identify main factors that influence pricing**, helping the used car dealers gain insight into vehicle valuation.

##### Model Performance
The **Random Forest Regressor** achieved the best performance with:
- **Test RMSE**: $4,364.67  
- **Best Parameters**: `n_estimators=50`, `max_features='sqrt'`, `max_depth=None`  

This result significantly outperforms linear models such as Linear Regression, Ridge, and Lasso (all around **$7,477 RMSE**), indicating Random Forest is better suited for capturing non-linear patterns in this dataset.

##### Feature Importance Insights
The model reveals that **year and mileage** are the top predictors of price:

- `year` (importance: 0.239832)
- `odometer` (importance: 0.236783)
- `transmission`, `drive`, and `engine cylinders`
- `fuel` and even `type`, such as sedan, pickup, etc.

##### Interpretation vs. Performance
- **Random Forest** offers high accuracy but limited interpretability.
- **Linear models** are more interpretable but underperform in prediction.
- Depending on client needs (explainability vs. accuracy), the model choice may vary.

##### Revisiting Earlier Phases?
- **No major gaps found**, but we could:
  - Drop low-importance features to simplify the model.
  - Consider using SHAP values to improve interpretability.
  - Tune further if more accuracy is needed.


### Deployment
Now that we've settled on our models and findings, it is time to deliver the information to the client.  You should organize your work as a basic report that details your primary findings.  Keep in mind that your audience is a group of used car dealers interested in fine-tuning their inventory.

##### Objective
- To help used car dealers **better understand what drives car prices**, and make smarter inventory and pricing decisions.

##### Key Insights
1. **Most Influential Factors on Price:**
   - **Year of Manufacture** : Newer cars retain higher prices.
   - **Mileage (`odometer`)** : Lower mileage = higher value.
   - **Transmission Type** : Uncommon transmissions impact pricing.
   - **Drive Type & Engine Cylinders** :– Drive configuration and engine size contribute to value.
   - **Vehicle Condition** : As expected, better condition boosts value.
   - **Fuel Type & Body Type** : Gas cars and sedans/trucks show distinct trends.

2. **Price Prediction Accuracy:**
   - Our best model (Random Forest) predicts car prices with an average error of **~$4,364.67**, which is significantly better than traditional models.
   - This model accounts for non-linear trends and complex feature interactions.

##### Model Summary

- **Linear Regression**  
  - RMSE: $7,477.78  

- **Ridge Regression**  
  - RMSE: $7,477.69  

- **Lasso Regression**  
  - RMSE: $7,477.71  

- **Random Forest**  
  - **RMSE: $4,364.67**

##### Recommendations for Dealers

- **Prioritize newer, low-mileage vehicles** : they retain more value.
- **Consider condition and transmission when buying/selling** : these features impact pricing.
- **Use this model as a pricing assistant** : to estimate fair market value.