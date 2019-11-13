---
layout: post
title:      "Racial Bias in Machine Learning Algorithm"
date:       2019-11-13 05:20:44 +0000
permalink:  racial_bias_in_machine_learning_algorithm
---


For this blog post requirement, I have chosen to discuss the research article,  

 "Dissecting racial bias in an algorithm used to manage the health of populations" 

https://science.sciencemag.org/content/366/6464/447.full 

     
In summary, a racial bias was uncovered with the medical algorithm that predicts which patients are at risk and will benefit from extra medical care and coordination program. The algorithm significantly underestimated the health needs of Black patients who were assigned the same level of risk but are sicker than White patients. As a result, a significant proportion of care management resources were withheld - reducing by almost half, the number of Black patients identified for extra care.   
  

The bias or racism in the algorithm was not deliberate. Race was excluded in the risk scoring.  Unfortunately, the bias occurred because the algorithm uses health costs as a representation for health needs. The algorithm risk scored patients based on the health costs they incurred, with the assumption that if their healthcare costs were high, it was because their healthcare needs were high. The bias emerged because the algorithm predicted the health care costs rather than illness but the unequal access to care resulted in less money spent caring for black patients.  
  

However, that assumption that high costs equates high need turned out to be false, because fewer healthcare dollars is spent on Black patients compared to White patients who have the same level of need.  The algorithm incorrectly concluded that Black patients are healthier than equally sick White patients. To remedy the racial bias in predicting who needs care coordination program, costs will no longer be used as a substitute to more effective measures for health needs.  
 

Awareness of bias in machine learning is important to me as a healthcare professional and a data science student as more and more machine learning models have been increasingly implemented in the healthcare setting and many other industries and have started to play bigger roles in important decision making.   


Many of the risk prediction machine learning software in healthcare work on unsupervised machine learning models, blinded to race and other features and using unlabeled data to cluster patients with similar characteristics, supplementing clinical data with third party purchased public data on environmental, census, transportation, digital habits, behavioral and other lifestyle factors to enrich socio-economic risk factors for risk prediction. ML vendors use proprietary algorithms which, unlike ordinary electronic medical software, there's no easy way for the Clinical Informatics Team to validate. How does one play a proactive role in this scenario?

 
As aspiring data scientists, we set out to learn different types of Machine Learning (ML) algorithms, strive to understand the metrics to evaluate ML algorithms, seek to improve model accuracy and recalibrate, acquire the skills to deploy predictive models, fine tune, and other technical knowledge and skills needed to consistently and reliably deliver high-quality predictions.   As data scientists develop  algorithms that may help diagnose a condition, or detect credit card fraud, or aid in deciding on loan applications, etc., we carry with it a responsibility to design with fairness in mind and a conscious effort to evaluate any prejudice it may create that will disadvantage or cause harm to specific groups.
