10 Minutes to pandas（十分钟入门 pandas）的中文翻译，是 pandas 官方入门文档，版本为0.22.0。

自己增加了一些解释，希望让内容更加易懂。

文中代码使用 Python2.7，在 jupyter notebook 环境运行。

<!--more-->

简单介绍 pandas 的用法，更多信息点击 <a title="Cookbook" href="http://naofu.net/archives/24">Cookbook</a>（正在翻译） 查看。

<a title="原文地址" href="http://pandas.pydata.org/pandas-docs/stable/10min.html">原文地址</a>

 
<h2>一、<strong>导入模块</strong></h2>
pandas 通常和 numpy、matplotlib.pyplot 在一起配合使用。

numpy（np） 集成了常用的数学计算库。

pandas（pd） 是在 numpy 基础上发展而来的一套超级健全的数据分析工具包，是 Python 数据分析的必会工具。

matplotlib.pyplot（plt） 负责数据可视化。

另外，pd、np、plt 也是大家约定俗成的简称，所以我们也这么用。
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
</code></pre>
 
<h2>二、<strong>创建对象</strong></h2>
更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/dsintro.html#dsintro">Data Structure Intro section</a>

pandas 包含四种数据结构，这里介绍最常用的两种：Series 和 DataFrame。
<h3>2.1、<strong>创建 Series</strong></h3>
<a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.html#pandas.Series">Series</a> 简单理解成是 Excel 中的一列数据就可以了，事实上也确实很类似。

先来看一下 Series 的完整参数：

<code>pd.Series(data=None, index=None, dtype=None, name=None, copy=False, fastpath=False)</code>

常用的参数有：data（数据）、index（行索引）、dtype（数据类型）、name（名称）
<ul>
 	<li>传递列表，创建一个默认为整数索引的 Series</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
s = pd.Series([1,3,5,np.nan,6,8])
s

输出：
0    1.0
1    3.0
2    5.0
3    NaN
4    6.0
5    8.0
dtype: float64
</code></pre>
可以看到，在结果中，左边为索引（index），右边为对应的值（values），很类似 Excel 中的一列数据。

向 Series 传递多个数值，需要使用数组类型（List 列表），其中可以包含空值（NaN_not a number）

index 默认从 0 开始，而且是自动创建的。

最后一行是对这个 Series 的说明，这个例子中只有数据类型为 float64，如果设置了其他参数，比如 name，这里会一起显示出来。
<ul>
 	<li>传递字典，创建 Series，字典中的键值对，会自动对应为 index 和 value。</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
dic = {2001: 17.8, 2002: 20.1, 2003: 16.5}
ser_dic = pd.Series(dic)
ser_dic

输出：
2001    17.8
2002    20.1
2003    16.5
dtype: float64
</code></pre>
<h3>2.2、<strong>创建 Dataframe</strong></h3>
非常简单，把几个 Series 横向拼接，就得到了 <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.html#pandas.DataFrame">Dataframe</a>。

Dataframe 完整参数：

<code>pd.DataFrame(data=None, index=None, columns=None, dtype=None, copy=False)</code>

常用参数有：data（数据）、index（行索引）、columns（列标签）、dtype（数据类型）
<ul>
 	<li>传递 numpy数组、时间索引（index）和列标签（columns）来创建 DataFrame</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
dates = pd.date_range('20130101', periods=6)  # 创建时间索引
df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD'))
df

输出：
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
</code></pre>
index 和 columns 都是从 0 开始，这里由于手动设置了这两个参数，所以看不到默认的位置编号，但依旧可以使用默认的位置标号。

<em>提示：这里创建的名为 df 的 Dataframe 会在本篇文档中会持续用到，不要忘了。</em>
<ul>
 	<li>传递字典来创建 Dataframe，字典中的对象会自动转换为 Series 类型</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df2 = pd.DataFrame({ 'A' : 1.,
   ....:             'B' : pd.Timestamp('20130102'),
   ....:             'C' : pd.Series(1,index=list(range(4)),dtype='float32'),
   ....:             'D' : np.array([3] * 4,dtype='int32'),
   ....:             'E' : pd.Categorical(["test","train","test","train"]),
   ....:             'F' : 'foo' })
df2

输出：
     A          B    C  D      E    F
0  1.0 2013-01-02  1.0  3   test  foo
1  1.0 2013-01-02  1.0  3  train  foo
2  1.0 2013-01-02  1.0  3   test  foo
3  1.0 2013-01-02  1.0  3  train  foo
</code></pre>
可以看到 Dataframe 自动将字典 A、B、F 三个键的值复制了4次（广播），用于对齐数据个数，并且把所有键值对转换为 Series 类型。
<ul>
 	<li>Dataframe 允许每一列数据是不同的数据类型，使用 <code>df.dtypes</code> 命令查看</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df2.dtypes

输出：
A           float64
B    datetime64[ns]
C           float32
D             int32
E          category
F            object
dtype: object
</code></pre>
<ul>
 	<li>如果你在IPython环境下，可以使用 TAB 键查看 Dataframe 所有属性（就是按 TAB 的自动补齐功能）</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df2.

显示提示：
df2.A                  df2.bool
df2.abs                df2.boxplot
df2.add                df2.C
df2.add_prefix         df2.clip
df2.add_suffix         df2.clip_lower
df2.align              df2.clip_upper
df2.all                df2.columns
df2.any                df2.combine
df2.append             df2.combine_first
df2.apply              df2.compound
df2.applymap           df2.consolidate
df2.as_blocks          df2.convert_objects
df2.asfreq             df2.copy
df2.as_matrix          df2.corr
df2.astype             df2.corrwith
df2.at                 df2.count
df2.at_time            df2.cov
df2.axes               df2.cummax
df2.B                  df2.cummin
df2.between_time       df2.cumprod
df2.bfill              df2.cumsum
df2.blocks             df2.D
</code></pre>
如你所见， A、B、C 和 D 这些属性都是按 tab 键提示出的（它们都是我们自己给 df2 创建的，不是 Python 默认的）。E 其实也在里面，但为了为了简洁起见, df2.D 之后的属性这里不做展示，大家自己尝试一下吧。
<h3><strong>2.3、用 ? 查看对象说明</strong></h3>
在 IPython 或者 jupyter notebook 环境下，可以使用 ? 查看对象说明。

比如上面创建时间索引的例子，我们想查看 pd.date_range() 这个函数的详细说明，就可以这样：
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
?pd.date_range 或者 ?pd.date_range()

输出：
Signature: pd.date_range(start=None, end=None, periods=None, freq='D', tz=None, normalize=False, name=None, closed=None, **kwargs)
Docstring:
Return a fixed frequency datetime index, with day (calendar) as the default frequency

