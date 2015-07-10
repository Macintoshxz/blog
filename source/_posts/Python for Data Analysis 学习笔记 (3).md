title: Python for Data Analysis 学习笔记 (3)
date: 2015-07-08 14:33:45
tags: Python
---

# Data Wrangling: Clean, Transform, Merge, Reshape

代码详见: [Python Notebook]()

## 1. Combining and Merging Data Sets
Data contained in pandas objects can be combined together in a number of built-in ways:
- **pandas.merge** connects rows in DataFrames based on one or more keys.
- **pandas.concat** glues or stacks together objects along a axis.
- **combine_first** instance method enables splicing together overlapping data to fill in missing values in one object with values from another.

### 1.1 Database-style DataFrame Merges
- `merge` function arguments:

Argument | Description
--- | ---
left | DataFrame to be merged on the left side
right | DataFrame to be merged on the right side
how | One of 'inner'(default), 'outer', 'left' or 'right'
on | Column names to join on. Must be found in both DataFrame objects. If not specified and no other join keys given, will use the intersection of the column names in `left` and `right` as the join keys
left_on | Columns in `left` DataFrame to use as join keys
right_on | Analogous to `left_on` for `left` DataFrame
left_index | Use row index in `left` as its join key (or keys, if a MultiIndex)
right_index | Analogous to `left_index`
sort | Sort merged data lexicographically by join keys; `True` by default. Disable to get better performance in some cases on large datasets
suffixes | Tuple of string values to append to column names in case of overlap; defaults to ('_x', '_y').
copy | If `False`, avoid copying data into resulting data structure in some exceptional cases. By default always copies

### 1.2 Merging on Index
- In some cases, the merge key or keys in a DataFrame will be found in its index. In this case, you can pass **left_index=True** or **right_index=True** (or both) to indicate that the index should be used as the merge key.
- DataFrame has a more convenient `join` instance for merging by index
- It also supports joining the index of the **passed** DataFrame on one of the columns of the calling DataFrame
- For simple index-on-index merges, you can pass a list of DataFrames to `join` as an alternative to using the more general `concat` function.

### 1.3 Concatenating Along an Axis
Argument | Description
--- | ---
objs | List or dict of pandas objects to be concatenated. The only required argument
axis | Axis to concatenate along; defaults to 0
join | One of `inner`, `outer`(default); whether to intersection (inner) or union (outer) together indexes along the other axes
join_axes | Specific indexes to use for the other n-1 axes instead of performing union/intersection logic
keys | Values to associate with objects being concatenated, forming a hierarchical index along the concatenation axis. Can either be a list or array of arbitrary values, an array of tuples, or a list of arrays (if multiple level arrays passed in `levels`)
levels | Specific indexes to use as hierarchical index level or levels if keys passed
names | Names for created hierarchical levels if `keys` and / or `levels` passed
verify_integrity | Check new axis in concatenated object for duplicates and raise exception if so. By default (False) allows duplicates.
ignore_index | Do not preserve indexes along concatenation `axis`, instead producing a new range (total_length) index
 
### 1.4 Combining Data with Overlap
- You can think of **combine_first** as "patching" missing data in the calling object with data from the object you pass

### 1.5 Reshaping and Pivoting
- There are a number of fundamental operations for rearranging tabular data. These are alternatively referred to as *reshape* or *pivot* operations.
- Hierarchical indexing provides a consistent way to rearrange data in a DataFrame
- **stack**: this "rotates" or pivots from the columns in the data to the rows
- **unstack**: this pivots from the rows into the columns
- **pivot** is just a shortcut for creating a hierarchical index using **set_index** and reshaping with **unstack**

## 2. Data Transformation
### 2.1 Removing Duplicates
- The DataFrame method **duplicated** returns a boolean Series indicating wether each row is a duplicate or not
- **drop_duplicates** returns a DataFrame where the **duplicated** array is **True**
- Both of these methods by default consider all of the columns; alternatively you can specify any subset of them to detect duplicates.
- **duplicated** and **drop_duplicates** by default keep the first observed value combination. Passing **take_last=True** will return the last one.

### 2.2 Transforming Data using a Function or Mapping
- The **map** method on a Series accepts a function or dict-like object containing a mapping.
- Using **map** is a convenient way to perform element-wise transformations and other data cleaning-related operations.

### 2.3 Replacing Values
- If you want to replace multiple values at once, pass a list then the substitute value.
- To use a different replacement for each value, pass a list of substitutes
- The argument passed can also be a dict

