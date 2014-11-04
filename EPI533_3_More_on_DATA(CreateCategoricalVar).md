Lab3
----

Creating a New Categorical Variable
-------------------------------------
IF / THEN / ELSE IF / ELSE statements are useful to make a new categorical variable.


Blue Message when missing values are proceessd
----------------------------------------------

When the missing values are generated on an operation on missing values, there is a note in a log.
"Number of times" means how many missing values were in the dataset.
"Line" and "Column" means the position in the program. 

(e.g.)
NOTE: Missing values were generated as a result of performing an operation on
      missing values.
      Each place is given by: (Number of times) at (Line):(Column).
      3 at 224:17   3 at 224:26   3 at 224:36



Finding out cut-points for numeric data and categorize by IF/THEN statement
--------------------------------------------------------------------------

PCTLPTS= option in OUTPUT satement in PROC UNIVARIATE statement can be used. 

~~~ SAS
PROC UNIVARIATE DATA=temp;
  var WEIGHT;
  output OUT=CUTPTS PCTLPTS=33.33 66.67 PCTLPRE = P_;
RUN;
PROC PRINT data=CUTPTS; RUN;

DATA temp2;
  set TEMP;
  IF WEIGHT GT 0 AND WEIGHT LE 126 THEN WT3LEV=0; 
  IF WEIGHT GT 126 AND WEIGHT LE 149 THEN WT3LEV=1;
  IF WEIGHT GT 149 THEN WT3LEV=2;
RUN;
~~~


Creating a New Categorical Variable Automatically for numeric data.
-------------------------------------------------------------------

PROC RANK statement can be used to categorize 

~~~ SAS
PROC RANK DATA=temp OUT=DividedTemp GROUPS=3;
 VAR WEIGHT;
 RANKS WT_LEV3;
RUN; 
~~~


FORMATS & LABEL
---------------

Refer to BIOS 500.


SOME FUNCTIONS about DATE
-------------------------

DATE()
MDY( MM, DD, YYYY)
YEAR( num )
MONTH( num )
DAY( num )
age = INT( (DAY1 - DAY2) / 365.25 );
