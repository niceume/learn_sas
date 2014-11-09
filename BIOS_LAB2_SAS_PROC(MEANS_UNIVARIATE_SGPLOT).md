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

~~~ SAS
PROC sgplot data = bios500.exposure;
vbox height / category = sex;
yaxis label="Height in inches";
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
* TITLE statement can be used.



The relationship of two or more continuous data = BIVARIATE (SGPLOT procedure)
------------------------------------------------------------------------------

The SGPLOT procedure can also create scatterplot.

* PROC SGPLOT statement
* SCATTER statement
  + option X=, Y= specify which variable to use as an x-axis and y-axis.
* Other options are the same as BOXPLOT option in SGPLOT

~~~ SAS
PROC SGPLOT DATA=bios500.exposure;
scatter x=age y=weight;
yaxis label="Height in inches";
xaxis label="sex";
RUN;
~~~


