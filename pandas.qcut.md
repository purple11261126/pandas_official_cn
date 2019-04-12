# pandas.qcut
pandas.**qcut**(x, q, labels=None, retbins=False, precision=3, duplicates='raise')

qcut，Quantile cut 的缩写，基于分位数的分段函数。

## 参数说明
- **x**：1d ndarray or Series。
要分组的数组。

- **q**：integer or array of quantiles
 分位数。 10 表示十分位数，4 表示四分位数等。或者，分位数组，例如 四分位数[0, .25, .5, .75, 1.]。

- **labels**：array or boolean, default None
分组标签。

- **retbins**：bool, optional
是否返回分组间隔（bins）。 如果按标量分组，则可能很有用。

- **precision**：int, optional
精确到小数点几位。

- **duplicates**：{default ‘raise’, ‘drop’}, optional
是否允许区间重叠。raise：不允许，会报错 ValueError。drop：允许，删除非唯一值。

## 返回值
- **out**：pandas.Categorical, Series, or ndarray
一个类似于数组的对象，表示 x 的每个值的相应区间。 如果指定了 labels，则返回对应的 label。

- **bins**：numpy.ndarray or IntervalIndex.
分隔后的区间，当指定 retbins=True 时返回。 如果 duplicates = drop，则 bin 将删除非唯一 bin。

## 例子
```python
# 年龄
ages = np.array([1,5,10,40,36,12,58,62,77,89,100,18,20,25,30,32])
ages

输出：
array([  1,   5,  10,  40,  36,  12,  58,  62,  77,  89, 100,  18,  20,
        25,  30,  32])
```

最简单的，按照 4 分位数分组数据并返回 bins。
```python
pd.qcut(x=ages, q=4, labels=['1/4', '2/4', '3/4', '4/4'], retbins=True)

输出：
([1/4, 1/4, 1/4, 3/4, 3/4, ..., 2/4, 2/4, 2/4, 2/4, 3/4]
 Length: 16
 Categories (4, object): [1/4 < 2/4 < 3/4 < 4/4],
 array([  1. ,  16.5,  31. ,  59. , 100. ]))
```