### 2.4 Renaming Axis Indexes
- Like values in a Series, axis labels can be similarly transformed by a function or mapping of some form to produce new, differently labeled objects. The axes can also be modified in place without creating a new data structure.
- If you want to create a transformed version of a data set without modifying the original, a useful method is **rename**
- Notably, **rename** can be used in conjunction with a dict-like object providing new values for a subset of the axis labels. **rename** saves having to copy the DataFrame manually and assign to its **index** and **columns** attributes.

### 2.5 Discretization and Binning
- **cut** function returns a special **Categorical** object. You can treat it like an array of strings indicating the bin name; internally it contains a **levels** array indicating the distinct category names along with a labeling  in the **labels** attribute
- You can pass your own bin names by passing a list or array to the **labels** option
- **qcut** function bins the data based on sample quantiles. Depending on the distribution of the data, using **cut** will not usually result in each bin having the same number of data points. Sine **qcut** uses sample quantiles instead, by definition you will obtain roughly equal-size bins

### 2.6 Detecting and Filtering Outliers
- **any** method on a boolean DataFrame
- **np.sign** returens an array of 1 and -1 depending on the sign of the values

### 2.7 Permutation and Random Sampling
- Permuting (randomly reordering) a Series or the rows in a DataFrame is easy to do using the **numpy.random.permutation** function.
- To select a random subset without replacement, one way is to slice off the first k elements of the array returned by **permutation**, where k is the desired subset size.
- To generate a sample with replacement, the fastest way is to use **np.random.randint** to draw random integers.

### 2.8 Computing indicator/Dummy Variables
- **get_dummies**: If a column in a DataFrame has k distinct values (categorical variable), you would derive a matrix or DataFrame containing k columns containing all 1's and 0's.
- A useful recipe for statistical applications is to combine **get_dummies** with a discretization function like **cut**

## 3. String Manipulation
### 3.1 String Object Methods
- Python built-in string methods

Argument | Description
--- | ---
count | Return the number of non-overlapping occurrences of substring in the string
endswith, startswith | Returns True if string ends with suffix (starts with prefix)
join | Use string as delimiter for concatenating a sequence of other strings
index | Return position of first character in substring if found in the string. Raises `ValueError` if not fount
find | Return position of first character of *first* occurrence of substring in the string. Like **index**, but returns -1 if not found
rfind | Return position of first character of *last* occurrence of substring in the string. Returns -1 if not found
replace | Replace occurrences of string with another string
strip, rstrip, lstrip | Trim whitespace, including newlines
split | Break string into list of substrings using passed delimiter
lower, upper | Convert alphabet characters to lowercase or uppercase, respectively
ljust, rjust | Left justify or right justify, respectively. Pad opposite side of string with spaces (or some other fill character) to return a string with a minimum width.

### 3.2 Regular expressions
- Regular expression methods

Argument | Description
--- | ---
findall, finditer | Return all non-overlapping matching patterns in a string. `findall` returns a list of all patterns while `finditer` returns them one by one from an iterator
match | Match pattern at **start** of string and optionally segment pattern components into groups. If the pattern matches, returns a match object, otherwise `None`
search | Scan string for match to pattern; returning a match object if so. Unlike `match`, the match can be anywhere in the string as opposed to only at the the beginning
split | Break string into pieces at each occurrence of pattern
sub, subn | Replace all (sub) or first n occurrences (subn) of pattern in string with replacement expression. Use symbols \1, \2, ... to refer to match group elements in the replacement string

### 3.3 Vectorized string functions in pandas
- String and regular expression methods can be applied (passing a **lambda** or other function) to each value using **data.map**, but it will fail on the NA. To cope with this, Series has concise methods for string operations that skip NA values. These are accessed through Series's **str** attribute
- Vectorzied string methods

Method | Description
--- | ---
cat | Concatenate strings element-wise with optional delimiter
contains | Return boolean array if each string contains pattern/regex
count | Count occurrences of pattern
endswith, startswith | Equivalent to **x.endswith(pattern)** or **x.startswith(pattern)** for each element
findall | Compute list of all occurrences of pattern/regex for each string
get | Index into each element (retrieve i-th element)
join | Join strings in each element of the Series with passed separator
len | Compute length of each string
lower, upper | Convert case; equivalent to **x.lower()** or **x.upper()** for each element
match | Use **re.match** with the passed regular expression on each element, returning matched groups as list
pad | Add whitespace to left, right, or both sides of strings
center | Equivalent to **pad(side='both')**
repeat | Duplicate values; for example **s.str.repeat(3)** equivalent to x * 3 for each string
replace | Replace occurrences of pattern/regex with some other string
slice | Slice each string in the Series
split | Split strings on delimiter or regular expression
strip, rstrip, lstrip | Trim whitespace, including newlines; equivalent to **x.strip()** (and **rstrip**, **lstrip**, respectively) for each element


