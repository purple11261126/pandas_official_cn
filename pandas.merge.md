# pandas.merge
pandas.**merge**(left, right, how='inner', on=None, left_on=None, right_on=None, left_index=False, right_index=False, sort=False, suffixes=('_x', '_y'), copy=True, indicator=False, validate=None)

merge，**合并** 的意思。

通过一个或多个字段将不同 DataFrame 中的行连接起来。

## 参数说明
- **left**：DataFrame
要合并的左侧 DataFrame。

- **right**：DataFrame or named Series
要合并的右侧 DataFrame。

- **how**：{‘left’, ‘right’, ‘outer’, ‘inner’}, default ‘inner’
连接方式。
left：左连接，按照左边 DataFrame 的合并键合并。
right：右连接，按照右边 DataFrame 的合并键合并。
outer：外连接，按照两边 DataFrame 合并键的并集合并。
inner：内连接，按照两边 DataFrame 合并键的交集合并。

- **on**：label or list
用于合并的 columns，必须在两个数据集都包含，如果未指定，则以两个数据集相交的 columns 进行合并。

- **left_on**：label or list, or array-like
左侧 DataFrame 用于合并的 columns。

- **right_on**：label or list, or array-like
右侧 DataFrame 用于合并的 columns。

- **left_index**：bool, default False
是否将左侧 DataFrame 中的 index 作为合并键。 如果它是 MultiIndex，则另一个 DataFrame 中的键数（索引或列数）必须与级别数相匹配。

- **right_index**：bool, default False
和 left_index 类似，只不过，这个是右侧。

- **sort**：bool, default False
是否对合并后的数据进行排序。

- **suffixes**：tuple of (str, str), default (‘_x’, ‘_y’)
设置重复列名的后缀。如果想遇到重复列名触发异常，则使用（False，False）

- **copy**：bool, default True
如果为 False，请尽可能避免复制。

- **indicator**：bool or str, default False
如果为 True，则添加一列列名为“__merge”的 DataFrame，其中包含每行的信息。 如果 string，具有每行源的信息的列将被添加到输出 DataFrame，并且列将被命名为 string 的值。信息列是分类型的，并且对于其合并键仅出现在“左”DataFrame中的观察值为“left_only”，对于其合并键仅出现在“右”DataFrame中的观察值为“right_only”，如果出现，则为“both_only” 观察的合并密钥可以在两者中找到。

- **validate**：str, optional
如果指定，则检查 merge 是否为指定类型。
“one_to_one” 或 “1：1”：检查合并键是否在左右数据集中都是唯一的。
“one_to_many” 或 “1：m”：检查合并键在左数据集中是否唯一。
“many_to_one” 或 “m：1”：检查合并键在右侧数据集中是否唯一。
“many_to_many” 或 “m：m”：允许，但不会导致检查。

## pandas.merge 类似函数
- **pandas.merge_ordered**
合并数据，并且可以填充、插入缺失数据。

- **pandas.merge_asof**
匹配最近的键合并，而不是相等的键。

- **DataFrame.join**
按照 DataFrame 的 index 进行合并，且能同时合并多个 DataFrame。

## 例子
```python
df1 = pd.DataFrame({'key-l': ['foo', 'bar', 'baz', 'foo'], 'value': [1, 2, 3, 5]})
df2 = pd.DataFrame({'key-r': ['foo', 'bar', 'baz', 'foo'], 'value': [5, 6, 7, 8]})

print(df1)
print()
print(df2)

输出：
  key-l  value
0   foo      1
1   bar      2
2   baz      3
3   foo      5

  key-r  value
0   foo      5
1   bar      6
2   baz      7
3   foo      8

```

通过 'key-l' 和 'key-r' 两列，合并 df1 和 df2，并且设置重复列名后缀 '-l' 和 '-r'
```python
pd.merge(left=df1, right=df2, left_on='key-l', right_on='key-r', suffixes=('-l', '-r'))

输出：
  key-l  value-l key-r  value-r
0   foo        1   foo        5
1   foo        1   foo        8
2   foo        5   foo        5
3   foo        5   foo        8
4   bar        2   bar        6
5   baz        3   baz        7

```

 ‘key-l’ 和 ‘key-r’ 中存在重复项 ‘foo’，如果想报错，则 suffixes=(False, False)
```python
pd.merge(left=df1, right=df2, left_on='key-l', right_on='key-r', suffixes=(False, False))

输出：
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
......
```

看一下 left-index 和 right-index 属性的作用
```python
df_l = pd.DataFrame({'A':['A0', 'A1', 'A2', 'A3'], 'B':['B0', 'B1', 'B2', 'B3'], 'key':['K0', 'K1', 'K0', 'K1']})
df_r = pd.DataFrame({'C': ['C0', 'C1'], 'D': ['D0', 'D1']}, index=['K0', 'K1'])

print(df_l)
print()
print(df_r)

输出：
    A   B key
0  A0  B0  K0
1  A1  B1  K1
2  A2  B2  K0
3  A3  B3  K1

     C   D
K0  C0  D0
K1  C1  D1
```

```python
pd.merge(left=df_l, right=df_r, left_on='key', right_index=True)

输出：
    A   B key   C   D
0  A0  B0  K0  C0  D0
2  A2  B2  K0  C0  D0
1  A1  B1  K1  C1  D1
3  A3  B3  K1  C1  D1

```