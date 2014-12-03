Numeric data (Continuous or discrete)
=====================================

Continuous data can be summarized by
1.means, std and 5 values : MEANS procedure
2.moments, quantiles (with plot)  :  UNIVARIATE procedure

Continuous data's density can be visualized by 
1.boxplot (density of continuous data ) : VBOX in SGPLOT ( UNIVARIATE PLOT) 
2.histogram  (counts by classes)  : HISTOGRAM in SGPLOT ( Originally UNIVARIATE PLOT,  HISTOGRAM procedure in UNIVARIATE )
  * Make sure that the x axis is not categorical data.
3.scatterplot  (relationship of two variables) : SCATTER in SGPLOT 


MEANS procedure
---------------

Show measures about continuous data.

* PROC MEANS statemenet
  + options : Indicate which data to use and which measures to calculate. The default measures are N, Mean, STD, MIN, MAX.
  + options : nmiss means number of missing values. mode means most frequent value.
  + options : q1 and q3 mean 1st quartile(=25percentile) and 3rd quartile(=75 percentile). QRANGE means Quartile range.
  + options : p1 p5 p10 p90 p95 p99 mean percentils.
* VAR statement  :  Indicate which variables to use.
* CLASS statement  :  The variable is dealt as categorical variable.(Works for characters and numerical data.)
* BY statement  : It's like CLASS statement but it's hard to use because the variable has to be sorted or have some manner in the order.

~~~ SAS
PROC MEANS data=bios500.exposure;
RUN ;

PROC MEANS data=bios500.exposure n nmiss mean std min q1 median q3 max maxdec=2;
var height;
class age;
RUN;

PROC MEANS data = bios500.heart n nmiss; /* shows number of missing values*/
  var cholesterol;
RUN;
~~~

* OUTPUT statement : Output to dataset. Usually you are happy if you use with <statistic>=<variable>
  + options : out=<dataset> specifies the dataset name to ouput.
  + options : <statistic>=<variable> adds a value of statistic to variable . e.g. sum, mean, std, uclm(upper confidence limit for population mean), lclm.
  + If you don't use <statistic>=<variable> option, the default statistics, n mean std min max are displayed in a longitudinal direction.
  + If you use <statistic>=<variable> option, the statistics 

~~~ SAS
data test;
 input name $ address $ pay age ;
 datalines;
 toshi briarcliff 12 32
 toshi tokyo 100 32
 yuji briarcliff 8 30
 yuji tokyo 20 30
 hatena kyoto 10 28
 hatena tokyo 20 28
 ; 
run;

PROC MEANS data=test;
 class name;
 output out=temp; /* Usually used with <statistic>=<variable> option. */
RUN;

PROC MEANS data=test;
 class name;
 output out=temp sum=sum mean=avg uclm=upper lclm=lower;
RUN;
~~~

The dataset of temp
* The _TYPE_ = 0 means the statistics of total.

| Obs | name | _TYPE_ | _FREQ_ | _STAT_ | pay | age | 
|-----|--------|-----|-------|-------|--------|------
| 1   |        | 0   |   6   |  N    | 6.000  | 6.0000 
| 2   |        | 0   |   6   |  MIN  | 8.000  | 28.0000 
| 3   |        | 0   |   6   |  MAX  | 100.000| 32.0000 
| 4   |        | 0   |   6   |  MEAN | 28.333 | 30.0000 
| 5   |        | 0   |   6   |  STD  | 35.472 | 1.7889 
| 6   | hatena | 1   |   2   |  N    | 2.000  | 2.0000 
| 7   | hatena | 1   |   2   |  MIN  | 10.000 | 28.0000 
| 8   | hatena | 1   |   2   |  MAX  | 20.000 | 28.0000 
| 9   | hatena | 1   |   2   |  MEAN | 15.000 | 28.0000 
| 10  | hatena | 1   |   2   |  STD  | 7.071  | 0.00000 

The dataset of temp2 (This output is easy to use.)

