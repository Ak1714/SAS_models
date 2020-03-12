# Decision Suppport Model


## Commands -try editing this

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.


## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.

## Data source

https://www.kaggle.com/mohansacharya/graduate-admissions

## Summary

 This project analyzes the graduate admission process in American universities from international students’ perspectives. The goal was to build a decision support model that provides undergraduate international applicants with important information as well as the ability to make an informed decision during the application, with high confidence of being accepted. The analysis considers factors such as standardized test scores and GPA as well as university reputation.

## Data Analysis
 Step 1:
 
 We start by first studying the relationships between each parameter using a pairwise correlation matrix
 As per the correlation matrix below it is clear that there is high correlation between GRE score, TOEFL score and CGPA with Chance of admit. 
 It also shows that GRE score and TOEFL score have a high correlation (R=0.836), which can also mean that people who score more in GRE also score more in TOEFL. 
 Also, with R = 0.83 between CGPA and GRE score shows that students with good CGPA scores perform well in GRE. 
  ![1](image1.jpg)

 Step 2:
 
 1) Regressing CGPA with Chance of admit
  Scatterplot looks homoscedastic.
  ![2](image2.jpg)
  ![3](image3.jpg)
  
  
  
  The Rsq value shows that the regression line is a good fit (i.e. 76% of the variability in the chance of admit is explained by our model with the help of CGPA alone). The RMSE is 0.0695 shows how spread out the residuals are.

## SAS code

 Correlation matrix code:

	ods noproctitle; 
	ods graphics / imagemap=on; 
	 
	proc corr data=_TEMP0.GRAD pearson nosimple noprob plots=none; 
		var 'GRE Score'n 'TOEFL Score'n 'University Rating'n SOP LOR CGPA Research  
			'Chance of Admit'n; 
	run; 

[Click for results](https://github.com/Ak1714/SAS_models/tree/master/Decision_Support/Correlation_matrix-results.pdf)

 Linear regression using each of 3 regressors with Chance of admit:
 
	ods noproctitle; 
	ods graphics / imagemap=on; 
	 
	proc glmselect data=_TEMP0.GRAD outdesign(addinputvars)=Work.reg_design; 
		model 'Chance of Admit'n=CGPA / showpvalues selection=none; 
	run; 
	proc glmselect data=_TEMP0.GRAD outdesign(addinputvars)=Work.reg_design; 
		model 'Chance of Admit'n='GRE Score'n / showpvalues selection=none; 
	run; 
	proc glmselect data=_TEMP0.GRAD outdesign(addinputvars)=Work.reg_design; 
		model 'Chance of Admit'n='TOEFL Score'n / showpvalues selection=none; 
	run; 
	proc reg data=Work.reg_design alpha=0.05 plots(only)=(diagnostics residuals  
			observedbypredicted); 
		where 'GRE Score'n is not missing and 'TOEFL Score'n is not missing and CGPA  
			is not missing; 
		ods select DiagnosticsPanel ResidualPlot ObservedByPredicted; 
		model 'Chance of Admit'n=CGPA /; 
		model 'Chance of Admit'n='GRE Score'n /; 
		model 'Chance of Admit'n='TOEFL Score'n /; 
		run; 
	quit; 
	proc delete data=Work.reg_design; 
	run; 

[Click for results](https://github.com/Ak1714/SAS_models/tree/master/Decision_Support/LR-results.pdf)

 Multiple linear regression code:

	ods noproctitle; 
	ods graphics / imagemap=on; 
	 
	proc glmselect data=_TEMP0.GRAD outdesign(addinputvars)=Work.reg_design; 
		model 'Chance of Admit'n='GRE Score'n 'TOEFL Score'n CGPA / showpvalues  
			selection=none; 
	run; 
	proc reg data=Work.reg_design alpha=0.05 plots(only)=(diagnostics residuals  
			observedbypredicted); 
		where 'GRE Score'n is not missing and 'TOEFL Score'n is not missing and CGPA  
			is not missing; 
		ods select DiagnosticsPanel ResidualPlot ObservedByPredicted; 
		model 'Chance of Admit'n=&_GLSMOD /; 
		run; 
	quit; 
	proc delete data=Work.reg_design; 
	run; 

[Click for results](https://github.com/Ak1714/SAS_models/tree/master/Decision_Support/MLR-results.pdf)