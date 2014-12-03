Inference statistics (two independent variables)
===============================================

Interpretation of two means is like this.

* The mean VARIABLE of A (mean= -- , SD= -- ) is higher than B (mean= --, SD= --)
  + (descriptive statistics)
* ( The mean difference is --, 95% CI : --, -- )
  + (inferel statistics : confidence interval)
* The observed difference is statistically significant.(test-statistic= -- , p= -- )
  + (inferel statistics : hypothesis test)


Interpretation of two proportions is like this.

* Exposure-A are more likely than Exposure-B to do outcome-Z ( --% vs --%). 
  + (Exposure-A result in outcome-Z More frequently )
  + OR
  + Exposure-A do outcome-Z at hgher rate than Exposure-B ( --% vs --%).
* There is a significant association between Exposure and Outcome.
  + (inferel statistics : hypothesis test)
* Exposure-A are --% less likely than exposure-B to outcome-z. (RR = ---, CI=--,-- )
  + OR
  + The risk (or the odds) of outcome-Z among exposure-A is --% lower than the risk (or the odds) among exposure-B. (RR or OR= ---, CI=--,-- )



These interpretations can be based on procedures as follows.

Confidence interval (two numeric variables)
-------------------------------------------

The main point is how to calculate marginal error.

Marginal error can be calculated as follows.

Is there assumption????

~~~ SAS
/* Confidence interval */
DATA temp;
 n1 = 589;  /* sample size 1 */
 n2 = 65;  /* sample size 2 */
 x1b = 2.5661;  /* sample mean 1 */
 x2b = 3.2769;  /* sample mean 2 */
 sd1 = 0.8505;  /* sample std 1 */
 sd2 = 0.7500;  /* sample std 2 */
 square_sp = ((n1-1) * (sd1**2) + (n2-1) * (sd2 **2)) / (n1+n2-2) ;
 sp = sqrt(square_sp);  /* pooled std */

 se = sp * sqrt( 1/n1 + 1/n2);
 umd = (x1b - x2b) + tinv( 0.975, (n1+n2-2)) * se;
 lmd = (x1b - x2b) - tinv( 0.975, (n1+n2-2)) * se;
PROC PRINT;
RUN;
/* -0.49492 -0.92668 */ 
/* We can be 95% confident that the difference between VARIABLEs(SBPs) of A and B is somewhere between -0.4942 and -9.2668 */
~~~

This confidence interval can be calcuted in one step by using "PROC TTEST"

~~~ SAS
/* Confirmation */
PROC TTEST data=fev alpha=0.05;
 var fev;
 class smoke;
RUN;
/* The same result. */
~~~



Confidence interval (two proportions : dichotomous)
---------------------------------------------------

In order to compare two proportions, here we use risk ratio or odds ratio. And calculate their estimates and confidence intervals.


~~~ SAS
/*       D(+) D(-)       */
/* E(+)   14  9033  9047 */
/* E(-)   29  3661  3690 */

/*RR*/
DATA temp;
 rr = ( 14/9047) / (29/3690) ; 
 zval = probit(0.995 );
 urr = exp( log(rr) + zval * sqrt( 1/14 - 1/9047 + 1/29 - 1/3690) ) ;
 lrr = exp( log(rr) - zval * sqrt( 1/14 - 1/9047 + 1/29 - 1/3690) ) ;
PROC PRINT;
RUN;

/*OR*/
DATA temp;
 or = ( 14 * 3661 ) / (9033 * 29);
 zval = probit(0.995 );
 uor = exp( log(or) + zval * sqrt(1/14 + 1/9047 + 1/29 + 1/3690 ) ) ;
 lor = exp( log(or) - zval * sqrt(1/14 + 1/9047 + 1/29 + 1/3690 ) ) ;
PROC PRINT;
RUN;
~~~

The same thing can be done by one step by using "PROC FREQ" & "table <var1> * <var2>  / chisq relrisk "

~~~ SAS
/*       D(+) D(-)       */
/* E(+)   14  9033  9047 */
/* E(-)   29  3661  3690 */

/*Confirmation*/
DATA q1confirm;
 input seatbelt injury count;
 datalines ;
 0 0 14
 0 1 9033
 1 0 29
 1 1 3661
 ;
