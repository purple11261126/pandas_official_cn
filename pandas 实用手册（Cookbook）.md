pandas 官方 Cookbook 的中文翻译。

Cookbook 原意是 “食谱、菜单”，字面上和计算机毫无关系，其实在计算机领域，它还有另外一个意思：实用手册。讲解常用、经典的案例，和常见问题的解决办法。

<!--more-->

原文链接：<a href="http://pandas.pydata.org/pandas-docs/stable/cookbook.html#cookbook">http://pandas.pydata.org/pandas-docs/stable/cookbook.html#cookbook</a>

<br />

<h2>一、实用手册（Cookbook）</h2>

这是一个为 pandas 用户记录简短而甜美（short and sweet）例子和链接的知识库，我们鼓励其他用户一起填充这个文档。

文中添加了不少 Stack-Overflow 和 GitHub 上面的实例以方便读者阅读。

文中只有 Pandas（pd）和 Numpy（np）在引用时使用缩略写法，其余包均采用全称。

本文实例采用 Python3.4 编写，如果使用较早版本的 Python 可能需要自行修改代码。

<br />

<h2>二、用法惯例（Idioms）</h2>

这是一些简洁的 pandas 习惯用法。

<a href="https://stackoverflow.com/questions/17128302/python-pandas-idiom-for-if-then-else" title="对一列使用 if-then / if-then-else，然后分配给另一个或多个列">对一列使用 if-then / if-then-else，然后分配给另一个或多个列</a>：

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame( {'AAA' : [4,5,6,7], 'BBB' : [10,20,30,40],'CCC' : [100,50,-30,-50]})  # 使用字典创建 DataFrame
df

输出：
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50
</code></pre>

<h3>2.1 如果 - 则 （if-then）</h3>

<ul>
<li>“如果 - 则” 作用在一列</li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc[df.AAA >= 5, 'BBB'] = -1  # 按照条件（df.AAA >= 5）筛选出对应的数据（对应到 BBB 列），进行操作（赋值为 -1）。换一种说法是：如果 df.AAA >= 5，则对应的 BBB 列被赋值 -1
df

输出：
   AAA  BBB  CCC
0    4   10  100
1    5   -1   50
2    6   -1  -30
3    7   -1  -50
</code></pre>

<ul>
<li>“如果 - 则” 作用在两列</li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc[df.AAA >= 5, ['BBB', 'CCC']] = 555
df

输出：
   AAA  BBB  CCC
0    4   10  100
1    5  555  555
2    6  555  555
3    7  555  555
</code></pre>

<ul>
<li>增加另外一个不同的逻辑，相当于执行 -else</li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc[df.AAA < 5, ['BBB', 'CCC']] = 2000
df

输出：
   AAA   BBB   CCC
0    4  2000  2000
1    5   555   555
2    6   555   555
3    7   555   555
</code></pre>

<ul>
<li>设置一个 “遮罩”（mask），然后使用 pandas 的 where() 函数判断</li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df_mask = pd.DataFrame({'AAA' : [True] * 4, 'BBB' : [False] * 4,'CCC' : [True,False] * 2})  # 创建用于布尔判断的 DataFrame，也就是 “遮罩”
df_mask

输出：
      AAA    BBB    CCC
0    True  False   True
1    True  False  False
2    True  False   True
3    True  False  False

输入： df.where(df_mask, -1000)  # 将 df_mask 遮挡在 df 上，False 则赋值为 -1000。 突然想不通，为什么这个是反着的......

输出：
   AAA   BBB   CCC
0    4 -1000  2000
1    5 -1000 -1000
2    6 -1000   555
3    7 -1000 -1000
</code></pre>

<ul>
<li><a href="https://stackoverflow.com/questions/19913659/pandas-conditional-creation-of-a-series-dataframe-column" title="“如果 - 则 - 否则”（if-then-else）和 numpy 的 where() 函数一起使用">“如果 - 则 - 否则”（if-then-else）和 numpy 的 where() 函数一起使用</a></li>
</ul>

