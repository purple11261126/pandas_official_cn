# pandas.pivot_table
 pandas.**pivot_table**(data, values=None, index=None, columns=None, aggfunc='mean', fill_value=None, margins=False, dropna=True, margins_name='All')

pivot_table，数据透视表的意思。

创建一个 DataFrame 类型的数据透视表，数据透视表中的级别将存储在结果 DataFrame 的索引和列上的 MultiIndex 对象（层次索引）中。

## 参数说明

- **data**：DataFrame。
要处理的数据集。

- **values**：optional。
数据透视表中显示的值。

- **index**： column, Grouper, array, or list of the previous。
数据透视表中的 index。如果传递数组，则它必须与数据的长度相同。 该列表可以包含任何其他类型（列表除外）。如果传递数组，则它的使用方式与列值相同。

- **columns**： column, Grouper, array, or list of the previous。
数据透视表中的 columns。如果传递数组，则它必须与数据的长度相同。 该列表可以包含任何其他类型（列表除外）。 如果传递数组，则它的使用方式与列值相同。

- **aggfunc**：function, list of functions, dict, default numpy.mean
“聚合函数”（aggregation function）的简称。遇到重复值时如何操作。如果传递函数列表，生成的数据透视表将具有分层列，其顶层是函数名称（从函数对象本身推断），如果传递 dict，则键是要聚合的列，值是函数或函数列表。

- **fill_value**：scalar, default None
填充缺失值。

- **margins**：boolean, default False
在末尾增加 ALL 列和行，显示总计信息。

- **dropna**：boolean, default True。
删除数值全部为 NaN 的列。

- **margins_name**：string, default ‘All’。
margins=True 时，设置其名称，默认是 ‘ALL’。

## 返回值
- **table** : DataFrame

## pandas.pivot_table 类似函数
- **DataFrame.pivot**

## 例子
```python
>>> df = pd.DataFrame({"A": ["foo", "foo", "foo", "foo", "foo",
...                          "bar", "bar", "bar", "bar"],
...                    "B": ["one", "one", "one", "two", "two",
...                          "one", "one", "two", "two"],
...                    "C": ["small", "large", "large", "small",
...                          "small", "large", "small", "small",
...                          "large"],
...                    "D": [1, 2, 2, 3, 3, 4, 5, 6, 7],
...                    "E": [2, 4, 5, 5, 6, 6, 8, 9, 9]})
>>> df
     A    B      C  D  E
0  foo  one  small  1  2
1  foo  one  large  2  4
2  foo  one  large  2  5
3  foo  two  small  3  5
4  foo  two  small  3  6
5  bar  one  large  4  6
6  bar  one  small  5  8
7  bar  two  small  6  9
8  bar  two  large  7  9
```

aggfunc=np.sum，聚合函数为求和。
```python
>>> table = pivot_table(df, values='D', index=['A', 'B'],
...                     columns=['C'], aggfunc=np.sum)
>>> table
C        large  small
A   B
bar one      4      5
    two      7      6
foo one      4      1
    two    NaN      6
```

fill_value=0，用数字 0 填充缺失值。
```python
>>> table = pivot_table(df, values='D', index=['A', 'B'],
...                     columns=['C'], aggfunc=np.sum, fill_value=0)
>>> table
C        large  small
A   B
bar one      4      5
    two      7      6
foo one      4      1
    two      0      6
```

aggfunc={'D': np.mean,'E': np.mean}，聚合函数还可以操作多个列
```python
>>> table = pivot_table(df, values=['D', 'E'], index=['A', 'C'],
...                     aggfunc={'D': np.mean,
...                              'E': np.mean})
>>> table
                  D         E
               mean      mean
A   C
bar large  5.500000  7.500000
    small  5.500000  8.500000
foo large  2.000000  4.500000
    small  2.333333  4.333333
```

aggfunc={'D': np.mean,'E': [min, max, np.mean]}，聚合函数可以对一个列使用多个函数操作。
```python
>>> table = pivot_table(df, values=['D', 'E'], index=['A', 'C'],
...                     aggfunc={'D': np.mean,
...                              'E': [min, max, np.mean]})
>>> table
                  D   E
               mean max      mean min
A   C
bar large  5.500000  9   7.500000   6
    small  5.500000  9   8.500000   8
foo large  2.000000  5   4.500000   4
    small  2.333333  6   4.333333   2
```