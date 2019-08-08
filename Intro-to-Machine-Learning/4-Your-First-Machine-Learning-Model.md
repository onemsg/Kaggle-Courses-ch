# 你的第一个机器学习模型

## 为模型选择数据

你的数据集有很多变量（variables）令你无法记下来，或甚至不能很好的打印显示出来。如何将大量的数据减少为你可以理解的内容？

首先，我们用直觉选几个变量，之后的课程会告诉你一些统计技术来自动确定变量的优先级。

要选择变量/列，我们需要查看关于在数据集中所有列（columns）的一个列表，下面的是用DataFramed的列（columns）属性完成的。

**In [1]**

```python
import pandas as pd

melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path) 
melbourne_data.columns
```

**Out [1]**

```text
Index(['Suburb', 'Address', 'Rooms', 'Type', 'Price', 'Method', 'SellerG',
       'Date', 'Distance', 'Postcode', 'Bedroom2', 'Bathroom', 'Car',
       'Landsize', 'BuildingArea', 'YearBuilt', 'CouncilArea', 'Lattitude',
       'Longtitude', 'Regionname', 'Propertycount'],
      dtype='object')
```

**In [2]**

```python
# The Melbourne data has some missing values (some houses for which some variables weren't recorded.)
# We'll learn to handle missing values in a later tutorial.  
# Your Iowa data doesn't have missing values in the columns you use. 
# So we will take the simplest option for now, and drop houses from our data. 
# Don't worry about this much for now, though the code is:

# dropna drops missing values (think of na as "not available")
melbourne_data = melbourne_data.dropna(axis=0)
```

有很多方法去选择以数据的子集，[Pandas微课程](https://www.kaggle.com/learn/pandas)有更深的介绍，但现在我们会专注两个方法。

1. 点表示法，我们用于选择“预测目标”（prediction target）

2. 选择包含列的列表，我们用来下选择“特征”（features)

## 选择预测目标

可以使用点表示法取出变量，此单列存储在一个 `Series` 中，它大致上类似于只有一列数据的 DataFrame。

使用点表示法选择我们想要预测的列,该列称为预测目标。按照惯例，预测目标称为 y。因此,我们需要在墨尔本数据中保存房价的代码是

**In [3]**

```python
y = melbourne_data.Price
```

## 选择特征

输入到我们的模型中的列(后来用于预测)称为"特征"。在我们的例子中，这些列是用来确定房价的列。有时，您将使用目标以外的所有列作为特征。其他时候，使用较少的特征会更好。

现在，构建一个只使用几个特征的模型。稍后，您将了解如何迭代和比较使用不同特征构建的模型。

我们通过在括号内提供列名列表来选择多个特征，该列表中的每个项都应是一个字符串(带引号)。
下
面是一个示例:

**In [4]**

```python
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'Lattitude', 'Longtitude']
```

按照惯例，我们把这种数据叫做 **X**

**In [5]**

```python
X = melbourne_data[melbourne_features]
```

让咱们快速查看下咱们用来预测房屋价格的数据，使用 `descibe` 方法和用来展示头几行数据的 `head` 方法

**In [6]**

```python
X.describe()
```

**Out [6]**

<table class="dataframe" border="1">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rooms</th>
      <th>Bathroom</th>
      <th>Landsize</th>
      <th>Lattitude</th>
      <th>Longtitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>6196.000000</td>
      <td>6196.000000</td>
      <td>6196.000000</td>
      <td>6196.000000</td>
      <td>6196.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>2.931407</td>
      <td>1.576340</td>
      <td>471.006940</td>
      <td>-37.807904</td>
      <td>144.990201</td>
    </tr>
    <tr>
      <th>std</th>
      <td>0.971079</td>
      <td>0.711362</td>
      <td>897.449881</td>
      <td>0.075850</td>
      <td>0.099165</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>-38.164920</td>
      <td>144.542370</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>2.000000</td>
      <td>1.000000</td>
      <td>152.000000</td>
      <td>-37.855438</td>
      <td>144.926198</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>3.000000</td>
      <td>1.000000</td>
      <td>373.000000</td>
      <td>-37.802250</td>
      <td>144.995800</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>4.000000</td>
      <td>2.000000</td>
      <td>628.000000</td>
      <td>-37.758200</td>
      <td>145.052700</td>
    </tr>
    <tr>
      <th>max</th>
      <td>8.000000</td>
      <td>8.000000</td>
      <td>37000.000000</td>
      <td>-37.457090</td>
      <td>145.526350</td>
    </tr>
  </tbody>
