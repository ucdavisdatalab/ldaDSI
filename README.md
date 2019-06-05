#### README ####

## ldaDSI ##

This is a modification of the 'lda' R package written by Jonathan Chang (a student of David Blei). [link to original](https://cran.r-project.org/web/packages/lda/lda.pdf). 
The modifications have been made to the function 'lda.collapsed.gibss.sampler'. Use this version if you want to run a topic model that will output the topic terms matrix at preset save state intervals (e.g every 200th iteration, save the topic terms matrix to a file).

## USAGE ##

Make sure to load the library. 


```r
library(ldaDSI)
```

Make a directory to output the model states. Set the savestate interval. Set trace to a number >= 1 to have the model print the current iteration.


```r
var_odir = paste(getwd(), "/output/", sep="")
dir.create(var_odir)
var_trace = 2
var_savestate_interval = 200
```

Call the lda.collabsed.gibbs.sampler function with the extra arguemnts (save_state_interval, odir)


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
													save_state_interval = var_savestate,
 													odir = var_odir,
                                                    compute.log.likelihood = TRUE)
```

Note that lda.collabsed.gibbs.sampler expects the corpus in a list form with $documents and $vocab. If you have a dtm you can create this data structure with the topicmodels package.



```r
library(topicmodels)

obj_list = dtm2ldaformat(obj_dtm, omit_empty = TRUE)
```

## MODIFYING AND REBUILDING ##

If you want to change the behavior of this function further, for example to save the doctopics matrix as well as topic terms, follow these steps. First, make your changes to the /src/gibbs.c file. Second, if you changed the way the function is called (i.e changed input arguments) then alter the wrapper function in /R/lda.collapsed.gibbs.sampler.R. 

Then to rebuild:


```bash
R CMD build ldaDSI
R CMD check ldaDSI
R CMD INSTALL lda_DSI_1.0.tar.gz
```