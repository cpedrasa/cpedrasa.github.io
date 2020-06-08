---
layout: post
title:      "Predicting 30-day Hospital Readmisssions"
date:       2020-06-08 04:08:02 +0000
permalink:  predicting_30-day_hospital_readmisssions
---

Hospital readmissions are associated with unfavorable patient outcomes and high financial costs.  Diabetes is one of the most frequently treated condition in US Hospitals with high readmission rate.  Healthcare Regulatory Agencies are focused on 30-day readmission rates as a measure of healthcare quality and emphasize its reduction as a strategy to reduce healthcare costs while also maintaining quality. In this Module 5 Project, we tested different binary classifier algorithms to predict 30-day hospital readmissions of patients with diabetes. 

 <center><img src="CareTransitions.png" alt="drawing" width="800"/></center>

**Business Drivers:** (Clinical, Patient Safety, Regulatory, Financial)
* Pinpoint patients with high readmission risk to reduce the occurrences of preventable hospital readmissions and avoidable admissions.  
* Out-predict common approaches to readmission risk stratification by rendering more precise and complete views into patient predispositions by using patient characteristics and other comorbidity index computations in enabling the patient level predictions  
* Improve resource utilization and increase operational efficiency  
* Improve hospital rating based on lower readmission rate and increased patient satisfaction  
* A positive financial return is expected from the readmission and avoidable admission reduction rate.  Revenue generation by decreasing the hospital’s excess readmission ratio that reduces payments for hospitals whose 30-day readmission rates are high relative to other facilities  

## Hospital Readmissions Diabetes Data Set

The Hospital Readmissions Diabetes Data Set was obtained from UCI Machine Learning repository. There are 101,766 diabetic encounters with length of stay between 1-14 days and 50 attributes including our target variable “readmitted” which was redefined as “readmitted <30” days or “not readmitted.

```console
import pandas as pd
df = pd.read_csv('diabetic_data.csv')
```

### Data Preparation

The dataset contains some attributes with cardinality of one, values coded with “?” or missing, duplicate patient encounters, and outliers were removed. Some aggregations were performed on attributes like, number of encounters and medications as well as mapping diagnoses into its specific system categories. Transformation of some attributes to same scale and identification of which specific contributing factors are most likely to impact a specific patient having undesired outcome.  
Identifies which specific contributing factors are most likely to impact a specific patient having the undesired outcome/readmissions.  
+ Target impactable patients with diabetes who are at risk for 30-day hospital readmissions
+ Excluded Discharge disposition: 
    + deceased 
    + left against medical advice
    + hospice
    + transfers
+ Remove duplicate patients – index visit

|Attributes with Null values|Description|
|:---|:---|
|race | 2273 |
|weight| 98569|
|payer_code| 40256 |
|medical_specialty| 49949 |
|diag_1 | 21|
|diag_1| 358|
|diag_3 | 1423|

Below are sample codes used to scrub the data.

```console
# delete all rows with missing diagnosis
Dx = df[(df['diag_1'] == '?' ) | (df['diag_2'] == '?' ) | (df['diag_3'] == '?'  )].index
df.drop(Dx, inplace=True)
df.shape
```
```console
# Change  ? race to 'Unknown'
df.replace({'race': {'?': 'Unknown'}}, inplace=True)
```
```console
# Change admission type urgent and trauma into emergency
df['admission_type_id'].replace([2,7],1, inplace=True)   
```
```console
# Change admission type Null and not mapped into not available (5)
df['admission_type_id'].replace([6,8],5, inplace=True)  
```
```console
#Consolidate discharge disposition reason 
df['discharge_disposition_id'].replace([6, 8, 9],1, inplace=True)                                #Home
df['discharge_disposition_id'].replace([3, 4, 5, 15, 22, 23, 24, 27,30],2, inplace=True)         #Another facility
df['discharge_disposition_id'].replace([16, 17],12, inplace=True)                                #outpatient
df['discharge_disposition_id'].replace([25, 26],18, inplace=True)                                #null
```
```console
#Consolidate admission source reason 
df['admission_source_id'].replace([2,3],1, inplace=True)                                     #Referral
df['admission_source_id'].replace([5, 6, 10, 18, 22, 25], 4, inplace=True)                   #transfer
df['admission_source_id'].replace([15, 17, 20, 21], 9, inplace=True)                         #not available
df['admission_source_id'].replace([12, 13, 14], 11, inplace=True)                            #delivery

```


