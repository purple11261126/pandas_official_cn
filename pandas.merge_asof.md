# pandas.merge_asof

pandas.**merge_asof**(left, right, on=None, left_on=None, right_on=None, left_index=False, right_index=False, by=None, left_by=None, right_by=None, suffixes=('_x', '_y'), tolerance=None, allow_exact_matches=True, direction='backward')

merge_asof，**合并_像** 的意思。
执行 asof 合并， 匹配“on”列数值最接近的键而不是相等的键。
此功能可用于对齐不同的日期时间频率，而无需重新采样。

与 pd.merge_ordered（）类似，pd.merge_asof（）函数也使用“on”列按顺序合并值，右侧 df 小于或大于左侧 df 的值会被保留，用作匹配。

对于左侧 DataFrame 中的每一行：
“向后”搜索，选择右侧 DataFrame 中的最后一行，其中“on”键小于或等于左侧的键。
“向前”搜索，选择右侧 DataFrame 中的第一行，其中“on”键大于或等于左侧的键。
“最近”搜索，选择右侧 DataFrame 中，其中“on”键与左侧键的绝对距离最近。

默认为“向后”，并且在0.20.0以下的版本中兼容。 方向参数在版本0.20.0中添加，并引入“向前”和“最近”。

在使用“on”搜索之前，可选择使用“by”匹配等效键。

## 参数说明
- **left**：DataFrame
左侧 DataFrame。

- **right**：DataFrame
右侧 DataFrame。

- **on**：label
用于合并的列，必须在两个 DataFrame 中都能找到。另外，该列的值必须是数字类型，例如 datetimelike，integer 或 float。必须指定 on 或 left_on 或 right_on。

- **left_on**：label
按照左侧 DataFrame 指定的列合并。

- **right_on**：label
按照右侧 DataFrame 指定的列合并。

- **left_index**：boolean
使用左侧 DataFrame 的 index 作为连接键。

- **right_index**：boolean
使用右侧 DataFrame 的 index 作为连接键。

- **by**：column name or list of column names
在执行合并操作之前排序这些列。

- **left_by**：column name
要在左侧 DataFrame 中排序的列。

- **right_by**：column name
要在右侧 DataFrame 中排序的列。

- **suffixes**：2-length sequence (tuple, list, …)
区别重复列名时添加的后缀。

- **tolerance**：integer or Timedelta, optional, default None
 宽容度，也就是大于或小于的范围。必须与合并索引兼容。

- **allow_exact_matches**：boolean, default True
是否匹配相同的“on”值。
如果为 True，则允许匹配相同的“on”值（即小于或等于/大于或等于）。
如果为 False，则不匹配相同的'on'值（即，严格小于/严格大于）。

- **direction**：‘backward’ (default), ‘forward’, or ‘nearest’
方向。
backward：向后搜索。
forward：向前搜索。
nearest：最近搜索。

## 返回值
- **merged**：DataFrame

## pandas.merge_asof 类似函数
- **pandas.merge**
- **pandas.merge_ordered**

## 例子
```python
left = pd.DataFrame({'a': [1, 5, 10], 'left_val': ['a', 'b', 'c']})
right = pd.DataFrame({'a': [1, 2, 3, 6, 7], 'right_val': [1, 2, 3, 6, 7]})

print(left)
print()
print(right)

输出：
    a left_val
0   1        a
1   5        b
2  10        c

   a  right_val
0  1          1
1  2          2
2  3          3
3  6          6
4  7          7
```

先是简单体验下，默认向后搜索，left a 列 1、5、10 匹配到 right a 列，1 小于最接近的就是 1，5 小于且最接近的是 3，10 小于且最接近的还是 3
```python
pd.merge_asof(left=left, right=right, on='a')

输出：
    a left_val  right_val
0   1        a          1
1   5        b          3
2  10        c          7
```

向前搜索
```python
pd.merge_asof(left, right, on='a', direction='forward')

输出：
    a left_val  right_val
0   1        a        1.0
1   5        b        6.0
2  10        c        NaN
```