Parameters
......
</code></pre>
Signature：函数签名，这里可以看到该函数包含所有参数时的样子。

Docstring：说明文档，函数说明。

Parameters：参数详解，每个参数的详细说明。

从 Docstring 我们知道 pd.date_range() 能以“天”为单位，返回我们需要的一段日期索引。
<h2>三、<strong>查看数据</strong></h2>
更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/basics.html#basics">Basics section</a>
<ul>
 	<li>预览开头和结尾的数据</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.head()  # 预览开头数据，默认5行

输出：
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401

输入：
df.tail(3)  # 预览末尾数据，需要指定多少行，这里是3行

输出：
                   A         B         C         D
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
</code></pre>
<ul>
 	<li>查看各项属性</li>
</ul>
常见属性：index（行索引）、values（数据，Series）、dtype（数据类型，Series）、name（列名，Series）、dtypes（数据类型，Dataframe）、columns（列标签，Dataframe）。

直接使用 <code>.属性名</code> 的方式查看。
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.index  # 查看索引

输出：
DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04','2013-01-05', '2013-01-06'],dtype='datetime64[ns]', freq='D')

输入：
df.columns  # 查看列

输出：
Index(['A', 'B', 'C', 'D'], dtype='object')

输入：
df.values  # 查看值

输出：
array([[ 0.4691, -0.2829, -1.5091, -1.1356],
       [ 1.2121, -0.1732,  0.1192, -1.0442],
       [-0.8618, -2.1046, -0.4949,  1.0718],
       [ 0.7216, -0.7068, -1.0396,  0.2719],
       [-0.425 ,  0.567 ,  0.2762, -1.0874],
       [-0.6737,  0.1136, -1.4784,  0.525 ]])
</code></pre>
<ul>
 	<li>汇总性统计</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.describe()

输出：
              A         B         C         D
count  6.000000  6.000000  6.000000  6.000000  # 数据个数
mean   0.073711 -0.431125 -0.687758 -0.233103  # 平均值
std    0.843157  0.922818  0.779887  0.973118  # 标准差
min   -0.861849 -2.104569 -1.509059 -1.135632  # 最小值
25%   -0.611510 -0.600794 -1.368714 -1.076610  # 1/4 值
50%    0.022070 -0.228039 -0.767252 -0.386188  # 1/2 值
75%    0.658444  0.041933 -0.034326  0.461706  # 3/4 值
max    1.212112  0.567020  0.276232  1.071804  # 最大值
</code></pre>
<ul>
 	<li>转置（Transposing）</li>
</ul>
转置数据就是把 Dataframe 的行列位置互换
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.T

输出：
   2013-01-01  2013-01-02  2013-01-03  2013-01-04  2013-01-05  2013-01-06
A    0.469112    1.212112   -0.861849    0.721555   -0.424972   -0.673690
B   -0.282863   -0.173215   -2.104569   -0.706771    0.567020    0.113648
C   -1.509059    0.119209   -0.494929   -1.039575    0.276232   -1.478427
D   -1.135632   -1.044236    1.071804    0.271860   -1.087401    0.524988
</code></pre>
<ul>
 	<li>按轴排序</li>
</ul>
完整参数：<code>df.sort_index(axis=0, level=None, ascending=True, inplace=False, kind='quicksort', na_position='last', sort_remaining=True, by=None)</code>

axis：0（排序 index）、1（排序 columns）

asending：升序排列

inplace：原地排序
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.sort_index(axis=1, ascending=False)

输出：
                   D         C         B         A
2013-01-01 -1.135632 -1.509059 -0.282863  0.469112
2013-01-02 -1.044236  0.119209 -0.173215  1.212112
2013-01-03  1.071804 -0.494929 -2.104569 -0.861849
2013-01-04  0.271860 -1.039575 -0.706771  0.721555
2013-01-05 -1.087401  0.276232  0.567020 -0.424972
2013-01-06  0.524988 -1.478427  0.113648 -0.673690
</code></pre>
<ul>
 	<li>按值排序</li>
</ul>
完整参数：<code>df.sort_values(by, axis=0, ascending=True, inplace=False, kind='quicksort', na_position='last')</code>

by：必须指定排序哪一列或者行，列标签（columns）需要加引号，行标签（index）不需要加引号

axis：0（index，纵向排序）、1（columns，横向排序）

ascending：升序排列

inplace：原地排序
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.sort_values(by='B')  # 排序 B 列

输出：
                   A         B         C         D
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-06 -0.673690  0.113648 -1.478427  0.524988
2013-01-05 -0.424972  0.567020  0.276232 -1.087401
</code></pre>
 
<h2>四、<strong>选择数据</strong></h2>
提示：虽然使用标准的 Python/Numpy 表达式也可以很方便的处理数据，但我们还是推荐使用 pandas 特有的函数来完成工作项目，比如：.at， .iat， .loc， .iloc， .ix 。

更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing">Indexing and Selecting Data</a> 或者 <a href="http://pandas.pydata.org/pandas-docs/stable/advanced.html#advanced">MultiIndex / Advanced Indexing</a>
<h3>4.1、<strong>得到数据</strong></h3>
<ul>
 	<li>选择单个列，直接得到 Series，相当于 df.A</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df['A']

输出：
2013-01-01    0.469112
2013-01-02    1.212112
2013-01-03   -0.861849
2013-01-04    0.721555
2013-01-05   -0.424972
2013-01-06   -0.673690
Freq: D, Name: A, dtype: float64
</code></pre>
<ul>
 	<li>选择多个列</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df[['A', 'C']]</code></pre>
注意：需要用 List 存放想要的列标签，作为一个参数传递给 df，不是直接 df['A', 'C']。
<ul>
    <li>使用 [ ] 切片，选择行</li>
</ul>
<pre><code class="lang-python">输入：
df[0:3]  # 行索引切片

输出：
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804

输入：
df['20130102':'20130104']  #  行标签切片

输出：
                   A         B         C         D
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
</code></pre>
注意：

使用 索引（位置） 切片时，[0:3] 显示的是索引号为 0、1、2 的三行数据，不包含 3，也就是不包含末尾索引。

使用 标签（名称） 切片时，['20130102':'20130104'] 显示的是 20130102、20130103、20130104 三行数据，也就是包含全部标签。别搞混。
<h3>4.2、<strong>标签索引</strong>（.loc）</h3>
更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-label">Selection by Label</a>

.loc：location，通过标签名称选择 Dataframe 数据。

