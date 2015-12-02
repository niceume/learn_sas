Lab6

Array statement in Data Step.
-----------------------------

Array statement can define array which has multiple "variables."
This is used for variables.

Array statement 
  + The format is 
  + ARRAY array-name {number of variables} variable-1 ... variable-n

Usually array is accessed by using do loop.
THe index starts with 1. (not 0)

~~~ SAS

DATA HEALTH;
Array SCORE {3} SMOKE VITAMIN PLANE;
DO i = 1 to 3; /* Index starts with 1. not 0.*/
  IF SCORE{i} = 1 THEN total = total + 1;
  ELSE IF 
END;
RUN;

~~~



FIRST, LAST internal variables
------------------------------

FIRST, LAST internal variables can be used,
0. SORT by group id. 
1. In Data Steps, use "by" statement. 
2. Then SAS dataset has internal variables, FIRST.<variable> and LAST.<variable>.
  + If FIRST.<variable> = 1, the observation is the first observation within each group devided by the "by statement".
  + If LAST.<variable> = 1, the observation is the last within each group devided by the "by statement"

  
~~~ SAS
/* If each individual has multiple observations, but only the first observation has values for some variables. */
/* For example, if each individual has multiple occasions, but some variables are measured only at the first occasion, and at other occasions the variables have missing values. */
/* In order to run something like mixed model (PROC MIXED), you need to fill in the missing values by the first observations. */

PROC SORT data= asthma;
  by id;
RUN;

DATA asthma;
  set asthma;
  by id;

  if FIRST.id = 1 then stored_wbc = wbc; /* */
  else wbc = stored_wbc;

  retain stored_wbc;  /* Refer to "retain statement" section. */
RUN;

~~~


RETAIN statement (should be used at the last of DATA step.)
----------------
### Cautions 
* RETAIN statement should be used at the end of DATA step. 
* I recommend that you use variable name which is not used in the dataset. You should define a new variable to store a value. This avoids confusion.

### How to use RETAIN.
* By using RETAIN <variable>, you can keep the value of the variable to the next DATA loop.
* In other words, by using RETAIN statment, you can use the previous observation's variable value.




