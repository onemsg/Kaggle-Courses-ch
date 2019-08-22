# 模型验证

你已经建立好了一个模型，但是它好用吗？

在这节课，你将学习使用模型验证去评估你模型的质量。评估模型质量是迭代地提升你模型的关键。

## 什么是模型验证

要去验证几乎你所有构建过的模型，在大多数（虽然不是全部）应用中，有价值的模型质量验证是预测准确度，换句话说，就是就是模型的预测结果到底有多接近真实结果。

很多人会犯一个大错误在他们验证预测准确度的时候。他们做出训练数据的预测结果和训练数据的标记值（预测目标）进行比较，你会知道这么做问题和待会如何解决它，但先让咱们想想第一步该怎么做。

第一步你需要把模型质量概括成一个容易理解的东西。如果你去比较10000个房屋的预测值和真实值，你大概会看到既有好的预测也有坏的预测。浏览10000个预测值和真实值并不值得做，我们需要把这个总结成一个单一指标。

有很多指标可以评估模型质量，但咱们从一个叫 **平均绝对误差/Mean Absolute Error/MAE** 的开始，先从最后一个词 **误差/error** 开始解析这个指标。

每个房屋的预测误差是：

```text
误差 = 真实值 - 预测值
```

所以，如果一个房屋价格是 150,000 美元，你预测的结果是100,000美元那么误差就是 50,000 美元。

通过MAE指标，咱们会取每个误差的绝对值，这能把每个误差转换成一个正数，然后我们回去这些绝对误差的平均值，这就是我们模型质量的度量。用纯 English 说，就是
>On average, our predictions are off by about X.

为了计算 MASE，我们首先需要一个模型。

### In [1]

```python
import pandas as pd

# Load data
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path) 
# Filter rows with missing price values
filtered_melbourne_data = melbourne_data.dropna(axis=0)
# Choose target and features
y = filtered_melbourne_data.Price
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'BuildingArea', 
                        'YearBuilt', 'Lattitude', 'Longtitude']
X = filtered_melbourne_data[melbourne_features]

from sklearn.tree import DecisionTreeRegressor
# Define model
melbourne_model = DecisionTreeRegressor()
# Fit model
melbourne_model.fit(X, y)
```

有了模型后，下面就是我们如何计算平均绝对误差。

### In [2]

```python
from sklearn.metrics import mean_absolute_error

predicted_home_prices = melbourne_model.predict(X)
mean_absolute_error(y, predicted_home_prices)
```

### Out [2]

```text
434.71594577146544
```

## 样本内得分的问题

我们刚才计算的度量值可以称为"样本内"分数，我们使用一个房屋"样本"来分别构建模型和评估模型，下面是为什么说这么做不对。

试想，在大型房地产市场,门的颜色与房价无关。

但是,在用于构建模型的数据示例中,所有带有绿色门的房屋都非常昂贵。模型工作是找到预测房价的模式,所以它将看到这种模式,它总是预测高价格的房屋与绿色门。

由于此模式是从训练数据派生的,因此模型在训练数据中将显示为准确。

但是,如果模型看到新数据时这种模式不起作用,则模型在实际使用时将非常不准确。

由于模型的实际价值来自对新数据进行预测,因此我们测量未用于构建模型的数据的性能。最直接的方法是从模型构建过程中排除一些数据,然后使用这些数据来测试模型对以前从未见过的数据的准确性。此数据称为 **验证数据/validation data**。

## Coding it

scikit-learn库有一个叫 `train_test_split` 的函数，用来把数据集分割成两份。我们会用其中一些数据作为训练数据来拟合模型，然后将其他数据作为验证数据来计算 `平均绝对误差`。

这是代码：

### In [3]

```python
from sklearn.model_selection import train_test_split

# split data into training and validation data, for both features and target
# The split is based on a random number generator. Supplying a numeric value to
# the random_state argument guarantees we get the same split every time we
# run this script.
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0)
# Define model
melbourne_model = DecisionTreeRegressor()
# Fit model
melbourne_model.fit(train_X, train_y)

# get predicted prices on validation data
val_predictions = melbourne_model.predict(val_X)
print(mean_absolute_error(val_y, val_predictions))
```

```text
259701.5978050355
```

## Wow!

你的样本内数据的平均绝对误差大约是500美刀，样本外的超过了250,000美刀。

这是几乎完全正确的模型与对于大多数实际情况不可用的模型之间的区别。作为参考点，验证数据中的平均房屋价值为 110 万美元，因此，新数据中的错误大约是平均房价的四分之一。

有很多方法去提升这个模型，比如尝试找出更好的特征或使用不同的模型。

## Your Turn

在我们着眼于提升这个模型之前，请自己去试试[模型验证](https://www.kaggle.com/kernels/fork/1259097 "进入kaggle Kernels")
