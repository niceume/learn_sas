Creating Distibution
====================

Probability P(X < k) 
1. "normal distribution" :  cdf("norm", x, mean, std )
2. "standardized normal distribution"  :  PROBNORM(z)
3. "binomial distribution" :  cdf("bino", n, p, k), PROBBNML(p, n, k)
4. "poisson distribution"  :  cdf("POISSON" , n, lambda )

* normal distribution is continuous. 
* binomial & poisson are discrete. Therefore be careful about inclusion.

z score
1. z score of standardized normal distribution :  PROBIT(p) (p is a cumulative probability. )



Creating Random Variable X and its distribution.
================================================

When creating random variable plot, dealing with X as discrete (categorical). Therefore use Barchart.

Throw three dices. Let those sum X.

~~~ SAS
/* Frequency or probability not known yet */
data dice ;
do x = 1 to 6 ;
 do y = 1 to 6 ;
  do z = 1 to 6 ;
   sum = x + y + z;
   output;
  end;
 end;
end;
run;

PROC SGPLOT data=dice;
 vbar sum;
RUN;

PROC FREQ data=dice;
 tables sum;
RUN;


/* Already know each probability of random variable.*/
DATA allergy;
 do i = 0 to 200;
  prob = pdf("bino", i, 0.8, 200);
  output;
 end;
RUN;

PROC SGPLOT data=allergy;
 scatter x = i y = prob ;
 pbspline x = i y = prob ;
RUN;
~~~ 


Random Sampling
===============

By using PROC SURVEYSELECT, we can select data randomly.


PROC SURVEYSELECT
-----------------

The FREQ procedure produces one-way to n-way frequency and contingency (crosstabulation) tables.  
In another word, the procedure describes "the relationship between two categorical characteristics".

* PROC SURVEYSELECT statement
  + options METHOD= specifies what kind of randomness method to use. 
  + options DATA= specifies the input dataset.
  + options OUT= specifies the output dataset.
  + options SAMPLESIZE= specifies how many observatoins to select.
    + options SAMPLERATE= specifies the proportion sampled. Can be used instead of SAMPLESIZE.
  + options SEED= specifies the seed to create random values.


~~~ SAS
/* Select 5 values from 1 to 268. */
DATA num;  /*Prepare data from 1 to 268.*/
  do wordnumber = 1 to 268;
    output;
  end;
RUN;
PROC SURVEYSELECT method=srs data=num out=five sampsize=5; /*Random selection*/
RUN;
~~~

