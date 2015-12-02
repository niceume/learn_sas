# SAS macro basics

SAS macro is useful when repeating the same kind of work again and again.




## HOW TO DEFINE MACRO VARIABLES & EXPRESSINS

### DEFINE MACRO VARIABLES ( %let )

define a macro variable. 

```
%let macro_var1=value1 ;
```

Interesting thing is that value1 can have spaces, and doesn't require quotation marks. ( Usual programming languages require those things.)

All the values in macro variable definitions are dealt as characters.

e.g.)
```
%let macro_var1=value1;
%let macro_var2=100+200; /*This is 100+200 as characters. not 300.*/
```

### DEFINE MACRO EXPRESSIONS ( %macro ) 

define a macro expression.

```
%macro macro_function( var1 var2 );
  /* Comments can be used in macro. */
%mend;
```
e.g.) Automate histogram plot.

``` SAS
%macro plothist(var);  /* In this case, this macro name is pl. At least one macro variable required.*/
  PROC SGPLOT data=asthma;
    histogram &var;
    title &var;
  RUN;
%mend;
```




## HOW TO USE

### EXPAND MACRO VARIABLES ( Use ampersand & in %let or %macro )

Macro variables can be expanded only in macro statements, like %let and %macro.

```
%let macro_var_1=&macro_var_0;

%macro (macro_var_a)
  &macro_var_a ;
  &macro_var_b ;
%mend;
```

e.g.)  Express U.S. style address. 
```
%let address=&number &street Atlanta;
```

#### Concatinate strings in macro variables. 

When concatinating two strings, we don't have to use plus oprator. Just put two string variables SIDE BY SIDE.

``` SAS
%let address=%number %street Atlanta;
```

When concatinating two variables with some characters between them, SAS is hard to identify the first variable's last character. e.g.) &firstvarplus&secondvar  In this case, the first variable name is &first, &firstvar or &firstvarplus ?

To clarify the ending of variable name, period is used.

``` SAS
&firstvar.plus&secondvar
``` 

In this example, &firstvar and &secondvar are expanded. "plus" is between them. Period is used just to clarify the ending and it's not used in terms of the result. 



#### How to use period in macro.

Use double periods. 

```
%macro ;
  &var1..&var2 ; /* The result has only one period. */
%mend ;
```




### EXECUTE MACRO EXPRESSION

```
%macro_fn_name ( value1, value2, value3 );
```

e.g.) This is how to call macro

``` SAS
%plothist( fev1 ) ;
%plotscatter( age , fev1, gender = male ) ;

```



## Macro variables in detail

### Two types of macro variables. 
There are two types of macro variables, Global macro variable and Local macro variable.

*Global macro variable
  + %let is defined out of %macro .
*LOcal macro variable
  + %let is inside %macro . Local macro variable expires when %macro ends. 



