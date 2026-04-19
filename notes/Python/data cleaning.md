## **NumPy**

it is a python library used to deal, and manipulate arrays

import numpy as np

###### **creating an array**

a = np.array(\[]) - single array

am = np.array(\[\[],\[],\[]], dtype = int32) - multi dimensional array

dtype - it is the data type of the array, yes python is dynamic but working with single datatype is the best to avoid complication

###### **working with arrays**

**default values**

these are arrays that contain one numbers

ad = np.full((arrays shape), fill value)

ad = np.full((2,3,4), 3)

//this is 2 arrays of 3x3 and filled with 3s

there are common values like 1s and 0s you can just give the shape of the array and use the following

np.zeros((2,3,4))

np.ones((4,5,6))

np.empty -- this creates an empty array, not 0s, it just allocate spaces but does not initialise the array.

np.arrange -- it allows to arrange values over a given range, for example you want to plot x values

np.arrange(first, last, step)

x_values = np.arrange(1,100,3)

np.linspace --

**special values**

nan - not a number

inf - infinite

used to fill some missing values in dataset or check for missing values in a dataset

###### **performing math operations with NumPy**

first of all its different from python lists, with python lists multiplication means duplicate but here it just does the math

its just like matrix math, ofc the array(matrices should be compatible)

you can do matric multiplication, add, sub, div and scalar multiplication

which is not available with normal python arrays

np have various **_math functions_** like sqrt, sin, cos, log, exp etc

_// these work per individual element of array_

an = np.array(\[4,16,25,36])

np.sqrt(ap)

will give \[2,4,5,6]

###### **Array functions**

af = np.array(\[1,2,4,6])

af = np.append(af, \[2,4,7]) -- add values at the end of array

af = np.insert(af, 3, \[5,8,0]) -- add values at the given position in this case 3

np.delete(af, obj, axis)

###### **structural methods**

reshape used to reshape the array

af = np.array(\[1,2,4,6],\[5,3,5,3], \[0.0.0.0])

af.reshape(4,3) - would change the array shape from 3x4 to a 4x3 ofc they should be compatible

af.flatten - returns a copy of the flattened array

af.ravel - flattens the array

af.transpose - this means changing rows to cols and cols to rows #print(af.T)

af.swapexis(2,4) - you swap the given axis not whole array

###### **merging arrays**

concatenate

stack

vstack

hstack

###### **Spliting arrays**

np.split(af, how many, axis(0or1))

###### **aggregate functions**

these allow us to do some operations per array

sum, max, mean, std(standard deviation),

###### **saving and importing files**

**saving**

as numpy array

np.save("myarray.npy", af)

as csv

np.savetxt("myarray.csv", af, delimeter=",")

**loading**

**from numpy array**

a = np.load("myarray.npy")

print(a)

from csv

a= np.loadtxt("myarray.csv", delimeter=",")

# **Pandas**

**Pandas is a Python library used for data manipulation and analysis.**

**Its main strength is working with tabular data (rows and columns), similar to Excel or a SQL table.**

**import pandas as pd**

**Core Data Structures in Pandas**

**1. Series**

**A Series is a one-dimensional array (like a list or a single column in a table).**

**Think of it as one column of data**

**Each value has an index**

**ages = pd.Series(\[20, 10, 50])**

**print(ages)**

**Example with labels:**

**ages = pd.Series(\[20, 10, 50], index=\["Mike", "Bob", "Mark"])**

**2. DataFrame**

**A DataFrame is a two-dimensional table made up of multiple Series.**

**Rows + Columns**

**Similar to an Excel spreadsheet or database table**

**df = pd.DataFrame({**

&nbsp; \*\*'name': \\\["Mike", "Bob", "Mark"],\*\*

    \*\*'age': \\\[20, 10, 50]\*\*

**})**

**Here:**

**Each column (name, age) is a Series**

**All columns together form a DataFrame**

**Saving Data (Exporting)**

**Pandas allows you to save DataFrames in many formats.**

**Save as CSV**

**df.to_csv("mydata.csv", index=False)**

**index=False prevents saving row numbers as a column**

**Other formats include:**

**df.to_excel("data.xlsx")**

**df.to_json("data.json")**

**df.to_html("data.html")**

**Most saving methods start with df.to\_...**

**Reading Data (Importing)**

**Read a CSV file**

**df = pd.read_csv("mydata.csv")**

**Other examples:**

**pd.read_excel("data.xlsx")**

**pd.read_json("data.json")**

**Working With Real-World Datasets**

**Pandas works well with datasets from machine learning libraries.**

**Example using Scikit-learn:**

**from sklearn.datasets import fetch_california_housing**

**df = fetch_california_housing(as_frame=True).frame**

**as_frame=True converts the dataset into a Pandas DataFrame**

**.frame extracts the actual DataFrame**

**Now you can use normal Pandas operations on it.**

**Common DataFrame Operations**

**Previewing Data**

**df.head(5) # first 5 rows**

**df.tail(5) # last 5 rows**

**Understanding the Data**

**df.info() # structure, column types, missing values**

**df.describe() # statistical summary (mean, min, max, std, etc.)**

**Visualization**

**df.plot() # quick plots**

**df.hist() # histograms for numerical columns**

**Useful for spotting trends and outliers.**

**Accessing Columns**

**Select a single column**

**df\['age']**

**You can perform operations on it:**

**df\['age'].mean()**

**df\['age'].max()**

**df\['age'].sum()**

**Selecting and Filtering Data**

**.loc\[] – label-based selection**

**df.loc\[0] # row with index 0**

**df.loc\[0, 'age'] # specific value**

**.iloc\[] – integer-based selection**

**df.iloc\[0] # first row**

