#### README ####

## ldaDSI ##

This is a modification of the 'lda' R package written by Jonathan Chang (a student of David Blei). [link to original](https://cran.r-project.org/web/packages/lda/lda.pdf). 
The modifications have been made to the function 'lda.collapsed.gibss.sampler'. Use this version if you want to run a topic model that will output the topic terms matrix at preset save state intervals (e.g every 200th iteration, save the topic terms matrix to a file).

## USAGE ##

Make sure to load the library. 


```r
library(ldaDSI)
```

Make a directory to output the model states. Set the printstate interval. By default the iterations are printed to console unless trace is set to 0.


```r
var_trace = 0L
var_print_topic_terms_interval = 200
```

Note that lda.collabsed.gibbs.sampler expects the corpus in a list form with $documents and $vocab. If you have a dtm you can create this data structure with the topicmodels package.

```r
library(topicmodels)

obj_list = dtm2ldaformat(obj_dtm, omit_empty = TRUE)
```

Call the lda.collabsed.gibbs.sampler function with the extra arguments.

```r
obj_fitted_lda_model <- lda.collapsed.gibbs.sampler(documents = obj_list$documents,
	K = var_number_of_topics, 
	vocab = obj_list$vocab,
	num.iterations = var_gibbs_sampler_iterations, 
	alpha = var_prior_alpha, 
	eta = var_prior_eta, 
	initial = var_initial_assignments, 
	burnin = var_burnin,
	trace = var_trace,
	print_topic_terms_interval = var_print_topic_terms_interval,
	compute.log.likelihood = TRUE)
```

## MODIFYING AND REBUILDING ##

If you want to change the behavior of this function further, for example to save the doctopics matrix as well as topic terms, follow these steps. First, make your changes to the /src/gibbs.c file. Second, if you changed the way the function is called (i.e changed input arguments) then alter the wrapper function in /R/lda.collapsed.gibbs.sampler.R. 

 Then to rebuild:


```bash
R CMD build ldaDSI
R CMD check ldaDSI
R CMD INSTALL ldaDSI
```
