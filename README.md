# The Harrall Algorithm

**A better performing algorithm for identification of implausible growth data from longitudinal pediatric medical records**

*Kylie K. Harrall, Sarah M. Bird, Keith E. Muller, Laruen A. Vanderlinden, Maya E. Payton, Anna Bellatorre, Dana Dabelea, and Deborah H. Glueck*

This anthropometric cleaning algorithm is available in both SAS and R. Both versions are accessible in the "AlgorithmProgs.zip" folder. For a detailed explanation of use cases for this algorithm, please see the example programs folder in the "AlgorithmProgs.zip" folder.


## Usage:

General calls to the algorithm will look slightly different across the SAS and R versions. This section will document what a general call looks like in both versions. An example Harrall Algorithm pipeline

### SAS Program:

*Cleaning the Heights*

* macro: IterativeLoop
    * variables: 
        * InLib: the library the dataset for analysis is stored in
        * OutLib: the library the output data will be stored in
              * InPath: Path to the raw data on laptop
              * OutPath: Desired path to output dataset
              * InDS: The name of the input dataset
              * OutDS: Name of desired output dataset
              * PIDVar: Name of identifying variable for participants
              * HeightVar: Name of height variable in original dataset
              * AgeVar: Name of age variable in original dataset
              * SourceVar: Name of the source variable in original dataset. The algorithm will sort this variable using "proc sort" and remove any duplicated measurements that occur at the same timepoint for a participant. The first observation after sorting will be retained
              * MaxIterations: The number of iterations to be completed as a maximum for computational efficiency
              * MaxPeaks: The number of peaks that the data should contain as an upper bound (typically 0)
              * OutFileExt: File extension for document that contains cleaning statistics across all iterations of the algorithm

* %iterativeLoop( InLib         = In01,
                 OutLib        = Out01,
                 InPath        = indata,
                 OutPath       = outdata,
                 InDS          = data_need_to_clean,
                 OutDS         = cleaned_heights,
                 PIDVar        = ID_Variable,
                 HeightVar     = lg_cm,
                 AgeVar        = ageyears,
                 SourceVar     = source,
                 MaxIterations = 100,
                 MaxPeaks      = 0,
                 OutFileExt    = xlsx);



*Cleaning the Weights*

        * macro: WeightLoop
              * variables: 
                          * InLib: the library the dataset for analysis is stored in
                          * OutLib: the library the output data will be stored in
                          * InPath: Path to the raw data on laptop
                          * OutPath: Desired path to output dataset
                          * InDS: The name of the input dataset
                          * OutDS: Name of desired output dataset
                          * PIDVar: Name of identifying variable for participants
                          * WeightVar: Name of weight variable in original dataset
                          * AgeVar: Name of age variable in original dataset
                          * SourceVar: Name of the source variable in original dataset. The algorithm will sort this variable using "proc sort" and remove any duplicated measurements that occur at the same timepoint for a participant. The first observation after sorting will be retained
                          * MaxIterations: The number of iterations to be completed as a maximum for computational efficiency
                          * MaxPeaks: The number of peaks that the data should contain as an upper bound (typically 0)
                          * OutFileExt: File extension for document that contains cleaning statistics across all iterations of the algorithm
%WeightLoop( InLib            = Out01,
                OutLib        = CleanedData,
                InPath        = indata,
                OutPath       = outdata,
                InDS          = cleaned_heights,
                OutDS         = final_data,
                PIDVar        = ID_Variable,
                WeightVar     = wt_kg,
                AgeVar        = ageyears,
                SourceVar     = source,
                MaxIterations = 100,
                MaxPeaks      = 0,
                OutFileExt    = xlsx);               


### R Program

/* create variables in R session for each of the following required arguments in 
        the iterative loop */
outputHt    <- "path to folder for height cleaning data"
outputWt    <- "path to folder for weight cleaning data"
outDSNameHt <- "Name of Excel File for height cleaning data"
outDSNameWt <- "Name of Excel File for weight cleaning data"
age         <- "Name of age variable in dataset"
height      <- "Name of height variable in dataset"
weight      <- "Name of weight variable in dataset"
ID          <- "Name of patient identifier in dataset"
source      <- "Name of source variable in dataset"

/* this step will clean the heights */
HtCleanedData <- Height_IterativeLoop(data=data, maxpeaks=0, maxiterations=100,
                                      userAgeVar = age, userHeightVar = height, 
                                      userIDVar = ID, userSourceVar = source,
                                      summaryPath = outputHt, outputName = outDSNameHt)

/* this step will clean the weights */
WtCleanedData <- Weight_IterativeLoop(data=CleanHts, maxpeaks=0, maxiterations=100,
                                      userAgeVar = age, userWeightVar = weight, 
                                      userIDVar = ID, userSourceVar = source,
                                      summaryPath = outputWt, outputName = outDSNameWt)






## Copyright Information 
Copyright 2023 The Regents of the University of the Colorado
The Harrall Algorithm is released under the GNU General Public License, version 2.




