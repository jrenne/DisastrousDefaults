# DISASTROUS DEFAULTS

Christian Gouriéroux, Alain Monfort,
Jean-Paul Renne and Sarah Mouabbi
Contact: jean-paul.renne@unil.ch

Attached R codes allow the replication of the results and figures displayed in "Disastrous Default", by Gouriéroux, Monfort, Mouabbi and Renne.

The present README file explains to use of theses codes.


# A - To run the codes with default options

The user just has to source "main.R" to produce the tables and figures of the paper.

The only required change is the specification of the appropriate path--line 18 of main.R--that should indicate where the Rcodes folder has been saved on the local disk.

Make sure all required libraries (see top of main.R) are loaded.


# B - Outputs

The outputs are generated if indic.produce.outputs == 1 (default option in main.R).

Outputs are stored in the "figures" and "tables" folders.

The production of all outputs takes about 1 hour using a laptop with a processor 2.3 GHz 8-Core Intel Core i9 (Memory 16 GB 2667 MHz DDR4). This time can be reduced if one diminishes the number of replications of the Monte-Carlo simulations ("nb.replications" in main.R) carried out to study the sensitivity of results to parameter uncertainty (confidence intervals in the plots showing the factors and the disaster indicators). Note that these replications make use of parallel computing; the number of cores used to do so is defined by "number.of.cores" in main.R.


# C - The different settings

At the beginning of main.R, the user can determine the type of computation carried out:

- In order to launch the Maximum-Likelihood estimation of a single model, set indic.estim.KF to 1 (otherwise 0). If indic.estim.KF == 1:
** then one can choose whether the contagion and macro-effect channels are present in the estimated model. Set indic.contagion to 1 (respectively 0) to have contagion (resp. no contagion) in the model. Set indic.macro.effect to 1 (respectively 0) for defaults to have a macroeconomic effect (resp. to have no macroeconomic effect).
** indic.compute.CovMat has to be set to 1 if one wants the covariance matrix of estimated parameters to be computed; indic.save.CovMat has to be set to 1 if one wants to save this covariance matrix (stored in "results/save_CovMatrix.Rdat").


- In order to run the parallel estimation of 27 alternative models based on 3 different values for (a) the average default frequency, (b) the unconditional variance of consumption and (c) the risk aversion, set indic.parallel to 1 (otherwise 0). If parallel computation is run, specify the appropriate number of cores that can be used (number.of.cores).

- In order to produce the different outputs (figures and charts) displayed in the paper, set indic.produce.outputs to 1 (otherwise 0).


# D - Data

The data are in csv files, in folder data.

The macroeconomic data (consumption) are in macro_data.csv. They are from the AWM database (https://eabcn.org/page/area-wide-model). The script that compares data sample moments and model-implied unconditional moments also makes use of awm19up18.csv, that contains the whole AWM database (while macro_data.csv contains a subsample).

The (main) iTraxx data are in ITRAXX4R.csv. These data are extracted from Datastream. The associated Excel Datastream extraction file is data/DatastreamExtractionFiles/ITRAXX.xlsx.

The prices of tranche products are in data4R.csv, the data are based on http://www.creditfixings.com/CreditEventAuctions/itraxx.jsp (This file has to be filled.)

The equity option prices are in AllOptionData.csv. The file Strike_and_ExpiryDates provides details regarding the expiry dates and strikes of considered options. The associated Excel Datastream extraction file is data/DatastreamExtractionFiles/AllOptionData.xlsx.

The estimation sample dates are in dates.csv.

The file Dc_KS.csv is created by data/make.bimonth.conso.R (see Part F). The later script makes use of ESI.csv, which is from the European Commission (https://ec.europa.eu/info/business-economy-euro/indicators-statistics/economic-databases/business-and-consumer-surveys/download-business-and-consumer-survey-data_en).



# E - Estimation

# ---- Single model estimation ----

The model is estimated by maximization of the likelihood function. If the user sets indic.estim.KF to 1 in main.R, then the numerical maximization of the likelihood function is launched (i.e. 'estimation/Estim_KF.R' is sourced). The estimation will start from a previously-estimated specification (depending on which channels--contagion and macro-effects-- are authorized).

Results are saved in "results/save_tempo_KF.Rdat". If the user wants to save the results in another file, she can do it by uncommenting the last line of 'estimation/Estim_KF.R'.

If the user sets indic.compute.CovMat to 1, then the covariance matrix of the parameter estimates is computed; it is further stored in "results/save_CovMatrix.Rdat" if indic.save.CovMat == 1.

# ---- Multi-model (parallel) estimation ----

If the user sets indic.parallel to 1, then a parallel estimation of 27 models will be run. These 27 models correspond to the 27 combinations of
  3 values of average default frequency
x 3 values of unconditional variance of consumption growth
x 3 values of risk aversion.
The number of cores used for this is specified in main.R (number.of.cores). Estimated models are stored in folder "results".



# F - Auxiliary codes

===> outputs/load.alternative.estimated.models.R <===
This script prepares outputs for models that have been estimated without some channels (i.e. with no contagion and/or no macro effects of defaults).
The three solved models are stored in "results/Saved_AlternativeSolvedModels/save_EstimatedAlternativeModels.Rdat"

===> outputs/load.alternative.modified.models.R <===
This script prepares outputs for models derived from the baseline model, switching some channels (i.e. with no contagion and/or no macro effects of defaults).
The three solved models are stored in "results/Saved_AlternativeSolvedModels/save_ModifiedAlternativeModels.Rdat"

===> data/make.bimonth.conso.R <===
This script produces the bi-monthly consumption growth series, using the methodology of Chow and Lin (1971). In this script, set indic.save.bimonthlyconso to 1 in order to save results (stored in "data/csvfiles/Dc_KS.csv").






