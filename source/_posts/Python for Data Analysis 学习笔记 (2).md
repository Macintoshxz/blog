title: Python for Data Analysis 学习笔记 (2)
date: 2015-07-06 14:53:46
tags: Python

---
# Getting Started with pandas

代码详见: [Python Notebook](http://nbviewer.ipython.org/github/bioxfu/ipynb/blob/master/getting%20started%20with%20pandas.ipynb)

## 1. pandas data structures
### 1.1 Series
- A Series is a one-dimensional array-like object containing an **array of data** and an associated **array of data labels**, called its **index**.
- Compared with a regular NumPy array, you can use values in the index when selecting **single** value or **a set of values**.
- Another way to think about a Series is as a fixed-length, **ordered** dict. You can create a Series from a Python dict.
- The **isnull** and **notnull** functions in pandas should be used to detect missing data.
- A critical Series feature for many applications is that it automatically aligns differently-indexed data in arithmetic operations.
- Both the Series object itself and its index have a **name** attribute, which integrates with other key areas of pandas functionality.
---
- 学习pandas的Series，可以和Python的dict和R的vector做对比。
- Python的dict有key和value，pandas的Series有index和value，R的vector有names和value。
- Series是加强版的dict，在取值的时候，Series可以一次取多个，dict一次只能取一个。R的vector也可以一次取多个值。
- 更强大的是，在做两个Series的计算的时候，它可以自动align index！不能align的index，值为NaN。R的vector不具备这个功能。


### 1.2 DataFrame
- The DataFrame has both a row and column index; it can be thought of as a dict of Series (one for all sharing the same index).
- A column in a DataFrame can be retrieved as a **Series** either by dict-like notation or by attribute.
- Assigning a column that doesn't exist will create a new column. The `del` keyword will delete columns as with dict.
- The column returned when indexing a DataFrame is a *view* on the underlying data, not a copy. Thus, any in-place modifications to the Series will be reflected in the DataFrame. The column can be explicitly copied using the Series's `copy` method
- Possible data inputs to DataFrame constructor:

Type | Notes
--- | ---
2D ndarray | A matrix of data, passing optional row and column labels
dict of arrays, lists, or tuples | Each sequence becomes a column in the DataFrame. All sequences must be the **same length**.
NumPy structured/record array | Treated as the 'dict of arrays' case
dict of Series | Each value becomes a column. Indexes from each Series are unioned together to from the result's row index if no explicit index is passed.
list of dicts or Series | Each item becomes a row in the DataFrame. Union of dict keys or Series indexes become the DataFrame's column labels.
list of lists or tuples | Treated as the '2D ndarray' case
another DataFrame | The DataFrame's indexs are used unless different ones are passed
NumPy MaskedArray | Like the '2D ndarray' case except masked values become NA/missing in the DataFrame result
---
最常见的构建DataFrame的方式是 (1)from a dict of equal-length lists or NumPy arrays 和 (2)from a nested dict of dicts/Series

### 1.3 Index Objects

## 2. Essential Functionality
### 2.1 Reindexing

### 2.2 Dropping entries from an axis

### 2.3 Indexing, selection, and filtering

### 2.4 Arithmetic and data alignment

### 2.5 Function application and mapping

### 2.6 Sorting and ranking

### 2.7 Axis indexes with duplicate values

## 3. Summarizing and Computing Descriptive Statistics


## 4. Handling Missing Data

## 5. Hierarchical Indexing

