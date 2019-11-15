---
layout: post
title:      "Module 3 Project"
date:       2019-11-15 01:39:04 -0500
permalink:  module_3_project
---

## Project Background
For the Data Science Module 3 project requirements, we worked with the Northwind database--a free, open-source dataset created by Microsoft containing data from a fictional company. This project involves gathering information from a real-world database and use of statistical analysis and hypothesis testing to generate analytical insights to answer the main question, “Does discount amount have a statistically significant effect on the quantity of a product in an order? If so, at what level(s) of discount?”

## Experimental Method: 
The Data Scientist would go to the following steps to conduct the experimental design to answer the business questions.

### 1.	Making Observations: 
Northwind would like to administer pricing policies involving quantity discounts but the company's knowledge of these discounts is quite limited. Northwind recognizes that to be competitive, they need to meet customer expectations on quantity discounts and need data-driven decision-making in the development, implementation of a profitable discount schedule.
###  2.	Examine the Research: 
The Data scientist will work with the Marketing Manager and other Business stakeholders to review the current best practice guidelines from the economics and marketing literature in the quantity discounts to understand the metrics for optimization. This step includes the Data Science Framework iterative processes of obtaining the data, cleansing the data, exploring the data to understand the business problem/question and help finding the appropriate hypothesis test (test statistic) to use.

 * Stating the problem and understanding the statistical methods that will be used during the framing of the problem.           
 * After loading all the packages/libraries needed to work on the project, the initial step is to obtain the data by querying the SQLite3 Northwind database. The database table system object was queried to get all the tables within the Northwind database and loaded them all as DataFrames. However, an error was encountered while querying the Orders table which, is the linking table between the two features Order and Discounts and the rest of the other features located in the Customer, Products, and Employee tables. As a workaround, the Orders table was extracted manually by exporting the table as a file and then importing it as a dataframe.   
* Exploratory Data Analysis was conducted to have an understanding of the data types, distributions, and relationship between the variables. Summary statistics and Data visualizations like histograms, QQ plots, violin plots, box plots, scatter matrix, heat maps, and bar charts aided in understanding the data distribution, detecting outliers and selecting imputation methods needed to clean our data.  
* Data Selection:  A random sample distribution was created for the experimental (with. Discounts) group and control group (no discount).  Repeated random sample of our quantitative variables was obtained to help with the hypothesis testing and selecting features that are most relevant to our outcome variable.  
* Based on our data understanding, we select the statistical testing method that is appropriate for the problem or question we are trying to answer.   
          
###  3.	Hypothesis Testing Steps: 
Before we run any hypothesis test, we need to specify the two hypotheses: the null hypothesis (H0) and the Alternate Hypothesis (Ha). we start by assuming the null hypothesis which is "there is no difference between the quantity of product ordered with discounts or without discounts and that what we're seeing is just random probability that the difference is due to chance versus not. We are interested in disproving the null hypothesis.  
 * We set the Alpha Level ( 𝛼 ) to 0.05 or 5% significance level. Alpha is a threshold value used to judge whether a test statistic is statistically significant.
 * Identify the test statistic to be used to assess the truth of the null hypothesis.
 * Compute the p-value. The smaller the P-value, the stronger the evidence against the null hypothesis. If the probability level is less than our chosen alpha level, we reject the null hypothesis of equal means and conclude that the means are different.
 * Compare the p-value to the significance value alpha. If p<=alpha, that the observed effect is statistically significant, the null hypothesis is ruled out, and the alternative hypothesis is valid. The confidence limits of the difference allow us to put bounds on the size of the difference. If these limits are narrow and close to zero, we might determine that even though our results are statistically significant, the magnitude of their difference is not of practical interest.  

###  4.  Conduct the Experiment. 
After we explored the data, formulated the hypothesis, and decided on the correct hypothesis test to use, we executed the statistical test. To find out whether there is statistical evidence that the experimental and control group are significantly different, the Independent Samples t Test was used.  The Independent Samples t Test is a parametric test with the following assumptions:

 * The data are continuous (not discrete). We are checking for means of quantity i.e. continuous data.
 * The data follow the normal probability distribution.
 * The variances of the two populations are equal. (If not, the Welch Unequal-Variance T test is used.)
 * The two samples are independent. There is no relationship between the individuals in one sample as compared to the other and cannot influence the other group.
 * Both samples are simple random samples from their respective populations. Each individual in the population has an equal probability of being selected in the sample. 
  * No outliers
  * Roughly balanced design (i.e., same number of subjects in each group) are ideal. Extremely unbalanced designs increase the possibility that violating any of the requirements/assumptions threaten the validity of the Independent Samples t Test.
 * When one or more of the assumptions for the Independent Samples t Test are not met, we may want to run the nonparametric Mann-Whitney U Test instead.

