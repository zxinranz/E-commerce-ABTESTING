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
Then we get:<br>
![](https://github.com/zxinranz/img-folder/blob/main/p_diffs.jpg)
To show that the proportion of the **p_diffs** are greater than the actual difference observed in **ab_data.csv**

![](https://github.com/zxinranz/img-folder/blob/main/obs_diff.png)

90.98% is the proportion of the p_diffs that are greater than the actual difference observed in ab_data.csv (**p-value**). This value means that **we cannot reject the null hypothesis and that we do not have sufficient evidence that the new_page has a higher conversion rate than the old_page.** 

### Build-in function
` import statsmodels.api as sm `<br>

use `stats.proportions_ztest` to compute your test statistic and p-value.<br>

z_score, p_value: (1.3109241984234394, 0.9050583127590245)<br>

The z-score and the p_value mean that one doesn't reject the Null. The Null being the converted rate of the old_page is the same or greater than the converted rate of the new_page. The p_value is 0.91 and is higher than 0.05 significance level. That means we can not be confident with a 95% confidence level that the converted rate of the new_page is larger than the old_page.

## Part III - A regression approach
> Acheived in the previous A/B test can also be acheived by performing regression.

Since each row represents either a conversion or no conversion, we should perform **logistic regression** in this case.

Our goal is to use **statsmodels** to fit the regression model specified in part a to determine if there is a significant difference in conversion based on the page received by each customer. However, before fitting the model, we need to create two additional columns. <br>

First, we need to add an **intercept column**. This column will contain a constant value of 1 for each observation in the dataset. It is necessary for the logistic regression model.<br>

Second, we need to create a **dummy variable column** to represent which page each user received. We will create an **"ab_page" column**, which will be assigned a value of 1 if an individual received the treatment (new page) and 0 if they were in the control group (old page).<br>

![](https://github.com/zxinranz/img-folder/blob/main/ab_logis.png)

Use statsmodels to import regression model. Instantiate the model, and fit the model using the two columns created in part b. to predict whether or not an individual converts.

## Conclusions

In conclusion, there is not enough evidence that the new_page increases the conversion rate as compared to the old_page. This is based on the probability figures, A/B test and regression. Moreover,there is no strong evidence that the countries (US, CA and UK) influence the conversion rate. 

Since the sample size is large continuing the testing of the new_page is likely not necessary. We suggested to focus on the development of another new landing page. 
