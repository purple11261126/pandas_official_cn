# pandas.cut
pandas.**cut**(x, bins, right=True, labels=None, retbins=False, precision=3, include_lowest=False, duplicates='raise')

cut，切的意思。

分段函数，将数据按间隔分组，显示各个元素在哪组中。

cut 函数用于把数组排序并分段整理。也是用与从连续变量转换为分类变量。例如，cut 函数可以将年龄转换为年龄范围组。

## 参数说明

- **x**：array-like。
需要分段的数组，必须是一维数组。

- **bins**：int, sequence of scalars, or pandas.IntervalIndex
分段标准，也就是分段的间隔。bins 直译桶、箱子的意思，意会成分段的区间的意思。
int：传递整数，表示分段段数，会自动把数组分段为整数段，一般不会用到。
sequence of scalars：传递标量序列，则按照这个序列分段，这种是最常用的。
pandas.IntervalIndex：定义精确区间，这个貌似也不太常用。

- **right**：bool, default True
是否包含区间右侧，默认包含。例如分段[1, 2, 3, 4]，包含右侧则是 (1,2], (2,3], (3,4]。

- **lables**：array or bool, optional
设置间隔名称。可以传递名称数组。如果 lables=False，则直接返回当前区间的原始名称。当 bings 属性是 IntervalIndex 时，将忽略此参数。

- **retbins**:bool, default False
是否返回分组间隔（bins），默认是 False。当 bings 设置为 int 时比较有用。

- **precision**：int, default 3
精确到小数点几位。

- **include_lowest**： bool, default False
第一个间隔是否应该是包含在内的。是否包含区间左部，默认 False。

- **duplicates**：{default ‘raise’, ‘drop’}, optional
是否允许区间重叠。raise：不允许，会报错 ValueError。drop：允许，删除非唯一值。

## 返回值
- **out**：pandas.Categorical, Series, or ndarray
一个类似于数组的对象，表示 x 的每个值的相应区间。 如果指定了 labels，则返回对应的 label。

- **bins**：numpy.ndarray or IntervalIndex.
分隔后的区间，当指定 retbins=True 时返回。 如果 duplicates = drop，则 bin 将删除非唯一 bin。

## 相关函数

- **pandas.qcut**
基于分位数的分段函数

- **pandas.Categorical**

- **Series**

- **pandas.IntervalIndex**

## 例子
```python
# 年龄
ages = np.array([1,5,10,40,36,12,58,62,77,89,100,18,20,25,30,32])
ages

输出：
array([  1,   5,  10,  40,  36,  12,  58,  62,  77,  89, 100,  18,  20,
        25,  30,  32])
```

最简单的，把年龄分为 4 个区间，不指定间隔。
```python
pd.cut(x=ages, bins=4)

输出：
[(0.901, 25.75], (0.901, 25.75], (0.901, 25.75], (25.75, 50.5], (25.75, 50.5], ..., (0.901, 25.75], (0.901, 25.75], (0.901, 25.75], (25.75, 50.5], (25.75, 50.5]]
Length: 16
Categories (4, interval[float64]): [(0.901, 25.75] < (25.75, 50.5] < (50.5, 75.25] < (75.25, 100.0]]
```

还是分 4 个区间，指定间隔。
```python
pd.cut(x=ages, bins=[0, 25, 50, 75, 100])

输出：
[(0, 25], (0, 25], (0, 25], (25, 50], (25, 50], ..., (0, 25], (0, 25], (0, 25], (25, 50], (25, 50]]
Length: 16
Categories (4, interval[int64]): [(0, 25] < (25, 50] < (50, 75] < (75, 100]]
```

分 4 个区间，设置间隔名称。
```python
pd.cut(x=ages, bins=[0, 25, 50, 75, 100], labels=['25 以下', '25 - 50', '50 - 75', '75 - 100'])

输出：
[25 以下, 25 以下, 25 以下, 25 - 50, 25 - 50, ..., 25 以下, 25 以下, 25 以下, 25 - 50, 25 - 50]
Length: 16
Categories (4, object): [25 以下 < 25 - 50 < 50 - 75 < 75 - 100]
```