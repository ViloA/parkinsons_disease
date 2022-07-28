## Table of Content

1. [Problem Statement](#problem-statement)
2. [The Data](#the-data)
3. [Data Dictionary](#data-dictionary)
4. [Executive Summary](#executive-summary)
5. [Conclusions and Recommendations](#conclusions-and-recommendations)
6. [Future Improvements](#future-improvements)
7. [References](#references)

**Notebooks**
1. [01_Cleaning_EDA](/Code/01_Cleaning_EDA.ipynb)
2. [02_Preprocessing](/Code/02_Preprocessing.ipynb)
3. [03_Modeling](/Code/03_Modeling.ipynb)


## Problem Statement

Parkinson's disease is a progressive disorder that affects the nervous system and the parts of the body controlled by the nerves. My goal for this project is to find a model that can predict Parkinson's disease in a patient to the highest degree of accuracy based on features derived from vocal recordings, which could then be possibly used as an additional screening tool in a hospital or examination room setting.

## The Data

This dataset is composed of a range of biomedical voice measurements from 31 people, 23 with Parkinson's disease (PD). Each column in the table is a particular voice measure, and each row corresponds one of 195 voice recording from these individuals ("name" column). The main aim of the data is to discriminate healthy people from those with PD, according to "status" column which is set to 0 for healthy and 1 for PD. The dataset was downloaded from the UC Irvine machine learning repository at [Parkinson's Dataset](https://archive-beta.ics.uci.edu/ml/datasets/parkinsons).



## Data Dictionary

|Feature|Type|Description|
|---|---|---|
|**name**|*object*|subject name and recording number|
|**MDVP:Fo(Hz)**|*float64*|Average vocal fundamental frequency|
|**MDVP:Fhi(Hz)**|*float64*|Maximum vocal fundamental frequency|
|**MDVP:Flo(Hz)**|*float64*|Minimum vocal fundamental frequency|
|**MDVP:Jitter(%)**|*float64*|MDVP jitter in percentage|
|**MDVP:Jitter(Abs)**|*float64*|MDVP absolute jitter in ms|
|**MDVP:RAP**|*float64*|MDVP relative amplitude perturbation|
|**MDVP:PPQ**|*float64*|MDVP five-point period perturbation quotient|
|**Jitter:DDP**|*float64*|Average absolute difference of differences between jitter cycles|
|**MDVP:Shimmer**|*float64*|MDVP local shimmer|
|**MDVP:Shimmer(dB)**|*float64*|MDVP local shimmer in dB|
|**Shimmer:APQ3**|*float64*|Three-point amplitude perturbation quotient|
|**Shimmer:APQ5**|*float64*|Five-point amplitude perturbation quotient|
|**MDVP:APQ**|*float64*|Measure of variation in amplitude|
|**Shimmer:DDA**|*float64*|Average absolute differences between the amplitudes of consecutive periods|
|**NHR**|*float64*|Noise-to-harmonics ratio|
|**HNR**|*float64*|Harmonics-to-noise ratio|
|**status**|*int64*|Target feature, health status of the subject (one) - Parkinson's, (zero) - healthy|
|**RPDE**|*float64*|Recurrence period density entropy measure|
|**DFA**|*float64*|Signal fractal scaling exponent of detrended fluctuation analysis|
|**spread1**|*float64*|Nonlinear measures of fundamental frequency variation|
|**spread2**|*float64*|Nonlinear measures of fundamental frequency variation|
|**D2**|*float64*|Correlation dimension|
|**PPE**|*float64*|Pitch period entropy|

#### Feature Descriptions

*Below is a more thorough description of features taken from an article that will be cited in the references section*

There are a total of 22 vocal features available in the data set. Details of the feature description are listed in the data dictionary above. For the convenience of voice perturbation feature presentation, we named the average, maximum, and minimum vocal fundamental frequency (in Hz) computed by the Kay Pentax multidimensional voice program (MDVP) with the abbreviations of MDVP:F0, MDVP:Fhi, and MDVP:Flo, respectively. The percentage and absolute jitter values are expressed as MDVP:Jitter(%) and MDVP:Jitter(Abs). The five-point period perturbation quotient and relative amplitude perturbation parameters calculated by the MDVP are written as MDVP:PPQ and MDVP:RAP. The Jitter:DDP denotes the average absolute difference of differences between jitter cycles. The original and logarithmic units of the MDVP local shimmer parameter are named MDVP:Shimmer and MDVP:Shimmer(dB). The abbreviations of Shimmer:APQ3 and Shimmer:APQ5 are short for the three-point and five-point shimmer perturbation quotient values, respectively. MDVP:APQ11 represents the 11-point amplitude perturbation quotient value. Shimmer:DDA is the average absolute differences between the amplitudes of consecutive periods. The noise-to-harmonics ratio and harmonics-to-noise ratio of the acoustic signals are abbreviated as NHR and HNR, respectively. Several nonlinear features include the correlation dimension (D2), recurrence period density entropy (RPDE), detrended fluctuation analysis (DFA), and pitch period entropy (PPE). Two nonlinear measures of fundamental frequency variation are presented as Spread1 and Spread2, respectively.

## Executive Summary

For this project I wanted to choose data where I could build a model that would help people that are suffering from a disease. I came upon the Parkinson's dataset that is found in the UC Irvine machine learning repository website. The summary of my problem statement, given the data, is to attempt to build a model that can predict Parkinson's disease to the highest degree of accuracy.

While cleaning the data I first checked null values, data types, and duplicates. None of these needed to be changed. The outliers were checked for all features. Although there were some within the dataset, most of them belonged to people who exhibited the disease. If I were to drop these outliers, given the small dataset, I could be removing traits of the disease as well as a removing a significant percentage of the dataset. I chose to keep the outliers in place and during the modeling phase I chose models that are robust to outliers. During EDA, I saw  clear evidence of significant differences in average values between a healthy patient and one who has Parkinson's disease based on vocal features.

As far as pre-processing the data before use in modeling, the data was already numerical and there were no categorical or ordinal features that needed to be encoded. I used standard scaling on all features before using SelectKBest to choose the 10 best features to be used in modeling. The remaining features were processed through principal component analysis to capture the signal and reduced to 5 features. The 10 features chose by SelectKBest and the 5 PCA features were combined to be used for modeling.

Due to previously keeping dataset outliers, certain models were chosen for being robust against outliers. Random forest, support-vector machine, and Gradient Boosting models were chosen for testing. From these models many different parameters were tested while using GridSearchCV. Confusion matrices and cross val scores were used to measure success metrics.



## Conclusions and Recommendations

Given the outcome of our models, it would seem Parkinson's disease can be successfully predicted using recordings of a patient's voice. I feel this would be an excellent tool for a doctor to use while in early stages of diagnosing if a patient may have Parkinson's disease. I believe that with some further work I would be able to build an algorithm that could take in raw vocal data and feed it through a pipeline where everything gets processed and sent through to the final production model.

As far as which model I would choose for production - the first model I would choose for production would be my Random Forest model. It performed the best at accurately predicting sick patients with only 1 false negative. It did falsely predict that 12 patients were sick when in fact they were healthy, but we could use this as a screener to get the patient examined again by other means. It is safer to tell a healthy patient that they may have the disease as opposed to telling a sick patient that they are completely healthy.

With that note I would only use the random forest model until I can improve the gradient boosting model to the level of accuracy at predicting sick patients to the level of the random forest model. The reason is that the gradient boosting performed better overall. It only had 2 false negatives and 1 false positive.

I recommend investing time and money in my work in order to complete these goals so we could have a non-invasive tool that could quickly and accurately determine if a patient may have Parkinson's disease. This could be used in a doctor's office or hospital setting where they could ask a the patient to say a predetermined phrase in a microphone and get a result in minutes.


## Future Improvements

- The main improvement would be to continue to work with the gradient boosting model by tweaking parameters to give us the best possible prediction.

- If we are able to gather more samples it would improve our model overall.

- Once enough samples are gathered for a robust model, I would begin building the structure needed to deploy the model for testing purposes in real world scenarios.

- Funding for research and development


## References
1. Wu Y, Chen P, Yao Y, Ye X, Xiao Y, Liao L, Wu M, Chen J. Dysphonic Voice Pattern Analysis of Patients in Parkinson's Disease Using Minimum Interclass Probability Risk Feature Selection and Bagging Ensemble Learning Methods. Comput Math Methods Med. 2017;2017:4201984. doi: 10.1155/2017/4201984. Epub 2017 May 3. PMID: 28553366; PMCID: PMC5434464.
[National Library of Medecine](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5434464/)
