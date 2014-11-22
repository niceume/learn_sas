Lab4

Rename variable name
--------------------

RENAME statement is used in DATA Step.

~~~ SAS
DATA temp;
 set epi533.combined;
 rename ill caco; /* New variable name is caco.*/
RUN;
~~~


Labels defined and used.
------------------------

Refer to BIOS500.


Formats defined and used
------------------------

Refer to BIOS500.


Introduction to SAS ODS
-----------------------

~~~ SAS
/* ODS : output delivery system.*/
ODS RTF FILE = 'H:\EPI533\Class1files\sample.rtf';
PROC FREQ DATA=TEMP2;
TABLES ENTRYAGE*SMOKE;
FORMAT ENTRYAGE AGEFMT.;
RUN;
ODS RTF CLOSE;
~~~ 


Compare datasets
----------------

* PROC COMPARE statement
  +options : DATA= specifies dataset as the base dataset.This option is the alias of BASE=
  +options : COMPARE= specifies dataset 
  +options : LISTvAR requires SAS to show variable differences.

* ID statement in PROC COMPARE
  + Observation differences will be shown by using this variable specified by ID statemtnt.

* VAR statement in PROC COMPARE
  + Restrict the variables to compare.

* Understanding the outputs.
  + There are variables summary(if specified) and observations summary.
  + A list of varables in the base dataset but not in compared dataset will be showed..
  + Number of observations with some unequal variables will be showed.
  + Maximum difference means (max - min) in numeric variable.


~~~ SAS
/* PROC COMPARE */
DATA TEMP3 (DROP=KIDS HOUSE);
 set toshi.COMBINED6;
 IF studyid = 29 THEN CACO = 0;
RUN;
PROC CONTENTS DATA = toshi.COMBINED6;
RUN;
PROC CONTENTS DATA = TEMP3;
RUN;

PROC COMPARE DATA=toshi.COMBINED6 COMPARE = TEMP3 LISTVAR;
 id studyid;
RUN;
~~~


PROC TRANSPOSE
--------------

TRANSPOSE procedure can be used for data which have multiple observations for each patient.

* PROC TRANSPOSE

* BY statement in PROC TRANSPOSE
  + This specifies the identifier which 
* VAR statement in PROC TRANSPOSE
  + This specifies values of the variable which I'd like to put in a table format.


~~~ SAS
DATA BLOOD;
 input studyid bp;
 datalines;
 01 126
 01 131
 01 135
 01 124
 02 145
 02 144
 02 140
 02 142
 03 150
 03 148
 03 152
 03 160
 04 120
 04 119
 04 115
 04 120
 05 125
 05 130
 05 135
 05 128
 ;
RUN;


/* The default behavior of TRANSPOSE procedure is just transpose rows and columns.*/
/*  */
/* Moreover, TRANSPOSE procedure can be used for data which have multiple observations for each patient*/
/*  */
The output data looks like this */
/*  */ 
/* Obs studyid _NAME_ TIME1 TIME2 TIME3 TIME4   */
/* 1 1 bp 126 131 135 124   */
/* 2 2 bp 145 144 140 142   */
/* 3 3 bp 150 148 152 160   */
/* 4 4 bp 120 119 115 120   */
/* 5 5 bp 125 130 135 128   */
/*  */

PROC TRANSPOSE data=blood out=blood2;
RUN;
PROC PRINT data= blood2;
RUN;

PROC TRANSPOSE DATA = BLOOD OUT=BLOOD3 (RENAME = (COL1 = TIME1 COL2 = TIME2 COL3 = TIME3 COL4 = TIME4));
 BY STUDYID;
 VAR BP;
RUN;

PROC PRINT DATA = BLOOD3;
RUN;
~~~
