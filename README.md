# Python basics
_Basic useful codes towards day-to-day automation_

### Basic requirements
* Anaconda / Jupyter  
  - Installation link -> https://www.anaconda.com/products/individual
* Browser - Chrome / Firefox

### Resources
* https://www.kaggle.com/datasets

### Setting up the start-up folder for Jupyter 
(Ref: - https://stackoverflow.com/questions/35254852/how-to-change-the-jupyter-start-up-folder)
  1. Open cmd (or Anaconda Prompt) and run jupyter notebook --generate-config
  2. This writes a file to C:\Users\username\.jupyter\jupyter_notebook_config.py
  3. Browse to the file location and open it in an Editor (Ex: Visual Studio Code editor)
  4. Search for the following line in the file: #c.NotebookApp.notebook_dir = ''
  5. Replace by c.NotebookApp.notebook_dir = 'Path to the desired folder'
  
  Note: 
  * Make sure you use forward slashes in your path and use /home/user/ instead of ~/ for your home directory, backslashes could be used if placed in double quotes even if folder name contains spaces as such : "D:\yourUserName\Any Folder\More Folders\"
  * Remove the # at the beginning of the line to allow the line to execute
  
### Downloading the dataset from public repo
-> Download any of the public datasets from Kaggle
-> For this exercise, I have downloaded 'Student Alcohol Consumption' dataset
   Link: https://www.kaggle.com/uciml/student-alcohol-consumption

### Setup the environment
- Start Anaconda
- Click jupyter notebook
- Go to the requisite folder from the set default folder
- Creater new jupyter notebook and give the desired name

### Short-cut tricks in jupyter notebooks
- Press letter 'A' for inserting new block above
- Press letter 'B' for inserting new block below
- Press shift+Enter to run the code in the particular block and insert another block below
- Press alt+Enter to just run the code in the block selected
- Select the block (without prompt inside the block, if prompt visible, press escape so that block is selected), enter # as per the header size requirement, type in the header and then run the block using shift/alt + Enter
- Press d two times simultaneously to delete the block

### Steps towards data analysis
1. Import importanat libraries
```
import pandas as pd # Useful for data manipulation and analysis
import os # Use operating system dependant functionality
import glob # Helps find pathnames that matches specific patterns
import matplotlib.pyplot as plt # Comprehensive library for creating static, animated, and interactive visualizations in Python
import numpy as np # Library to perform mathematical and logical operations on arrays
import seaborn as sns # Helps build scatter plot with regression
from scipy import stats # Helps calculate the intercept, slope of the linear regression line
```
2. Read requisite files and storing to dataframe
```
# Setting the path to the requisite folder
path = os.getcwd()
csv_files = glob.glob(os.path.join(path+'.\\','student*.csv'))

# Declaring empty dataframe and appending data from all files matching the filename above
stud_df = pd.concat((pd.read_csv(file) for file in csv_files), ignore_index=True)
```
3. A bird's eye-view on the dataset
```
stud_df.head() # Give visibility on the top 5 rows along with headers
stud_df.describe() # Executes major statistical analysis
```
4. Data cleaning
```
stud_df.dtypes # lists out the data types in each column
stud_df.columns = stud_df.columns.str.strip() # Removing white-space on the headers
stud_df = stud_df.convert_dtypes() # To identify and convert each column to the right data type; check again using dtype forumula

stud_df.drop(['reason','nursery'],axis='columns',inplace=True) # Dropping columns that aren't required
# Inplace will modify the results in the dataframe itself; else you might have to assign the results to the dataframe

# Removing the zero values of requisite columns and storing in another dataset
temp = stud_df[(stud_df.G3!=0) & (stud_df.G1!=0)]

# Converting to float datatype towards requisite scatter-plot
temp.G1 = temp.G1.astype(float)
temp.G3 = temp.G3.astype(float)
```
5. Data analysis
```
# Findout average age of Female and Male students separately
# Findout average age of Female and Male students separately
age_mean = np.mean(stud_df['age'])
age_max = stud_df.age.max()
age_min = stud_df.age.min()
print("Mean age:",round(age_mean,2),"\nMax. Age: ",round(age_max,2),"\nMin age: ",round(age_min,2))
f_age_mean = np.mean(stud_df[stud_df['sex']=='F'].age)
m_age_mean = np.mean(stud_df[stud_df['sex']=='M'].age)
print("Mean age - Female:",round(f_age_mean,2),"\nMean age - Male:",round(m_age_mean,2))

# Findout average alcohol consumption score of Female and Male students separately with 75% weightage to weekday consumption and 25% to weekend consumption
alc_mean = np.mean(stud_df['Dalc']*0.75+stud_df['Walc']*0.25)
f_alc_mean = np.mean(stud_df[stud_df['sex']=='F'].Dalc*0.75+stud_df[stud_df['sex']=='F'].Walc*0.25)
m_alc_mean = np.mean(stud_df[stud_df['sex']=='M'].Dalc*0.75+stud_df[stud_df['sex']=='M'].Walc*0.25)
print("Mean alcohol score:",round(alc_mean,2),"\nMean alcohol score - Female:",round(f_alc_mean,2),"\nMean alcohol score - Male:",round(m_alc_mean,2))
```
6. Visualizaiton
- Some of the useful tools in this are histogram and heatmap
- Histogram help to understand the distribution of data while heatmap reveals the correlation between all variables in the dataset
- Histogram analysis showed the tapering of age in the given dataset beyond 18 years of age and becoming negligible post 20
- Heatmap revealed that there is good coorelation between the education of Father & Mother (0.62), workday vs weekend alcoholism (0.65) and between 1st, 2nd and final grades
```
# Plotting simple histogram of the age
plt.hist(stud_df.age)
plt.show()

# Plotting heatmap between all variable to understand the correlation
plt.figure(figsize=(20,10))
sns.heatmap(stud_df.corr(), annot=True)

# Scatter plot example for highly corelated variables using seaborn - regplot and lmplot
# Regplot - Performs a simple linear regression model fit and plot

# Get coeffs of linear fit
slope, intercept, r_value, p_value, std_err = stats.linregress(temp['G1'],temp['G3'])
plt.figure(figsize=(15,10))

# Use line_kws to set line label for legend
ax = sns.regplot(x="G1", y="G3", data=temp, color='b',
 line_kws={'label':"y={0:.2f}x+{1:.2f}".format(slope,intercept)})

# Plot legend
ax.legend()

plt.show()

# Lmplot - Combines Regplot with facetGrid class helps in visualizing the distribution of one variable as well as the relationship between multiple variables
# Executing the scatter plot along with linear regression
sns.set_style('whitegrid')
reg = sns.lmplot(x='G1', y='G3', data=temp, hue = 'sex', markers =['o', 'v'],height=7, aspect=1.5)
```
7.Data Manipulation
```
# Find rows with null values in a particular column
null_set = stud_df[stud_df['address'].isnull()] # Can use isna() as well

# Find rows with no null values in a particular column
full_set = stud_df[stud_df['address'].notna()]

# Extract requisite columns using iloc
demo_set = stud_df.iloc[:,np.r_[0:3, 6:9, 13:30]]

# Splitting the string
split_string = stud_df['Mjob'].str.split('e').str[0]

# Column Insertion
stud_df.insert(loc=4,column='Split-String', value=split_string)

stud_df.drop('Split-String', axis='columns', inplace=True) # Dropping the extra column added for demo
# Rearranging columns
column_names = ['Mjob','Fjob','school','Dalc','absences','sex','famsize']
demo_df = stud_df.reindex(column_names, axis='columns')

# Renaming a column
demo_df.rename(columns={'Mjob':'Mothers job'}, inplace=True)

# Merging 2 columns and inserting to a dataset
jobs_merged_column = demo_df['Mothers job']+' & '+ demo_df['Fjob']
demo_df.insert(loc=2, column='Job_merged',value=jobs_merged_column)

# Merging two datasets
new_df = pd.merge(stud_df,demo_df,on='Fjob',how='left')
new_df

# Converting string to requisite format
stud_df['famsize'] = stud_df['famsize'].astype(str).str.zfill(4)

# Using lambda to identify substrings
stud_df['famsize'] = stud_df['famsize'].astype(str).map(lambda x: x[1:])

# Sorting the rows and inserting serial no.
stud_df.sort_values(by='age', inplace=True)
stud_df.insert(loc=0,column='Sl.No.',value=np.arange(1,len(stud_df)+1))
stud_df.reset_index(inplace=True, drop=True)
stud_df

# Writing to a CSV or Excel file in the current folder
path = os.path.join(os.getcwd(),'stud_Alc_worked.csv')
stud_df.to_csv(path, index=False)
path = os.path.join(os.getcwd(),'stud_Alc_worked.xlsx')
stud_df.to_excel(path, index=False)
```

 
