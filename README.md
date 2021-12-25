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
```
5. Data analysis
```
# Findout average age of Female and Male students separately
age_mean = np.mean(stud_df['age'])
f_age_mean = np.mean(stud_df[stud_df['sex']=='F'].age)
m_age_mean = np.mean(stud_df[stud_df['sex']=='M'].age)
print("Mean age:",round(age_mean,2),"\nMean age - Female:",round(f_age_mean,2),"\nMean age - Male:",round(m_age_mean,2))

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

# Scatter plot example for highly corelated variables
plt.scatter(stud_df.G1, stud_df.G3)
```


 
