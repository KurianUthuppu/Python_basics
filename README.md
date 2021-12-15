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

### Reading/Writing files in a folder

 