参数顺序（.loc 和 .iloc 相同）：<code>df.loc[index, columns]</code>
<ul>
 	<li>标签索引选择一行数据</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc[dates[0]]

输出：
A    0.469112
B   -0.282863
C   -1.509059
D   -1.135632
Name: 2013-01-01 00:00:00, dtype: float64
</code></pre>
可以看到，这里打印出了第 0 行数据，并且将数据转换为 Series，索引就是原来的列名。
<ul>
 	<li>标签索引选择多列数据</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc[:, dates['A', 'B']]

输出：
                   A         B
2013-01-01  0.469112 -0.282863
2013-01-02  1.212112 -0.173215
2013-01-03 -0.861849 -2.104569
2013-01-04  0.721555 -0.706771
2013-01-05 -0.424972  0.567020
2013-01-06 -0.673690  0.113648
</code></pre>
<ul>
 	<li>标签切片，包含两端</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc['20130102':'20130104', ['A','B']]  # 包含两端

输出：
                   A         B
2013-01-02  1.212112 -0.173215
2013-01-03 -0.861849 -2.104569
2013-01-04  0.721555 -0.706771
</code></pre>
<ul>
 	<li>减少返回对象的维度</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc['20130102', ['A', 'B']]

输入：
A    1.212112
B   -0.173215
Name: 2013-01-02 00:00:00, dtype: float64
</code></pre>
这样就只打印出 20130102 行上，A、B 两列数据，然后自动转换为 Series。
<ul>
 	<li>得到一个标准值</li>
</ul>
就是得到一个具体的数据，能够看到数据的完整值
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.loc[dates[0], 'A']

输出：
0.46911229990718628
</code></pre>
<ul>
 	<li>快速得到一个标准值（df.at）</li>
</ul>
效果和上面的例子相同
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.at[dates[0],'A']

输出：
0.46911229990718628
</code></pre>
<h3>4.3、<strong>整型位置索引</strong>（.iloc）</h3>
更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/indexing.html#indexing-integer">Selection by Position</a>

.iloc：int location，通过整型位置编号选择 Dataframe 数据。

参数顺序（.loc 和 .iloc 相同）：df.iloc[index, columns]
<ul>
 	<li>通过传递整型位置编号选择数据</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.iloc[3]  # 打印索引为 3 的行

输出：
A    0.721555
B   -0.706771
C   -1.039575
D    0.271860
Name: 2013-01-04 00:00:00, dtype: float64
</code></pre>
<ul>
 	<li>通过整型位置切片选择</li>
</ul>
整型位置切片（也就是索引切片）不包含末尾
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.iloc[3:5, 0:2]  # 不包含末尾元素

输出：
                   A         B
2013-01-04  0.721555 -0.706771
2013-01-05 -0.424972  0.567020
</code></pre>
<ul>
 	<li>通过整型位置列表选择</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.iloc[[1, 2, 4], [0, 2]]

输出：
                   A         C
2013-01-02  1.212112  0.119209
2013-01-03 -0.861849 -0.494929
2013-01-05 -0.424972  0.276232
</code></pre>
<ul>
 	<li>行切片</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.iloc[1:3, :]

输出：
                   A         B         C         D
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
</code></pre>
<ul>
 	<li>列切片</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.iloc[:, 1:3]

输出：
                   B         C
2013-01-01 -0.282863 -1.509059
2013-01-02 -0.173215  0.119209
2013-01-03 -2.104569 -0.494929
2013-01-04 -0.706771 -1.039575
2013-01-05  0.567020  0.276232
2013-01-06  0.113648 -1.478427
</code></pre>
<ul>
 	<li>得到一个标准值</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.iloc[1, 1]

输出：
-0.17321464905330858
</code></pre>
<ul>
 	<li>快速得到一个标准值（.iat）</li>
</ul>
和上面例子效果相同
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.iat[1, 1]

输出：
-0.17321464905330858
</code></pre>
<h3>4.4、<strong>布尔索引</strong></h3>
<ul>
 	<li>使用单个列的值来选择数据</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df[df.A > 0]

输出：
                   A         B         C         D
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
2013-01-02  1.212112 -0.173215  0.119209 -1.044236
2013-01-04  0.721555 -0.706771 -1.039575  0.271860
</code></pre>
<ul>
 	<li>在整个 Dataframe 中，筛选满足条件的值，不满足的填充 NaN</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df[df > 0]

输出：
                   A         B         C         D
2013-01-01  0.469112       NaN       NaN       NaN
2013-01-02  1.212112       NaN  0.119209       NaN
2013-01-03       NaN       NaN       NaN  1.071804
2013-01-04  0.721555       NaN       NaN  0.271860
2013-01-05       NaN  0.567020  0.276232       NaN
2013-01-06       NaN  0.113648       NaN  0.524988
</code></pre>
<ul>
 	<li>使用 <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.Series.isin.html#pandas.Series.isin">isin()</a> 函数进行筛选</li>
</ul>
isin() 顾名思义，是否在其中，返回 Ture、False。

完整参数：<code>df.isin(values)</code>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df2 = df.copy()  # df 复制给 df2
df2['E'] = ['one', 'one','two','three','four','three']  # df2 增加 E 列
df2

输出：
                   A         B         C         D      E
2013-01-01  0.469112 -0.282863 -1.509059 -1.135632    one
2013-01-02  1.212112 -0.173215  0.119209 -1.044236    one
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804    two
2013-01-04  0.721555 -0.706771 -1.039575  0.271860  three
2013-01-05 -0.424972  0.567020  0.276232 -1.087401   four
2013-01-06 -0.673690  0.113648 -1.478427  0.524988  three

输入：
df2[df2['E'].isin(['two', 'four'])]  # 使用 isin() 函数选择出 df2 的 E 列中，包含 two、four 的行

输出：
                   A         B         C         D     E
2013-01-03 -0.861849 -2.104569 -0.494929  1.071804   two
2013-01-05 -0.424972  0.567020  0.276232 -1.087401  four
</code></pre>
<h3>4.5、设置</h3>
<ul>
 	<li>设置一个新列，自动将数据按索引对齐</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
s1 = pd.Series([1, 2, 3, 4, 5, 6], index=pd.date_range('20130102', periods=6))
s1

输出：
2013-01-02    1
2013-01-03    2
2013-01-04    3
2013-01-05    4
2013-01-06    5
2013-01-07    6
Freq: D, dtype: int64

输入：
df['F'] = s1
</code></pre>
<ul>
 	<li>通过标签设置值</li>
</ul>
df.at 和 df.loc 类似，都是通过标签选择 Dataframe 数据，但 at 比 loc 速度快。
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.at[date[0], 'A'] = 0  # 0 行 A 列赋值为 0
</code></pre>
<ul>
 	<li>通过位置设置值</li>
