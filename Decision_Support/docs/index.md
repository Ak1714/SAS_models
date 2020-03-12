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

