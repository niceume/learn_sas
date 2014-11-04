Rename the variable name of dataset
-----------------------------------

RENAME statement is used.

Be careful that the SAS variable name is limited to 32 characters.

~~~ SAS
DATA TEMP2;
 set TEMP1;
 rename ill = caco;
RUN;
~~~


Label variable name (LABEL in DATA Step)
------------------------------------------

LABEL statement in DATA Step is used. Because we'd like to label for variable of dataset.

~~~ SAS
DATA weightloss2;
 set weightloss;
 label wtloss_kg = "weight loss in kg" 
       Wtloss = "Weight loss (lbs)";
RUN;
~~~


categorize values (PROC FORMAT)
-------------------------------

PROC FORMAT statement is used to categorize observations based on values.

* PROC FORMAT statement
* value statement
  + options : value <format_name> <value> = <display_name> ;
  + When you define FORMAT for categorical values, you have to add dollar mark ($) just in front of the variable.


The defined Format is used by FORMAT statement.

* FORMAT statement can be used both in DATA step and PROC Step


~~~ SAS
/* How to define formats */
PROC FORMAT;
value genderf 0 = 'female'  /* For numerical data */
              1 = 'male';

value sxf LOW-1 = "extreme low"  /* Range can be used for numerical data. */
          2-3 = "low"
          4-5 = "moderate"
          6-7 = "high"
          8-HIGH = "extreme high"; /* Special variables "LOW" and "HIGH" are used for range.*/

value $trt_typef "A" = "Diet Only"  /* For character data */
                 "B" = "Exercise Only"
                 "C" = "Diet and Exercise";
RUN;


/* How to use in DATA Step */
DATA weightloss2;
 set weightloss1;
 FORMAT trt $trt_typef. ;
RUN;

PROC FREQ DATA=weightloss1;
 FORMAT gender genderf. ;
 tables gender ; 
RUN;

~~~


Orders to be displayed
----------------------

The order to be displayed follows an alphabetical order displayed. 

* missing < blank(s)+characters < numbers < capital letters < lower letters



Additional Point
----------------

"How to Create a new dataset from a Scratch" is described in LAB1 note.