</ul>
df.iat 和 df.iloc 类似，通过整型位置编号选择 Dataframe 数据，速度比 iloc 快。
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.iat[0, 1] = 0</code></pre>
<ul>
    <li>赋值 NumPy 数组</li>
</ul>
<pre><code class="lang-python">输入：
df.loc[:, 'D'] = np.array([5] * len(df))
</code></pre>
经过上面的设置以后，结果如下：
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">                   A         B         C  D    F
2013-01-01  0.000000  0.000000 -1.509059  5  NaN
2013-01-02  1.212112 -0.173215  0.119209  5  1.0
2013-01-03 -0.861849 -2.104569 -0.494929  5  2.0
2013-01-04  0.721555 -0.706771 -1.039575  5  3.0
2013-01-05 -0.424972  0.567020  0.276232  5  4.0
2013-01-06 -0.673690  0.113648 -1.478427  5  5.0
</code></pre>
再设置一下
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df2 = df.copy()  # 复制 df 给 df2
df2[df2 > 0] = -df2  # 将 df2 中所有正数取反，变为负数
df2

输出：
                   A         B         C  D    F
2013-01-01  0.000000  0.000000 -1.509059 -5  NaN
2013-01-02 -1.212112 -0.173215 -0.119209 -5 -1.0
2013-01-03 -0.861849 -2.104569 -0.494929 -5 -2.0
2013-01-04 -0.721555 -0.706771 -1.039575 -5 -3.0
2013-01-05 -0.424972 -0.567020 -0.276232 -5 -4.0
2013-01-06 -0.673690 -0.113648 -1.478427 -5 -5.0
</code></pre>
<h2>五、<strong>缺失值</strong></h2>
pandas 主要使用 np.nan 表示缺失值。默认情况下, 它不包括在计算中。

更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/missing_data.html#missing-data">Missing Data section</a>
<ul>
 	<li>reindex() 更改/添加/删除指定坐标轴上的索引，并返回数据的副本</li>
</ul>
reindex() 完整参数：<code>df.reindex(index=None, columns=None, **kwargs)</code>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df1 = df.reindex(index=date[0:4], columns=list(df.colunms)+['E'])  # 设置新的 index 和 columns，并新增 E 列
df1.loc[dates[0]:dates[1],'E'] = 1  # 为 E 列 前两行赋值 1
df1

输出：
                   A         B         C  D    F    E
2013-01-01  0.000000  0.000000 -1.509059  5  NaN  1.0
2013-01-02  1.212112 -0.173215  0.119209  5  1.0  1.0
2013-01-03 -0.861849 -2.104569 -0.494929  5  2.0  NaN
2013-01-04  0.721555 -0.706771 -1.039575  5  3.0  NaN
</code></pre>
<ul>
 	<li>dropna() 删除所有带丢失数据的行</li>
</ul>
dropna() 完整参数：<code>df.dropna(axis=0, how='any', thresh=None, subset=None, inplace=False)</code>

axis：0（index，横向删除，删除行）、1（columns，纵向删除，删除列）

how：如何删除，any（任何值为 nan 则删除该行或列）、all（所有值都为 nan 则删除该行或者列）

inplace：是否原地操作
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df1.dropna(how='any')

输出：
                   A         B         C  D    F    E
2013-01-02  1.212112 -0.173215  0.119209  5  1.0  1.0
</code></pre>
<ul>
 	<li>fillna() 填充缺失值</li>
</ul>
fillna() 完整参数：<code>df.fillna(value=None, method=None, axis=None, inplace=False, limit=None, downcast=None, **kwargs)</code>

value：填充值

method：

axis：

inplace：是否原地操作

limit：和 method 属性搭配使用的
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df1.fillna(value=5)

输出：
                   A         B         C  D    F    E
2013-01-01  0.000000  0.000000 -1.509059  5  5.0  1.0
2013-01-02  1.212112 -0.173215  0.119209  5  1.0  1.0
2013-01-03 -0.861849 -2.104569 -0.494929  5  2.0  5.0
2013-01-04  0.721555 -0.706771 -1.039575  5  3.0  5.0
</code></pre>
<ul>
 	<li>isnull() 查看哪些值为 nan，返回布尔遮罩</li>
</ul>
顾名思义，是否为空。使用 isnull() 查看值是否为 nan，返回结果以 True、False 表示。
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df1.isnull() 或者 pd.isnull(df1)

输出：
                A      B      C      D      F      E
2013-01-01  False  False  False  False   True  False
2013-01-02  False  False  False  False  False  False
2013-01-03  False  False  False  False  False   True
2013-01-04  False  False  False  False  False   True
</code></pre>
 
<h2>六、<strong>操作</strong></h2>
更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/basics.html#basics-binop">Basic section on Binary Ops</a>
<h3>6.1、<strong>统计</strong></h3>
统计数据之前，一般要先处理掉缺失值。
<ul>
 	<li>描述性统计</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.mean()  # 平均值，默认为每列的平均值

输出：
A   -0.004474
B   -0.383981
C   -0.687758
D    5.000000
F    3.000000
dtype: float64
</code></pre>
<ul>
 	<li>换一个轴执行相同的操作</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.mean(1)  # 每行的平均值

输出：
2013-01-01    0.872735
2013-01-02    1.431621
2013-01-03    0.707731
2013-01-04    1.395042
2013-01-05    1.883656
2013-01-06    1.592306
Freq: D, dtype: float64
</code></pre>
<ul>
 	<li>计算两个不同维度的对象时，pandas 会自动进行广播，对齐两个对象</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
s = pd.Series([1,3,5,np.nan,6,8], index=dates).shift(2)  # shift() 移动数据框
s

输出：
2013-01-01    NaN
2013-01-02    NaN
2013-01-03    1.0
2013-01-04    3.0
2013-01-05    5.0
2013-01-06    NaN
Freq: D, dtype: float64

输入：
df.sub(s, axis='index')  # df.sub() 元素相减。

输出：
                   A         B         C    D    F
2013-01-01       NaN       NaN       NaN  NaN  NaN
2013-01-02       NaN       NaN       NaN  NaN  NaN
2013-01-03 -1.861849 -3.104569 -1.494929  4.0  1.0
2013-01-04 -2.278445 -3.706771 -4.039575  2.0  0.0
2013-01-05 -5.424972 -4.432980 -4.723768  0.0 -1.0
2013-01-06       NaN       NaN       NaN  NaN  NaN
</code></pre>
<h3>6.2、<strong>apply() 函数</strong></h3>
<ul>
 	<li>使用函数处理数据</li>
