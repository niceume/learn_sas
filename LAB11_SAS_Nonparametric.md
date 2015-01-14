Non parametric tests
====================

This nonparametric test can be used

* When the sample size is small 
* When the underlying distribution is not normal distribution.


One sample test
===============

* When the distribution is symmetric (but not normal), Wilcoxon singed rank test is used.
* When the distribution is non-symmetric, sign test is used.

1. Wilcoxon singed rank test
----------------------------

(e.g.) Null Hypothesis mu = 157   
Xi        143  138  169  152  149  150  135   
di=Xi-mu  -14  -19   12   -5   8    -7   -22   
|di|       14   19   12   5    8    7    22   
Rank       5     6   4    1    3    2    7   
Signed rank -5  -6   4    -1   3    -2   -7     =>  Used when Wilcoxon signed test.


Under null hypothesis
  + Sum of positive ranks should be : 14
  + Sum of negative ranks should be : -14
  + However in real, the sum of positive ranks is 7.
  + The test statistic (W) is calculated 7 - 14 = -7

~~~ SAS
PROC UNIVARIATE
~~~


2. Sign test
------------

In the above example, there are 5 negative values. The test statistic(D) is 5.

Under null hypotesis, this statistic follows    
D ~ Bi(7, 1/2)

Calculate 2P.


~~~ SAS
PROC UNIVARIATE // can output.
~~~


Two samples test
===============

* When the distributions are normal with the same variance, use pooled t-test
* When the distributions are normal with different variances, use unpooled t-test
* When the distributions are not normal(skewed) but the same, use Mann-Whitney.
* When the distributions are not normal or the same, consult a statistician.


3. Mann-whitney test
--------------------

Two sample non parametric test.

(Procedure)
Create rank based on two groups.  
The sum of ranks in one group can be the test statistic.  


~~~ SAS
PROC NPAR1WAY;
 class category;
 var value;
 exact;
RUN;
~~~


Three samples test
===================
4 Kruskal–Wallis test
---------------------

Non parametric version of ANOVA.



