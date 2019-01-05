---
layout: post
title:      "More Pandas"
date:       2019-01-04 23:29:07 -0500
permalink:  more_pandas
---

For today's blog assignment, we are supposed to write about something that we did not cover on the course. It took me a while to think about what I could possibly add since the data science course materials are truly comprehensive. So I thought it would be useful to provide more Pandas references on accessing data, saving data and the pandas.cut data manipulation example that we may not have covered in the sections in detail.

The Pandas sections provided numerous examples on accessing data and saving data using the following:
```
pd.read_csv() -- to_csv 
pd.read_excel() -- to_excel 
pd.read_json() -- to_json
```
A reference to the full pandas documentation was provided but here are additional file formats that can be read and saved in pandas.


| Data Description          |              Read           |  Write/Save      |
| ------------------------ | ------------------- | ---------------  |
| Feather Format             | read_feather         | to_feather        |
| Msgpack                         | read_msgpack     | to_msg_pack  |
| Stata                                 | read_stata             | to_stata            |
| SAS                                   | read_sas                |                              |
| Python Pickle Format| read_pickle           |  to_pickle          |
| SQL                                   | read_sql                 | to_sql                 |



The course provided examples to read from SQLite database but for those who would like to connect to the Microsoft SQL database, the following code worked for me.

```
import pandas as pd 
import pyodbc

#Parameters
server = 'server' *#replace 'server' with your database server name *
db = 'database' *#enter the database name Uid = 'uid' password ='pwd'*

#Connection
conn = pyodbc.connect('DRIVER={SQL Server};SERVER=' + server + ';DATABASE=' + db + ';Uid=;PWD=;Trusted_Connection=yes')

#query db
sql = ''' SELECT TOP 5* FROM Medications;'''

df = pd.read_sql(sql, conn) df.head()
```

Finally, a pandas data manipulation function that could be useful for changing a continuous variable to a categorical variable is the pandas function called ***cut***.   It could be a useful binning tool for our data exploration tasks. Please see the example of how pandas.cut could convert ages to groups of age ranges.

```
import pandas as pd
A= [8,34,86,90,3,31,44,77,38,16,45,35,32,12,42,34,34,48,51]

Age_Col = pd.DataFrame({'Age':A})

ages = ['0-4','5-9','10-14','15-19','20-24','25-29','30-34','35-39','40-44','45-49','50-54','55-59','60-64','65-69','70-74','75-79','80-84','85-89','90-95']
Age_Col.head()

Age_Col['age_group'] = pd.cut(Age_Col.Age,range(0,100,5),right=False, labels=ages)

Age_Col[['Age','age_group']].head(10)
```

Output:  
|     | Age	| age_group|
| --| ----- | ------------|
| 0	|8	      | 5-9               |             
| 1	|34	    |	30-34          |
| 2	|86	    |	85-89          |
| 3	|90	    |	90-95          |
| 4	|3		    |	0-4               |
| 5	|31	    | 30-34          |
| 6	|44	    |	40-44          |
| 7	|77	    |	75-79          |
| 8	|38	    |	35-39          |
| 9	|16	    |	15-19          |
