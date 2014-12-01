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


Creating an Order Variable for numeric data.
--------------------------------------------

PROC RNAK creates a new variable which represents the order of a variable.

* PROC RANK statement
  + options : DATA= specifies input dataset
  + options : OUT=  specifies ouput dataset
* VAR statement in PROC RANK
* RANKS statement in PROC RANK
  + specifies new variable which has order.

~~~ SAS
PROC RANK DATA=georgia_with_obs OUT=georgia_with_rank;
 VAR AdultObs MedianIncome PhysInact FoodEnvIdx rural_prop;
 RANKS AdultObsO MedianIncomeO PhysInactO FoodEnvIdxO RuralPropO;
RUN;
~~~


Creating a New Categorical Variable Automatically for numeric data.
-------------------------------------------------------------------

PROC RANK statement can be used to categorize observations.
It is done based on values of variable specified by "VAR statement".
The new variable specified by "RANKS statement" will be added. And the variable has values from 0 to number of groups specified by "GOUPS=" option.

* PROC RANK statement
  + options : DATA= specifies input dataset
  + options : OUT=  specifies ouput dataset
  + options : GROUPS=  specifies number of categories.
* VAR statement in PROC RANK
  + specifies variable by which observations are categorized.
* RANKS statement in PROC RANK
  + specifies new variable which has categorization number.

~~~ SAS
PROC RANK DATA=temp OUT=DividedTempDataset GROUPS=3;
 VAR WEIGHT;
 RANKS WT_LEV3;
RUN;

/*The output dataset has WT_LEV3 variable. It has 0 to 2 value based on the value of WEIGHT.*/ 

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


YYMMDD8.