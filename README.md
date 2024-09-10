# # What factors have the most significant impact on employee job satisfaction? 
# ##### Personal project #####

# #### Company wants to improve employee job satisfaction. The company has collected data on various factors such as age, gender, marital status, job level, experience, department, employee type, work-life balance (WLB), work environment (WorkEnv), physical activity hours, workload, stress levels, sleep hours, commute mode, commute distance, number of companies worked at, team size, number of reports, education level, overtime (haveOT), training hours per year, and job satisfaction. ####

# #### In this project to answer the company question, I will be below steps:
# 
# 1. Data Cleaning: Ensure no missing or incorrect values are in the used dataset.
# 2. Exploratory Data Analysis (EDA):
#    * Visualize the distribution of job satisfaction scores,
#    * Analyze the relationships between job satisfaction and other variables using scatter plots, bar charts, and correlation metrics.
# 3. Feature selection: Identify which factors are most correlated with job satisfaction.
# 4. Interpret Results. Determinate which factors are the most significant predictors of job satisfaction and provide recommendations to the company.
#    I will check if there is a strong correlation between job satisfaction and factors like workload, stress, and work-life balance. 

# In[123]:


# import libraries
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns


# In[124]:


# load data into Pandas DataFrame 
df = pd.read_csv('/Users/agnieszkagresham/Desktop/data_for_poftfolio/employee_survey.csv')


# In[125]:


# Taking look for first few rows of data
df.head()


# In[126]:


df.describe()


# In[127]:


# Checking Number of rows
num_rows = len(df.index)
print(f"Number of rows: {num_rows}")

# Checking Number of columns
num_columns = len(df.columns)
print(f"Number of columns: {num_columns}")


# In[128]:


# Checking data shape
df.info()


# ### Data Cleaning
# 

# In[129]:


# Looking for missing Values

df.isnull().sum()


# In[130]:


# Filter only rows with missing values 
rows_with_missing = df[df.isna().any(axis=1)]
rows_with_missing 


# In[131]:


# Looking at rows_with_missing I want to keep only these rows that have metrics needed
#for analysis. In this case, will drop rows of EmpID 147 and 367.  
# Will create a new dataframe without no needed rows.
df = df[~df['EmpID'].isin([147,367])]


# In[132]:


# Filling rest of missing values with unknown 
df.fillna('Unknown',inplace= True)


# In[133]:


# Checking if all missing values have been filled as unknown,
df[df['EmpID'].isin([7,147,151,367])]


# In[134]:


# Filter duplicates
duplicates=df[df.duplicated()]
duplicates


# In[135]:


# Removing duplicates
df.drop_duplicates(inplace = True)


# In[ ]:






# In[136]:


df.isnull().sum()


# In[140]:


# converting data types for Metrics
df['Age'] = df['Age'].astype(int)
df['Experience'] = df['Experience'].astype(int)



# In[141]:


df.info()


# ### Data Manipulation

# In[144]:


# Creating new column 'Age groups'
df['Age_Group']= pd.cut(df['Age'],bins=[20,30,40,50,60],labels=['20-30','30-40','40-50','50-60'])


# In[145]:


df


# In[154]:


# Grouping Data and looking at the average
departament_group = df.groupby('Dept').mean(numeric_only = True).round(2)
departament_group


# In[158]:


df.describe().round(1)


# In[166]:


# Looking  at the distribution of Age
#The majority of individuals are in the youngest age bracket, indicating a younger workforce.
# There is a general decrease in the number of individuals as age increases. While the younger age groups dominate,
# there are still significant counts in older age brackets, suggesting some age diversity.
sns.histplot(df['Age'],kde=True)
plt.title('Age Distribution')
plt.show()


# In[163]:


# Looking at Job Satisfaction by Department Highest Job Satisfaction: The IT and HR departments have the highest median job satisfaction scores.
# Lowest Job Satisfaction: The Customer Service department has the lowest median job satisfaction score.
sns.boxplot(x='Dept', y='JobSatisfaction', data=df)
plt.title('Job Satisfaction by Department')
plt.show()


# In[165]:


# Looking at correlation
plt.figure(figsize=(12, 8))
sns.heatmap(df.corr(numeric_only = True), annot=True, cmap='coolwarm')
plt.title('Correlation Heatmap')
plt.show()


# ## Insights and Recomendations

# ### Experience and Age:
# There is a strong positive correlation between ‘Experience’ and ‘Age’. This suggests that as employees get older, their experience in the field increases, which is quite intuitive. Leverage the experience of older employees by involving them in mentoring programs to share their knowledge with younger staff.
# ### Work-Life Balance (WLB) and Stress:
# There is a negative correlation between ‘Work-Life Balance’ and ‘Stress’. This indicates that better work-life balance is associated with lower stress levels among employees. Promote flexible working hours and remote work options to improve work-life balance and reduce stress.
# ### Physical Activity Hours and Stress:
# There is a negative correlation between ‘Physical Activity Hours’ and ‘Stress’. Employees who engage in more physical activity tend to have lower stress levels. Encourage physical activity by offering gym memberships, organizing fitness challenges, or providing on-site exercise facilities.
# ### Commute Distance and Job Satisfaction:
# There is a negative correlation between ‘Commute Distance’ and ‘Job Satisfaction’. Longer commute distances are associated with lower job satisfaction. Consider offering remote work options or flexible hours to reduce the impact of long commutes on job satisfaction.
# ### Training Hours Per Year and Job Satisfaction:
# There is a positive correlation between ‘Training Hours Per Year’ and ‘Job Satisfaction’. Employees who receive more training tend to be more satisfied with their jobs.
# These insights can help in understanding the relationships between different aspects of employee data and can be useful for making informed decisions in HR and management. Invest in regular training and development programs to enhance job satisfaction and employee engagement.
