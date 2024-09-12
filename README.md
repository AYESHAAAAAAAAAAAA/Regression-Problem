# Import Libraries
import numpy as np
import pandas as pd
from datetime import datetime as dt

# Import Data Visualisation Libraries
import matplotlib.pyplot as plt
import seaborn as sns
import plotly as pl
import plotly.express as px
from plotly.subplots import make_subplots
import plotly.graph_objects as go
from pandas.plotting import scatter_matrix
%matplotlib inline

# Set the plot style and display options
plt.style.use('ggplot')
sns.set()

# To display all the columns in Dataframe
pd.set_option('display.max_columns', None)
# Import Library to visualise missing data
import missingno as mno

# Import and Ignore warnings for better code readability,
import warnings
warnings.filterwarnings('ignore')

# Load Dataset
from google.colab import drive
drive.mount('/content/drive')

# importing the data set
data_raw = pd.read_csv('/content/drive/MyDrive/dataset - KAG_energydata_complete.csv')

data = data_raw.copy()
# Dataset First Look
data.head()
# Dataset Rows & Columns count
num_rows, num_cols = data.shape

print("Number of rows:", num_rows)
print("Number of columns:", num_cols)
# Dataset Info
data.info()
# Assuming your date column is named "date_column"
data['date'] = pd.to_datetime(data['date'])
# Setting date as the index:
data.set_index('date', inplace=True)

# Dataset Duplicate Value Count assinged a dataframe name 'df'
df = data[data.duplicated()]
#There is no duplicate rows in the data
df.head()
# Missing Values/Null Values Count
data.isna().sum()
#  Dataset Columns
data.columns
# Dataset Describe
data.describe(include='all')
# Checking Unique Values count for each variable.
for i in data.columns.tolist():
  print("The unique values in",i, "is",data[i].nunique(),".")


# Round the unique values to two decimal places
rounded_unique_values = data.apply(lambda x: set(round(val, 2) for val in x))

# Print the unique values for each feature
for feature, unique in rounded_unique_values.items():
    print(f'{feature}: {unique}')
# Separating columns:
temperature_column = [i for i in data.columns if "T" in i]
humidity_column = [i for i in data.columns if "RH" in i]
other = [i for i in data.columns if ("T" not in i)&("RH" not in i)]


# close look on temprature column
data[temperature_column].describe(include='all')
# close look on humidity column
data[humidity_column].describe()
data[other].describe()

# Create a dictionary to map current column names to new column names
column_mapping = {'T1': 'KITCHEN_TEMP',
    'RH_1': 'KITCHEN_HUM',
    'T2': 'LIVING_TEMP',
    'RH_2' :'LIVING_HUM',
    'T3': 'BEDROOM_TEMP',
    'RH_3':'BEDROOM_HUM',
    'T4' : 'OFFICE_TEMP',
    'RH_4' : 'OFFICE_HUM',
    'T5' : 'BATHROOM_TEMP',
    'RH_5': 'BATHROOM_HUM',
    'T6':'OUTSIDE_TEMP_build',
    'RH_6': 'OUTSIDE_HUM_build',
    'T7': 'IRONING_ROOM_TEMP',
    'RH_7' : 'IRONING_ROOM_HUM',
    'T8' :'TEEN_ROOM_2_TEMP',
    'RH_8' : 'TEEN_ROOM_HUM',
    'T9': 'PARENTS_ROOM_TEMP',
    'RH_9': 'PARENTS_ROOM_HUM',
    'T_out' :'OUTSIDE_TEMP_wstn',
    'RH_out' :'OUTSIDE_HUM_wstn'}
# Rename the columns using the mapping
data.rename(columns=column_mapping, inplace=True)
