title: Python for Data Analysis 学习笔记 (1)
date: 2015-07-02 12:40:46
tags: Python

---

## 1. Python用于数据分析的包
### 1.1 [NumPy](http://www.numpy.org/)
Numpy, short for Numerical Python, is the **foundational package** for scientific computing in Python.
For numerical data, NumPy arrays are a much more efficient way of storing and manipulating data than the other built-in Python data structures.

NumPy是最重要的一个package，pandas是建立在它的ndarray基础上的。

### 1.2 [pandas](http://pandas.pydata.org/)
pandas combines the high performance **array-computing** features of NumPy with the flexible **data manipulation** capabilities of spreadsheets and relational database.
The functionality provided by **data.frame** in R is essentially a strict subset of that provided by the pandas **DataFrame**.

在数据分析的实践过程中，我们的大部分时间和精力都花在了对原始数据的预处理上。我对R的data.frame的操作已经很熟练，但pandas能提供更多的数据操作功能，这也是我学习pandas的主要目的和动力，希望能够更进一步的提高生产力。

### 1.3 [matplotlib](http://matplotlib.org/)
matplotlib is the most popular Python library for producing plots and other 2D data visualizations.

我个人还是比较喜欢R的画图功能，很少用Python画图。相比较而言，R画的图还是更漂亮一些。但是Python画图还是值得一学。

### 1.4 [IPython](http://ipython.org/)
IPython is an enhanced Python shell designed to accelerate the writing, testing, and debugging of Python code. It is particularly useful for interactively working with data and visualizing data with matplotlib.

IPython非常适合交互式的数据分析和可视化。用Python做数据分析不能缺少IPython就如同用R做数据分析离不开RStudio一样。

### 1.5 [SciPy](http://scipy.org/)
SciPy is a collection of packages addressing a number of different standard problem domains in scientific computing.

SciPy的主要功能就是完成特定的科学计算任务，比如微积分、线性代数、概率统计等等。NumPy+SciPy基本上就可以算一个开源的MATLAB了。

## 2. 装Python和相关的包
对于初学者来说，最简单的方法就是用Python distribution，我个人比较喜欢[**Anaconda**](https://store.continuum.io/cshop/anaconda/)。一个命令就可以把所有常用的数据分析的包安装好。

对于有一定Python经验的人也可以用[virtualenv](http://docs.python-guide.org/en/latest/dev/virtualenvs/) 和 [PyPI](https://pypi.python.org/)，按需安装。

## 3. IDE的选择
我用过的Python的IDE有：

 - Eclipse with PyDev Plugin
 - Spyder
 - IPython

相比较而言，我更加喜欢IPython，小巧灵活，Spyder也不错。Eclipse就有点杀鸡用牛刀的感觉，而且安装PyDev也麻烦。IPython和Spyder都是包括在Anaconda里面的，所以免去了安装的麻烦。
当然还有很多其他的IDE，我没有用过也不做评价。我认为没有必要去花太多时间找最好的IDE，很多IDE的功能也用不上，对于数据分析的初学者来说用IPython足够了。









