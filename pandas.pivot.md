# pandas.pivot
pandas.**pivot**(data, index=None, columns=None, values=None)

**pivot** 是 **转动** 的意思。

返回通过 index 或 column 重构的 DataFrame，通俗地讲就是合并同类项，不过一般 **DataFrame.pivot_table()** 使用更广泛。

根据列值重塑数据（生成“数据透视表”）。 使用指定索引/列中的唯一值来形成生成的DataFrame的轴。 此函数不支持数据聚合，多个值将导致列中的MultiIndex。

## 参数说明

- **data**：DataFrame。
需要重构的数据集。

- **index**：string、object。
重构后数据集的 index，如果 None，则为现有的 index。

- **columns**：string、object。
重构后数据集的 columns。

- **values**：string、object、columns list。
重构后数据集的 values。 如果未指定，将使用所有剩余列，结果将具有分层索引列。
从 0.23.0 版本开始，也能接收列名 list。

## 返回值

- **DataFrame**
重构后的 DataFrame。

## 额外说明

- **ValueError**
如果重构后的数据集中 index、columns 存在重复的组合，则无法判断选择哪个，pivot() 函数会报错 ValueError，这时应该使用 DataFrame.pivot_table() 函数避免这个问题。

## pandas.pivot 类似函数

- **DataFrame.pivot_table()**
可以看做 pivot 的扩展，可以结局额外说明中的 ValueError 报错问题。

- **DataFrame.unstack()**
基于索引值而不是列进行透视。

## 例子
```python
>>> df = pd.DataFrame({'foo': ['one', 'one', 'one', 'two', 'two',
                            'two'],
                    'bar': ['A', 'B', 'C', 'A', 'B', 'C'],
                    'baz': [1, 2, 3, 4, 5, 6],
                    'zoo': ['x', 'y', 'z', 'q', 'w', 't']})
>>> df


	foo 	bar 	baz 	zoo
0 	one 	A 	1 		x
1 	one 	B 	2 		y
2 	one 	C 	3 		z
3 	two 	A 	4 		q
4 	two 	B 	5 		w
5 	two 	C 	6 		t
```

```python
>>> df.pivot(index='foo', columns='baz', values='zoo')


baz 	1 	2 	3 	4 	5 	6
foo 						
one 	x 	y 	z 	NaN 	NaN 	NaN
two 	NaN 	NaN 	NaN 	q 	w 	t
```