</table>

**In [7]**

```python
X.head()
```

**Out [7]**

<table class="dataframe" border="1">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Rooms</th>
      <th>Bathroom</th>
      <th>Landsize</th>
      <th>Lattitude</th>
      <th>Longtitude</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1.0</td>
      <td>156.0</td>
      <td>-37.8079</td>
      <td>144.9934</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>2.0</td>
      <td>134.0</td>
      <td>-37.8093</td>
      <td>144.9944</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>1.0</td>
      <td>120.0</td>
      <td>-37.8072</td>
      <td>144.9941</td>
    </tr>
    <tr>
      <th>6</th>
      <td>3</td>
      <td>2.0</td>
      <td>245.0</td>
      <td>-37.8024</td>
      <td>144.9993</td>
    </tr>
    <tr>
      <th>7</th>
      <td>2</td>
      <td>1.0</td>
      <td>256.0</td>
      <td>-37.8060</td>
      <td>144.9954</td>
    </tr>
  </tbody>
</table>

## 构建你的模型

你将使用 **scikit-learn** 库来生成你的而模型，当编程时，这个库写作 **sklearn**，你会在示例代码中看到。Scikit-learn 是用于对通常存储在 DataFrame 中的数据类型进行建模的最流行的库。

建立和使用模型的步骤：

- **定义/Define** 它是什么类型的模型？决策树？还是其他模型？还有模型的其他参数也需要被确定。
  
- **拟合/Fit** 从提供的数据采集模式，这是生成模型的核心。

- **预测/Predict** 跟它的名字一样。

- **验证/Evaluete** 检测模型的预测能力有多准确。

**In [8]**

```python
from sklearn.tree import DecisionTreeRegressor

# Define model. Specify a number for random_state to ensure same results each run
melbourne_model = DecisionTreeRegressor(random_state=1)

# Fit model
melbourne_model.fit(X, y)
```

**Out [8]**

```text
DecisionTreeRegressor(criterion='mse', max_depth=None, max_features=None,
           max_leaf_nodes=None, min_impurity_decrease=0.0,
           min_impurity_split=None, min_samples_leaf=1,
           min_samples_split=2, min_weight_fraction_leaf=0.0,
           presort=False, random_state=1, splitter='best')
```

很多机器学习模型在模型训练中允许一些随机性，为 `random_state` 指定一个数字可以确保你每次运行都能得到一个相同的结果，这被认为是一种好的做法。你能使用任何数据，而且模型质量不会有意义地依赖于你选择地值。

我们现在有一个拟合好的模型，我们可以用它来进行预测。

在实践中,你会想预测市场上新的房子，而不是我们已经有价格的房子。但是，要对训练数据的前几行进行预测，以查看预测函数的运转情况。

**In [9]**

```python
print("Making predictions for the following 5 houses:")
print(X.head())
print("The predictions are")
print(melbourne_model.predict(X.head()))
```

```text
Making predictions for the following 5 houses:
   Rooms  Bathroom  Landsize  Lattitude  Longtitude
1      2       1.0     156.0   -37.8079    144.9934
2      3       2.0     134.0   -37.8093    144.9944
4      4       1.0     120.0   -37.8072    144.9941
6      3       2.0     245.0   -37.8024    144.9993
7      2       1.0     256.0   -37.8060    144.9954
The predictions are
[1035000. 1465000. 1600000. 1876000. 1636000.]
```

## 接下来

尝试它在[模型构建练习](https://www.kaggle.com/scratchpad/kerneldc4e8a7b51/edit)
