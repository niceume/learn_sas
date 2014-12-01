ANOVA
=====

Compare more than three or theree means
---------------------------------------

When you do ANOVA, you can use "PROC GLM" in SAS.

~~~ SAS
DATA ache;
 input brand relief;
 datalines ;
 1 24.5
 1 23.5
 1 26.4
 1 27.1
 1 29.9
 2 28.4
 2 34.2
 2 29.5
 2 32.2
 2 30.1
 3 26.1
 3 28.3
 3 24.3
 3 26.2
 3 27.8
 ;
RUN;

PROC GLM data=ache;
  class brand;
  model relief = brand;
  means brand; /* Just get means of groups & stds of groups.*/
RUN;
~~~


Output of ANOVA
----------------

* example output
|------------------------------------------------------------------
| Source | DF | Sum of Squares | Mean Square | F Value | Pr > F   |
|-----------------------------------------------------------------
| Model  | 2  | 66.7720000     | 33.3860000  | 7.14    | 0.0091   |
| Error  | 12 | 56.1280000     | 4.6773333   |         |          |
|Corrected Total|14 | 122.9000000|           |         |          |


|--------------------------------------------------------|
| R-Square | Coeff Var| Root MSE| relief Mean            |
|--------------------------------------------------------|
| 0.543303 | 7.751664 |2.162714 |27.90000                |


Background (F distribution)
---------------------------

* X ~ N( mu_x , sigma_x ^ 2)
* Y ~ N( mu_y , sigma_y ^ 2)
* sample size of A sample : n_x
* sample size of B sample : n_y
* Unbiased variance of A sample : U_x
* Unbiased variance of B sample : U_y

* ( ( U_x ^ 2 / sigma_x ^ 2 ) /  ( U_y ^ 2 / sigma_y ^ 2 ) ) ~ F( n_x -1 , n_y - 1)


Background (ANOVA) ??
---------------------

There are k groups. n_1, n_2 ... n_k. barx_1, barx_2 ... barx_k.

* SSR (between) = SUM of all obs of (group effect)^2
* SSE (within) =  SUM of all obs of (error)^2
  + Each ob = total average + group effect + error

Hypotheis : All the groups have the same population mean <= group effect of sample and error of each sample follow the same spread of normal distribution. (same population std. sigma_group = sigma_error)

* SSR / sigma_group ^2
* SSE / sigma_error ^2
follow chi-square distributions.

* ( SSR / sigma_group^2 / df of SSR ) / ( SSE / sigma_group^2 / df of SSE ) ~ F (df of SSR , df of SSE)
* = ( SSR / df of SSR ) / ( SSE / df of SSE ) 
* Therefore MSR / MSE ~ F( df of SSR , df of SSE )


