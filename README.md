# what_drives_the_price_of_a_car
Used car price analysis using Ridge regression, polynomial features, and tiered market segmentation.




# Used Car Price Analysis

## CRISP-DM Framework


## BUSINESS UNDERSTANDING

The goal of this project is to better understand what drives used car prices.

From a dealership perspective, pricing vehicles accurately is critical.

* Overpricing leads to slow turnover
* Underpricing leaves money on the table

The business question becomes:

What vehicle attributes most strongly influence price, and does that influence change across price tiers?

Reframed as a data problem:

* Use supervised learning (regression) to model vehicle price.

* Identify features that contribute most to predicted price

* Evaluate whether different price segments behave differently

* Provide interpretable insights to inform inventory and pricing strategy

The objective is not just predictive accuracy, but insight into how the used car market behaves overall.


## DATA UNDERSTANDING

Before modeling, the dataset was explored to understand its structure and quality.

Key steps included:

* Reviewing missing values

* Examining feature cardinality

* Investigating extreme price values (including $0 listings)

* Visualizing price distributions (raw and trimmed)

* Exploring the structure of the `model` field


Key findings:

* A large number of vehicles were listed at $0 (likely placeholders)

* The price distribution is heavily skewed

* The model column contains noisy and inconsistent values

These observations directly informed the cleaning and feature engineering process.


## DATA PREPARATION

The dataset was filtered and prepared prior to modeling.


Filtering:

* Restricted price to 1,000 - 100,000

* Restricted odometer to 100 - 500,000 miles

* Removed vehicles older than 1990

* Dropped rows missing year, price, or odometer


Feature Engineering:

* Created vehicle age from year

* Applied log transformations to price and odometer

* Standardized categorical missing values

* Cleaned the model field and reduced it to a first-token feature

* Grouped infrequent model tokens into "other_model"


Final Feature Groups:

Numeric: age, log_odometer

Categorical: manufacturer, title_status, type, fuel, transmission, drive, condition, cylinders, paint_color, state, region, model_clean_first_token, size

The dataset was then split into training and test sets.


## MODELING

Several regression approaches were evaluated to understand how model complexity affects performance:

* Linear Regression (baseline)

* Polynomial Linear Regression (degree sweep)

* Ridge Regression (alpha sweep)

* Ridge + Polynomial Features with 5-fold cross validation


Models were compared using:

* Train and Test Mean Squared Error (MSE)

* Test R^2

* Cross-validation performance for hyperparameter selection


### Final Model

The selected model was:

* Ridge Regression

* Polynomial degree = 4

* Alpha = 0.1

This configuration provided strong test performance while maintaining stability across folds. 

Increasing the polynomial degree beyond 4 produced only marginal gains, suggesting diminishing returns from added complexity.


## EVALUATION

After selecting the final ridge model (degree = 4, alpha = 0.1), performance was examined both globally and across price tiers.


Global Observations:

* Age is the strongest overall predictor (P.I. = 0.48)

* Model identity is second (P.I. = 0.25)

* Mileage contributes consistently (P.I. = 0.17)

* Manufacturer and fuel contribute smaller but measurable effects overall

* Prediction errors increase at both the lower and upper ends of the price range

* The model captures broad pricing structure but does not fully explain extreme listings.


Tiered Observations:

When retrained within price bands, feature importance shifts meaningfully.

### 0 - 10k
* Age dominates
* Mileage remains relevant
* Model and manufacturer play smaller roles


### 10k - 20k
* Model identity becomes strongest
* Age and mileage remain influential
* Manufacturer importance increases

### 20k - 30k
* Model and manufacturer drive pricing
* Age and mileage become secondary

### 30k - 40k
* Model remains strong
* Manufacturer and age are similar in magnitude
* Mileage influence declines
* Residual volatility is lowest in this band

### 40k+
* Model and manufacturer remain important
* Fuel type increases substantially in influence
* Age and mileage play smaller relative roles

Overall, feature importance varies across price tiers, reinforcing that the used car market does not behave as a single uniform pricing system.


## DEPLOYMENT

The analysis indicates that pricing behavior differs across market segments. A single global pricing rule may not reflect these structural differences.

Tier-specific pricing adjustments may therefore be more effective than a single uniform pricing approach.

### Practical Insights by Tier

#### 0 - 10k (Utility Segment)
Pricing is primarily influenced by age and mileage. Buyers appear focused on reliability and remaining lifespan.

#### 10k - 20k (Transitional Segment)
Model identity becomes dominant. This tier shows the highest predictability (R^2 = 0.49), suggesting pricing strategies can be tighter and more systematic.

#### 20k - 30k (Brand-Sensitive Segment)
Model and manufacturer drive pricing. Brand differences outweigh wear-related factors. 

#### 30k - 40k (Stable Segment)
Volatility is lowest here. Pricing appears more structured and less driven by mechanical variation.

#### 40k+ (Premium Segment)
Fuel type becomes significantly influential, likely reflecting hybrid, electric, and performance vehicles.


## FINAL SUMMARY

Used car pricing is structured but not uniform.

* Age and mileage remain consistent drivers of value.

* Model identity, manufacturer, and fuel type gain influence in higher price tiers.

* A single global pricing rule does not fully capture these shifts.


By combining regression modeling with tier-specific analysis, this framework moves beyond prediction and toward understanding pricing dynamics across segments.

It provides a foundation for more informed inventory decisions and more disciplined pricing strategies.