</ul>
apply() 能够让 df 使用其他函数处理数据

完整参数：<code>df.apply(func, axis=0, broadcast=False, raw=False, reduce=None, args=(), **kwds)</code>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.apply(np.cumsum)  # np.cumsum 累计求和。就是第二项的结果等于第一项加第二项，依次累加。在 Dataframe 中是每列累加，也可以用在 List 中。

输出：
                   A         B         C   D     F
2013-01-01  0.000000  0.000000 -1.509059   5   NaN
2013-01-02  1.212112 -0.173215 -1.389850  10   1.0
2013-01-03  0.350263 -2.277784 -1.884779  15   3.0
2013-01-04  1.071818 -2.984555 -2.924354  20   6.0
2013-01-05  0.646846 -2.417535 -2.648122  25  10.0
2013-01-06 -0.026844 -2.303886 -4.126549  30  15.0

输入：
df.apply(lambda x: x.max() - x.min())  # 匿名函数求每列的最大值和最小值的差

输出：
A    2.073961
B    2.671590
C    1.785291
D    0.000000
F    4.000000
dtype: float64
</code></pre>
<h3>6.3、<strong>直方图</strong></h3>
更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/basics.html#basics-discretization">Histogramming and Discretization</a>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
s = pd.Series(np.random.randint(0, 7, size=10))  # 在 0-7 中随机产生 10 个整数
s

输出：
0    4
1    2
2    1
3    2
4    6
5    4
6    4
7    6
8    4
9    4
dtype: int64

输入：
s.value_counts()  # 对数据进行计数

输出：
4    5
6    2
2    2
1    1
dtype: int64
</code></pre>
<h3>6.4、<strong>字符串处理</strong></h3>
Series 中配备了一组字符串处理方法，使其在数组的每个元素上都易于操作，如下面的代码片段所示。注意，str 中的模式匹配通常默认使用 <a href="https://docs.python.org/2/library/re.html">正则表达式</a> （在某些情况下总是使用它们）。

更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/text.html#text-string-methods">Vectorized String Methods</a>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
s = pd.Series(['A', 'B', 'C', 'Aaba', 'Baca', np.nan, 'CABA', 'dog', 'cat'])
s.str.lower()  # 所有的英文字母变成小写

输出：
0       a
1       b
2       c
3    aaba
4    baca
5     NaN
6    caba
7     dog
8     cat
dtype: object
</code></pre>
 
<h2>七、<strong>合并</strong>（Merge）</h2>
<h3>7.1、<strong>连接</strong>（Concat）</h3>
pandas 提供了很多对 Series、DataFrame、Panel object（面板对象）进行逻辑操作的方法。

更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/merging.html#merging">Merging section</a>
<ul>
 	<li>使用 <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.concat.html#pandas.concat">concat()</a> 函数连接 pandas 对象</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.Dataframe(np.random.rand(10, 4))  # 生成 10 行 4 列、符合标准正态分布的二维数组
df

输出：
          0         1         2         3
0 -0.548702  1.467327 -1.015962 -0.483075
1  1.637550 -1.217659 -0.291519 -1.745505
2 -0.263952  0.991460 -0.919069  0.266046
3 -0.709661  1.669052  1.037882 -1.705775
4 -0.919854 -0.042379  1.247642 -0.009920
5  0.290213  0.495767  0.362949  1.548106
6 -1.131345 -0.089329  0.337863 -0.945867
7 -0.932132  1.956030  0.017587 -0.016692
8 -0.575247  0.254161 -1.143704  0.215897
9  1.193555 -0.077118 -0.408530 -0.862495

输入：
pieces = [df[:3], df[3:7], df[7:]]  # 拆解 df，切成三块
pd.concat(pieces)  # 重新合并 df

输出：
          0         1         2         3
0 -0.548702  1.467327 -1.015962 -0.483075
1  1.637550 -1.217659 -0.291519 -1.745505
2 -0.263952  0.991460 -0.919069  0.266046
3 -0.709661  1.669052  1.037882 -1.705775
4 -0.919854 -0.042379  1.247642 -0.009920
5  0.290213  0.495767  0.362949  1.548106
6 -1.131345 -0.089329  0.337863 -0.945867
7 -0.932132  1.956030  0.017587 -0.016692
8 -0.575247  0.254161 -1.143704  0.215897
9  1.193555 -0.077118 -0.408530 -0.862495
</code></pre>
<h3>7.2、<strong>连接</strong>（Join）</h3>
SQL 风格，更多信息点击：<a href="http://pandas.pydata.org/pandas-docs/stable/merging.html#merging-join">Database style joining</a>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
left = pd.DataFrame({'key': ['foo', 'foo'], 'lval': [1, 2]})  # 使用字典创建 Dataframe
right = pd.DataFrame({'key': ['foo', 'foo'], 'rval': [4, 5]})
left

输出：
   key  lval
0  foo     1
1  foo     2

输入：
right

输出：
   key  rval
0  foo     4
1  foo     5

输入：
pd.merge(left, right, on='key')

输出：
   key  lval  rval
0  foo     1     4
1  foo     1     5
2  foo     2     4
3  foo     2     5
</code></pre>
另外一个例子：
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
left = pd.DataFrame({'key': ['foo', 'bar'], 'lval': [1, 2]})
right = pd.DataFrame({'key': ['foo', 'bar'], 'rval': [4, 5]})
left

输出：
   key  lval
0  foo     1
1  bar     2

输入：
right

输出：
   key  rval
0  foo     4
1  bar     5

输入：
pd.merge(left, right, on='key')

输出：
   key  lval  rval
0  foo     1     4
1  bar     2     5
</code></pre>
<h3>7.3、<strong>追加</strong>（Append）</h3>
追加行到 Dataframe，更多信息点击：<a href="http://pandas.pydata.org/pandas-docs/stable/merging.html#merging-concatenation">Appending</a>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame(np.random.randn(8, 4), columns=['A','B','C','D'])
df

输出：
          A         B         C         D
0  1.346061  1.511763  1.627081 -0.990582
1 -0.441652  1.211526  0.268520  0.024580
2 -1.577585  0.396823 -0.105381 -0.532532
3  1.453749  1.208843 -0.080952 -0.264610
4 -0.727965 -0.589346  0.339969 -0.693205
5 -0.339355  0.593616  0.884345  1.591431
6  0.141809  0.220390  0.435589  0.192451
7 -0.096701  0.803351  1.715071 -0.708758

输入：
s = df.iloc[3]
df.append(s, ignore_index=True)

输出：
          A         B         C         D