|Obs | name | _TYPE_ | _FREQ_ | sum | avg | upper | lower 
|----|------|--------|--------|-----|-----|-------|------
|1   |      |  0     |    6   |170  |28.3333| 65.559| -8.892 
|2   |hatena|  1     |    2   |30   |15.0000| 78.531| -48.531 
|3   |toshi |  1     |    2   |112  |56.0000| 615.073| -503.073 
|4   |yuji  |  1     |    2   |28   |14.0000| 90.237| -62.237 


UNIVARIATE procedure (histogram, boxplot)
-------------------------------------

Aanalyze and show distribution of specific variable(s).
Each variable has its distribution and it can be showed by using "histogram" or "boxplot" can be used for continuous data.

* PROC UNIVARIATE statement 
  + The PLOT option shows histogram and boxplot.
* VAR statement  : Indicate which variables to use.
* CLASS statement  : The variable is dealt as categorical variable.(Works for characters and numerical data.)
* HISTOGRAM statement  : Shows histogram.
  + Options come after slash(/). The NORMAL option displays a fitted normal curve on the histogram, the MIDPOINTS= option specifies midpoints for the histogram, and the CTEXT= option specifies the color of the text  
* FREQ statement  : Treat the variable as the frequency of the observation. 
* INSET statement : show inset box
  + MEAN option shows mean value in an inset box.

~~~ SAS
PROC UNIVARIATE DATA=bios500.exposure;
var height;
RUN;

PROC UNIVARIATE DATA=bios500.exposure plot;
var height;
histogram height / normal;  /* For continuous data */
class sex;
RUN;

PROC UNIVARIATE DATA=bios500.exposure plot;
var height;
class sex;
inset mean;
run;
~~~


SGPLOT procedure (box plot version)
-----------------------------------

This is a procedure to display various plots. This time, we will create a boxplot.

* PROC SGPLOT statement
* VBOX statement, HBOX statement
  + options come after slash(/). 
  + The CATEGORY=<variable> option divide observations based on the category of the variable.
  + X2AXIS, Y2AXIS, LEGENDLABEL= options can be used.
* XAXIS, YAXIS 
  + option : LABEL= statement
  + MIN=, MAX=, TYPE=LOG, LOGBASE=, VALUES=(value1 TO value2)
* TITLE statement
* REFLINE Statement
  + REFLINE <num> , REFLINE "<DATEFORMAT>" etc.
  + options come after slash(/).
  + axis= x or y. axis= x draws a line which is orthogonal to xaxis.


~~~ SAS
PROC sgplot data = bios500.exposure;
vbox height / category = sex;
yaxis label="Height in inches" min=55 max= 75;
xaxis label="sex";
title "Distribution of height by sex";
RUN;
~~~ 


SGPLOT procedure (histogram version)
-----------------------------------

We can create histogram.

* PROC SGPLOT statement
* HISTOGRAM statement : Multiple histogram statements can be used and it overlaps many histograms (Transparency option works well).
  + options come after slash(/). 
  + TRANSPARENCY= numeric-value
  + The CATEGORY option cannot be used as VBOX statement.
  + X2AXIS, Y2AXIS, LEGENDLABEL= options can be used.
* XAXIS, YAXIS can be used.
* TITLE, REFLINE statement can be used.



The relationship of two or more continuous data = BIVARIATE (SGPLOT procedure)
------------------------------------------------------------------------------

The SGPLOT procedure can also create scatterplot.

* PROC SGPLOT statement
* SCATTER statement
  + option X=, Y= specify which variable to use as an x-axis and y-axis.
  + option /group=<variable> specify which variable to use as group.
* Other options are the same as BOXPLOT option in SGPLOT

~~~ SAS
PROC SGPLOT DATA=bios500.exposure;
scatter x=age y=weight;
yaxis label="Height in inches";
xaxis label="sex";
RUN;

proc sgplot data=iris;
  title 'Fisher (1936) Iris Data';
  scatter x=petallength y=petalwidth / group=species;
run;
~~~


