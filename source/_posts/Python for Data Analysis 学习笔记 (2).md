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


## 2. Essential Functionality
### 2.1 Reindexing
- A critical method on pandas objects is `reindex`, which means to create a new object with the data *conformed* to a new index, introducing missing values if any index values were not already present.
- With DataFrame, reindex can alter either the (row) index, columns, or both. When passed just a sequence, the rows are reindexed in the result.
- The columns can be reindexed using the **columns** keyword.
- reindex function arguments:

Argument | Description
--- | ---
index | New sequence to use as index. Can be Index instance or any other sequence-like Python data structure.
method | Interpolation (fill) method. ffill (fill forward)or bfill (fillbackward)
fill_value | Substitute value to use when introducing missing data by reindexing
limit | When forward- or backfilling, maximum size gap to fill
level | Match simple Index on level of MultiIndex, otherwise select subset of 
copy | Do not copy underlying data if new index is equivalent to old index. `True` by default (i.e. always copy data)

### 2.2 Dropping entries from an axis
- The `drop` method will return a new object with the indicated value or values deleted from an axis.
- With DataFrame, index values can be deleted from either axis.
---
这个功能非常简单实用，在实践过程中，经常会出现要删除某几行或某几列的情况。这个比R中的方法要简单许多。

### 2.3 Indexing, selection, and filtering
- Series indexing (obj[...]) works analogously to NumPy array indexing, except you can use the Series's **index values** instead of only integers.
- **Slicing with labels** behaves differently than normal Python slicing in that the endpoint is inclusive.
- The special indexing field **ix** enables you to select a subset of the rows and columns from a DataFrame with NumPy-like notation plus axis labels. This is also a less verbose way to to reindexing.
- Indexing options with DataFrame

Type | Notes
--- | ---
obj[val] | Select single column or sequence of columns from the DataFrame. Special case conveniences: boolean array (filter rows), slice (slice rows), or boolean DataFrame (set values based on some criterion).
obj.ix[val] | Selects single row or subset of rows from the DataFrame.
obj.ix[:, val] | Selects single column or subset of columns.
obj.ix[val1, val2] | Select both rows and columns.
`reindex` method | Conform one or more axes to new indexes.
`xs` method | Select single row or column as a Series by label.
`icol`, `irow` methods | Select single column or row, respectively, as a Series by integer location.
`get_value`, `set_value methods` | Select single value by row and column label.

### 2.4 Arithmetic and data alignment
- One of the most important pandas features is the behavior of arithmetic between objects with different indexes.
- When adding together objects, if any index pairs are not the same, the respective index in the result will be the union of the index pairs.
- The internal data alignment introduces NA values in the indices that don't overlap. Missing values propagate in arithmetic computations.
- Using arithmetic methods (`add`, `sub`, `div`, `mul`) and *fill_value=val* argument to fill the NA with a special value.
- By default, arithmetic between DataFrame and Series matches the index of the Series on the DataFrame's **columns**, broadcasting down the rows.
- If you want to instead broadcast over the columns, matching on the rows, you have to use one of the arithmetic methods (`add`, `sub`, `div`, `mul`) and the *axis=0* argument.

### 2.5 Function application and mapping
- Applying a function on 1D arrays to each column or row.
- `apply` and `applymap` methods
- Many of the most common array statistics (like `sum` and `mean`) are DataFrame methods, so using `apply` is not necessary.

### 2.6 Sorting and ranking
- To sort lexicographically by row or column index, use the `sort_index` method.
- To sort a Series by its values, use its `order` method.
- On DataFrame, you may want to sort by the values in one or more columns. To do so, pass one or more column names to the `by` option.
- *Ranking* is assigning ranks from one through the number of valid data points in an array. 
- DataFrame can compute ranks over the rows or the columns.
- Tie-breaking methods with rank

Method | Description
--- | ---
average | Defalut: assign the average rank to each entry in the equal group.
min | Use the minimum rank for the whole group.
max | Use the maximum rank for the whole group.
first| Assign ranks in the order the values appear in the data.
---
对DataFrame排序，尤其是按照多列排序，pandas的`sort_index`方法更方便.

### 2.7 Axis indexes with duplicate values
- While many pandas functions (like `reindex`) require that the labels be unique, it's not mandatory.
- Indexing a value with multiple entries returns a Series with single entries returns a scalar value.
---
- 这和R不同，注意区别

## 3. Summarizing and Computing Descriptive Statistics
- pandas objects are equipped with a set of common mathematical and statistical methods. Most of these fall into the category of *reductions* or *summary statistics*, methods that extract a single value (like the sum or mean) from a Series or a Series of values from the rows or column s of a DataFrame.
- NA values are excluded unless the entire row or column is NA. This can be disabled using the `skipna` option.
- Options for reduction methods:

Method | Description
--- | ---
axis | Axis to reduce over. 0 for DataFrame's rows and 1 for columns.
skipna | Exclude missing values, `True` by default.
level | Reduce grouped by level if the axis is hierarchically-indexed (MultiIndex).

- Descriptive and summary statistics:

Method | Description
--- | ---
count | Number of non-NA values
describe | Compute set of summary statistic for Series or each DataFrame column
min, max | Compute minimum and maximum values
argmin, argmax | Compute index **location (integers)** at which minimum or maximum value obtained, respectively
idxmin, idxmax | Compute index **value** at which minimum or maximum value obtained, respectively
quantile | Compute sample quantile ranging from 0 to 1
sum | Sum of values
mean | Mean of values
median | Arithmetic median (50% quantile) of values
mad | Mean absolute deviation from mean value
var | Sample variance of values
std | Sample standard deviation of values
skew | Sample skewness(3rd moment) of values
kurt | Sample kurtosis(4th moment) of values
cumsum | Cumulative sum of values
cummin, cummax | Cumulative minimum or maximum of values, respectively
cumprod | Cumulative product of values
diff | Compute 1st arithmetic difference (useful for time series)
pct_change | Compute percent changes

- Unique value, value counts, and membership

Method | Description
--- | ---
isin | Compute boolean array indicating whether each Series value is contained in the passed sequence of values.
unique | Compute array of unique values in a Series, returned in the order observed.
value_counts | Return a Series containing unique values as its index and frequencies as its values, ordered count in descending order.

## 4. Handling Missing Data
- NA handling methods:

Argument | Description
--- | ---
dropna | Filter axis labels based on whether values for each label have missing data, with varying thresholds (`thresh` argument) for how much missing data to tolerate. With DataFrame objects, dropna by default drops and row containing a missing value. Passing `how='all'` will only drop rows that are all NA. Dropping columns in the same way is only a matter of passing `axis=1`.
fillna | Fill in missing data with some value or using an interpolation method such as `ffill` or `bfill`
isnull | Return like-type object containing boolean values indicating which values are miising/NA.
notnull | Negation of isnull.

- fillna function arguments:

Argument | Description
--- | ---
value | Scalar value or dict-like object to use to fill missing values
method | Interpolation, by default 'ffill' if function called with no other arguments
axis | Axis to fill on, default axis=0
inplace | Modify the calling object without producing a copy
limit | For forward and backward filling, maximum number of consecutive periods to fill

## 5. Hierarchical Indexing
- *Hierarchical indexing* is an important feature of pandas enabling you to have multiple index *level* on an axis.
- 这是一个非常强大的功能，要通过不断实践熟练掌握，可以提高操作DataFrame时的效率
