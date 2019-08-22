# 随机森林

## 前言

决策树留给你一个艰难的决定，一棵有很多叶子的深树会造成过拟合，因为每个预测都来自叶子上只有少量房屋的历史数据。但是一棵有少量叶子的浅树性能较差因为它不能捕获尽原始数据中可能多的特征。

即使当今最先进的模型，也面临着欠拟合和过拟合的矛盾。不过，很多模型有聪明的方法能带来更好的表现，以**随机森林/random forest**为例。

随机森林使用很多树，它通过平均每个组件树的预测结果进行预测。它通常有好得多的预测准确性相比一棵单独的决策树，而且它用默认参数也能运行的很好。如果继续建模，您可以学得更多具有更好性能的模型，但其中许多模型对获取正确的参数非常敏感。

## Example

您已经看到加载数据的代码好几次了，在数据加载结束时，我们有以下变量：

- train_X
- val_X
- train_y
- val_y

### In [1]

```python
import pandas as pd

# Load data
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path) 
# Filter rows with missing values
melbourne_data = melbourne_data.dropna(axis=0)
# Choose target and features
y = melbourne_data.Price
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'BuildingArea', 
                        'YearBuilt', 'Lattitude', 'Longtitude']
X = melbourne_data[melbourne_features]

from sklearn.model_selection import train_test_split

# split data into training and validation data, for both features and target
# The split is based on a random number generator. Supplying a numeric value to
# the random_state argument guarantees we get the same split every time we
# run this script.
train_X, val_X, train_y, val_y = train_test_split(X, y,random_state = 0)
```

我们建立一个随机森林模型，这类似于我们用scikit-learn怎么建立决策树模型，这次用`RandomForestRegressor` 类代替 `DecisionTreeRegressor`。

### In [2]

```python

from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error

forest_model = RandomForestRegressor(random_state=1)
forest_model.fit(train_X, train_y)
melb_preds = forest_model.predict(val_X)
print(mean_absolute_error(val_y, melb_preds))
```

```text
/opt/conda/lib/python3.6/site-packages/sklearn/ensemble/forest.py:245: FutureWarning: The default value of n_estimators will change from 10 in version 0.20 to 100 in 0.22.
  "10 in version 0.20 to 100 in 0.22.", FutureWarning)

202888.18157951365
```

## 结语

可能还有进一步改进的余地，但与 250,000 的最佳决策树误差相比，这是一个很大的进步。有些参数允许您更改随机林的性能，就像我们更改了单个决策树的最大深度一样。但是，随机林模型的最佳特点之一是，即使没有这种调整，它们通常也能工作地相当不错。

您很快就会学习 XGBoost 模型，该模型在使用正确的参数调整良好时提供更好的性能（但需要一些技能才能获得正确的模型参数）。

## Your Turn

试试[使用随机森林](https://www.kaggle.com/kernels/fork/1259186 "进入kaggle Kernels")，看看它提升了你的模型多大性能。