np.where() 完整参数：<code>where(condition, [x, y])</code>。根据 condition（条件），满足返回 x，不满足返回 y。

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'AAA' : [4,5,6,7], 'BBB' : [10,20,30,40],'CCC' : [100,50,-30,-50]})  # 创建 DataFrame
df

输出：
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

输入：
df['logic'] = np.where(df['AAA'] > 5, 'high', 'low')  # 使用 np.where() 函数为 df 增加 logic 列，如果 df['AAA'] > 5 为 ture，则赋值为 high，否则为 low
df

输出：
   AAA  BBB  CCC logic
0    4   10  100   low
1    5   20   50   low
2    6   30  -30  high
3    7   40  -50  high
</code></pre>

<h3>2.2 <strong>拆分</strong>（Splitting）</h3>

<ul>
<li><a href="https://stackoverflow.com/questions/14957116/how-to-split-a-dataframe-according-to-a-boolean-criterion" title="使用布尔条件拆分数据">使用布尔条件拆分数据</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'AAA' : [4,5,6,7], 'BBB' : [10,20,30,40],'CCC' : [100,50,-30,-50]})
df

输出：
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

输入：
dflow = df[df.AAA <= 5]  # 使用布尔条件拆分 DataFrame
dflow

输出：
   AAA  BBB  CCC
0    4   10  100
1    5   20   50

输入：
dfhigh = df[df.AAA > 5]
dfhigh

输出：
   AAA  BBB  CCC
2    6   30  -30
3    7   40  -50
</code></pre>

<h3>2.3 <strong>书写规范</strong>（Building Criteria）</h3>

<a href="https://stackoverflow.com/questions/15315452/selecting-with-complex-criteria-from-pandas-dataframe" title="选择和多列">选择和多列</a>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'AAA' : [4,5,6,7], 'BBB' : [10,20,30,40],'CCC' : [100,50,-30,-50]})
df

输出：
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50
</code></pre>

<ul>
<li>...and 逻辑且（没有设置返回一个 Series）</li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
newseries = df.loc[(df['BBB'] < 25) & (df['CCC'] >= -40), 'AAA']  # 逻辑且，用 & 表示
newseries

输出：
0    4
1    5
Name: AAA, dtype: int64
</code></pre>

<ul>
<li>...or 逻辑或（没有设置返回一个 Series）</li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
newseries = df.loc[(df['BBB'] > 25) | (df['CCC'] >= -40), 'AAA']  # 逻辑或，用 | 表示
newseries
</code></pre>

<ul>
<li>...or（赋值修改 Dataframe）</li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc[(df['BBB'] > 25) | (df['CCC'] >= 75), 'AAA'] = 0.1
df

输出：
   AAA  BBB  CCC
0  0.1   10  100
1  5.0   20   50
2  0.1   30  -30
3  0.1   40  -50
</code></pre>

<ul>
<li><a href="https://stackoverflow.com/questions/17758023/return-rows-in-a-dataframe-closest-to-a-user-defined-number" title="使用 argsort() 选择最接近特定值的数据行">使用 argsort() 选择最接近特定值的数据行</a></li>
</ul>

np.argsort() 完整参数：<code>np.argsort(a, axis=-1, kind='quicksort', order=None)</code>。对结果从小到大排序并返回索引号。

np.abs()：取绝对值

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'AAA' : [4,5,6,7], 'BBB' : [10,20,30,40],'CCC' : [100,50,-30,-50]})
df

输出：
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

输入：
aValue = 43.0  # 指定一个值
df.loc[(df.CCC - aValue).abs().argsort()]  # 按照接近特定值的程度从大到小排序数据行。这是个好办法，两个数相减，差的绝对值越小，说明两个数越相近。

输出：
   AAA  BBB  CCC
1    5   20   50
0    4   10  100
2    6   30  -30
3    7   40  -50
</code></pre>

