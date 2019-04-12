# pandas.crosstab

pandas.**crosstab**(index, columns, values=None, rownames=None, colnames=None, aggfunc=None, margins=False, margins_name='All', dropna=True, normalize=False)

crosstab，交叉表的意思，用于统计分组频率的特殊透视表

计算两个（或更多）因子的简单交叉列表。默认情况下，计算因子的频率表，除非传递值数组和聚合函数

## 参数说明

- **index**： array-like, Series, or list of arrays/Serie
新表的 index。

- **columns**：array-like, Series, or list of arrays/Series
新表的 columns。

- **values**： array-like, optional
新表的 values。选填。

- **rownames / colnames**：sequence, default None
新表行 / 列名称。如果设置，必须和 index / columns 属性的列表值数量一致。

- **aggfunc**：function, optional
“聚合函数”（aggregation function）的简称。

- **margins**：boolean, default False
在末尾增加 ALL 列和行，显示总计信息。

- **margins_name**：string, default ‘All’
margins=True 时，设置其名称，默认是 ‘ALL’。

- **dropna**：boolean, default True。
删除数值全部为 NaN 的列。

- **normalize**：boolean, {‘all’, ‘index’, ‘columns’}, or {0,1}, default False
通过将所有值除以值的总和来归一化。
如果传递 ‘all’ 或 ‘True’，将对所有值进行标准化。
如果传递 ‘index’ 将在每行上标准化。
如果传递 ‘columns’ 将在每列上标准化。
如果 margins=True，则还会标准化 margins 值。

## 返回值
- **crosstab** : DataFrame

## 例子

```python
>>> a = np.array(["foo", "foo", "foo", "foo", "bar", "bar",
...               "bar", "bar", "foo", "foo", "foo"], dtype=object)
>>> b = np.array(["one", "one", "one", "two", "one", "one",
...               "one", "two", "two", "two", "one"], dtype=object)
>>> c = np.array(["dull", "dull", "shiny", "dull", "dull", "shiny",
...               "shiny", "dull", "shiny", "shiny", "shiny"],
...               dtype=object)
```
```python
>>> pd.crosstab(index=a, columns=[b, c], rownames=['A'], colnames=['B', 'C'])

B   one        two
C   dull shiny dull shiny
A
bar    1     2    1     0
foo    2     2    1     2
```
```python
>>> foo = pd.Categorical(['a', 'b'], categories=['a', 'b', 'c'])
>>> bar = pd.Categorical(['d', 'e'], categories=['d', 'e', 'f'])
>>> pd.crosstab(index=foo, columns=bar)

col_0  d  e
row_0
a      1  0
b      0  1

>>> pd.crosstab(index=foo, columns=bar, dropna=False)

col_0  d  e  f
row_0
a      1  0  0
b      0  1  0
c      0  0  0
```