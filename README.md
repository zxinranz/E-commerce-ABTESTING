# Data Insights for UI Change Evaluation

We get data about users who enroll/visit a course page. Our goal is to come up with recommendations for the product and the marketing teams to implement the changes or not. We have a new UI change and we want to investigate whether to launch the change given the data below. For this, we decided to analyze in three directions: probability, A/B tests and regressions.
We then found that the new page did not result in a significant increase in conversions, and we turned to consider whether national factors might affect conversions.

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

![](https://github.com/zxinranz/img-folder/blob/main/logis_reg.png)

Based on the provided regression analysis results, the following conclusions can be drawn:

- We used a Logit regression model to predict conversion rate.
- The number of observations in the sample is 290,584, which represents the total number of observations analyzed.
- The model was fitted using the Maximum Likelihood Estimation (MLE) method.
- The model has 1 degree of freedom for the model and 290,582 degrees of freedom for residuals.
- The pseudo R-squared value is 8.077e-06, which is close to zero, indicating that the model has a weak explanatory power for the target variable.
- The model has a good fit as indicated by the converged value of True, indicating that the model has converged.
- The Log-Likelihood value is -1.0639e+05, which can be used to compare the goodness of fit among different models.
- In terms of statistical significance testing, the intercept has a very small p-value (close to zero), indicating that the intercept term is significant in the model.
- The coefficient for ab_page is -0.0150 with a p-value of 0.190, which is relatively large (greater than 0.05), suggesting that the ab_page variable is not statistically significant and does not have a significant relationship with the conversion rate.

It is important to note that while the model fits the overall dataset well, **the coefficient and p-value for ab_page indicate that there is no significant relationship between ab_page and the conversion rate**. Therefore, further exploration of other variables or reconsideration of the model's construction may be necessary.

Now along with testing if the conversion rate changes for different pages, also add an effect based on which country a user lives. 

![](https://github.com/zxinranz/img-folder/blob/main/logis_countri.png)

The country does not appear to have influence on the convertion rate. P-values for the two dummy country variables are above 0.05. Note the CA variable get closes to 0.05.<br>

We would now like to look at an interaction between page and country to see if there significant effects on conversion.By looking at an interaction, I will explore whether the influence of the landing_page might work in the US but not in the other countries, or Canada but not in other countries. Or the other way around.

![]()

The p_value for both interaction terms is higher than 0.05.

Thus, the influence of landing_page in the US is not different to the influence of landing_page in the other countries. 

And the influence of landing_page in Canada is not different to the influence of landing_page in the other countries. 
## Conclusions

In conclusion, there is not enough evidence that the new_page increases the conversion rate as compared to the old_page. This is based on the probability figures, A/B test and regression. Moreover,there is no strong evidence that the countries (US, CA and UK) influence the conversion rate. 

Since the sample size is large continuing the testing of the new_page is likely not necessary. We suggested to focus on the development of another new landing page. 