<ul>
<li><a href="https://stackoverflow.com/questions/21058254/pandas-boolean-operation-in-a-python-list/21058331" title="使用二进制运算符动态减少条件列表">使用二进制运算符动态减少条件列表</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'AAA' : [4,5,6,7], 'BBB' : [10,20,30,40],'CCC' : [100,50,-30,-50]})
df

输出：
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

输入：
Crit1 = df.AAA <= 5.5
Crit2 = df.BBB == 10.0
Crit3 = df.CCC > -40.0
</code></pre>

一段硬编码

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
AllCrit = Crit1 & Crit2 & Crit3
</code></pre>

也可以使用动态生成的标准列表完成

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入:
CritList = [Crit1,Crit2,Crit3]
AllCrit = functools.reduce(lambda x,y: x & y, CritList)
df[AllCrit]

输出：
   AAA  BBB  CCC
0    4   10  100
</code></pre>

<br />

<h2>三、 <strong>选择</strong>（Selection）</h2>

<h3>3.1 <strong>DataFrame</strong></h3>

点击查看：<a href="http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing" title="indexing">indexing</a> 文档

<ul>
<li><a href="https://stackoverflow.com/questions/14725068/pandas-using-row-labels-in-boolean-indexing" title="使用行标签和值作为条件">使用行标签和值作为条件</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'AAA' : [4,5,6,7], 'BBB' : [10,20,30,40],'CCC' : [100,50,-30,-50]})
df

输出：
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

输入：
df[(df.AAA <= 6) & (df.index.isin([0,2,4]))]

输出：
   AAA  BBB  CCC
0    4   10  100
2    6   30  -30
</code></pre>

<ul>
<li><a href="https://github.com/pandas-dev/pandas/issues/2904" title="标签（loc）切片和位置（iloc）切片">标签（loc）切片和位置（iloc）切片</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
data = {'AAA' : [4,5,6,7], 'BBB' : [10,20,30,40],'CCC' : [100,50,-30,-50]}
df = np.Dataframe(data = data, index = ['foo','bar','boo','kar'])
df

输出：
     AAA  BBB  CCC
foo    4   10  100
bar    5   20   50
boo    6   30  -30
kar    7   40  -50
</code></pre>

两个显式切片的方法，第三个例子是一般的做法：

<ol>
<li>位置选择（Python 切片风格：不包含末尾元素）</li>
<li>标签选择（非 Python 切片风格：包含末尾元素）</li>
<li>一般的（切片样式: 取决于切片是否包含标签或位置）</li>
</ol>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc['bar':'kar']  # 标签选择

输出：
     AAA  BBB  CCC
bar    5   20   50
boo    6   30  -30
kar    7   40  -50

输入：
df.iloc[0 : 3]  # 位置选择

输出：
     AAA  BBB  CCC
foo    4   10  100
bar    5   20   50
boo    6   30  -30

输入：
df.loc['bar':'kar']  # 一般的做法

输出：
     AAA  BBB  CCC
bar    5   20   50
boo    6   30  -30
kar    7   40  -50
</code></pre>

<ul>
<li>当 index 从非零开始或者由非单位增量组成时，就容易产生歧义（所以尽量不要这样）</li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df2 = pd.DataFrame(data=data,index=[1,2,3,4])  # index 被设置成从 1 开始，而不是 0
df2.iloc[1:3] # 位置选择

输出：
   AAA  BBB  CCC
2    5   20   50
3    6   30  -30

输入：
df2.loc[1:3] # 标签选择

输出：
   AAA  BBB  CCC
1    4   10  100
2    5   20   50
3    6   30  -30
</code></pre>

<ul>
<li><a href="https://stackoverflow.com/questions/14986510/picking-out-elements-based-on-complement-of-indices-in-python-pandas" title="使用取反运算符（~）取掩码的补数">使用取反运算符（~）取掩码的补数</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'AAA' : [4,5,6,7], 'BBB' : [10,20,30,40], 'CCC' : [100,50,-30,-50]})
df

输出：
   AAA  BBB  CCC
0    4   10  100
1    5   20   50
2    6   30  -30
3    7   40  -50

输入：
df[~((df.AAA <= 6) & (df.index.isin([0,2,4])))]

输出：
   AAA  BBB  CCC
1    5   20   50
3    7   40  -50
</code></pre>

<h3>3.2 <strong>面板</strong> （Panels）</h3>

<ul>
<li><a href="https://stackoverflow.com/questions/15364050/extending-a-pandas-panel-frame-along-the-minor-axis" title="通过换用扩展面板框架, 添加新维度, 并将其移回原始维度">通过换用扩展面板框架, 添加新维度, 并将其移回原始维度</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
rng = pd.data_range('1/1/2013', periods=100, freq='D')  # 创建时间序列，index
data = np.random.randn(100, 4)  # data
cols = ['A','B','C','D']  # column
df1, df2, df3 = pd.DataFrame(data, rng, cols), pd.DataFrame(data, rng, cols), pd.DataFrame(data, rng, cols)  # 创建三个 DataFrame
pf = pd.Panel({'df1' : df1, 'df2' : df2, 'df3' : df3})  # 创建 Panel
df

输出：
<class 'pandas.core.panel.Panel'>
Dimensions: 3 (items) x 100 (major_axis) x 4 (minor_axis)
Items axis: df1 to df3
Major_axis axis: 2013-01-01 00:00:00 to 2013-04-10 00:00:00
Minor_axis axis: A to D

输入：
pf.loc[:, :, 'F'] = pd.DataFrame(data, rng, cols)  # 为 pf 增加新维度
pf

输出：
<class 'pandas.core.panel.Panel'>
Dimensions: 3 (items) x 100 (major_axis) x 5 (minor_axis)
Items axis: df1 to df3
Major_axis axis: 2013-01-01 00:00:00 to 2013-04-10 00:00:00
Minor_axis axis: A to F
</code></pre>

<a href="stackoverflow.com/questions/14650341/boolean-mask-in-pandas-panel" title="使用 np.where 遮罩一个面板，然后重建这个面板和新的遮罩值">使用 np.where 遮罩一个面板，然后重建这个面板和新的遮罩值</a>

<h3>3.3 <strong>新的列</strong>（New Columns）</h3>

<ul>
<li><a href="stackoverflow.com/questions/16575868/efficiently-creating-additional-columns-in-a-pandas-dataframe-using-map" title="使用 applymap() 高效、动态地创建新列">使用 applymap() 高效、动态地创建新列</a></li>
</ul>

applymap() 完整参数：<code>df.applymap(func)</code>。使用函数（func）处理 df 中的每一个元素，并返回一个 Dataframe。

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'AAA' : [1,2,1,3], 'BBB' : [1,1,2,2], 'CCC' : [2,1,3,1]})
df

输出：
   AAA  BBB  CCC
0    1    1    2
1    2    1    1
2    1    2    3
3    3    2    1

输入：
source_cols = df.columns  # 或者一些别的子集也可以
new_cols = [(str(x) + '_cat') for x in source_cols]  # 新的 columns
categories = {1 : 'Alpha', 2 : 'Beta', 3 : 'Charlie' }  # 类似于指定分类的名称
df[new_cols] = df[soruce_cols].applymap(categories.get)  # 创建多个列。
df

输出：
   AAA  BBB  CCC  AAA_cat BBB_cat  CCC_cat
0    1    1    2    Alpha   Alpha     Beta
1    2    1    1     Beta   Alpha    Alpha
2    1    2    3    Alpha    Beta  Charlie
3    3    2    1  Charlie    Beta    Alpha
</code></pre>

<ul>
<li><a href="stackoverflow.com/questions/23394476/keep-other-columns-when-using-min-with-groupby" title="使用 groupby 和 min() 找到最小值，并保持其他列不变">使用 groupby 和 min() 找到最小值，并保持其他列不变</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'AAA' : [1,1,1,2,2,2,3,3], 'BBB' : [2,1,3,4,5,1,2,3]})
df

