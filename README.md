# Data-Analysis-and-visualization
The given Project is about the analysis of Adidas sales across the United States.

# Code
## Import necessary libraries
```python
import pandas as pd
import os
import matplotlib.pyplot as plt
```

## Read all the data files(each month file)
```python
files=os.listdir(r'path+/Sales_Data')
# Initialize merged DataFrame with the first DataFrame
All_month_file= pd.DataFrame() # Initialize merged DataFrame with the first DataFrame
for file in files: # Iterate through the rest of the DataFrames
    df=pd.read_csv(r"path+/Sales_Data/" +file)
    All_month_file = pd.concat([All_month_file,df])
    
All_month_file
#Saving result as csv file
All_month_file.to_csv("all_data.csv", index=False) #Saving result as csv file
```
# Read merged data file
```pyhton
all_data = pd.read_csv(r"path\all_data.csv")
all_data.head()
```
# DATA CLEANING
#### Finding and Droping null raws
```python
all_data.isnull().sum() # returns total number of null values in each column

null_data = all_data[all_data["Order ID"].isnull()].index #return index of all null values in Order id column(or you can select any column name since whole rows are null)

all_data = all_data.drop(index=null_data) #droping those rows from the data from the table and saving it as new file

```

#### Removes all the data that has invalid entry
```python
temp_df = all_data[all_data["Order Date"].str[0:2] == 'Or'].index
temp_df
all_data = all_data.drop(index=temp_df)

```

#### Covert Quantity Ordered and Price Each column in float datatype
```pyhon 
all_data.dtypes
all_data['Quantity Ordered'] = pd.to_numeric(all_data['Quantity Ordered'], errors='coerce')
all_data['Price Each'] = pd.to_numeric(all_data['Price Each'], errors='coerce')
```

### Q1: Calculate the month with highest sales values

#### Adding month column from order date

```python
all_data["Order Date"] = pd.to_datetime(all_data["Order Date"])
all_data.dtypes

all_data['Month']=all_data["Order Date"].dt.month_name()

```
#### Calculating Total Sales


```python
all_data['Total sales']= all_data['Quantity Ordered']*all_data['Price Each']
results= all_data.groupby('Month').sum('Total sales')

# Sort DataFrame by the "Month" column
all_data = all_data.sort_values(by='Order Date')

results
```
#### Ploting the results data in the Bar Chart
```python
months = range(1,13)
plt.bar(months, results['Total sales'])
plt.xticks(range(1,13))
plt.ylabel('Sales in US')
plt.xlabel('Month')

```
#### Result Bar Chart
![Screenshot (71)](https://github.com/siddjoshi19/Data-Analysis-and-visualization/assets/89629408/8f1aec4b-e8d1-4740-838d-03d6273678c4)












