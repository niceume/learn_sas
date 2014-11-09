TTEST 
------------------------------------------

LABEL statement in DATA Step is used. Because we'd like to label for variable of dataset.

~~~ SAS
DATA weightloss2;
 set weightloss;
 label wtloss_kg = "weight loss in kg" 
       Wtloss = "Weight loss (lbs)";
RUN;
~~~