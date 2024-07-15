###### New Dataset
Always start by investigating the data structure
```python
ted = pd.read_csv("/kaggle/input/ted-talks/ted_main.csv")

# 1 - Look at chunk of data
ted.head()

# 2 - Look at dimensions
ted.shape

# 3 - Look at types
ted.dtypes

# 4 - Look at info
ted.info()

# 5 - Look at metrics
ted.describe()
```

Then, manage missing values
```python
# 1 - Count number of missing values
ted.isna().sum()

# 2 - Fill missing values with mean for numerics, mode for objects
ted['speaker_occupation'] = ted.speaker_occupation.fillna(ted.speaker_occupation.mode()[0])
```

Then, look at data with histograms and correlations
```python
ted.hvplot.hist(subplots=True, height=250, width=250, shared_axes=False, value_label='Rate').cols(3)

plt.figure(figsize=(12, 10))
sns.heatmap(ted.corr(), annot=True)

ted.hvplot.hist(y='languages', height=350, width=450)
```
###### Analyzing
Always start with questions you want to answer.
1. Get relevant data
2. Perform any new computation to get new values
###### Reading
Reading a csv: 
```python
import pandas as pd

df = pd.read_csv('koinly_2021.csv', header=1) # Specify which row is header

head = df.head() # Get the first few pieces of data
headers = df.columns # Get the column names

column_data = df["MyHeader"] # Just like a dictionary

df = df.drop('col_name', axis=1) # Drop a column - 0 for rows, 1 for cols. Typically, we set axis=1 to drop columns.
```

Creating a dataframe
```python
import pandas as pd

data = [["tom", 10], ["nick", 15]]
df = pd.DataFrame(data, columns=["Name", "age"])
```