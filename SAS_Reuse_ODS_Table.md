# Reuse ODS tables 
 
* Sometimes you want to reuse ODA table results in the following analysis.
* Two steps are required to do this.
    1. Get to know the name of ODS tables.
    2. Run "ODS OUTPUT <ods table name>=<SAS dataset name>;" just before the procedure of which you want the result.
        + You need to re-run the procedure.

## Get the name of ODS table.

```{SAS}
ods trace on;

/* DATA step or PROC step */
PROC FREQ data=xyz;
  tables abc ; 
RUN;

ods trace off;
```

* In log window, names of ODS tables are shown.
    + Use it with "ODS OUTPUT" statement.
    + Re-run the statement of that you want to use the result.

## Re-run the procedure with ODS OUPUT statement just before the procedure.

```{SAS}
ODS OUTPUT <ODS tablename>=<SAS dataset name> ;  /* <ODS tablename> is replaced with the name of the table you want to  */
PROC FREQ data=xyz;
  tables abc ; 
RUN;
```

* Then you can use the result as <SAS dataset name>

