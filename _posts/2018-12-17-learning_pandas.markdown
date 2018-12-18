---
layout: post
title:      "Learning Pandas"
date:       2018-12-17 23:45:52 -0500
permalink:  learning_pandas
---


Eight weeks have passed since I started the self-paced data science program and the sections I have found challenging so far are the ones about pandas.  I think I spent more than two weeks on those sections trying to learn the syntax for data analysis and manipulation, until I came across the Data Science Career Student Resource on Pandas Recipe for Success with links to tutorial videos.  I am a visual learner and I found it really helpful to build up the muscle memory for python pandas by coding along and spending time practicing DataFrames manipulations and performing operations. Part of the reason I think that I spent more time learning pandas is because I currently have an issue at work where I still need to deal with flat file interfaces, and I wanted to explore time-saving tools to help with data analysis across flat files, table outputs from SQL, and cache databases.  After learning pandas DataFrames, here are a few tips on how pandas has helped me to be more organized and to speed up my work.

* I could quickly read files in various formats (i.e. csv, tab delimited, json, pipe-delimited files) without opening every single flat file and filtering columns to find specific information.
    
 ```
import pandas as pd 
df = pd.read_table(‘file.csv')
df = pd.read_table(r'file.txt', sep="|")
df[df['Insurance1'].str.startswith('MCARE') & df[‘ProcedureDescription’].str.startswith('Coronary artery bypass graft’)]

import json
with open('NSUH.json') as data_file:    
data = json.load(data_file)
```

* I didn’t have to import multiple flat files into SQL in order to link the files/tables to perform data analysis and manipulations. 
     
```
pd.concat([df1, df3], join=“inner")
```

* Creating filters, pivots, and charts can be completed easily. There is no longer a need to create multiple versions of excel files to show the different filters, pivot tables, and charts that I have created for data exploration. 

 ```
import matplotlib.pyplot as plt
%matplotlib inline
df = pd.DataFrame()

df.plot.<TAB>
df.plot.area     df.plot.barh     df.plot.hist     df.plot.line     
df.plot.bar      df.plot.box       df.plot.pie.    df.plot.scatter
```

* Pandas bridges the limitation I have on not having access to linked database servers by providing copies of the data frames from different source systems for analysis and manipulation.

   ```
from pandasql import sqldf
   Pysqldf = lambda q: sqldf(q, globals())
   q = """select Name from df order by PassengerId desc limit 10”""
```

```
import pandas as pd 
import pyodbc

    #Parameters
server = 'server'
db = 'database'
Uid = 'uid'
password ='pwd'

    #Connection
conn = pyodbc.connect('DRIVER={SQL Server};SERVER=' + server + ';DATABASE=' + db + ';Uid=<UID>;PWD=<password>;Trusted_Connection=yes')

    #query db
sql = ''' SELECT TOP 5* 
                    FROM Medications;'''
    
df = pd.read_sql(sql, conn)
df.head()
```
   
It’s been a great learning experience so far and I look forward to learning more data science tools to help improve and speed up my work.