最近搜索
```python
pd.merge_asof(left, right, on='a', direction='nearest')

输出：
    a left_val  right_val
0   1        a          1
1   5        b          6
2  10        c          7
```

严格大于或小于
```python
pd.merge_asof(left, right, on='a', allow_exact_matches=False)

输出：
    a left_val  right_val
0   1        a        NaN
1   5        b        3.0
2  10        c        7.0
```

也可以使用 DataFrame 的 index 来合并
```python
left = pd.DataFrame({'left_val': ['a', 'b', 'c']}, index=[1, 5, 10])
right = pd.DataFrame({'right_val': [1, 2, 3, 6, 7]}, index=[1, 2, 3, 6, 7])
print(left)
print()
print(right)

输出：
   left_val
1         a
5         b
10        c

   right_val
1          1
2          2
3          3
6          6
7          7
```

```python
pd.merge_asof(left, right, left_index=True, right_index=True)

输出：
   left_val  right_val
1         a          1
5         b          3
10        c          7
```

下面是一个时间序列的例子，和真实的应用场景很接近了。
```python
quotes

输出：
                     time ticker     bid     ask
0 2016-05-25 13:30:00.023   GOOG  720.50  720.93
1 2016-05-25 13:30:00.023   MSFT   51.95   51.96
2 2016-05-25 13:30:00.030   MSFT   51.97   51.98
3 2016-05-25 13:30:00.041   MSFT   51.99   52.00
4 2016-05-25 13:30:00.048   GOOG  720.50  720.93
5 2016-05-25 13:30:00.049   AAPL   97.99   98.01
6 2016-05-25 13:30:00.072   GOOG  720.50  720.88
7 2016-05-25 13:30:00.075   MSFT   52.01   52.03

trades

输出：
                     time ticker   price  quantity
0 2016-05-25 13:30:00.023   MSFT   51.95        75
1 2016-05-25 13:30:00.038   MSFT   51.95       155
2 2016-05-25 13:30:00.048   GOOG  720.77       100
3 2016-05-25 13:30:00.048   GOOG  720.92       100
4 2016-05-25 13:30:00.048   AAPL   98.00       100
```

默认把 quotes 合并到 trades 中
```python
pd.merge_asof(trades, quotes, on='time', by='ticker')

输出：
                     time ticker   price  quantity     bid     ask
0 2016-05-25 13:30:00.023   MSFT   51.95        75   51.95   51.96
1 2016-05-25 13:30:00.038   MSFT   51.95       155   51.97   51.98
2 2016-05-25 13:30:00.048   GOOG  720.77       100  720.50  720.93
3 2016-05-25 13:30:00.048   GOOG  720.92       100  720.50  720.93
4 2016-05-25 13:30:00.048   AAPL   98.00       100     NaN     NaN
```

容忍度设置为 2ms，那么 trades 的 time 列只能匹配到相差 2ms 之内的值。
```python
pd.merge_asof(trades, quotes, on='time', by='ticker', tolerance=pd.Timedelta('2ms'))

输出：
                     time ticker   price  quantity     bid     ask
0 2016-05-25 13:30:00.023   MSFT   51.95        75   51.95   51.96
1 2016-05-25 13:30:00.038   MSFT   51.95       155     NaN     NaN
2 2016-05-25 13:30:00.048   GOOG  720.77       100  720.50  720.93
3 2016-05-25 13:30:00.048   GOOG  720.92       100  720.50  720.93
4 2016-05-25 13:30:00.048   AAPL   98.00       100     NaN     NaN
```

```python
pd.merge_asof(trades, quotes, on='time', by='ticker', tolerance=pd.Timedelta('10ms'), allow_exact_matches=False)

输出：
                     time ticker   price  quantity     bid     ask
0 2016-05-25 13:30:00.023   MSFT   51.95        75     NaN     NaN
1 2016-05-25 13:30:00.038   MSFT   51.95       155   51.97   51.98
2 2016-05-25 13:30:00.048   GOOG  720.77       100     NaN     NaN
3 2016-05-25 13:30:00.048   GOOG  720.92       100     NaN     NaN
4 2016-05-25 13:30:00.048   AAPL   98.00       100     NaN     NaN
```