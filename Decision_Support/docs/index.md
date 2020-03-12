# Welcome to MkDocs

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands -try editing this

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.


## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.
		
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
