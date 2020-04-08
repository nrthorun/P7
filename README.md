Project 7 - A/B Testing
===========================
### Author: Nikolas Thorun

### Abstract

This experiment is based on a modification made after clicking on the _'start free trial'_ button on the Udacity website. Instead of taking the student directly to the enrollment part, a window appears with a field for him to answer how many hours a week he has available for his studies. If he replies that he has more than 5 hours a week, the student goes to the enrollment area as usual, if not, he receives a message saying that Udacity courses generally require more time to complete and suggests that he access the free materials.
The hypothesis is that expectations are aligned, in order to reduce the number of students frustrated by not having enough time without significantly reducing the number of students who enroll and finish the course.

### Choice of metrics

** Invariant metrics: ** Number of cookies, number of clicks, click probability <br>
** Valuation Metrics: ** Conversion, retention and net conversion

* Number of cookies: Unique number of cookies related to website visitors. It should not vary between the control and experiment groups and, therefore, a good choice for invariant metrics.

* Number of clicks: Number of clicks on the _'start free trial'_ button. It should also not vary between groups, since it is before the part of the experiment. Invariant metric.

* Click probability: Relationship between the two previous metrics. As it is based on metrics that should not change, neither should it change. Invariant metric.

* Conversion: Unique number of users to register for the free trial divided by the unique number of cookies that clicked on the _'start free trial'_ button. In the experiment group, the window with the question regarding the available weekly hours should appear but not in the control group. Therefore, it is a good evaluation metric. The expectation in this experiment is that the conversion will decrease, since part of the students will be directed to the page of free materials.


* Retention: Number of users who remained enrolled for more than 14 days (and therefore making at least 1 payment) over the number of enrolled users. These steps happen after the experiment, the numbers in the groups can be different. It could be used as an evaluation metric, but it will not for reasons that will be addressed later.


* Net conversion: Number of users who remained enrolled for more than 14 days on the number of clicks on the _'start free trial'_ button. As the number of registered users may be different in the two groups, we will use it as an evaluation metric.

Unused metrics: Number of unique users. As it depends on the experiment on the _'start free trial'_ page, it cannot be used as an invariant metric. It could be used as an evaluation metric, but it is not as robust as it is not standardized and is redundant to other evaluation metrics.

The expected result is that the number of frustrated students will decrease, without causing a decrease in the company's profits. In this way, we expect the conversion to decrease and we can even tolerate a drop in net conversion, but this cannot exceed the practical limit of significance.

### Measuring variability

Considering a binomial distribution with population `N` and probability `p`, we have that the standard error is `SE = sqrt((p*(1-p)/N)`.

* Conversion:
> `p = 0.20625` // probability to enroll per click <br>
> `N = 5000 * 0.08` // number of people to click the button <br>
> `SE = 0.0202`

The unit of analysis is a person who clicks the button and the unit of deviation is a cookie. The units are not the same, which indicates that the empirical and analytical variability do not coincide. However, they are close enough to suggest these will not be very divergent.

* Retention:
>`p = 0.53` // probability of payment per enrollment <br>
>`N = 5000 * 0.08 * 0.20625` // enrollment probability <br>
>`SE = 0.0549`

The unit of analysis is a person who has enrolled and the bypass unit is a user-id. As the units are not the same, the variability does not coincide. But as it is very likely that each person will only enroll once, variability must be approximated.

* Conversão líquida:
>`p = 0.1093125` // Probability of pay per click <br>
>`N = 5000 * 0.08` // Number of people to click the button <br>
>`SE = 0.0156`

Here the units of analysis and deviation are the same and, therefore, the variability coincides. Therefore, we must have the most precise analytical variability.


### Sample size

