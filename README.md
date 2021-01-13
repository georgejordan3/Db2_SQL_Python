Analyzing Data with IBM Db2, SQL, and Python
George Jordan
Last update: January 13, 2021

I am currently working through the 'IBM Data Science Professional Certificate' through Coursera. The certificate is a nine-course suite, with the fifth course being 'Databases and SQL for Data Science'. I have recently completed this course and will use the work here to demonstrate my skills in these tools.

First, I will provide a brief overview of the queries that I wrote to fulfill the requirements of the assignment (which scored 100%). Then, I will visually display the data by comparing variables via scatterplot. I will use regression analysis to describe the relationships between variables. In doing so, I hope to provide interpretation that might otherwise not be obvious by simply viewing the data within the databases.

I believe that data does not speak for itself and requires a storyteller to communicate findings so that stakeholders can make informed decisions with the data. I firmly believe that this work is a waste of time if teams cannot use the work either for lack of interpretation or techincal knowledge. I hope that I can be that bridge to connect the data to those otherwise unable to use it.

In this project, I used three separate data sets to gain insight into Chicago's neighborhoods and schools. The data is made available through the city of Chicago's public data portal. For the scope of the project, the crime data has been truncated to around five hundred rows, as opposed to the original which contained around 6.5 million rows.

Socioeconomic Indicators in Chicago
Chicago Public Schools
Chicago Crime Data
Preparation
In [1]:
%load_ext sql
I connected to Db2 by instantiating an API token which allowed me to run all of my queries through a Jupyter notebook. For the sake of streamlining the project, doing this project through the Jupyter notebooks on Watson Labs allows for all of the IBM technologies to come packaged without having to reinstall each time I run the notebook.

In [2]:
%sql ibm_db_sa://tjk69312:mkjv%402qs5xgnc01k@dashdb-txn-sbox-yp-dal09-12.services.dal.bluemix.net:50000/BLUDB
Out[2]:
'Connected: tjk69312@BLUDB'
In [3]:
# Verifying the connection was established with a simple query
%sql select count(*) from CHICAGO_CRIME_DATA
 * ibm_db_sa://tjk69312:***@dashdb-txn-sbox-yp-dal09-12.services.dal.bluemix.net:50000/BLUDB
Done.
Out[3]:
1
533
Sample Queries
In the next few cells, I will demonstrate some of the queries that I wrote in order to fulfill the requests for the assignment.

List the top 5 Community Areas by average College Enrollment [number of students]
In [4]:
%sql select COMMUNITY_AREA_NAME, avg(COLLEGE_ENROLLMENT) \
as AVERAGE_COLLEGE_ENROLLMENT \
from CHICAGO_PUBLIC_SCHOOLS \
group by COMMUNITY_AREA_NAME \
order by avg(COLLEGE_ENROLLMENT) desc limit 5
 * ibm_db_sa://tjk69312:***@dashdb-txn-sbox-yp-dal09-12.services.dal.bluemix.net:50000/BLUDB
Done.
Out[4]:
community_area_name	average_college_enrollment
ARCHER HEIGHTS	2411.500000
MONTCLARE	1317.000000
WEST ELSDON	1233.333333
BRIGHTON PARK	1205.875000
BELMONT CRAGIN	1198.833333
Use a sub-query to determine which Community Area has the least value for school Safety Score?
In [5]:
%sql select COMMUNITY_AREA_NUMBER, COMMUNITY_AREA_NAME, SAFETY_SCORE \
from CHICAGO_PUBLIC_SCHOOLS \
where SAFETY_SCORE = (select min(SAFETY_SCORE) from CHICAGO_PUBLIC_SCHOOLS)
 * ibm_db_sa://tjk69312:***@dashdb-txn-sbox-yp-dal09-12.services.dal.bluemix.net:50000/BLUDB
Done.
Out[5]:
community_area_number	community_area_name	safety_score
40	WASHINGTON PARK	1
Which schools in Community Areas 10 to 15 are healthy school certified?
In [6]:
%sql select NAME_OF_SCHOOL from CHICAGO_PUBLIC_SCHOOLS \
where COMMUNITY_AREA_NUMBER between 10 and 15 \
and HEALTHY_SCHOOL_CERTIFIED = 'Yes'
 * ibm_db_sa://tjk69312:***@dashdb-txn-sbox-yp-dal09-12.services.dal.bluemix.net:50000/BLUDB
Done.
Out[6]:
name_of_school
Rufus M Hitch Elementary School
Visualizing the Data
In [7]:
# Run matplotlib to graph some of the schools on scatter plot
# Centered around Community_area, 77 different neighborhoods
# From CHICAGO_PUBLIC_SCHOOLS: Safety_Score, Rate_of_Misconduct, College_Enrollment_rate
# From CENSUS_DATA: Percent_of_housing_crowded, Perent_of_households_below_poverty, per_capita_income, hardship_index
# From CHICAGO_CRIME_DATA: Community
I will use the Pandas library to organize the data within the notebook environment. For the visualization of the data, I will be using the Python libraries Matplotlib and Numpy.

In [23]:
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
Then I created an localized instance of the data within the notebook for each data set.

In [16]:
CENSUS_DATA_local = pd.read_csv('https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera/data/Census_Data_-_Selected_socioeconomic_indicators_in_Chicago__2008___2012-v2.csv')
In [17]:
CHICAGO_PUBLIC_SCHOOLS_local = pd.read_csv('https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera/data/Chicago_Public_Schools_-_Progress_Report_Cards__2011-2012-v3.csv')
In [21]:
CHICAGO_CRIME_DATA_local = pd.read_csv('https://cf-courses-data.s3.us.cloud-object-storage.appdomain.cloud/IBMDeveloperSkillsNetwork-DB0201EN-SkillsNetwork/labs/FinalModule_Coursera/data/Chicago_Crime_Data-v2.csv')
HEAVILY UNDER CONSTRUCTION FROM THIS POINT DOWN
In [28]:
df = pd.DataFrame(CENSUS_DATA_local, 
                  columns=["COMMUNITY_AREA_NUMBER", "PER_CAPITA_INCOME", "HARDSHIP_INDEX"])
df.plot.scatter(x="COMMUNITY_AREA_NUMBER", y="HARDSHIP_INDEX")
Out[28]:
<AxesSubplot:xlabel='COMMUNITY_AREA_NUMBER', ylabel='HARDSHIP_INDEX'>

In [ ]:

In [ ]:

In [ ]:

In [15]:
# DELETE

plt.plot(np.arange(10))
Out[15]:
[<matplotlib.lines.Line2D at 0x7fe7b2af2278>]

Regression Analysis
In [ ]:
# Use the scatter plots from above and scikit learn to create regression lines to predict the impact of variables on others
In [ ]:
