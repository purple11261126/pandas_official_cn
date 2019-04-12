# pandas.melt

pandas.**melt**(frame, id_vars=None, value_vars=None, var_name=None, value_name='value', col_level=None)

**melt** 是 **熔化** 的意思。

更改 Dataframe 格式，从按列显示特征，转换为按行显示特征。
官网解释是：Unpivots a DataFrame from wide format to long format

为 DataFrame 指定“标识符”（id_vars）和需要拆开查看的列（value_vars），value_vars 列原来的列名和数据值转化到每行。转变后，每行的格式为：标识符（id_vars），列名（value_vars），数据值。

## 参数说明

- **frame**：DataFrame。
要处理的数据集。

- **id_vars**：tuple, list, ndarray。
用作“标识符变量”的列，也就是不变的列。

- **value_vars**：tuple, list, ndarray。
要拆开的列，如果未指定，则使用未设置为 id_vars 的所有列。

- **var_name**：标量，默认“variable”。
设置 value_vars 列的 column。

- **value_name**：标量，默认“value”。
设置 value_vars 值列的 column。

- **col_level**：int, string。 
如果列是 MultiIndex，则使用此级别进行融合。

## 类似函数
- **DataFrame.melt**
- **pivot_table**
- **DataFrame.pivot**

## 例子：
```python
>>> df = pd.DataFrame({'日期': {0: '1.01', 1: '1.02', 2: '1.03'},
...                    '项目1': {0: 1, 1: 3, 2: 5},
...                    '项目2': {0: 2, 1: 4, 2: 6}})
>>> df

 	日期 	项目1 项目2
0 	1.01 	1 	2
1 	1.02 	3 	4
2 	1.03 	5 	6
```

```python
>>> pd.melt(df, id_vars=['日期'], value_vars=['项目1'])

	日期 	variable 	value
0 	1.01 	项目1 	1
1 	1.02 	项目1 	3
2 	1.03 	项目1 	5
```

```python
>>> pd.melt(df, id_vars=['日期'], value_vars=['项目1', '项目2'], var_name='myVarname', value_name='myValname')

 	日期 	myVarname 	myValname
0 	1.01 	项目1 		1
1 	1.02 	项目1 		3
2 	1.03 	项目1 		5
3 	1.01 	项目2 		2
4 	1.02 	项目2 		4
5 	1.03 	项目2 		6
```

如果列是 MultiIndex，需要设置 col_level 属性。
```python
>>> df.columns = [['日期1', '项目1', '项目2'], ['日期2', '子项1', '子项2']]
>>> df

	日期1 	项目1 	项目2
	日期2 	子项1 	子项2
0 	1.01 	1 		2
1 	1.02 	3 		4
2 	1.03 	5 		6
```

```python
>>> pd.melt(df, id_vars=['日期1'], value_vars=['项目2'], col_level=0)


	日期1 	variable 	value
0 	1.01 	项目2 		2
1 	1.02 	项目2 		4
2 	1.03 	项目2 		6
```

```python
>>> # 注意：MultiIndex 时，[('日期1', '日期2')] list 里面是 tuple。
>>> pd.melt(df, id_vars=[('日期1', '日期2')], value_vars=[('项目1', '子项1')])


	(日期1, 日期2) 	variable_0 	variable_1 	value
0 	1.01 			项目1 			子项1 		1
1 	1.02 			项目1 			子项1 		3
2 	1.03 			项目1 			子项1 		5
```