RUN;
PROC FREQ data=q1confirm  ;
 weight count;
 tables seatbelt * injury /nocol nopercent expected chisq relrisk alpha=0.01;
RUN;
/*The same answer.*/
~~~



Hypothesis test (Two independent variables)
===========================================

Two sample t-test (Test for two numeric variables)
--------------------------------------------------

~~~ SAS
/* Step1 : Hypothesis */
/* H0 : mu0 = mu1 */
/* H1 : mu0 not= mu1 */

/*Step2 : Assumption */
PROC UNIVARIATE data = fev;
 var fev;
 class smoke;
 histogram fev/normal;
 probplot fev;
RUN;
/* Simple random samples. Sample sizes are large enough (589 vs 65). Independent samples. */
/* Equal variance. */

/*Step3 : statistic*/
/* From the previous result, x1b = 2.5661 , x2b = 3.2769 , sd1 = 0.8505, sd2 = 0.7500 */
DATA temp;
 n1 = 589;
 n2 = 65;
 x1b = 2.5661;
 x2b = 3.2769;
 sd1 = 0.8505;
 sd2 = 0.7500;
 square_sp = ((n1-1) * (sd1**2) + (n2-1) * (sd2 **2)) / (n1+n2-2) ;
 sp = sqrt(square_sp);
 tstatistic = ( x1b - x2b - 0 )/ ( sp * sqrt(1/n1 + 1/n2) ) ;
PROC PRINT;
RUN;
/* t statistic is -6.46533 */

/*Step4 : pvalue */
DATA temp;
 set temp;
 pvalue = 2 * cdf( "t", -6.46533, (n1 + n2 -2));
PROC PRINT;
RUN;
/* p value is 1.982E-10 */

/* Step5 : conclusion */
/* At the 5% significance level, there is a significant difference in mean FEVs between smoking children and nonsmoking children.*/
/* The important point in interpretation is that we compare means. */
~~~~

The same thing can be done by one step by using "PROC TTEST"

~~~ SAS
/* Confirmation */
PROC TTEST data=fev alpha=0.05;
 var fev;
 class smoke;
RUN;
/* The same result. */
~~~


Chi-squared test (Test for two Categorical variables)
----------------------------------------------------

Comparing two population proportions of two variables.

* HYPOTHESIS TEST : Chi-square test
* Confidence Interval : RR(Risk ratio) or OR(Odds ratio)


~~~ SAS
/* Step1 : Hypothesis */
/* H0 :  p1 = p2. In English there are no association between A and B. */
/* H1 :  p1 not= p2 */

/* Step2 : Assumption */
PROC FREQ data=sb;
 tables seatbelt * injury /nocol norow nopercent expected;
RUN;
/* Two independent samples. Simple random samples. */
/* Expected frequencies are 1 or greater. */
/* At most 20% of the expected frequencies are less than 5. */

/* Step3 : Calculate test statistics. */
DATA temp;
 chisq = (14-12.457)**2 / 12.457 + (9033-9016.5)**2 / 9016.5 + (29-30.543)**2 / 30.543 + (3661-3677.5)**2 / 3677.5 ;
PROC PRINT;
RUN;
/* chisq = 0.37330 */

/* Step4 : P value*/
DATA temp;
 pvalue = 1 - cdf("chisq", 0.37330, 1 ); 
PROC PRINT;
RUN;

/* Step5 : Conclusion */
/* alpha = 0.01 */
/* p value is 0.54. This is much larger than the alpha. */
/* At the level of 5% significance, there is not sufficient evidence to believe that there is an association between A(seatbelt) and B(injury)*/
/* The important point in interpretation is that we checked the association . */
~~~


The same thing can be done by one step by using "PROC FREQ" & "table <var1> * <var2>  / chisq "

~~~ SAS
/*       D(+) D(-)       */
/* E(+)   14  9033  9047 */
/* E(-)   29  3661  3690 */

/*Confirmation*/
DATA q1confirm;
 input seatbelt injury count;
 datalines ;
 0 0 14
 0 1 9033
 1 0 29
 1 1 3661
 ;
RUN;
PROC FREQ data=q1confirm  ;
 weight count;
 tables seatbelt * injury /nocol nopercent expected chisq relrisk alpha=0.01; /*row percents should be printed out.*/
RUN;
/*The same answer.*/
~~~

