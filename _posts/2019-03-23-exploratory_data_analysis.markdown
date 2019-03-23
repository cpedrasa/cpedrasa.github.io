---
layout: post
title:      "Exploratory Data Analysis"
date:       2019-03-23 23:03:08 +0000
permalink:  exploratory_data_analysis
---


I may have already mentioned in my introduction that my background is in nursing informatics. I have worked on compliance/quality and health information exchange, so the only technical tool I was equipped with before joining this course was my SQL knowledge. It was very tempting for me to just leverage that skill set in the exploratory data analysis (EDA) phase of the project as I am not as comfortable extracting the information I need for the data analysis using pandas. I feel I really need to demonstrate understanding of the different tools I have learned in Module 1, so I thought I’d share how I could have done the EDA using the SQL tools I was familiar with.  


```
#We begin by importing the libraries needed for reading the file and processing the dataset
import pandas as pd
import numpy as np

#Import the libraries needed for visualization
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns

df = pd.read_csv('kc_house_data.csv')
date = pd.to_datetime(df['date'])
df['month'] = pd.DatetimeIndex(df['date']).month
df['day'] = pd.DatetimeIndex(df['date']).dayofweek
df.head()
```


```
from pandasql import sqldf
pysqldf = lambda q: sqldf(q, globals())
q0 = """SELECT count(*) as TotalRows , * FROM df;"""
RESULT = pysqldf(q0)
RESULT.columns.tolist()
```
> 'TotalRows', 'id', 'date', 'price', 'bedrooms', 'bathrooms',
       'sqft_living', 'sqft_lot', 'floors', 'waterfront', 'view', 'condition',
       'grade', 'sqft_above', 'sqft_basement', 'yr_built', 'yr_renovated',
       'zipcode', 'lat', 'long', 'sqft_living15', 'sqft_lot15', 'month',
       'day'
			 
Here are the SQL queries I would use before I knew about value_counts().			
			 
```
q1 = """SELECT  view, count(view) as ctview FROM df GROUP BY view ORDER BY 2 desc;"""
q2 = """SELECT  floors, count(floors) as ctfloors FROM df GROUP BY floors ORDER BY 2 desc;"""
q3 = """SELECT  bedrooms, count(bedrooms) as ctbedrooms FROM df GROUP BY bedrooms  ORDER BY 2 desc;"""
q4 = """SELECT  waterfront, count(waterfront) as ctwaterfront FROM df GROUP BY waterfront ORDER BY 2 desc;"""
dview = pysqldf(q1)
dfloors = pysqldf(q2)
dbedrooms = pysqldf(q3)
dwaterfront = pysqldf(q4)

display(dview,dfloors,dbedrooms,dwaterfront)

fig, axes = plt.subplots(ncols=4, nrows=1, figsize=(16, 4))
dview.plot(kind='bar',ax=axes[0])
dfloors.plot(kind='bar',ax=axes[1])
dbedrooms.plot(kind='bar',ax=axes[2])
dwaterfront.plot(kind='bar',ax=axes[3])
```
> View  
0.0   -   19422  
2.0   -         957  
3.0   -      508  
1.0   -      330  
4.0   -      317   
Floors   
1.0   -    10673    
2.0   -     8235  
1.5   -     1910  
3.0   -      611  
2.5   -      161  
3.5   -        7  
Bedrooms  
3   -     9824  
4   -     6882  
2   -     2760  
5   -     1601  
6   -      272  
1   -      196  
7   -       38  
8   -       13  
9   -        6  
10   -       3  
11   -       1  
33   -       1  
Waterfront  
0.0   -    19075  
1.0   -     146  

And here are the queries I would use to identify the number of rows with null values.

