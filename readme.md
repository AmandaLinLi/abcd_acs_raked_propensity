### Propensitiy score calculation for ABCD
Author: Steven Heeringa

I have attached a compressed file that includes R program code and supporting data sets for the computation of the final population weights for the 11,875 ABCD baseline records.  My plan is to develop a paper/document that details the methodology and results but for the current purpose I will provide an outline of the files and how they relate to the computational sequence.

SAS to Rds conversion November 2018 data.R -- This is R code that you probably will not need.  It simply converts three input files from SAS V9 to R data sets:

- ACS2011_15_I.Rds  - This is the large ACS 2011-2015 data set with the demo/SES variables that will be used as the benchmark for the propensity weight estimation.  The "I" suffix indicates that the missing data in this input file are already imputed with a single multivariate imputation.  This file will be input in the Step 2 program below.  It serves as our permanent benchmark/reference for the population.
- Nov2018_demo.Rds - This is the R version of the Demographics table in the final Nov 2018 data dump for the Health and Mental Health workbook.  
- Nov2018_ACS.Rds - This is the R version of the "ACS" table  in the final Nov 2018 data dump for the Health and Mental Health workbook.  It is used only to identify the twin/multiple birth status of a child. In our pooled propensity weight,  single births and twins will not be differentiated in the weighting process.  (I am investigating the impact of twin/single birth "exchangeability" in a separate analysis).
ABCDDemo_SES_112018_R.R  (Step 1): This is R code to integrate selected variables from the ABCD data dump, recode as needed, impute missing data and output a processed file that is ready (in Step 2) to be merged with the ACS benchmark for the weight calculation:
- Nov2018ABCD_I.Rds - thisi s the output data set that contains the selected set of demographic and SES constructs that will be merged in Step 2 with the identical set from the ACS 2011-2015 benchmark.  You can rerun this data prep R code and I assume that if the same seed is used that the stochastic output of the imputation of ABCD missing data should match that produced when Pat ran it.
Nov_2018_ACS_ABCD_Poolweight_R.R:  (Step 2); This is the R code that merges the prepped ACS and ABCD data files (with missing data imputed in previous steps), estimates the propensity model, computes and scales/trims the propensity weights and finally rakes the scaled weights to final ACS control totals by age, sex and race/ethnicity.
- Nov2018abcd_R.Rds- This is the final output file that contains the final raked, propensity weight. It currently is labeled "rpwgtmeth1" but could be renamed to something less mysterious if you choose.
Finally, the .zip file contains an MS Word file that includes output from the R computation of the weights and my earlier SAS V9 programming.  I have checked the comparative results and everything is OK.  There will be very minor differences in the two systems' outputs due to the fact that they use similar but not equivalent programs for imputation of the item missing data in the ABCD data dump.  The raking software code in the SAS and R versions may also have slightly different convergence criteria.

(Note from Rong Zablocki: rpwgtmeth1 has been renamed to acs_raked_propensity_score in NDA2 release).