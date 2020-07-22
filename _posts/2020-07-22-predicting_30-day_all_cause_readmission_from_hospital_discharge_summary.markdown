---
layout: post
title:      "Predicting 30-day All Cause Readmission from Hospital Discharge Summary"
date:       2020-07-22 17:57:56 +0000
permalink:  predicting_30-day_all_cause_readmission_from_hospital_discharge_summary
---


In Module 5, I worked on different binary classifier algorithms to predict 30-day readmission for diabetic patients utilizing structured information from an electronic health record. Building on the knowledge gained from the previous project, I plan to explore natural language processing for the capstone to expand that work to optimally predict 30-day, all-cause hospital readmissions, which occur when a patient is readmitted to an inpatient hospital for any reason within 30 days of a prior inpatient discharge. 

Why is predicting patients that are at risk for hospital readmissions important? 

Hospital readmissions are associated with unfavorable patient outcomes and high financial costs.  Healthcare Regulatory Agencies are focused on 30-day readmission rates as a measure of healthcare quality and emphasize its reduction as a strategy to reduce healthcare costs while also maintaining quality. 

* CMS began penalizing hospitals for 30-day readmissions on Oct. 1, 2012 at 1 percent, upping the penalty rate to 2 percent for fiscal year 2014. 
* In 2020, CMS will cut payments to the penalized hospitals by as much as 3 percent for each Medicare case. 
* The average cost of a readmission for any given cause (All-cause readmissions) is $11,200, with a 21.2 percent readmission rate.  

If data is collected that helps providers identify and care for high-risk patients, hospital readmissions can be better managed through preparation and will ultimately improve the quality of care, patient satisfaction, ultimately reducing overall health care costs while potentially avoiding CMS penalties for excessive rehospitalization rates.

Currently, clinical data use is limited to the structured information. Dashboards are limited to reporting discrete data elements and coded information. However, it was reported that more than 80 percent of a healthcare organization's data is unstructured, including physician notes, clinical assessments, registration forms, discharge summaries and other nonstandardized electronic forms, which makes data collection and analysis difficult using standard methods. 
Insights we could get from the doctors notes that are free text in nature and if we are able to identify risk factors from the sea of data, we might be able to supplement prediction of readmission risk and improve outcomes for the patients.

Unstructured data represents a greater challenge to analyze and interpret than structured data.  Unstructured Data 
does not always have the pre-defined ontology as the structured data and are difficult to search.  Several data standards have been developed to record and store structured data in a consistent manner.  For example, the Logical Observation Identifiers Names and Codes (LOINC) is the universal standard identifying medical laboratory observations. Health Level 7 (HL7) is a broader standard that includes administrative health data in addition to clinical health data for exchanging structured health data between databases and systems.  However, advances in artificial intelligence and machine learning have the potential to transform and to streamline the way unstructured data is utilized.

A lot of unstructured data is being created in the hospital but for this initiative, we would like to like to limit the scope to the discharge summary note.  We will use natural language processing to turn the unstructured discharge summary data into information that will help identify at-risk patients and allow the clinicians to intervene. 

Hospital discharge summaries serve as the primary documents communicating a patient’s care plan to the post-hospital care team. The discharge summary is the form of communication that accompanies the patient to the next setting of care. High-quality discharge summaries are generally thought to be essential for promoting patient safety during transitions between care settings, particularly during the initial post-hospital period. It plays an important role in preventing avoidable hospital readmissions.  

The Joint Commission, an accrediting and certifying organization that serves to improve healthcare, mandates that six components be present in all U.S. hospital discharge summaries:
1. Reason for hospitalization
2. Significant findings
3. Procedures and treatment provided
4. Patient’s discharge condition
5. Patient and family instructions (as appropriate)
6. Attending physician’s signature


We will utilize the MIMIC-III (Medical Information Mart for Intensive Care III), a free hospital database. Mimic III is a relational database that contains de-identified data from over 40,000 patients who were admitted to Beth Israel Deaconess Medical Center in Boston, Massachusetts from 2001 to 2012. MIMIC-III contains detailed information regarding the care of real patients, and as such requires credentialing before access.

In order to get access to the data for this project, you will need to request access at this link (https://mimic.physionet.org/gettingstarted/access/) and complete the required training course at CITI “Data or Specimens Only Research"

1. Register for the required training course as “Massachusetts Institute of Technology" affiliate
2. Request access to the Mimic Database:
3. Download the full MIMIC-III dataset from: https://doi.org/10.13026/C2XW26
4. Access import.py at https://github.com/MIT-LCP/mimic-code/tree/master/buildmimic/sqlite to generate a SQLite database from the MIMIC-III csv files.

The two main tables that will be utilized for the project are "admissions" and "noteevents."
The admissions table will be filtered to the non-elective and non-newborn encounters and new attributes will be created to determine readmitted encounters, our target variable.  The noteevents table will be filtered to the discharge summary category and will filter out for notes that are marked in error.
The admissions and notes will then be merged to obtain the discharge summary text.

Raw text needs to be scrubbed before fitting a machine learning or deep learning model. There is a whole suite of text NLTK preparation libraries that are available for use. Text cleaning is task specific and based on the review of the text, we will keep it simple for this project as we are dealing with medical terminologies and abbreviations, thus we will perform manual tokenization and cleaning. 

Once we have split the discharge note raw text into tokens (words), removed punctuations, non-alphabetic, new line characters and carriage returns, we will filter out tokens that are stop words. These are the most common words that do not contribute to the deeper meaning of the phrase e.g. "is" ,"the"
Some documents may benefit from stemming,  i.e.  process of reducing each word to its root or base. We noticed a significant amount of incorrect words when they have been reduced to their stems and so we are skipping this process.  

After cleaning the data, we need to convert the discharge text to numbers. We may want to perform classification of documents, so each discharge summary from the admission is an input and a class label ("Readmission") is the output for our predictive algorithm. Algorithms take vectors of numbers as input, therefore we need to convert documents to fixed-length vectors of numbers.  For this project we will be utilizing the Bag-of-Words Model, or BoW. This model doesn't focus about the order of words but focuses on the occurrence of words in a document or the degree to which they are present in encoded. 

"CountVectorizer" will be utilized to tokenize a collection of text documents,  build a vocabulary of known words and also to encode new documents using that vocabulary. Here are the steps:

1.	Create an instance of the CountVectorizer class.

```console
vector = CountVectorizer(max_features = 3000, tokenizer = clean_tokenize, stop_words = stop_words)
```
2.	 Call the fit() function in order to learn a vocabulary from one or more documents.

```console
vector.fit(df_train.TEXT.values)
```

3.	Call the transform() function on one or more documents as needed to encode each as a vector.  
```console
X_train_tf = vect.transform(df_train.TEXT.values)
X_valid_tf = vect.transform(df_valid.TEXT.values)
```
The encoded vector is returned with a length of the entire vocabulary and an integer count for the number of times each word appeared in the document. The encoded vectors can then be used directly with a machine learning algorithm.

For more details on the model selection, tuning, and evaluation, please refer to the full project code that is available in github https://github.com/cpedrasa/dsc-capstone-project-v2-online-ds-sp-000

