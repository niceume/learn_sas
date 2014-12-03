Categorical data
================

Summarized by
1. contigency tables: FREQ procedure

Visualize by 
1. "barchart" : VBAR in SGPLOT procedure.  FreqPlot option in FREQ procedure. 
  * Make sure that the x axis is categorical.
2. "piechart" : ?


FREQ procedure (The relationship of two or more categorical data)
----------------------------------------------------------------

The FREQ procedure produces one-way to n-way frequency and contingency (crosstabulation) tables.  
In another word, the procedure describes "the relationship between two categorical characteristics".

* PROC FREQ statement
  + options DATA= specifies the dataset.
* TABLES statement <variable_for_row> * <variable_for_column> 
  + options come after slash(/). 
  + plots=FreqPlot can generates bar-chart.
  + NOPERCENT, NOROW, NOCOL, NOFREQ, NOCUM suppresses the output of percentages, row paercentages, column percentages, frequencies and cumulative frequencies.
  + LIST displays n-way tables as a list (on line for every combination.)
  + MISSING treats missing values as a valid nonmissing level. (To include missing value in table.)
* TABLES statement <variable1> * <variable2> * <variable3> ... (More than two way)

~~~ SAS
LIBNAME bios500 'S:\course\bios500\Binongo\DATASETS'; 

/*Show barchart*/
PROC FREQ data=bios500.m_and_m_colors2004;
tables color / plots=FreqPlot;
RUN;

/*Show number of missing values by default.*/
PROC FREQ data = bios500.heart;
  tables Weight_Status * sex;
RUN;

/*Multiple tables at once.*/
PROC FREQ data=bios500.exposure;
  tables sex * ( hx_smoke sex Marital_status) / nocol nopercent;
RUN;
~~~


VBAR statement in SGPLOT
---------------

Plot bar chart.

* PROC SGPLOT statement
* VBAR statement 
  + options come after slash(/). 
  + TRANSPARENCY= numeric-value
  + The CATEGORY option cannot be used as VBOX statement.
  + Instead of CATEGORY, GROUP option can be used, which doesn't divide plots but seperate areas of each group by color in the same plot.
  + X2AXIS, Y2AXIS, LEGENDLABEL= options can be used.
* XAXIS, YAXIS can be used.
* TITLE statement can be used.


~~~ SAS
DATA cats;
INPUT size type $ ;
DATALINES;
07 kitten 
05 kitten 
09 kitten
10 kitten 
08 kitten 
07 kitten
06 kitten 
08 kitten
09 cat 
10 cat 
12 cat
13 cat 
14 cat 
12 cat
;
RUN;

PROC SGPLOT data=cats;
  VBOX size / category=type;
RUN;

PROC SGPLOT data=cats;
  VBAR size / group=type;
RUN;
~~~


GROUPING by categorical data
----------------------------

* UNIVARIATE procedure
  + use CLASS= statement.

* SGPLOT procedure
  + use CLASS= statement.

* VBOX in UNIVARIATE procedure
  + use /CATEGORY=<variable> statement.


~~~ SAS
PROC univariate data=bios500.exposure;
  var weight;
  class sex;
  histogram weight;
  inset mean median std ;
RUN;

PROC SGPLOT data=bios500.exposure;
scatter y=weight x=age / group=sex ;
xaxis label="Age in years";
yaxis label="Weight in pounds";
RUN;
~~~


MORE WAYS to MAKE PLOTS
-----------------------

* PROC SGSCATTER procedure

* PROC SGPANEL procedure

~~~ SAS
PROC SGSCATTER data=bios500.exposure;
matrix age height weight / diagonal=(HISTOGRAM);
RUN;

PROC SGPANEL data=bios500.fev_data;
panelby sex;
reg x=height y=fev /degree=2 ;
RUN;
~~~


