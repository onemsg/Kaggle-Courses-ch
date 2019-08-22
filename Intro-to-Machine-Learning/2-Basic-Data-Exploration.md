# 基础数据探索

## 使用Pandas去熟悉你的数据

在任何机器学习项目的第一步就是熟悉你的而数据。你会使用Pandas库做这些，Pandas是数据科学家探索和操作数据的重要工具。大多数人在他们的代码里把Pandas缩写为 `pd`，通过如下命令

### In [1]

```python
import pandas as pd
```

Pandas库最重要部分是 `DataFrame`，一个DataFrame保存的数据类型你可以当作是一个表，这有点向你Excel里的表格或SQL数据库里的表。

Pandas有强大丰富的方法能让你处理这种数据时做很多的事儿。

举个例子，咱们先看看澳大利亚墨尔本的房价，再动手练习时，你会用同样的流程去处理新数据，这个新数据是关于爱荷华州的。

示例数据（墨尔本）在这个路径内 `../input/melbourne-housing-snapshot/melb_data.csv`

### In [2]

``` python
# 把路径保存为变量，来更方便的传入值
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
# 读取和保存数据到DataFrame中，给变量命名为 melbourne_data
melbourne_data = pd.read_csv(melbourne_file_path)
# 打印墨尔本数据的数据描述信息
melbourne_data.describe()
```

### Out [2]

<table class="dataframe" border="1">
    <thead>
        <tr style="text-align: right;">
            <th></th>
            <th>Rooms</th>
            <th>Price</th>
            <th>Distance</th>
            <th>Postcode</th>
            <th>Bedroom2</th>
            <th>Bathroom</th>
            <th>Car</th>
            <th>Landsize</th>
            <th>BuildingArea</th>
            <th>YearBuilt</th>
            <th>Lattitude</th>
            <th>Longtitude</th>
            <th>Propertycount</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th>count</th>
            <td>13580.000000</td>
            <td>1.358000e+04</td>
            <td>13580.000000</td>
            <td>13580.000000</td>
            <td>13580.000000</td>
            <td>13580.000000</td>
            <td>13518.000000</td>
            <td>13580.000000</td>
            <td>7130.000000</td>
            <td>8205.000000</td>
            <td>13580.000000</td>
            <td>13580.000000</td>
            <td>13580.000000</td>
        </tr>
        <tr>
            <th>mean</th>
            <td>2.937997</td>
            <td>1.075684e+06</td>
            <td>10.137776</td>
            <td>3105.301915</td>
            <td>2.914728</td>
            <td>1.534242</td>
            <td>1.610075</td>
            <td>558.416127</td>
            <td>151.967650</td>
            <td>1964.684217</td>
            <td>-37.809203</td>
            <td>144.995216</td>
            <td>7454.417378</td>
        </tr>
        <tr>
            <th>std</th>
            <td>0.955748</td>
            <td>6.393107e+05</td>
            <td>5.868725</td>
            <td>90.676964</td>
            <td>0.965921</td>
            <td>0.691712</td>
            <td>0.962634</td>
            <td>3990.669241</td>
            <td>541.014538</td>
            <td>37.273762</td>
            <td>0.079260</td>
            <td>0.103916</td>
            <td>4378.581772</td>
        </tr>
        <tr>
            <th>min</th>
            <td>1.000000</td>
            <td>8.500000e+04</td>
            <td>0.000000</td>
            <td>3000.000000</td>
            <td>0.000000</td>
            <td>0.000000</td>
            <td>0.000000</td>
            <td>0.000000</td>
            <td>0.000000</td>
            <td>1196.000000</td>
            <td>-38.182550</td>
            <td>144.431810</td>
            <td>249.000000</td>
        </tr>
        <tr>
            <th>25%</th>
            <td>2.000000</td>
            <td>6.500000e+05</td>
            <td>6.100000</td>
            <td>3044.000000</td>
            <td>2.000000</td>
            <td>1.000000</td>
            <td>1.000000</td>
            <td>177.000000</td>
            <td>93.000000</td>
            <td>1940.000000</td>
            <td>-37.856822</td>
            <td>144.929600</td>
            <td>4380.000000</td>
        </tr>
        <tr>
            <th>50%</th>
            <td>3.000000</td>
            <td>9.030000e+05</td>
            <td>9.200000</td>
            <td>3084.000000</td>
            <td>3.000000</td>
            <td>1.000000</td>
            <td>2.000000</td>
            <td>440.000000</td>
            <td>126.000000</td>
            <td>1970.000000</td>
            <td>-37.802355</td>
            <td>145.000100</td>
            <td>6555.000000</td>
        </tr>
        <tr>
            <th>75%</th>
            <td>3.000000</td>
            <td>1.330000e+06</td>
            <td>13.000000</td>
            <td>3148.000000</td>
            <td>3.000000</td>
            <td>2.000000</td>
            <td>2.000000</td>
            <td>651.000000</td>
            <td>174.000000</td>
            <td>1999.000000</td>
            <td>-37.756400</td>
            <td>145.058305</td>
            <td>10331.000000</td>
        </tr>
        <tr>
            <th>max</th>
            <td>10.000000</td>
            <td>9.000000e+06</td>
            <td>48.100000</td>
            <td>3977.000000</td>
            <td>20.000000</td>
            <td>8.000000</td>
            <td>10.000000</td>
            <td>433014.000000</td>
            <td>44515.000000</td>
            <td>2018.000000</td>
            <td>-37.408530</td>
            <td>145.526350</td>
            <td>21650.000000</td>
        </tr>
    </tbody>
</table>

## 解释下数据描述

结果中显示了，在你原数据中的每列都有8个数字。第一个数字 **count**，是指所有行中不是缺失值的数量。

有许多原因会产生 **缺失值/Missing value**。比如，当你调查一栋只有一间卧室的房屋时不会搜集第二个卧室的大小。让我们回到缺失值的话题上来。

第二个数字 **mean**，平均数。下面的 **std** 是标准差/standard deviation，它是数值分布的度量。

要解释 **min, 25%, 50%, 75%** 和 **max** 的意思，想象下每列数字按值大小从最小到最大排序。第一个数（也就是最小的数）就是 **min**。穿过列表的四分之一，你会找到一个数字大于25%的数且小于75%的数，这个就是 **25%** 值（咱大学统计课上叫“下四分位数”），**50%** 和 **75%** 也是相似的规定，当然 **max** 就是最后一个（最大的）的数。

>**译者延申：**  
>可以参考 “四分位数” 的百度百科  
>第一四分位数 (Q1)，又称“下四分位数、较小四分位数”，等于该样本中所有数值由小到大排列后第25%的数字。  
>第二四分位数 (Q2)，又称“中位数”，等于该样本中所有数值由小到大排列后第50%的数字。  
>第三四分位数 (Q3)，又称“上四分位数、较大四分位数”，等于该样本中所有数值由小到大排列后第75%的数字。  
>第三四分位数与第一四分位数的差距又称四分位距（InterQuartile Range,IQR）。  

## Your Turn

开始你的[第一次代码练习](https://www.kaggle.com/kernels/fork/1258954 "进入kaggle Kernels")
