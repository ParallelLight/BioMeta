## Python中Dataframe常用的一些简单操作

DataFrame是一个以命名列方式组织的分布式数据集，是一种特殊的数据结构，简单理解起来类似Excel中的一个数据表，每个单元格都对应特定行特定列的值。

- Pandas在线API文档：[https://pandas.pydata.org/pandas-docs/stable/reference/index.html#api](https://pandas.pydata.org/pandas-docs/stable/reference/index.html#api)
- Dataframe在线API文档：[https://pandas.pydata.org/pandas-docs/stable/reference/frame.html](https://pandas.pydata.org/pandas-docs/stable/reference/frame.html)

DataFrame是处理矩阵型（Array）数据的有力工具，对于生信分析中的各种基因表达谱数据、转录组数据等行列式数据大有脾益，这里总结记录了DataFrame常用的一些简单操作。

### Dataframe创建/初始化

```python
import pandas as pd
import numpy as np

df = pd.DataFrame(np.random.randint(low=0, high=10, size=(10, 10)), columns=['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j'])
df

# >>>
    a	b	c	d	e	f	g	h	i	j
0	4	8	3	9	5	7	9	6	6	2
1	5	2	1	5	0	1	2	6	4	6
2	3	4	7	9	1	6	9	9	0	7
3	3	1	2	8	1	5	1	5	9	8
4	5	2	9	7	7	0	8	6	8	4
5	0	6	9	1	2	8	5	0	6	7
6	1	5	9	8	3	5	0	4	2	1
7	3	2	5	4	1	7	8	7	7	9
8	7	7	6	8	1	4	2	4	4	6
9	6	9	7	3	5	2	9	3	8	6
```

### Dataframe设置索引与取消索引

```python
df.reset_index()     # 取消索引
df.set_index('the_column')	# 设置某列为索引，覆盖原始索引
```

### Dataframe排序取均值

```python
df.groupby(df.index).mean()
df.groupby(["the_column"]).mean()
```

### Dataframe保留小数

```python
# Dataframe保留小数
# 所有的列保留2位小数点
>>> df.round(2)
           A     B     C
first   0.03  0.99  0.17
second  0.04  0.65  0.58
third   0.88  0.15  0.49

# 指定的列，保留指定的小数点（通过hash传递）
>>> df.round({'A': 1, 'C': 2})
          A         B     C
first   0.0  0.992815  0.17
second  0.0  0.645646  0.58
third   0.9  0.149370  0.49

# 指定的列，保留指定的小数点（通过series传递）
>>> decimals = pd.Series([1, 0, 2], index=['A', 'B', 'C'])
>>> df.round(decimals)
          A  B     C
first   0.0  1  0.17
second  0.0  1  0.58
third   0.9  0  0.49

# ============================================================

# numpy保留小数
import numpy as np
aa=np.random.rand(2,3)
print(aa)
np.set_printoptions(precision=4)
print(aa)
```


### Dataframe取前两列

```python
df.iloc[:, :2]
```

### Dataframe删除行或者列

```python
df.drop('the_row')		# drop a row
df.drop('the_column', axis=1)	# drop a column
```

### Dataframe删除多行或多列

```python
df.drop(['the_row1', 'the_row2'], axis=0)		# 删除行
df.drop(['the_column1', 'the_column2'], axis=1)		# 删除列
```

### Dataframe删除一行全为0的数据

```python
# 方法1
# 筛选全为0的行
df.loc[(df==0).all(axis=1)]

# 筛选不全为0的行=删除全为0的行
df.loc[~(df==0).all(axis=1)]


# 方法2
df.loc[(df!=0).any(1)]


# 曲线救国：求一行之和，若和为0则删除，存在不足，可能会影响分析
df[df.apply(np.sum, axis=1)!=0]
```

### Dataframe删除一行全为nan的数据

```python
df.dropna(axis=0, how='all')
```

### Dataframe重置列名

```python
df.rename(columns={'a':'A', 'b':'B'})
```
