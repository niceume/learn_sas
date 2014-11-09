Test of proportion (Test for Categorical variable)
--------------------------------------------------

When comparing proportions of two groups by t-test, use PROC FREQ.

The data structure looks like this.

| category  | response
| ----------------------
| A         | Yes
| A         | No
| A         | Yes
| A         | No
| A         | No
| A         | Yes
| A         | Yes
| A         | Yes
| B         | No
| B         | Yes
| B         | No
| B         | No
| B         | No
| B         | Yes
| B         | No
| B         | No



## Assumption
* independent variables (e.g dependent menas "paired observation")
* random sampling
* Expected counts must be more than 1 or 1.
* Expected counts are more than 5 



~~~ SAS
DATA weightloss2;
 set weightloss;
 label wtloss_kg = "weight loss in kg" 
       Wtloss = "Weight loss (lbs)";
RUN;
~~~