### Data Exploration

The attributes contributing to early readmission on the index visit were investigated.  In addition to patient administrative data, glycemic factors such as Hemoglobin A1c, max_glucose serum, the number and changes in treatment were explored.  A frequency chart of all the attributes were retrieved using the code below.


```
#preview the unique values from each column 
for col in df[df.columns.difference(['encounter_id', 'patient_nbr'])]:
    try:     
        print('\n',"-- %s --" % col,'\n',df[col].unique()[:30]) #if unique values are > 30
    except:
        print('\n',"-- %s --" % col,'\n',df[col].unique())
```

```console
# Distribution of Readmission 
sns.countplot(df['readmitted']).set_title('Class Distribution')
plt.show();
```
 <center><img src="Readmissions.png" alt="drawing" width="800"/></center>
## Imbalanced Class

Most machine learning algorithms for classification predictive models are designed and demonstrated on problems that assume an equal distribution of classes. 
In most clinical diagnostic predictive analytics projects, this is not usually the case for clinical classification problems/disease detection. In this project, "readmitted" is the minority class and is much harder to predict because there are fewer examples of this class for the model to learn and differentiate examples from the majority class ("not readmitted class"). 
A few strategies employed in this project to correct class imbalance are as follows:
+ Resampling - oversampling/undersampling
+ Repeated Stratified Kfold Cross validation
+ Resample with different ratios/class weights
+ Ensemble different resampled datasets
+ Use the right evaluation metrics

## Comparing various classification models

First, spot check different classification algorithms and measure the baseline performance for benchmarking. 
Apply different sampling techniques and tuning configurations to improve the model performance.  
Accuracy Score can be misleading since we are working with a dataset with where there is a a great difference between the number of positive and negative labels.
Below is the summary of the model comparisons.

|Algorithm |Accuracy Initial|ROCAUC KFold-10|ROCAUC Repeated Stratified Kfold|Tuned|Execution time |
|:---|:---|:---|:---|:---|:---|
|Logistic Regression()|0.9147|0.6192|0.6234|0.626|00:00:03s/4s/12|
|KNNeighbors()|0.9104|0.5158|0.5161|0.515|00:00:11s/10s/32|
|Decision Tree()|0.8294|0.5127|0.5138|0.608|00:00:02s/3s/9|
|Random Forest()|0.9136|0.5593|0.5546|0.622|00:00:04s/4s/13|
|Balanced Random Forest()|0.6038|0.6239|0.6267|0.628|00:00:24s/24s/1:14|
|Bagging Classifier()|0.9112|0.552|0.5551|0.623|00:00:20s/19s/58|
|Balanced Bagging Classifier()|0.7642|0.5785|0.579|0.628|00:00:11s/11s/34|
|ADABoost()|0.9147|0.6181|0.6181|0.624|00:00:15s/15s/47|
|Gradient Boost()|0.9146|0.6282|0.6321|0.633|00:00:55s/55s/2:49|
|XGBoost()|0.9147|0.6283|0.632|0.634|00:01:38s/1:37/4:51|

### Summary
The final model XGBoost Classifier was selected as the best model to predict readmissions.  
Compared to random predictions, results from our predictive model (AUC=.64: Accuracy=91.47%) is encouraging and a good baseline for further improving our model
Fine tuning of the model will help the adoption of the model as a clinical decision system for evaluating readmission. 

While the results from our predictive model may initally have high accuracies, it could be misleading and may need further exploration and consideration of other sampling techniques, algorithm tuning, and use of other metrics that work well with unbalanced data.

Here are some suggestions for future work.

    Implement dashboard of Inpatient encounters and their predicted readmission risk scores and rankings.
    Measure and monitor reduction in readmission rates
    Explore further model improvements/tuning resampling techniques to improve the accuracy of the predictive model
    Experiment on further Feature selection and elimination
    Take best models and Ensemble
    Use alternative metrics for evaluation & thresholding
    Explore optimal tuning parameters to improve accuracy of imbalanced classes
    Use metrics that are robust against unbalanced datasets

* [Please check Github](https://github.com/cpedrasa/dsc-mod-5-project-online-ds-sp-000) - for the project codes.


