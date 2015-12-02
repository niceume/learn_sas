Analysis Flow
=============

Import dataset
---------------

~~~ SAS
LIBNAME bioslib "C:\course\BIOS\Datasets";
ODS html close; ods html;
DATA asthma;
  set bioslib.asthma;
RUN;
~~~
