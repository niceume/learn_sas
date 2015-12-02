Linear Regression
==================

Linear regression can do more than than t-test.
One thing linear regression doesn't cover it cannot compare meanso of two groups with different variations.


Here, various models will be shown and its relatoinship with t-test.


Linear regression and t-test are not so much as mathematically identical but the intepretaion is same. As a result, you can understand they do the same thing.


Compare arighmetic means
========================
1. FEV = beta0 + error
---------------------

### meaning of coefficient
* beta0 is mean FEV


### Null Hypothesis
* T-test H0 : mu = 0 (usully compare with some value.)
corresponds to 
* Beta   H0 : beta0 = 0 (meaningless.)


### SAS procedures
* PROC MEANS (with clb option.)
* PROC TTEST (var FEV;)
* PROC REG (model FEV = / clb; )
* (PROC GLM seems to be also available, but I dont know.)


2. FEV = beta0 + beta1 MALE + error
-----------------------------------

### meaning of coefficient
* beta0 = mean FEV when FEMALE
* beta1 = difference of FEV btw MALE and FEMALE (because (beta0 + beta1) is mean FEV when MALE)

### Null Hypothesis
* T-test H0 : mu0 = mu1 (mu0 = population mean of male=0, mu1 = population mean of male = 1 )
corresponds to 
* Beta H0 : beta1 = 0 (population mean difference is zero or not.)

All the coefficient beta has null hypothesis.
* Beta H0 : beta0 = 0 (meanningless. )

### SAS procedures
* PROC TTEST ( var FEV; class MALE; )
* PROC REG (model FEV = MALE / clb; )


Compare geometric means
=======================

When to use logarighmic transformation ?? 

It's when 
* there is an outlier or extreme value.
* the underlying distribution is right skewed distribution.

3. log(FEV) = beta0 + error
---------------------------

### meaning of coefficient
* exp(beta0) = geometric mean FEV

### Null Hypothesis
* Beta  H0 : beta0 = 1 (meaningless.)
no comparable t-test.

### SAS procedure
a. create new variable logFEV 
b. PROC REG (model logFEV = /clb; )
c. Exponential the point estimate and CIs.


4. log(FEV) = beta0 + beta1 MALE + error
----------------------------------------

### meaning of coefficient
* exp(beta0) = geometric mean FEV when FEMALE 
* exp(beta1) = geometric mean ratio(GMR) of FEV btw MALE and FEMALE (because exp(beta0 + beta1) is geometric mean FEV when MALE)

### Null Hypothesis
* Beta1  H0 : beta1 = 0
* Beta0  H0 : meaningless

### SAS procedure
a. create new variable logFEV 
b. PROC REG (model logFEV = MALE /clb; )
c. Exponential the point estimate and CIs.

### Additional 
If you want to calculate the point estimate and CI of geometric mean FEV of MALE, you have to create new model,
* log(FEV) = beta0 + beta1 FEMALE + error
because, confidence interval of (beta0 + beta1) which iS GM(FEV) of males cannot be calculated directly.(Point estimate is able to be calculated.)


Paired variables (paired samples)
=================================
5. FEVpost - FEVpre = beta0 + error 
------------------------------------

### meaning of coefficient

Basically coefficient means the difference of means between groups of its variable 1 unit apart. And the p value output by SAS is calculated under the hypothesis that the difference is 0.


* beta0  = mean change in FEV


### Null Hypothesis
* t-test H0 : mu0 = mu1 (mu0 is mean FEVpre, mu1 is mean FEVpost) 
corresponds to
* beta0 H0   : beta0 = 0 (meaningful)


### SAS procedure
* PROC means (after creating new variable, FEVchane )
* PROC REG   (after creating new variable, FEVchane )
* PROC TTEST (paried FEVpre FEVpost;)



6. FEVpost - FEVpre = beta0 + beta1 MALE + error
------------------------------------------------

### meaning of coefficient
* beta0 = mean change in FEV among FEMALEs
* beta0 + beta1  = mean change in FEV among MALEs
* beta1 = difference in mean FEV change btw two groups of MALEs and FEMALEs


### Null Hypothesis
* t-test H0 : change_mu0 = change_mu1
corresponds to   
* beta1 H0   : beta1 = 0 (meaningful)

All the coefficient beta has null hypothesis.
* beta0 H0  : beta0 = 0 (change in FEV among FMALEs is 0 or not.)


### SAS procedure
* PROC TTEST  (class MALE)
* PROC REG   (model difference = MALE / clb)


Contiuous vs continuous 
=======================

T-test can only deal with categorical variable of two values. 


7. FEV = beta0 + beta1 (AGE - 17) + error
-----------------------------------------

### meaning of coefficient
* beta0 = mean FEV of 17 yo.
* beta1 = difference in FEV between two groups of people that are 1 year apart.

### Null Hypothesis
* beta0 = 0
* beta1 = 0

### SAS procedure
* PROC REG 

### Additional analysis
Calculating correlation is also preferred.
The good point of correlation is that it is no affected by units.

* PROC CORR (var FEV AGE)


Modified by another variable
=============================

8. FEV = beta0 + beta1 (AGE - 17) + beta2 FH + error
-----------------------------------------------------

### meaning of coefficient 
When FH = 0

FEV = beta0 + beta1 (AGE - 17)

When FH = 1

FEV = (beta0 + beta2) + beta1 (AGE - 17)

* beta0 = mean FEV of 17 yo without FH
* beta1 = difference in mean FEV btw two groups of people that are 1 year apart.(With FH or without FH is the same )
* beta0 + beta2 = mean FEV of 17 yo with FH
* beta2 = difference in mean FEV btw two groups of people that have FH and that do not.

One important notification.
*  ** This beta1 could be affected by FH status**


9. FEV = beta0 + beta1 (AGE - 17) + beta2 FH + beta3 (AGE-17) FH + error
------------------------------------------------------------------------

### When FH = 0

* FEV = beta0 + beta1 (AGE - 17)

### When FH = 1

* FEV = beta0 + beta1 (AGE - 17) + beta2 * 1 + beta3 (AGE - 17) * 1 + error
*     = (beta0 + beta2 ) + (beta1 + beta3) (AGE - 17)  + error

### meaning of coefficients 
* beta0  : mean FEV of 17 yo people without FH
* beta1  : Among people without FH, change rate by 1 year is beta1.
* beta2  : diference in mean FEV between groups of 17 yo peole with FH and without FH.
* beta1 + beta3   : Among people with FH, change rate is beta1 + beta3. The rate of change by 1 year is affected  by beta3.


### Null Hypothesis
* beta0 H0 : beta0 = 0  
* beta1 H0 : beta1 = 0 
* beta2 H0 : beta2 = 0
* beta3 H0 : beta3 = 0


### SAS procedure
* PROC REG 




