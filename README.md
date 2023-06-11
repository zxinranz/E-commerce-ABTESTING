# Data Insights for UI Change Evaluation

We get data about users who enroll/visit a course page. Our goal is to come up with recommendations for the product and the marketing teams to implement the changes or not. 
For this, we decided to analyze in three directions: probability figures, A/B tests and regressions.

Half of the population received the old_page and half of the population received the new_page. 

Population: 290584 users

## What is A/B testing?
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

## Part I - Probability
11.98% that received the old_page were converted. 11.88% that received the new_page were converted. In conclusion, the new_page did not increase the conversion rate. <br>
There is not sufficient evidence to say that the new treatment page leads to more conversions.

## Part II - A/B test
### Difficulty:

Notice that because of the time stamp associated with each event, you could technically run a hypothesis test continuously as each observation was observed.<br>  

However, the challenging question for us is whether we stop as soon as one page is considered significantly better than another, or does it need to happen consistently for a certain amount of time? How long do we run to render a decision that neither page is better than another? 

These questions are the **difficult parts** associated with A/B tests in general. 


### Hypothesis testing
Based on all the data provide， assume that the old page is better unless the new page proves to be definitely better at a Type I error rate of 5%

- Null hypothesis $H_{0}$: the conversion rate of the old_page is greater or the same than the conversion rate of the new_page. **$p_{old}$** >= **$p_{new}$**

- Alternative hypothesis $H_{1}$: the conversion rate of the old_page is less than the conversion rate of the new_page. 
    **$p_{old}$** < **$p_{new}$**
    

    
## Conclusions

In conclusion, there is not enough evidence that the new_page increases the conversion rate as compared to the old_page. This is based on the probability figures, A/B test and regression. Moreover,there is no strong evidence that the countries (US, CA and UK) influence the conversion rate. 

Since the sample size is large continuing the testing of the new_page is likely not necessary. We suggested to focus on the development of another new landing page. 