```
from pandasql import sqldf
pysqldf = lambda q: sqldf(q, globals())
q5 = """select 
sum(case WHEN bathrooms IS NULL THEN 1 ELSE 0 END) as nbathrooms,
sum(case WHEN bedrooms IS NULL THEN 1 ELSE 0 END) as nbedroom,
sum(case WHEN condition IS NULL THEN 1 ELSE 0 END) as ncondition,
sum(case WHEN floors IS NULL THEN 1 ELSE 0 END) as nfloors,
sum(case WHEN grade IS NULL THEN 1 ELSE 0 END) as ngrade,
sum(case WHEN price IS NULL THEN 1 ELSE 0 END) as nprice,
sum(case WHEN sqft_living IS NULL THEN 1 ELSE 0 END) as nsqft_living,
sum(case WHEN waterfront IS NULL THEN 1 ELSE 0 END) as nwaterfront,
sum(case WHEN view IS NULL THEN 1 ELSE 0 END) as nview,
sum(case WHEN yr_renovated IS NULL THEN 1 ELSE 0 END) as nyr_renovated,
sum(case WHEN yr_built IS NULL THEN 1 ELSE 0 END) as nyr_built from df;"""

NumberOfNulls = pysqldf(q5)
display(NumberOfNulls)
```
> nbathrooms = 0  
nbedroom	 = 0  
ncondition	 = 0  
nfloors	 = 0  
ngrade	 = 0  
nprice	 = 0  
nsqft_living	 = 0  
nwaterfront	 =2376  
nview	= 63  
nyr_renovated	= 3842  
nyr_built = 0  

Here are the queries to explore the data on zip code where the most expensive home and thehighest number of homes sold, and the best month or day of the week to sell a home.

```
q6 = """SELECT zipcode, count(zipcode) as HighestNumHomeSold FROM df GROUP BY zipcode ORDER BY 2 desc LIMIT 1;"""
q7 = """SELECT zipcode, max(price) as MostExpensiveHomeSold FROM df  ORDER BY 1 desc;"""
q8 = """SELECT month, count(month) as NumberHomesSold FROM df GROUP BY month ORDER BY 2 desc ;"""
q9 = """SELECT (case WHEN day = 0  THEN 'Monday'
                    WHEN day = 1  THEN 'Tuesday'
                    WHEN day = 2  THEN 'Wednesday'
                    WHEN day = 3  THEN 'Thursday'
                    WHEN day = 4  THEN 'Friday'
                    WHEN day = 5  THEN 'Saturday'
                    ELSE 'Sunday' END ) as dayofweek, 
count(day) as NumberHomesSold FROM df GROUP BY day ORDER BY 2 desc ;"""


HighestNumHomeSold = pysqldf(q6)
display(HighestNumHomeSold)

ZipCodeMostExpensiveHomeSold = pysqldf(q7)
display(ZipCodeMostExpensiveHomeSold)

MoNumberHomesSold = pysqldf(q8)
display(MoNumberHomesSold)

DayNumberHomesSold = pysqldf(q9)
DayNumberHomesSold

```

> zipcode	= 98103           HighestNumHomeSold = 602  
> zipcode	= 98102           MostExpensiveHomeSold = 7700000.0  
> Month     -                           NumberHomesSold    
5	   -    2414            #May  
4	   -    2229  
7	   -    2211  
6	   -    2178  
8	   -    1939  
10 -    1876  
3   -    	1875  
9   -    	1771  
2   -     1470  
11   -  1409  
2   -    	1247  
1   -    	978  


In the end, I am thankful that I did not go that route and went out of my comfort zone. As I read about more of the different pandas tools and how I could leverage python's loops and functions, I am glad that I really took the time to use and understand the breadth of tools introduced in this course. It is incredibly useful to know pandas functions (e.g. df.value_counts, df.describe, df.nunique, df.isna, df.value_counts, df.fillna, df.replace, df.drop, pd.cut) to quickly explore series or dataframes. I honestly would have taken a longer time doing it via SQL, as my use of it is limited to SELECT statements and I haven't used much of SQL's INSERT, DELETE, and UPDATE syntax. In the past, I would create all the joins in SQL, save my dataset in excel, and do all of my data exploration in excel, which is not as reproducible compared to creating the code using python pandas in jupyter notebook. Learning all these new techniques allowed me to start incorporating them into my daily work. 
