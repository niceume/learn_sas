Inference statistics (two paired variables)
===========================================

* Two paired variables are variables which have correspondence with each other. 
* An example is like pre-treatment and port-treatment. Each individual has 


### Can I use PROC GLM ? 

* Yes, but only after taking difference.
* If you do not take difference, and if you code two occasions as 0 and 1, you end up with treating two variables independent. 
* The CI of slope is the same as the result of usual TTEST.
  + Under the setting of paired TTEST, there are n individuals and 2 occasions.
  + But usual TTEST assumes, there are 2*n individuals, and assign exposure to n people and control to n people. 
  

### Interpretation of hypothesis test for paired variables.

(Paired T-test)
* The mean VARIABLE of A (mean= -- , SD= -- ) is higher than B (mean= --, SD= --) (mean difference is ---, 95% CI= --, -- )
  + descriptive statistics
* There is sufficient evidence to believe that the OUTCOME difference is significant. (T= ---, p=---)
  + hypothesis test


(McNemer's Test)
* -- % did --. -- % did --.
  + descriptive statistics
* There is sufficient evidence to believe that OUTCOME differs on two EXPOSUREs. (S= ---, p=---)
  + hypothesis test


Compare Two paired means (same as One sample t-test)
----------------------------------------------------

This is the same as one sample mean t-test.

~~~ SAS
DATA ex1_145;
 input id normal glaucoma diff;
 datalines ;
 1 484 488 -4
 2 478 478 0
 3 492 480 12
 4 444 426 18
 5 436 440 -4
 6 398 410 -12
 7 464 458 6
 8 476 460 16
 ;
RUN;

PROC TTEST data=q1_145 alpha=0.1;
 paired normal * glaucoma;
RUN;
~~~



compare two correlated proportions (McNemer Test)
-------------------------------------------------


The McNemar test assesses the significance of the difference between two correlated proportions, 
such as might be found in the case where the two proportions are based on the same sample of subjects
or on matched-pair samples


|   |  XY |      |      | 
|---|-----|------|------|
|XX | S   | S'   | Total|
|---|-----|------|------|
|S  |   2 |   9  |  11  |
|S' |   1 |   3  |  4   |
|---|-----|------|------|
|Total | 3 |  12 |  15  |

+ p1hat = 3 / 15 = 0.2
+ p2 hat = 11 / 15 = 0.733


* Step 1 : Hypothesis
  + H_0:  p_1=p_2
  + H_1:  p_1not=p_2
* Step 2 : Assumption
  + The sum of off-diagonal cells >= 10.
* Step 3 : Test statistic
  + z = (b-c)/sqrt(b+c)
* Step 4 : p-value
  + 2 * P( z > 2.53 ) = 0.011
* Step5 : Conclusion
  + There is enough evidence to believe that there is a difference between the two population proportions of pass between before session and after session. 


By using "PROC FREQ" with table <var1> <var2> / agree; option, You can do Mcnemer test.

~~~ SAS
DATA sunscreen;
 input DrugXX $ DrugXY $ count;
 datalines;
 satisfactory  satisfactory 2
 satisfactory  unsatisfactory 9
 unsatisfactory  satisfactory 1
 unsatisfactory  unsatisfactory  3 
 ;
RUN;
PROC FREQ data=sunscreen order=data;
 weight count;
 tables DrugXX * DrugXY / agree; /* Do McNemer test. Calculate kappa.*/
RUN;

/* The same result. */
~~~

