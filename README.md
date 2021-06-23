# Probability
#import pandas, numby, random and matplotlib
import pandas as pd
import numpy as np
import random
import matplotlib.pyplot as plt
%matplotlib inline
#We are setting the seed to assure you get the same answers on quizzes as we set up
random.seed(42)
#read CSV file
df = pd.read_csv('ab_data.csv')
df.head()
# the number of rows & columns, 294478 rows and 5 cloumns
df.shape
#the number of unique users
df['user_id'].nunique()
# uses mean() to find the proportion of users converted
df.converted.mean()
# use query to find out the number of times the new_page and treatment don't line up
treatment_old = df.query('group == "treatment" and landing_page == "old_page"').nunique()
control_new = df.query('group == "control" and landing_page == "new_page"').nunique()
treatment_old, control_new, treatment_old + control_new
df[((df['group'] == 'treatment') == (df['landing_page'] == 'new_page'))== False].shape[0]
# check for any missing data in this dataset - no missing data since we had the same number of value for each column
df.info()
# creat new dataset stored in new dataframe df2
df2 = df[((df['group'] == 'treatment') == (df['landing_page'] == 'new_page'))]
df2.shape[0]
# Check for the correct rows is removed 
df2[((df2['group'] == 'treatment') == (df2['landing_page'] == 'new_page')) == False].shape[0]
# the number of unique users in df2
df2['user_id'].nunique()
# check for any duplicated rows - there's 1 duplicated row
sum(df2['user_id'].duplicated())
#find the information of the duplicated row
ids = df2["user_id"]
df2[ids.isin(ids[ids.duplicated()])].sort_values("user_id")
#remove the duplicated row
df2.drop([2893], inplace=True)
df2.shape[0]
#check again to ensure the duplicated row was removed 
sum(df2['user_id'].duplicated())
#uses converted column to check for the probability of all users converted
df2['converted'].mean()
#uses group column to check of the probability of converted users in control group
df2.query('group == "control"')['converted'].mean()
#uses group column to check of the probability of converted users in treatment group
df2.query('group == "treatment"')['converted'].mean()
#uses landing_page column to check of the probability that an individual received the new page
df2.query("landing_page == 'new_page'").count()[0]/df2.shape[0]
