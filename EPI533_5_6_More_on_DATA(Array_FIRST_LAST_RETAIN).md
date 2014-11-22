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
1. In Data Steps
2. ** ONly when "by statement" is used. **

FIRST.<variable> , LAST.<variable> are how to use them.
These internal variables should be used after SORTED (by PROC SORT) by these variables.

~~~ SAS


~~~


RETAIN statement
----------------

By using RETAIN <variable>, you can keep the value of the variable to the next DATA loop.
In another word, by using RETAIN statment, you can use the previous observation's variable value.
(One point you should know is that RETAIN statement should be at the end of DATA step. Because, before RETAIN statement is used, the RETAINED variable should have some value. )



