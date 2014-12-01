Lab2
----

Subsets based on number of observations
--------------------------

Use (OBS= ) clause for the dataset in SET statement.

~~~ SAS
DATA EXTRACTED;
  set epi533.COMBINED(OBS=15);
RUN;
~~~


Subsets based on variables
--------------------------

Use (KEEP= ) clause or (DROP= ) clause for the dataset in DATA statement.

~~~ SAS
DATA EXTRACTED(KEEP= ID SEX SCORE);
  set epi533.COMBINED;
RUN;

/* Multiple datasets can be specified at the same time. */
DATA EXTRACTED1(KEEP= ID SEX) 
     EXTRACTED2(DROP= SCORE)  ;
RUN;
~~~


Subsets based on values & manipulate values
-------------------------------------------

IF / THEN / ELSE IF / ELSE statements are useful to make a new categorical variable.
In contrast, WHERE statement can be used for PROC Step.

* IF statement is followed by condition.
  + In conditions following comparison operators can be used.
  + Numerical comparison operators
    + equals to     = EQ
    + Greater than  > GT
    + Less than     < LT
    + Greater than or equal to  >= GE
    + Less than or equal to     <= LE
    + Not equal to   ^=  NE
  + Set comparison operators
    + the value is included?  IN ( value1 value2 ... )
    + Values can be 
    + When the value is numeric range can be specified  IN ( value_start:value_end )
  + Logical (Boolean) Operators
    + AND operator   &  AND
    + OR operator    |  OR
    + NOT operator   ~ ^ not


~~~ SAS
/* Manipulate value. */
IF STUDYID = 286 THEN CALORIE= 2500;

/* Multiple changes at the same time */
DATA ;
IF STUDYID = 286 THEN DO ;
 smoke = 0;
 sleep = 6;
 plane = 1;
 vitamin = .;
END;
RUN;

/* Specify values in PROC step. */
PROC PRINT;
 WHERE STUDYID IN (268, 286, 306, 347); /* This illustrates use of a SQL statement.*/
RUN;
~~~~