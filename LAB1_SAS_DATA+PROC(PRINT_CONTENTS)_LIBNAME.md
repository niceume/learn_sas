LIBNAME statement
------------------

Assign a libref name to a directory. Moreover, once assign a library name, the library exists until the SAS system ends (Not until the end of each program). 

* LIBNAME statement
  + The first option : Libref name. 
  + The second option : Directory name which can work as a library and has datasetes.


~~~ SAS
LIBNAME bios500 "S:\course\bios500\Binongo\DATASETS" ;
~~~


Dataset Naming
--------------
Dataset of one-level name (e.g. exposure2) indicates datasets in  "WORK" LIBRARY. Which is a TEMPORARY library.
Dataset of two-level name (e.g. bios500.exposure2 ) indicates datasets in a PERMANENT library.


Creating a New Dataset (or from Existing Dataset)
-------------------------------------------------
DATA statement begins DATA step and creates(outputs) new dataset.

SET statement in DATA step import existing dataset.

* DATA statement
  + The first option : "OUTPUT" dataset name.

* Set statement in DATA step
  + The first option is an existing dataset.

~~~ SAS
DATA NEW_DATASET ;
  set EXISTING_DATASET;
  < statements >
RUN ;
~~~


DATASET immediate modification
------------------------------
Dataset can be modified easily by using ( ) options.

~~~ SAS
PROC PRINT bios500.exposure ( obs = 10 )  
/* This (obs=10) is an option of dataset. Not an option of PROC PRINT. */
~~~


SAS comments
------------
Start with asterisk and end with semicolon

~~~ SAS
* Start with asterisk and end with semicolon ;
/* This is also OK. */
~~~



CONTENTS procedure
------------------

* PROC CONTENTS statemenet :  Show all the properties of column names.
  + options
  + DATA= specifies dataset.
  + VARNUM, POSITION options show observations in the order in which they were created (rather than the default alphabetical organization.)

~~~ SAS
PROC CONTENTS DATA=bios500.exposure;
RUN;

PROC CONTENTS DATA=bios500.exposure POSITION;
RUN;
~~~



PRINT procedure
---------------

* PROC PRINT statement :  Show observations.
  + options. 
  + DATA= specifies dataset.
* VAR (variable_name) statement : PRINT only variable_name
* VAR (variable_name1) -- (variable_name2) statement : PRINT from variable_name1 to variable_name2. If variable_name2 is left to 1, it raises an error.
  
~~~ SAS
PROC PRINT DATA=bios500.exposure;
RUN;

PROC PRINT DATA=bios500.exposure;
var id--age;  /*cf. age--id is not allowed. */
RUN;
~~~










