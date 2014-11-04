PROC MENAS

output option.

*right-skewed distribution;
data chisq;
sampsize = 50;
do sample=1 to 5000;
  do i=1 to sampsize;
    x=rand("chisq",5);
    output;
  end;
end;
run;

proc means data=chisq noprint;
by sample;
var x;
output out=results mean=xbar;
run;

proc sgplot data=results;
histogram xbar;
run;