###  5.	Analyze experimental results:  
We rejected the NULL hypothesis and have evidence that there are significant differences in the mean quantity of product between our control and experimental groups, so the next step, is to measure which level of discount is significant.
We've seen that there are 10 distinct discount amounts namely 0.05,0.10,0.20, 0.15,0.25,0.03, 0.02, 0.01,0.04, and 0.06.
We utilized the one-way ANOVA as the statistical test since we are comparing means for three or more populations and are testing only one factor i.e. to see whether the variations in the quantity or product ordered are due to the discount levels or whether the variations are purely random.
To use ANOVA, the following conditions must be present:
 * The populations of interest must be normally distributed.
 * The samples must be independent of each other.
 * Each population must have the same variance.

We know from our previous testing that our sample has unequal variances and that the distributions are not perfectly normal. ANOVA is robust to the homogeneity of variance violation and to data that are not normally distributed. Due to our sufficiently large random sample we proceeded with the test as we are assuming that the distribution of means calculated from repeated sampling will approach normality. We can consider normality as a given for us due to the central limit theorem for large samples.

###  6.	Draw conclusions: 
When we find strong enough evidence against the null hypothesis,(p value <= significance level) we reject the null hypothesis/the result is statistically significant. When we do not find evidence against the null hypothesis,(p value >= significance level) we fail to reject the null hypothesis/the result is not statistically significant.  However, ANOVA result alone does not tell us specifically which means were different from one another. To determine that, we would need to follow up with multiple comparisons (or post-hoc) tests to find out exactly at what discount amount the differences are.

We rejected the null hypothesis at the 5%, 15%, 20% and 25% discount levels when compared with the control group. In other words, when discounts of 5, 15, 20 and 25% are applied, we observe a statistically significant effect on the quantity of product ordered but not at 10%.    

The results of our test were significant, but we need to determine if the effect is so small that it has little consequence. Effect size is a quantitative measure of the magnitude of the discount (experimenter) effect. The larger the effect size the stronger the relationship between two variables.  

Our test has an effect size of 6.9 which is a large effect and the power of the statistical test =1, a High Statistical Power indicating that the test correctly rejected the false null hypothesis. There is a small risk of committing Type II errors. Our test shows that Discounts have an effect in boosting sale volumes.

### 7.	Interpret Findings:   
#### Does discount amount have a statistically significant effect on the quantity of a product in an order? If so, at what level(s) of discount?
* Discount has a significant effect on quantity ordered.  

#### What levels of discounts have a statistically significant effect on the quantity of a product in an order?
* There is enough evidence to support the claim that there is a difference in mean quantity of orders between discount levels. 5%, 15%, 20%, and 25% discount levels have a significant effect on quantity of product ordered.
* We also looked at other features that may have an effect on quantity ordered and discount motivations such as product categories, salespersons granting discounts, and customers.
* There are certain products that are more strongly in the customer focus. Knowing these primary products will help set competitive price. Knowing your secondary products could give you more flexibility with pricing.  

#### Is there a difference in the mean quantity of order between product categories?
* There is NOT enough evidence to support the claim that there is a difference in mean quantity between product categories.  

#### Is there a difference in the mean quantity of order between product categories and discounts?
* There is NOT enough evidence to support the claim that there is a difference in mean quantity between product categories and whether or not a discount is applied.  
#### Is there a difference in the mean quantity of order between salespersons and whether or not discounts are applied?
* There is NOT enough evidence to support the claim that there is statistically significant difference in average quantity of product ordered between salespersons.   
* 
#### Is there a difference in the Average Total Sales between Countries?  

* There is a statistically significant difference in the average Total Sales between customers from Austria & Argentina, Austria & Brazil, Austria & Finland, Austria & France, Austria & Italy, Austria & Mexico, Austria & Spain, Austria & UK, Austria & Venezuela. While Austria has the largest Total sales followed by Portugal, the test however didn't find a significant difference in the mean Total sales between these two countries.  

I hope the findings above could provide a baseline information to help the Marketing Team in discount pricing and help guide further exploration to determine where discount has an effect in boosting volume of sales and profit. Further study is recommended to determine the variety of motivations for quantity discounts to help provide guidelines to the organization in designing quantity discount schedules and set out the most fruitful directions for future research.


### 8.	 Recommendations:
While discounts significantly impact the quantity of products ordered and increase overall sales volume, it would be advantageous to understand the true impact of giving a discount to ensure profitability when offering discount pricing. We recommend that other metrics impacting discount pricing methods be considered for future research.
 * How can we identify customers with below-average margins? How could we segment discount offers to different types of customers (first-time, dormant, repeat) to entice new sales without losing out on the margins of sales?
 * What type of discount (e.g. volume discount, event or seasonal discount, free shipping) has a higher impact on the volume of sales?
 * Analyze granted discount frequencies and how the Sales Team is using the full range of percentages to understand how to link discounts with sales incentives. In addition to analyzing which salesperson sells the most, further focus on profit analysis is equally important.
 * Does giving a 20% discount mean the company has to sell 20% more product to make up for what the company gave away? How much more will Northwind need in sales volume to generate the same amount of gross profit dollars as before?
 * Is there enough market demand to generate 20 percent more sales with a 10% drop in price? Is the discount really necessary?



