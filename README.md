# Data Insights for UI Change Evaluation

We get data about users who enroll/visit a course page. Our goal is to come up with recommendations for the product and the marketing teams to implement the changes or not. We have a new UI change and we want to investigate whether to launch the change given the data below. For this, we decided to analyze in three directions: probability figures, A/B tests and regressions.

Half of the population received the old_page and half of the population received the new_page. 

Population: 290584 users

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
Based on all the data provide， assume that the old page is better unless the new page proves to be definitely better at a Type I error rate of 5%<br>
We state our hypothesis in terms of words or in terms of **$p_{old}$** and **$p_{new}$**, which are the converted rates for the old and new pages.

- Null hypothesis $H_{0}$: the conversion rate of the old_page is greater or the same than the conversion rate of the new_page. **$p_{old}$** >= **$p_{new}$**

- Alternative hypothesis $H_{1}$: the conversion rate of the old_page is less than the conversion rate of the new_page. 
    **$p_{old}$** < **$p_{new}$**
    
Assume under the null hypothesis, $p_{new}$ and $p_{old}$ both have "true" success rates equal to the **converted** success rate regardless of page - that is $p_{new}$ and $p_{old}$ are equal. Furthermore, assume they are equal to the **converted** rate in **ab_data.csv** regardless of the page. <br>

Use a sample size for each page equal to the ones in **ab_data.csv**.  <br><br>
Perform the sampling distribution for the difference in **converted** between the two pages over 10,000 iterations of calculating an estimate from the null.  <br><br>
Then we get:

    
## Conclusions

In conclusion, there is not enough evidence that the new_page increases the conversion rate as compared to the old_page. This is based on the probability figures, A/B test and regression. Moreover,there is no strong evidence that the countries (US, CA and UK) influence the conversion rate. 

Since the sample size is large continuing the testing of the new_page is likely not necessary. We suggested to focus on the development of another new landing page. 