0  1.346061  1.511763  1.627081 -0.990582
1 -0.441652  1.211526  0.268520  0.024580
2 -1.577585  0.396823 -0.105381 -0.532532
3  1.453749  1.208843 -0.080952 -0.264610
4 -0.727965 -0.589346  0.339969 -0.693205
5 -0.339355  0.593616  0.884345  1.591431
6  0.141809  0.220390  0.435589  0.192451
7 -0.096701  0.803351  1.715071 -0.708758
8  1.453749  1.208843 -0.080952 -0.264610
</code></pre>
<h2>八、<strong>分组</strong>（Grouping）</h2>
“group by” 一般涉及以下一个或多个步骤：
<ul>
 	<li>拆分：根据条件将数据拆分为组。</li>
 	<li>应用：对每组数据进行操作。</li>
 	<li>结合：将结果合并到数据结构中。</li>
</ul>
更多信息点击：<a href="http://pandas.pydata.org/pandas-docs/stable/groupby.html#groupby">Grouping section</a>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'A' : ['foo', 'bar', 'foo', 'bar', 'foo', 'bar', 'foo', 'foo'],
                   'B' : ['one', 'one', 'two', 'three', 'two', 'two', 'one', 'three'],
                   'C' : np.random.randn(8),
                   'D' : np.random.randn(8)})
df

输出：
     A      B         C         D
0  foo    one -1.202872 -0.055224
1  bar    one -1.814470  2.395985
2  foo    two  1.018601  1.552825
3  bar  three -0.595447  0.166599
4  foo    two  1.395433  0.047609
5  bar    two -0.392670 -0.136473
6  foo    one  0.007207 -0.561757
7  foo  three  1.928123 -1.623033
</code></pre>
<ul>
 	<li>分组, 然后使用 sum() 函数求和</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.groupby('A').sum()

输出：
            C        D
A
bar -2.802588  2.42611
foo  3.146492 -0.63958</code></pre>
</code></pre>
<ul>
 	<li>按多个列分组形成层级索引, 然后对它使用函数</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.groupby(['A', 'B']).sum()

输出：
                  C         D
A   B
bar one   -1.814470  2.395985
    three -0.595447  0.166599
    two   -0.392670 -0.136473
foo one   -1.195665 -0.616981
    three  1.928123 -1.623033
    two    2.414034  1.600434
</code></pre>
 
<h2>九、<strong>重构</strong>（Reshaping）</h2>
更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/advanced.html#advanced-hierarchical">Hierarchical Indexing</a> 和 <a href="http://pandas.pydata.org/pandas-docs/stable/reshaping.html#reshaping-stacking">Reshaping</a>
<h3>9.1、<strong>堆栈</strong></h3>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
tuples = list(zip(*[['bar', 'bar', 'baz', 'baz', 'foo', 'foo', 'qux', 'qux'], ['one', 'two', 'one', 'two', 'one', 'two', 'one', 'two']]))  # 用 zip() 函数打包元素，得到一个由 tuple 组成的 list
index = pd.MultiIndex.from_tuples(tuples, names=['first', 'second'])  # 将数据转换为 MultiIndex（层级索引）结构
df = pd.DataFrame(np.random.randn(8, 2), index=index, columns=['A', 'B'])  # 创建 Dataframe，并指定 index 和 columns
df2 = df[:4]  # 取出 df 前 4 行数据
df2

输出：
                     A         B
first second
bar   one     0.029399 -0.542108
      two     0.282696 -0.087302
baz   one    -1.575170  1.771208
      two     0.816482  1.100230
</code></pre>
<ul>
 	<li><a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.stack.html#pandas.DataFrame.stack">stack()</a> 函数 "压缩" DataFrame 的列中的一个级别</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
stacked = df2.stack()
stacked

输出：
first  second
bar    one     A    0.029399
               B   -0.542108
       two     A    0.282696
               B   -0.087302
baz    one     A   -1.575170
               B    1.771208
       two     A    0.816482
               B    1.100230
dtype: float64
</code></pre>
<ul>
 	<li>对于 “堆栈” 的 DataFrame 或 Series (或者层级索引的 Series)， stack() 的逆操作为 <a href="http://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.unstack.html#pandas.DataFrame.unstack">unstack()</a>， 默认情况下，解压缩最后一个级别</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
stacked.unstack()

输出：
                     A         B
first second
bar   one     0.029399 -0.542108
      two     0.282696 -0.087302
baz   one    -1.575170  1.771208
      two     0.816482  1.100230

输入：
stacked.unstack(1)

输出：
second        one       two
first
bar   A  0.029399  0.282696
      B -0.542108 -0.087302
baz   A -1.575170  0.816482
      B  1.771208  1.100230

输入：
stacked.unstack(0)

输出：
first          bar       baz
second
one    A  0.029399 -1.575170
       B -0.542108  1.771208
two    A  0.282696  0.816482
       B -0.087302  1.100230
</code></pre>
<h3>9.2、<strong>数据透视表</strong>（Pivot Tables）</h3>
更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/reshaping.html#reshaping-pivot">Pivot Tables</a>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({'A' : ['one', 'one', 'two', 'three'] * 3,
                   'B' : ['A', 'B', 'C'] * 4,
                   'C' : ['foo', 'foo', 'foo', 'bar', 'bar', 'bar'] * 2,
                   'D' : np.random.randn(12),
                   'E' : np.random.randn(12)})
df

输出：
        A  B    C         D         E
0     one  A  foo  1.418757 -0.179666
1     one  B  foo -1.879024  1.291836
2     two  C  foo  0.536826 -0.009614
3   three  A  bar  1.006160  0.392149
4     one  B  bar -0.029716  0.264599
5     one  C  bar -1.146178 -0.057409
6     two  A  foo  0.100900 -1.425638
7   three  B  foo -1.035018  1.024098
8     one  C  foo  0.314665 -0.106062
9     one  A  bar -0.773723  1.824375
10    two  B  bar -1.170653  0.595974
11  three  C  bar  0.648740  1.167115
</code></pre>
<ul>
 	<li>我们可以很容易的从这些数据中生成数据透视表</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.pivot_table(df, values=['A', 'B'], columns=['C'])

输出：
C             bar       foo
A     B
one   A -0.773723  1.418757
      B -0.029716 -1.879024
      C -1.146178  0.314665
three A  1.006160       NaN
      B       NaN -1.035018
      C  0.648740       NaN
two   A       NaN  0.100900
      B -1.170653       NaN
      C       NaN  0.536826
</code></pre>
 
<h2>十、时间序列</h2>
pandas 能够很方便的处理频率转换时的重新采样问题（比如将按秒采样的数据转换为按5分钟为单位进行采样）。这个操作非常普遍，特别是在金融领域。更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/timeseries.html#timeseries">Time Series section</a>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
rng = pd.date_range('1/1/2012', periods=100, freq='S')  # 创建 100 个以秒为单位的时间序列，'S' 表示秒
ts = pd.Series(np.random.randint(0, 500, len(rng)), index=rng)  #创建Series， 0-500 之间的 len(rng) 个随机整数，index 为 rng
ts.resample('5Min').sum()  # 将 ts 变为按 5 分钟为单位（就是间隔 5 分钟采样一次）的序列，并求和

输出：
2012-01-01    25083
Freq: 5T, dtype: int64
</code></pre>
<ul>
 	<li>时区表示</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
rng = pd.date_range('3/6/2012 00:00', periods=5, freq='D')  # 创建 5 个以天为单位的时间序列，'D' 表示天
ts = pd.Series(np.random.randn(len(rng)), rng)  # 创建 Series
ts

输出：
2012-03-06    0.464000
2012-03-07    0.227371
2012-03-08   -0.496922
2012-03-09    0.306389
2012-03-10   -2.290613
Freq: D, dtype: float64

输入：
ts_utc = ts.tz_localize('UTC')  # 本地化时区
ts_utc

输出：
2012-03-06 00:00:00+00:00    0.464000
2012-03-07 00:00:00+00:00    0.227371
2012-03-08 00:00:00+00:00   -0.496922
2012-03-09 00:00:00+00:00    0.306389
2012-03-10 00:00:00+00:00   -2.290613
Freq: D, dtype: float64
</code></pre>
<ul>
 	<li>转换到其他时区</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
ts_utc.tz_convert('US/Eastern')

输出：
2012-03-05 19:00:00-05:00    0.464000
2012-03-06 19:00:00-05:00    0.227371
2012-03-07 19:00:00-05:00   -0.496922
2012-03-08 19:00:00-05:00    0.306389
2012-03-09 19:00:00-05:00   -2.290613
Freq: D, dtype: float64
</code></pre>
<ul>
 	<li>在各个时间单位之间转换</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
rng = pd.date_range('1/1/2012', periods=5, freq='M')  # 'M' 表示月
ts = pd.Series(np.random.randn(len(rng)), index=rng)
ts

输出：
2012-01-31   -1.134623
2012-02-29   -1.561819
2012-03-31   -0.260838
2012-04-30    0.281957
2012-05-31    1.523962
Freq: M, dtype: float64

输入：
ps = ts.to_period()
ps

输出：
2012-01   -1.134623
2012-02   -1.561819
2012-03   -0.260838
2012-04    0.281957
2012-05    1.523962
Freq: M, dtype: float64

输入：
ps.to_timestamp()

输出：
2012-01-01   -1.134623
2012-02-01   -1.561819
2012-03-01   -0.260838
2012-04-01    0.281957
2012-05-01    1.523962
Freq: MS, dtype: float64
</code></pre>
<ul>
 	<li>使用函数可以很方便的在周期和时间戳之间的转换。在下面的例子中，我们将季度结束时间从 11月份转换为季末结束时的上午 9 点：</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
prng = pd.period_range('1990Q1', '2000Q4', freq='Q-NOV')
ts = pd.Series(np.random.randn(len(prng)), prng)
ts.index = (prng.asfreq('M', 'e') + 1).asfreq('H', 's') + 9
ts.head()

输出：
1990-03-01 09:00   -0.902937
1990-06-01 09:00    0.068159
1990-09-01 09:00   -0.057873
1990-12-01 09:00   -0.368204
1991-03-01 09:00   -1.144073
Freq: H, dtype: float64
</code></pre>
<h2>十一、<strong>分类</strong>（Categoricals）</h2>
pandas 可以在 Dataframe 中包含 categorical data （分类数据）。更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/categorical.html#categorical">categorical introduction</a> 和 <a href="http://pandas.pydata.org/pandas-docs/stable/api.html#api-categorical">API documentation</a>。
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame({"id":[1,2,3,4,5,6], "raw_grade":['a', 'b', 'b', 'a', 'a', 'e']})
df

输出：
   id raw_grade
0   1         a
1   2         b
2   3         b
3   4         a
4   5         a
5   6         e
</code></pre>
<ul>
 	<li>将原始分组转换为 categorical 类型</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df["grade"] = df["raw_grade"].astype("category")  # df 新增 grade 列，值是转换为 categorical 类型的 raw_grade 列
df["grade"]

输出：
0    a
1    b
2    b
3    a
4    a
5    e
Name: grade, dtype: category
Categories (3, object): [a, b, e]
</code></pre>
Categories (3, object): [a, b, e] 显示有 a, b, e 三个分类
<ul>
 	<li>为 categories 换上有意义的名字（Series.cat.categories）</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df["grade"].cat.categories = ["very good", "good", "very bad"]  # 分别对应 a, b, e 三个分类，上面例子的最后一行显示了对应顺序
df["grade"]

输出：
0    very good
1         good
2         good
3    very good
4    very good
5     very bad
Name: grade, dtype: category
Categories (3, object): [very good, good, very bad]
</code></pre>
看到最后一行显示类别，由原来的 a, b, e 改名成了 very good, good, very bad。
<ul>
 	<li>重新排序并添加缺失的数据（series.cat 下的函数，默认返回一个新的 Series）</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df["grade"] = df["grade"].cat.set_categories(["very bad", "bad", "medium", "good", "very good"])
df["grade"]

输出：
0    very good
1         good
2         good
3    very good
4    very good
5     very bad
Name: grade, dtype: category
Categories (5, object): [very bad, bad, medium, good, very good]
</code></pre>
<ul>
 	<li>排序是按类别排序的，而不是词汇顺序</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.sort_values(by="grade")

输出：
   id raw_grade      grade
5   6         e   very bad
1   2         b       good
2   3         b       good
0   1         a  very good
3   4         a  very good
4   5         a  very good
</code></pre>
<ul>
 	<li>将一列分组，也会显示其中的空白类别</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.groupby("grade").size()  # 统计 grade 列，每个分类中元素的个数

输出：
grade
very bad     1
bad          0
medium       0
good         2
very good    3
dtype: int64
</code></pre>
 
<h2>十二、<strong>绘图</strong></h2>
更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/visualization.html#visualization">Plotting</a>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
ts = pd.Series(np.random.randn(1000), index=pd.date_range('1/1/2000', periods=1000))  # 创建 Series
ts = ts.cumsum()  # 累积值
ts.plot()

输出：
 <matplotlib.axes._subplots.AxesSubplot at 0x109464b70>

