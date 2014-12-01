Sample size
===========

Definition
----------

* Type I error : The error that the null hypothesis is rejected when it is in fact true.
* Type II error : The error that the null hypothesis fails to be rejected when it is in fact false.
* Significant Level : The probability of making a type I error.
* Power : 1 – ( The probatiliby of making Type II error)


One sample mean
---------------

The sample size/power analysis of "one sample t-test."

Use "onesamplemeans" statement.

~~~ SAS
proc power;
 onesamplemeans
 nullmean = 55 /* Null Hypothesis */
 mean = 50  /* Assumed Hypothesis */
 ntotal = 21 /* sample size*/
 stddev = 6.8
 power = .
 alpha = 0.01
 ;
run;
~~~


Two sample means
----------------

The sample size/power analysis of "two sample t-test."

Use "twosamplemeans" statement.

~~~ SAS
proc power;
 twosamplemeans
 /* TEST = diff /*This is default option.*/ */
 groupmeans = (0 5)  /* Assumed Hypothesis. Null hypothesis is zero difference. */
 ntotal = .  /* sample size */
 stddev = 17
 power = 0.90
 alpha = 0.01
 ;
run;
~~~

Two paired sample means
-----------------------

The same way as "one sample mean".


Two smple proportions
---------------------

The sample size/power analysis of "chi-square test."

Use "twosamplefreq" 

~~~ SAS
proc power;
 twosamplefreq
 test=pchi
 groupproportions  = (0.2 0.1)
 npergroup = .
 power = 0.80
 alpha = 0.05
 ;
run;
~~~


Two dependent sample proportions
--------------------------------


The sample size/power analysis of "McNemer test."

Use "pairedfreq" 


~~~ SAS
proc power;
pairedfreq dist=normal
method=connor
discproportions= 0.2532 | 0.2025
npairs=.
power=0.80
alpha=0.05
; 
run;
~~~