输出：
   AAA  BBB
0    1    2
1    1    1
2    1    3
3    2    4
4    2    5
5    2    1
6    3    2
7    3    3
</code></pre>

方法一：使用 idxmin() 得到最小值的 index

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc[df.groupby('AAA')['BBB'].idxmin()]  # 分组 AAA 列，映射到 BBB 列，找出每个分组中 BBB 的最小值

输出：
   AAA  BBB
1    1    1
5    2    1
6    3    2
</code></pre>

方法二：从小到大排序，然后取得第一个元素

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.sort_values(by="BBB").groupby("AAA", as_index=False).first()

输出：
   AAA  BBB
0    1    1
1    2    1
2    3    2
</code></pre>

注意：两个方法结果相同但索引不同

<br />

<h2>四、 <strong>层级索引</strong>（MultiIndexing）</h2>

点击查看：<a href="pandas.pydata.org/pandas-docs/stable/advanced.html#advanced-hierarchical" title="multindexing">multindexing</a> 文档

<ul>
<li><a href="stackoverflow.com/questions/14916358/reshaping-dataframes-in-pandas-based-on-column-labels" title="使用标签框架创建层级索引">使用标签框架创建层级索引</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'row' : [0,1,2], 'One_X' : [1.1,1.1,1.1], 'One_Y' : [1.2,1.2,1.2], 'Two_X' : [1.11,1.11,1.11], 'Two_Y' : [1.22,1.22,1.22]})
df

输出：
   One_X  One_Y  Two_X  Two_Y  row
0    1.1    1.2   1.11   1.22    0
1    1.1    1.2   1.11   1.22    1
2    1.1    1.2   1.11   1.22    2

输入：
df = df.set_index('row')  # 设置 index，将 row 列设置为 index
df

输出：
One_X One_Y Two_X Two_Y
row
0 1.1 1.2 1.11 1.22
1 1.1 1.2 1.11 1.22
2 1.1 1.2 1.11 1.22

输入：
df.columns = pd.MultiIndex.from_tuples([tuple(c.split('_')) for c in df.columns])  # 设置多层 columns
df

输出：
     One        Two
       X    Y     X     Y
row
0    1.1  1.2  1.11  1.22
1    1.1  1.2  1.11  1.22
2    1.1  1.2  1.11  1.22

输入：
df = df.stack(0).reset_index(1) # 堆栈（stack）和复位（Reset）
df

输出：
    level_1     X     Y
row
0       One  1.10  1.20
0       Two  1.11  1.22
1       One  1.10  1.20
1       Two  1.11  1.22
2       One  1.10  1.20
2       Two  1.11  1.22

输入：
df.columns = ['Sample','All_X','All_Y'] # 修改 columns（注意：上面结果中的标签 “level_1” 是自动添加的）
df

输出：
    Sample  All_X  All_Y
row
0      One   1.10   1.20
0      Two   1.11   1.22
1      One   1.10   1.20
1      Two   1.11   1.22
2      One   1.10   1.20
2      Two   1.11   1.22
</code></pre>

<h3>4.1 <strong>算法</strong>（Arithmetic）</h3>

<ul>
<li><a href="https://stackoverflow.com/questions/19501510/divide-entire-pandas-multiindex-dataframe-by-dataframe-variable/19502176#19502176" title="执行算法和一个层级索引需要广播">执行算法和一个层级索引需要广播</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
cols = pd.MultiIndex.from_tuples([(x,y) for x in ['A','B','C'] for y in ['O','I']])
df = pd.DataFrame(np.random.randn(2,6),index=['n','m'],columns=cols)
df

输出：
          A                   B                   C
          O         I         O         I         O         I
n  1.920906 -0.388231 -2.314394  0.665508  0.402562  0.399555
m -1.765956  0.850423  0.388054  0.992312  0.744086 -0.739776

