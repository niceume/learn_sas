Analysis Flow
=============

Import dataset
---------------

~~~ SAS
LIBNAME bioslib "C:\course\BIOS\Datasets";
ODS html close; ods html;
DATA asthma;
  set bioslib.asthma;
RUN;
~~~


Dataset overall characteristics
-------------------------------

~~~ SAS
/*Know variables*/
PROC CONTENTS VARNUM data=asthma; RUN;
PROC PRINT data=asthma; RUN;
~~~


Check duplicates
----------------

~~~ SAS
/*Check if there are duplicates*/
PROC SORT data=asthma nodupkey out=nodups_here dupout=duplicate_obs; 
/* duplicated observations go into the dataset, duplicate_obs. */
  by id name;
RUN;
PROC PRINT data=duplicate_obs;
RUN;
~~~


Before analysis, check variables you are interested in.
--------------------------------------------------------------------------------

Find out outliers or implausible values, normality, missing values.

~~~ SAS
/* Check mean and median.*/
/* Check outliers by max and min values.*/
/* Check missing values if there are.*/
/* Check normality if necessary. Magnitudes of skewness and Kurtosis are < 1. */
/* Check normality if necessary. Use histogram or normal probability plot. */
/* Even when it's not normal, when the dist is moderately normal and the sample size is large enough, CLT can be applied and the mean can follow normal distribution. */

PROC UNIVARIATE DATA=nodups_here;
 var fev;
 histogram fev / normal;
 probplot fev /normal;
RUN; 

PROC UNIVARIATE DATA=nodups_here;
 var fev;
 histogram fev / normal;
 probplot fev /normal;
 class sex;
RUN; 
~~~ 


* Categorical variable

~~~ SAS
/* Check missing values if there are.*/
PROC FREQ data=asthma;
  tables birthplace / list missing;
RUN;
~~~


Analysis
--------

Use t-test, chisquare, and so on .