**df.iloc\[0, 1] # first row, second column**

**.at\[] – fast access to a single value**

**df.at\[0, 'age'] = 25 # modify a specific cell**

**Dropping Columns or Rows**

**Drop a column**

**df = df.drop('age', axis=1)**

**axis=1 → column**

**axis=0 → row**

**Example:**

**df = df.drop(0, axis=0) # drop first row**

##### **data cleaning**

In real datasets, data is usually:

Missing values

Wrong data types

Duplicate rows

Outliers

Inconsistent text (e.g. "Male", "male", " M ")

**1. Load the Data**

import pandas as pd

df = pd.read_csv("data.csv")

First rule: never clean blind.

**2. Initial Inspection**

df.head()

df.tail()

df.info()

df.describe()

**check:**

Column names

Data types (int, float, object)

Missing values

Weird numbers (e.g. negative ages)

**3. Rename Columns**

Messy column names = messy code.

df.columns = df.columns.str.lower().str.replace(" ", "\_")

Example:

"Monthly Salary" → "monthly_salary"

**4. Handle Missing Values**

Check missing values

df.isna().sum()

Strategy depends on the column

A. Drop rows (when few are missing)

df = df.dropna()

B. Fill with a fixed value

df\['children'] = df\['children'].fillna(0)

C. Fill with mean / median (numerical data)

df\['age'] = df\['age'].fillna(df\['age'].mean())

Median is better when data has outliers:

df\['salary'] = df\['salary'].fillna(df\['salary'].median())

C. Fill categorical data

df\['gender'] = df\['gender'].fillna('unknown')

**5. Fix Data Types**

Very common problem.

Convert strings to numbers

df\['salary'] = pd.to_numeric(df\['salary'], errors='coerce')

errors='coerce' converts invalid values to NaN

Then you handle the NaNs again

Convert dates

df\['created_at'] = pd.to_datetime(df\['created_at'])

**6. Remove Duplicates**

df.duplicated().sum()

df = df.drop_duplicates()

Based on a column:

df = df.drop_duplicates(subset='email')

**7. Clean Text Data**

Standardize text

df\['gender'] = df\['gender'].str.lower().str.strip()

Fix inconsistent values:

df\['gender'] = df\['gender'].replace({

  'm': 'male',

  'f': 'female'

})

**8. Filter Invalid Data**

Example: age should be realistic.

df = df\[(df\['age'] > 0) \& (df\['age'] < 120)]

Remove negative salaries:

df = df\[df\['salary'] >= 0]

9\. Handle Outliers (Basic Method)

Using IQR (Interquartile Range)

Q1 = df\['salary'].quantile(0.25)

Q3 = df\['salary'].quantile(0.75)

IQR = Q3 - Q1

df = df\[

  (df\['salary'] >= Q1 - 1.5 \* IQR) \&

  (df\['salary'] <= Q3 + 1.5 \* IQR)

]

This removes extreme values that distort analysis.

10. Final Validation

df.info()

df.describe()

df.isna().sum()

Make sure:

No unexpected NaNs

Correct data types

Data looks realistic

11. Save Clean Data

# SCIKIT LEARN

##### Loading data

from sklearn.datasets import load_breast_cancer

from skleanr.datasets import fetch_california_housing

from sklearn.datasets import make_blobs

\# load - use from the local sk datasets

\# fetch - download from the internet

\# make -

to use the dataset just call it like a function

load_breast_cancer()

but this usually gives a dict with a lot of information

you can split it

x,y = load_breast_cancer(return_x_y=True)

but the easy and clean way is it use it as a pandas frame

df = load_breast_cancer(as_frame=True).frame

now you can perfom all the pandas df actions on the dataset

you can also load data the pandas way, reading a csv from Kaggle

##### splitting data

we split the fucking data, because you cant use same data to train then test on it, the model wont be good because it will just memorise

in sk we use

from sklearn.model_selection import train_test_split

this does all the work to slpit the data,

x, y = load_breast_cancer(return_X_y=True)

x_train, x_test, y_train, y_test = train_test_split(x, y, test_size=0.2)

test_size=0.2: this means you use 0.8 to train and 0.2 to test the data

this data is split randomly, **so sometimes** it can become uneven having more data of certain class and much less of the other class, which can affect results of test

sklearn has stratifiedShuffleSplit - this allows the data to be split evenly among the classes

example:

there are a lot of split methods!!

##### pre processing

sometimes you have data but its not perfect for the kind of training you want to do, so pre processing helps you make it suit whatever agenda you have

and we use the **sklearn.preprocessing** lib/class for that and its got a lot of pre processors

like

_standardScaler for scaling_

from sklearn.preprocessing import StandardScaler

scaler = StandardScaler()

x_train_scaled = scaler.fit_transform(x_train)

x_test_scaled = scaler.transform(x_test)

the standard scaler makes the data scaled into the same scale/range so the the algorithms which care much about distance penalties would not put more importance on other features more than others

_from sklearn.preprocessing import OrdinalEncoder - for encoding_

encoding is a way of changing data to a more suitable format, most algorithms work with numbers and some data might contain stuff like small medium high, so you have to encode it to 123, but this only work if the data is relatable you can encode countries or names?

for those you can use a _onehotencoder_

it splits the data to encode into various cols and give specific numbers

##### algorithms/classifiers

import the model

create an instance

fit

score

predict

classifiers - predict yes/no, they predict classes. is it cat or dog

regression - it is a continuous spectrum,you try to find the value of something on the scale like what is the price of the house.

clustering - for unsupervised learning, it create clusters and group the data

##### metrics

are used to evaluate the classifiers

from sklearn.metrics import accuracy_score, precision_score, recall_score, fl_score
