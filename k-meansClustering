proc print data=hsb;
run;

data hsb_combo;
	set hsb;
	car_combo = 0;
run;

data hsb_combo;
	set hsb_combo;
	if car in (1, 2, 3, 5, 8, 9, 13, 15, 16) then car_combo = 1;
	else if car in (6, 7, 10, 11, 12, 14) then car_combo = 2;
	else if car in (4, 17) then car_combo = 3;
	drop car;
run;
* variables are on very different scales, so standardized measurements;
* used complete linkage to obtain ccc values for number of clusters;
proc cluster data=hsb_combo method=complete ccc pseudo std print=15 ;
  var sex race ses sctyp hsp locus concpt mot rdg wrtg math sci civ ;
  id id;
  copy car_combo;
run;
* used k-means clustering for cluster analysis;
proc fastclus data=hsb_combo maxc=5 maxiter=10 out=clus;
var sex race ses sctyp hsp locus concpt mot rdg wrtg math sci civ;
run;
proc freq;
tables cluster*car_combo/ nopercent norow nocol;
run;
* performed principal components analysis on cluster data to 
extract 2 most prominent features;
proc princomp data=clus n=2 out=pcout ;
  var sex race ses sctyp hsp locus concpt mot rdg wrtg math sci civ;
run;
* visualized the data points in the first two principal 
  coordinates to see where the clustered values are in this space;
proc sgplot data=pcout;
  scatter y=prin2 x=prin1 / group=cluster name='cluster';
  title2 'Plot of first two principal components by cluster';
run;