输入：
df = df.div(df['C'],level=1)
df

输出：
          A                   B              C
          O         I         O         I    O    I
n  4.771702 -0.971660 -5.749162  1.665625  1.0  1.0
m -2.373321 -1.149568  0.521518 -1.341367  1.0  1.0
</code></pre>

<h3>4.2 <strong>切片</strong>（Slicing）</h3>

<ul>
<li><a href="https://stackoverflow.com/questions/12590131/how-to-slice-multindex-columns-in-pandas-dataframes" title="使用 xs() 进行层级索引切片">使用 xs() 进行层级索引切片</a></li>
</ul>

df.xs() 完整参数：<code>df.xs(key, axis=0, level=None, drop_level=True)</code>。从 Series / DataFrame 返回一个横截面（行或列）。

key：columns 或者 index

axis：

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
coords = [('AA','one'),('AA','six'),('BB','one'),('BB','two'),('BB','six')]
index = pd.MultiIndex.from_tuples(coords)
df = pd.DataFrame([11,22,33,44,55],index,['MyData'])
df

输出：
        MyData
AA one      11
   six      22
BB one      33
   two      44
   six      55
</code></pre>

取得横截面的第一行和第一列的 index

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.xs('BB',level=0,axis=0)  # 注意： level（行）和 axis（列）是可选的，默认为 0

输出：
     MyData
one      33
two      44
six      55
</code></pre>

现在是第二行的第一列

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.xs('six',level=1,axis=0)

输出：
    MyData
AA      22
BB      55
</code></pre>

层级索引切片 方法2：

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
index = list(itertools.product(['Ada','Quinn','Violet'],['Comp','Math','Sci']))  # 关于 itertools，可参考：http://python.jobbole.com/87455/
headr = list(itertools.product(['Exams','Labs'],['I','II']))
indx = pd.MultiIndex.from_tuples(index,names=['Student','Course'])
cols = pd.MultiIndex.from_tuples(headr)
data = [[70+x+y+(x*y)%3 for x in range(4)] for y in range(9)]
df = pd.DataFrame(data,indx,cols)
df

输出：
               Exams     Labs    
                   I  II    I  II
Student Course                   
Ada     Comp      70  71   72  73
        Math      71  73   75  74
        Sci       72  75   75  75
Quinn   Comp      73  74   75  76
        Math      74  76   78  77
        Sci       75  78   78  78
Violet  Comp      76  77   78  79
        Math      77  79   81  80
        Sci       78  81   81  81

输入：
All = slice(None)  # 切片选择所有元素，相当于 “:”。参考：http://www.runoob.com/python/python-func-slice.html
df.loc['Violet']

输出：
       Exams     Labs    
           I  II    I  II
Course                   
Comp      76  77   78  79
Math      77  79   81  80
Sci       78  81   81  81

输入：
df.loc[(All, 'Math'), All]  # 选出所有 Math

输出：
               Exams     Labs    
                   I  II    I  II
Student Course                   
Ada     Math      71  73   75  74
Quinn   Math      74  76   78  77
Violet  Math      77  79   81  80

输入：
df.loc[(slice('Ada','Quinn'),'Math'),All]

输出：
               Exams     Labs    
                   I  II    I  II
Student Course                   
Ada     Math      71  73   75  74
Quinn   Math      74  76   78  77

输入：
df.loc[(All,'Math'),('Exams')]

输出：
                 I  II
Student Course        
Ada     Math    71  73
Quinn   Math    74  76
Violet  Math    77  79

输入：
df.loc[(All,'Math'),(All,'II')]

输出：
               Exams Labs
                  II   II
Student Course           
Ada     Math      73   74
Quinn   Math      76   77
Violet  Math      79   80
</code></pre>

<br>

<h3>4.3 排序（Sorting）</h3>

<ul>
<li><a href="https://stackoverflow.com/questions/14733871/multi-index-sorting-in-pandas" title="通过指定列或有序列表，排序层级索引">通过指定列或有序列表，排序层级索引</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.sort_values(by=('Labs', 'II'), ascending=False)

