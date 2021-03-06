# CumulativeProspectTheory
The files uploaded run Bayesian Parameter Estimation for Cumulative Prospect Theory (CPT). The R code loads in the data and runs RStan to execute the Bayesian parameter estimation. 

There are six files. Two files for data with only positive valued gamble options. Two files for data with only negative valued gample options. Two files for data with positive and negative valued gamble options. According to CPT, there are two sets of parameters: one for positive values and one for negative values. Therefore, separating the estimation into positive, negative, and mixed is necessary, as the absence of positive or negative values means there is no data to estimate parameters.

An important note is that for negative only data, it is impossible to estimate lambda and phi(also called luce). The reason comes from the form CPT. Given two options to choose from A and B, both with outcomes x and probabilities p. 

Outcomes and probabilities associated with positive outcomes are weighted using the formulas:
         
         v(x+)=x^alpha
         w(p+)= p^gamma/(p^gamma - (1-p)^gamma)^(1/gamma)
Outcomes and probabilities associated with positive outcomes are weighted using the formulas:
                  
         v(x-)=lambda*x^beta
         w(p-)= p^delta/(p^delta - (1-p)^delta)^(1/delta)
The value of option is now:
     
     A=SUM(v(x_i)*w(p_i))
     
The likelihood of a subject choosing option B over option A is then:
    
    Pr(B>A)= 1/(1+exp(luce*(B-A)))

If all outcomes are negative, the 

    B-A=SUM(lambda*x_j^beta*w(p_j))-SUM(lambda*x_j^beta*w(p_j))=lambda*(SUM(x_j^beta*w(p_j))-SUM(x_j^beta*w(p_j)))
So effectively lambda and luce become lambda * luce and cannot be estimated separately. 
         
## How to use the code
There are two ways to use the code. The first is running it on data to determine the parameters for CPT. The other is to validate it. This is the same for all three R files.

### Experimental Use
 Set testingKnownData=0
 
 Set the workingDirectory
 
 Set the nameOfFilesSave to say where to save the data
 
 firstSubject- the index (in rawdata) of the first subject being run. If you are running all the subjects, just set to 1.
 
 subsetOfSubjects - the position of the last subject being run in the data files.
 
 prospects_b - option B. There should be four columns for each subject in the data file. Columns are tab separated.
 
     The file should have the following structure:
     
     Column 1 - value of outcome 1 for the first subject
     
     Column 2 - probability of outcome 1  for the first subject
     
     Column 3 - value of outcome 2 for the first subject
     
     Column 4 - probability of outcome 2 for the first subject
     
     Repeat for n subjects. 
     
     If this is a sure option, then the values entered for columns 3 and 4 should be 0.
     Example: An option will return 100 10% of the time, and 50 90% of the time. In the data it will appear as 100  .1  50  .9
     
     Add the 2nd subject's data in the 5th through 8th columns and the 3rd subject's in the 9th through 12th columns, and so forth. If the number of trials is not equal across subjects, enter NaN in the text file until the total number of rows is equal.
 
 prospects_a - See prospects_b
 
 rawdata - contains the subjects choices. A 1 indicates prospects_b was chosen, a 0 indicates prospects_a was chosen. Each subject's data is
 in a column. If the number of trials is not equal across subjects, enter NaN in the text file until the total number of rows is equal.
 
 items_n - 3xn row of numbers indicating how many data points exist for each subject, excluding NaNs. The first row contains the number of positive only
             data points. The second row contains the number of negative only data points. The third row contains the number of mixed (positive and
             negative only) data points. If you have positive and negative data and you only want one set of parameters, you need only the third row. If 
             htat is the case, you can set the other rows to 0. 
             
             If subjects 1, 2, 3, and 4 had 100, 130, 124, and 0 positive only data points, 0, 10, 20, and 200 negative only data points, the file would
             look like this:
             100 130 124 0
             100 140 144 200
             100 140 144 200

### Testing use.
 If you are testing and want to check how the code works by generating data from known parameters, set the following parameters to the files continaing  them:
 knownAlpha, knownBeta, knownGamma, knownDelta, knownLambda, knownLuce - These are the known values of the parameters alpha, gamma, and luce where alpha=(0,1), beta=(0,1), gamma=(0,1), delta=(0,1), Lambda=(1,5), luce=(.5,1.5)

## Data
The data folder contains 300 randomly generated data sets using randomly generated parameters. The files with the parameters in the name contain the parameters for each subject. The data is stored in the folders dataset18, dataset17, and dataset16. Each folder contains choice option A, choice option B, choice data, and the items_n file. 

dataset16: uses outcomes and probabilities are randomly generated values.

dataset17: uses outcome results that are a larger scale than dataset17. The outcome values are -4, -3, -2, 0, 2, 3, and 4. The probabilities are .2, .5, and .8.

dataset18: uses outcome results that are a larger scale than dataset17. The outcome values are -75, -50, -25, 0, 25, 50, and 75. The probabilities are .2, .5, and .8

## Results
The results folder contains the results of using the CPT bayesian estimation code on a data set.

The data is coded as follows:
     
     - "all" means that positive, negative, and mixed were used
     - "mixed" means that only mixed were used
     - "negative" means that only negative options were used
     - "positive" means that only positive options were used
     - "var" means that a random number between 0 and 10 was choosen for each subject and each type of data. That number of decision events was removed. Example: a subject with all might have gotten 8,0,5. Therefore 8 decisions were removed from positive, 0 from negative, and 5 from mixed.
     - Number indicates what the maximum number of decision events there were in that file (not separated by type so positive 539 means there were 539 decision events across the positive, negative and mixed decision events).
     
Within each folder are:

    - The .csv files with "parameter"estimate_dataset in the name that that has the estimates of each parameter, including the standard deviation, the median, and the 95% and 75% confidence intervals. 
    - The .png files that contain plots of the actual parameter values vs the estiamted parameter values with 95% error bars and a fit line
    - The .csv file with covariancematrixestimate that contains all the covariance matrices produced in estimating the data
    - The .csv file with chisquared in the name. This file lists the chi squared value and the degrees of freedom for each parameter and all the parameter values using the covariance matrix.