The following calculations were made using the [online calculator](http://www.evanmiller.org/ab-testing/sample-size.html)
with `alfa` = 0.05 and 1 - `beta` = 0.2. 

* Conversion:
With the conversion probability equal to 0.20625 and the minimum detectable effect (`d_min`) equal to 0.01, the number of samples given by the calculator is equal to 25835. Since we have two groups and the probability of clicking is 0.08, we have that the number of views (`V`) required is equal to:  <br>
>`V = 25835 * 2 / 0.08 = 645875`

* Retention:
With the probability of retention equal to 0.53 and `d_min` = 0.01, we have that the number of samples given by the calculator is 39155.
As we want the enrollment probability for both groups, we have:  <br>
>`V = 39155 * 2 / 0.08 / 0.20625 = 4741212`

* Net Conversion:
With the conversion probability equal to 0.1093125 and `d_min` = 0.0075, we have that the number of samples given by the calculator is 27413. For two groups and with the probability of click equal to 0.08 we have:  <br>
>`V = 27413 * 2 / 0.08 = 685325`

In this case, we will disqualify Retention because, to get 4.7 million page views, we would need 119 days with 100% of the traffic directed to the experiment, which does not sound reasonable.


### Duration x Exposure


If we consider 100% of the flow for the experiment, we would need 18 days to get the results:  <br>
`685325 / 40000 = 17.13`

The risk is low as a radical drop in enrollments is not expected.
However, if we redirect only 60% of the traffic to the experiment, we will have:  <br>
`685325 / (40000 * 0.6) = 28.555`

In other words, 29 days, which is a reasonable time and with less risk, since it does not use all the traffic.



### Sanity Check

* Number of cookies:

Control: `345543`  <br>
Experiment: `344660`

Considering the binomial distribution with the probability of 0.5, the standard error must be: `SE = sqrt (0.5 * 0.5 / N1 + N2)`
The margin of error will be `SE * 1.96`
The lower limit will be `0.5 - ME` and the upper limit will be` 0.5 + ME`
Observed is the number of the control over the sum of the control and the experiment

>`SE = 0.0006018` <br>
>`ME = 0.0006018 * 1.96 = 0.001179` <br>
>`LS = 0.5 - 0.001179 = 0.4988` <br>
>`LI = 0.5 + 0.001179 = 0.5012` <br>
>`Observado = 345543 / (345543 + 344660) = 0.5006`

* Number of clicks:

Control: `28378 ` <br>
Experiment: `28325`

>`SE = 0.002099` <br>
>`ME = 0.0006018 * 1.96 = 0.004116` <br>
>`LS = 0,5 - 0.004116 = 0.4959` <br>
>`LI = 0,5 + 0.004116 = 0.5041` <br>
>`Observado = 28378/ (28378 + 28325) = 0.5005`

* Click probability:

Control group click probability = `28378 / 345543 = 0.082126` <br>
Experiment group cookies = `344660`

>`SE = sqrt(0.082126 * (1-0.082126) / 344660) = 0.000468` <br>
>`ME = 1.96 * 0.000468 = 0.00092` <br>
>`LI = 0.082126 - 0.00092 = 0.0812` <br>
>`LS = 0.082126 + 0.00092 = 0.0830` <br>
>`Observado  = 28325 / 344660 = 0.082182`


### Size effect tests

To calculate a click probability, for example, the numerator (`X`) is the number of clicks and the denominator (`N`) is the number of cookies. In `P_pooled`, the numerators of the control and experiment groups (1 and 2, respectively) are added.

 `P_pooled = (X1 + X2) / (N1 + N2)`  <br>
 `SE_pooled = sqrt(P_pooled * (1 - P_pooled) * (1 / N1 + 1 / N2))`


The difference in the probabilities is given by:  <br>
 `D = X2 / N2 - X1 / N1`


Thus, the confidence interval occurs with the margin of error in the middle and the lower and upper limits as follows:  <br>
 `ME = SE * 1.96` (95% Confidence interval)  <br>
 `LI = D - ME`  <br>
 `LS = D + ME`


* Conversion:
To calculate the pooled probability of conversion, the numerator is the number of registrations and the denominator is the number of clicks on the _'start free trial'_ button.

>`P_pooled = (3725 + 3423) / (17293 + 17260) = 0,.2086` <br>
>`SE_pooled = sqrt(0.2086 * (1-0.2086) * (1 / 17293 + 1 / 17260)) = 0.00437` <br>
>`ME = 0,00437 * 1,96 = 0,00857` <br>
>`D = 3423/ 17260 - 3785 / 17293 = -0.02055` <br>
>`LI = -0.0291` <br>
>`LS = -0.0120`

The range does not contain 0, so the metric is statistically relevant. The range also does not contain 0.01 or -0.01 (`d_min`), so it is also practically significant.

* Net conversion:
To calculate the pooled probability of net conversion, the numerator is the number of users who remained enrolled after 14 days and the denominator is the number of clicks on the _'start free trial'_ button.

>`P_pooled = (2033 + 1945) / (17293 + 17260) = 0.1151` <br>
>`SE_pooled = sqrt(0.1151 * (0.1151) * (1 / 17293 + 1 / 17260)) = 0.00343` <br>
>`ME = 0,00343 * 1.96 = 0.00672` <br>
>`D = 1945 / 17260 - 2033 / 17293 = -0.0048` <br>
>`LI = -0.0116` <br>
>`LS = 0.0019`

The range contains 0, so it is not statistically significant and, therefore, is practically not significant.

### Signal testing

Using the [online calculator](http://www.evanmiller.org/ab-testing/sample-size.html) to do the signal tests, we have:
* For conversion, in the experiment group we have 4 days of success in a total of 23 days. For the 0.5 sign test the calculator gives the `p-value` of 0.0026. As this is less than the `alpha` (0.05), the change is statistically significant.
* For net conversion, in the experiment group we have 10 days of success in a total of 23 days. For the 0.5 sign test the calculator gives us the `p-value` of 0.6777. As this is larger than the `alpha`, the change is not statistically significant.

### Results summary

The results of the effect and signal size tests are consistent. As previously described, a drop in the conversion of the experiment group was expected, and it did happen. There was also a drop in net conversion, but this is not practically significant. The null hypothesis, in this case, is that there are practically no significant changes in the two assessment metrics and, therefore, we will not be able to reject it.

Bonferroni's correction was not used, as it is adequate to reduce the chances of false positives (type I errors) in experiments in which any practically significant metric can validate the hypothesis. In the case of our experiment, as we need the results of all metrics, the risk of false negatives (type II errors) increases as the number of metrics increases and, therefore, it is not necessary to control false positives.


### Recommendation

This experiment was developed to filter the most engaged students, in order to improve their experience and optimize the time of the mentors without significantly decreasing the number of students who remain enrolled. After analyzing the results, we realized that there was a statistically and practically significant decrease in conversion, which is good and was expected. The net conversion does not show a statistically significant change, but its confidence interval includes the negative value of the limit of practical significance. This means that we cannot be sure of this effect at the 95% confidence level. In other words, it is possible that the number of students who remain enrolled for more than 14 days has decreased in a way that would significantly influence the company's profit. For this reason, my recommendation is not to launch this experiment and to start new experiments.

### Subsequent experiment

For the next experiment, I would try an approach known as _onboarding_. This is the term to describe the first contact with the customer, in which we try to deliver initial value to him and make his eyes sparkle. _Onboarding_ is to take advantage of the customer's initial purchase energy to make them engage. This can be done in several ways, but the focus here is on delivering the first value. In this case, it would be very interesting to create an institutional video explaining what Udacity is, showing its position in the market, bringing a light portfolio of partners and highlighting its strengths in comparison to competitors. It is also valid to show cases of success, in which certified students tell how their apprenticeships helped them to overcome barriers, to get better jobs or better salaries. Statistics can serve as support for support cases, showing the relationship of students who completed their studies with the improvement of professional life, satisfaction rates, etc. Anyway, the idea is to make the eyes of the newly enrolled student sparkle, motivating him to be part of the successful team of certified students from Udacity.
When adding this video as the first video of the course, we do not risk reducing clicks or enrollments. It is expected that the number of students enrolled for more than 14 days will increase significantly.

* Initialization: After enrollment, students will be randomly divided between the control and experiment groups. In the experiment group the institutional video will appear as the first video of the course, in the control group it will not.
* Null hypothesis: The number of students who remain enrolled after 14 days will not increase significantly.
* Deviation unit: The deviation unit will be the user-id, as the experiment changes after registration.
* Invariant metrics: Clicks, cookies and user-ids can be used as invariant metrics in this case.
* Valuation Metrics: The valuation metric would be Net Conversion. A statistically significant increase would mean success of the experiment.

If a statistically significant and practically significant increase in retention is observed, the experiment will be launched.