输出：
               Exams     Labs    
                   I  II    I  II
Student Course                   
Violet  Sci       78  81   81  81
        Math      77  79   81  80
        Comp      76  77   78  79
Quinn   Sci       75  78   78  78
        Math      74  76   78  77
        Comp      73  74   75  76
Ada     Sci       72  75   75  75
        Math      71  73   75  74
        Comp      70  71   72  73
</code></pre>

<br>

<h3>4.4 行（Levels）</h3>

<br>

<h3>4.5 panelnd</h3>

查看 <a href="http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dsintro-panelnd" title="panelnd">panelnd</a> 文档

<br />

<h2>五、缺失的数据（Missing Data）</h2>

查看 <a href="http://pandas.pydata.org/pandas-docs/stable/missing_data.html#missing-data" title="miss data">miss data</a> 文档

<ul>
<li>向前填充并逆序一个时间序列</li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame(np.random.randn(6,1), index=pd.date_range('2013-08-01', periods=6, freq='B'), columns=list('A'))
df.loc[df.index[3], 'A'] = np.nan
df

输出：
                   A
2013-08-01 -1.054874
2013-08-02 -0.179642
2013-08-05  0.639589
2013-08-06       NaN
2013-08-07  1.906684
2013-08-08  0.104050

输入：
df.reindex(df.index[::-1]).ffill()

输出：
                   A
2013-08-08  0.104050
2013-08-07  1.906684
2013-08-06  1.906684
2013-08-05  0.639589
2013-08-02 -0.179642
2013-08-01 -1.054874
</code></pre>

<br>

<h3>5.1 原地操作（Replace）</h3>

<br />

<h2>六、分组（Grouping）</h2>

查看 <a href="http://pandas.pydata.org/pandas-docs/stable/groupby.html#groupby" title="grouping">grouping</a> 文档

<a href="https://stackoverflow.com/questions/15322632/python-pandas-df-groupby-agg-column-reference-in-agg" title="分组基础和应用">分组基础和应用</a>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'animal': 'cat dog cat fish dog cat cat'.split(),
                    'size': list('SSMMMLL'),
                    'weight': [8, 10, 11, 1, 20, 12, 12],
                    'adult' : [False] * 5 + [True] * 2})
df

输出：
   adult animal size  weight
0  False    cat    S       8
1  False    dog    S      10
2  False    cat    M      11
3  False   fish    M       1
4  False    dog    M      20
5   True    cat    L      12
6   True    cat    L      12

输入：
df.groupby('animal').apply(lambda subf: subf['size'][subf['weight'].idxmax()])

输出：
animal
cat     L
dog     M
fish    M
dtype: object
</code></pre>

<ul>
<li><a href="https://stackoverflow.com/questions/14734533/how-to-access-pandas-groupby-dataframe-by-key" title="使用 get_group（）">使用 get_group（）</a></li>
</ul>

<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
gb = df.groupby(['animal'])
gb.get_group('cat')

输出：
   adult animal size  weight
0  False    cat    S       8
2  False    cat    M      11
5   True    cat    L      12
6   True    cat    L      12
</code></pre>

<br />

<h2>七、 <strong>时间序列</strong>（Timeseries）</h2>

<br />

<h2>八、 <strong>合并</strong>（Merge）</h2>

<br />

<h2>九、 <strong>绘图</strong>（Ploting）</h2>

<br />

<h2>十、 <strong>文件操作</strong>（Data In/Out）</h2>

<br />

<h2>十一、 <strong>计算</strong>（Computation）</h2>

<br />

<h2>十二、 <strong>时间增量</strong>（Timedeltas）</h2>

<br />

<h2>十三、 <strong>混叠轴名称</strong>(Aliasing Axis Names)</h2>

<br />

<h2>十四、 <strong>创建实例数据</strong>（Creating Example Data）</h2>