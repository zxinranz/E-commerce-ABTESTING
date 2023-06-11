# Data Insights for UI Change Evaluation

We get data about users who enroll/visit a course page. Our goal is to come up with recommendations for the product and the marketing teams to implement the changes or not. We have a new UI change and we want to investigate whether to launch the change given the data below. 
Half of the population received the old_page and half of the population received the new_page. The population is considerable in size (290584 users). 

## A/B Testing
A/B testing, also called split testing, is a method of determining which of two versions of a product feature, web page, campaign element, or other asset performs better for a certain goal. You can use A/B testing to improve the product development workflow, user interface (UI), or conversion rates. Essentially, A/B testing eliminates all the guesswork out of website optimization and enables experience optimizers to make data-backed decisions. In A/B testing, A refers to ‘control’ or the original testing variable. Whereas B refers to ‘variation’ or a new version of the original testing variable.

## Database

### `ab_data.csv` 

1. `user_id`: users who visit the course page
2. `timestamp`: Webpage Dwell Time
3. `group`: Control group or experimental group
4. `landing_page`: new page or old page
5. `converted`: whether the user made a purchase


### `countries.csv` 
1. `user_id`: users who visit the course page
2. `country`: Login Country

## Data cleaning:
- Remove the outlier:The number of times the `new_page` and `treatment` don't line up.（mismatch）
- Remove the missing value
- Remove duplicate values

## Check the probability of the converted rate for both control and treatment
