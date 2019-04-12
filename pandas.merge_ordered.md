# pandas.merge_ordered
pandas.**merge_ordered**(left, right, on=None, left_on=None, right_on=None, left_by=None, right_by=None, fill_method=None, suffixes=('_x', '_y'), how='outer')

merge_ordered，**合并_有序整齐** 的意思。

合并时间序列和其他有序数据集，特别的一点是可以使用 fill_method 属性填充缺失值。可以选择执行分组合并（参见示例）。

## 参数说明
- **left**：DataFrame
要合并的左侧 DataFrame。

- **right**：DataFrame
要合并的右侧 DataFrame。

- **on** : label or list
要合并的字段名称。 必须在两个 DataFrame 中都能找到。

- **left_on**：label or list, or array-like
左侧 DataFrame 用于合并的 columns。

- **right_on**：label or list, or array-like
右侧 DataFrame 用于合并的 columns。

- **left_by**：column name or list of column names
将左侧 DataFrame 按列分组，并将其与右侧 DataFrame 逐段合并。

- **right_by**：column name or list of column names
将左侧 DataFrame 按列分组，并将其与右侧 DataFrame 逐段合并。

- **fill_method** : {‘ffill’, None}, default None
填充缺失值的函数

- **suffixes**：2-length sequence (tuple, list, …)
设置重复列名的后缀。

- **how**：{‘left’, ‘right’, ‘outer’, ‘inner’}, default ‘outer’
连接方式。
left：左连接，按照左边 DataFrame 的合并键合并。
right：右连接，按照右边 DataFrame 的合并键合并。
outer：外连接，按照两边 DataFrame 合并键的并集合并。
inner：内连接，按照两边 DataFrame 合并键的交集合并。

## 返回值
- **merged**：DataFrame
 如果输出类型是 DataFrame 的子类，则输出类型将与“left”相同。

## pandas.merge_ordered 类似函数
- **pandas.merge**

- **pandas.merge_asof**

## 例子
```python
df_l = pd.DataFrame({'k': ['K0', 'K1', 'K1', 'K2'], 'lv': [1, 2, 3, 4], 's': ['a', 'b', 'c', 'd']})
df_r = pd.DataFrame({'k': ['K1', 'K2', 'K4'], 'rv': [1, 2, 3]})

print(df_l)
print()
print(df_r)

输出：
    k  lv  s
0  K0   1  a
1  K1   2  b
2  K1   3  c
3  K2   4  d

    k  rv
0  K1   1
1  K2   2
2  K4   3
```

最简单的，直接用 k 列（重复的 columns）合并
```python
pd.merge_ordered(left=df_l, right=df_r)

输出：
    k   lv    s   rv
0  K0  1.0    a  NaN
1  K1  2.0    b  1.0
2  K1  3.0    c  1.0
3  K2  4.0    d  2.0
4  K4  NaN  NaN  3.0
```

fill_method 属性填充缺失值
```python
pd.merge_ordered(left=df_l, right=df_r, fill_method='ffill')

输出：
    k  lv  s   rv
0  K0   1  a  NaN
1  K1   2  b  1.0
2  K1   3  c  1.0
3  K2   4  d  2.0
4  K4   4  d  3.0
```