输入：
plt.show()  # 使用 show() 函数才能把图像展现出来
</code></pre>
<img src="http://naofu.net/wp-content/uploads/2017/10/318-01.png" alt="" />
<ul>
 	<li>plot() 函数可以把 Dataframe 中的所有列绘制在一张图中</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df = pd.DataFrame(np.random.randn(1000, 4), index=ts.index, columns=['A', 'B', 'C', 'D'])
df = df.cumsum()
plt.figure(); df.plot(); plt.legend(loc='best')

输出：
<matplotlib.legend.Legend at 0x109a28860>

输入：
plt.show()
</code></pre>
<img src="http://naofu.net/wp-content/uploads/2017/10/318-02.png" alt="" />

 
<h2>十三、<strong>文件操作</strong></h2>
<h3>13.1、<strong>CSV</strong></h3>
<ul>
 	<li><a href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-store-in-csv">写入 CSV 文件</a></li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.to_csv('foo.csv')
</code></pre>
<ul>
 	<li><a href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-read-csv-table">读取 CSV 文件</a></li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
pd.read_csv('foo.csv')

输出：
     Unnamed: 0          A          B         C          D
0    2000-01-01   0.266457  -0.399641 -0.219582   1.186860
1    2000-01-02  -1.170732  -0.345873  1.653061  -0.282953
2    2000-01-03  -1.734933   0.530468  2.060811  -0.515536
3    2000-01-04  -1.555121   1.452620  0.239859  -1.156896
4    2000-01-05   0.578117   0.511371  0.103552  -2.428202
5    2000-01-06   0.478344   0.449933 -0.741620  -1.962409
6    2000-01-07   1.235339  -0.091757 -1.543861  -1.084753
..          ...        ...        ...       ...        ...
993  2002-09-20 -10.628548  -9.153563 -7.883146  28.313940
994  2002-09-21 -10.390377  -8.727491 -6.399645  30.914107
995  2002-09-22  -8.985362  -8.485624 -4.669462  31.367740
996  2002-09-23  -9.558560  -8.781216 -4.499815  30.518439
997  2002-09-24  -9.902058  -9.340490 -4.386639  30.105593
998  2002-09-25 -10.216020  -9.480682 -3.933802  29.758560
999  2002-09-26 -11.856774 -10.671012 -3.216025  29.369368

[1000 rows x 5 columns]
</code></pre>
<h3>13.2、<strong>HDF5</strong></h3>
写入、读取 <a href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-hdf5">HDFStores</a>
<ul>
 	<li>写入 HDF5 文件</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.to_hdf('foo.h5', 'df')
</code></pre>
<ul>
 	<li>读取 HDF5 文件</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
pd.read_hdf('foo.h5', 'df')

输出：
                    A          B         C          D
2000-01-01   0.266457  -0.399641 -0.219582   1.186860
2000-01-02  -1.170732  -0.345873  1.653061  -0.282953
2000-01-03  -1.734933   0.530468  2.060811  -0.515536
2000-01-04  -1.555121   1.452620  0.239859  -1.156896
2000-01-05   0.578117   0.511371  0.103552  -2.428202
2000-01-06   0.478344   0.449933 -0.741620  -1.962409
2000-01-07   1.235339  -0.091757 -1.543861  -1.084753
...               ...        ...       ...        ...
2002-09-20 -10.628548  -9.153563 -7.883146  28.313940
2002-09-21 -10.390377  -8.727491 -6.399645  30.914107
2002-09-22  -8.985362  -8.485624 -4.669462  31.367740
2002-09-23  -9.558560  -8.781216 -4.499815  30.518439
2002-09-24  -9.902058  -9.340490 -4.386639  30.105593
2002-09-25 -10.216020  -9.480682 -3.933802  29.758560
2002-09-26 -11.856774 -10.671012 -3.216025  29.369368

[1000 rows x 4 columns]
</code></pre>
<h3>13.3、<strong>Excel</strong></h3>
写入、读取更多内容点击：<a href="http://pandas.pydata.org/pandas-docs/stable/io.html#io-excel">MS Excel</a>
<ul>
 	<li>写入 Excel 文件</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
df.to_excel('foo.xlsx', sheet_name='Sheet1')
</code></pre>
<ul>
 	<li>读取 Excel 文件</li>
</ul>
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">输入：
pd.read_excel('foo.xlsx', 'Sheet1', index_col=None, na_values=['NA'])

输出：
                    A          B         C          D
2000-01-01   0.266457  -0.399641 -0.219582   1.186860
2000-01-02  -1.170732  -0.345873  1.653061  -0.282953
2000-01-03  -1.734933   0.530468  2.060811  -0.515536
2000-01-04  -1.555121   1.452620  0.239859  -1.156896
2000-01-05   0.578117   0.511371  0.103552  -2.428202
2000-01-06   0.478344   0.449933 -0.741620  -1.962409
2000-01-07   1.235339  -0.091757 -1.543861  -1.084753
...               ...        ...       ...        ...
2002-09-20 -10.628548  -9.153563 -7.883146  28.313940
2002-09-21 -10.390377  -8.727491 -6.399645  30.914107
2002-09-22  -8.985362  -8.485624 -4.669462  31.367740
2002-09-23  -9.558560  -8.781216 -4.499815  30.518439
2002-09-24  -9.902058  -9.340490 -4.386639  30.105593
2002-09-25 -10.216020  -9.480682 -3.933802  29.758560
2002-09-26 -11.856774 -10.671012 -3.216025  29.369368

[1000 rows x 4 columns]
</code></pre>
 
<h2>十四、<strong>报错</strong>（Gotchas 直译是陷阱）</h2>
如果你尝试一个操作时看到如下异常：
<pre class="line-numbers prism-highlight" data-start="1"><code class="language-python">>>> if pd.Series([False, True, False]):
    print("I was true")
Traceback
    ...
ValueError: The truth value of an array is ambiguous. Use a.empty, a.any() or a.all().
</code></pre>
那么请看看 <a href="http://pandas.pydata.org/pandas-docs/stable/basics.html#basics-compare">Comparisons</a> 里面是怎么说的。

或者参考 <a href="http://pandas.pydata.org/pandas-docs/stable/gotchas.html#gotchas">Gotchas</a>。

至此，《十分钟入门 pandas》翻译完了，以后再陆续翻译文章中所有链接的文档，已经换成中文的链接表示已经翻译过了，还是英文的则没有。

后面会一直不定时维护这篇翻译，争取做到人人都能看懂，真的对新手有帮助。

最后希望大家看完翻译，再对照英文原版重新看一篇，慢慢习惯英文，会对你大有帮助。				