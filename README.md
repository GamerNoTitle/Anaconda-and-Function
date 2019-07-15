# Anaconda-and-Function

这是一个关于Anaconda库的船新挑战！使用Anaconda库来完成里面的题目。用来练习Anaconda中Pandas和matplotlib中的pyplot的各种用法。

目录中的文件中带有finished的已经完成的，没带的是原题。*.ipynb文件需要运行在Anaconda的Jupyter Notebook里面。





# Pandas & Matplotlib

## Introduction

我们将使用Iris数据集。Iris也称鸢尾花卉数据集，是一类多重变量分析的数据集。通过花萼长度，花萼宽度，花瓣长度，花瓣宽度4个属性预测鸢尾花卉属于（Setosa，Versicolour，Virginica）三个种类中的哪一类。

```python
from __future__ import print_function
import os
data_path = ['data']
```

## Question 1

读取Iris数据集,并查看前5行数据

```python
import pandas as pd
filepath = '.\data\Iris_Data.csv'
data = pd.read_csv(filepath)
print(data.head())
```

```
   sepal_length  sepal_width  petal_length  petal_width      species
0           5.1          3.5           1.4          0.2  Iris-setosa
1           4.9          3.0           1.4          0.2  Iris-setosa
2           4.7          3.2           1.3          0.2  Iris-setosa
3           4.6          3.1           1.5          0.2  Iris-setosa
4           5.0          3.6           1.4          0.2  Iris-setosa
```

## Question 2

查询数据前10行的后3列数据

```python
print(data.iloc[-3:])
```

```
     sepal_length  sepal_width  petal_length  petal_width         species
147           6.5          3.0           5.2          2.0  Iris-virginica
148           6.2          3.4           5.4          2.3  Iris-virginica
149           5.9          3.0           5.1          1.8  Iris-virginica
```

## Question 3

确定以下内容:

- 样本数量 (行). (*提示:* 使用dataframe的 `.shape` 属性.)
- 列名. (*提示:* 使用dataframe 的`.columns` 属性.)
- 每列的数据类型. (*提示:* 使用dataframe 的`.dtypes` 属性.)

```python
print(data.shape)
print(data.columns)
print(data.dtypes)
```

```
(150, 5)
Index(['sepal_length', 'sepal_width', 'petal_length', 'petal_width',
       'species'],
      dtype='object')
sepal_length    float64
sepal_width     float64
petal_length    float64
petal_width     float64
species          object
dtype: object
```

## Question 4

检查种类名称，注意它们都以“iris-”开头。删除名称的这一部分，使种类名称变短。

*提示:* 你可以使用这两种方式： [string processing methods](http://pandas.pydata.org/pandas-docs/stable/text.html) or the [apply method](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.apply.html).

```python
print(data.species.str.replace('Iris-',''))
```

```
0         setosa
1         setosa
2         setosa
3         setosa
4         setosa
5         setosa
6         setosa
7         setosa
8         setosa
9         setosa
10        setosa
11        setosa
12        setosa
13        setosa
14        setosa
15        setosa
16        setosa
17        setosa
18        setosa
19        setosa
20        setosa
21        setosa
22        setosa
23        setosa
24        setosa
25        setosa
26        setosa
27        setosa
28        setosa
29        setosa
         ...    
120    virginica
121    virginica
122    virginica
123    virginica
124    virginica
125    virginica
126    virginica
127    virginica
128    virginica
129    virginica
130    virginica
131    virginica
132    virginica
133    virginica
134    virginica
135    virginica
136    virginica
137    virginica
138    virginica
139    virginica
140    virginica
141    virginica
142    virginica
143    virginica
144    virginica
145    virginica
146    virginica
147    virginica
148    virginica
149    virginica
Name: species, Length: 150, dtype: object
```

## Question 5

确定以下内容:

- 每个种类的数量. (*提示:* 使用series 的`.value_counts` 方法.)
- 每个特征的平均值、25%分位数、中值、75%分位数和范围（最大值- 最小值）放在一个DataFrame里

*提示:* 对于第二个问题, `.describe` 可以获得中值, 但在这里它不叫中值，而叫*50%* 分位数. `.describe` 不能获得范围, 你可以在 `.describe` 表中添加一行名字为`range`的行, 计算为 `max - min`.

```python
print(data.species.value_counts())
df=data.describe()
df=df.drop('count')
df=df.drop('std')
df.loc['range']=df.loc['max']-df.loc['min']
df=df.rename(index={'50%':'median'})
print(df)
```

```
Iris-setosa        50
Iris-versicolor    50
Iris-virginica     50
Name: species, dtype: int64
        sepal_length  sepal_width  petal_length  petal_width
mean        5.843333        3.054      3.758667     1.198667
min         4.300000        2.000      1.000000     0.100000
25%         5.100000        2.800      1.600000     0.300000
median      5.800000        3.000      4.350000     1.300000
75%         6.400000        3.300      5.100000     1.800000
max         7.900000        4.400      6.900000     2.500000
range       3.600000        2.400      5.900000     2.400000
```

## Question 6

**为每个物种** 计算下面的值:

- 均值 (sepal_length, sepal_width, petal_length, and petal_width).
- 中位数(sepal_length, sepal_width, petal_length, and petal_width).

*提示:* 你可以在计算这些值之前使用 Pandas中的 [`groupby` 方法](http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.groupby.html) 去对每个物种做分组.

```python
data = pd.read_csv(filepath)
group = (data.groupby('species').mean())
print(group)
```

```
                 sepal_length  sepal_width  petal_length  petal_width
species                                                              
Iris-setosa             5.006        3.418         1.464        0.244
Iris-versicolor         5.936        2.770         4.260        1.326
Iris-virginica          6.588        2.974         5.552        2.026
```

## Question 7

用Matplotlib给 `petal_length` 和 `petal_width` 画一个折线图，使用不同颜色

```python
from matplotlib import pyplot as plt
%matplotlib inline
```

```python
plt.plot(data.petal_length,color='c')
plt.plot(data.petal_width,color='b')
plt.show()
```

![img](.\img\Q7.png)



## Question 8

用Matplotlib给 `sepal_length` vs `sepal_width` 画一个散点图. 并标出轴、添加标题、透明度和添加Legend

```python
plt.scatter(data.sepal_length,data.sepal_width,color='c',alpha=0.5)
plt.title('sepal_length vs sepal_width')
plt.legend()
plt.show()
```

![img](.\img\Q8.png)

## Question 9

画出Sepal Length特征的直方图. 标出轴并给出标题名.

```python
plt.hist(data.sepal_length,bins=20,color='c',rwidth=0.8,alpha=0.5)
plt.title('Sepal Length')
plt.show()
```

![img](.\img\Q9.png)