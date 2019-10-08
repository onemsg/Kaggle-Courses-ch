# Order By

## 介绍

到现在 为止，你已经学习了如何使用以下从句：

```SQL
SELECT ... 
FROM ...
(WHERE) ...
GROUP BY ...
(HAVING) ...
```

你还学习了聚合，像**COUNT()**。

现在，你将了解 如何使用**ORDER BY**子句更改结果顺序，并通过对日期应用排序来探索一个流行的案例。

## ORDER BY

**ORDER BY**通常是查询中的最后一个子句，它对查询其余部分返回的结果进行排序。

让我们来看看这个熟悉的表上的一个例子。

**按数字列排序**
我们可以使用以下命令重新排序。

```SQL
SELECT ID, Name, Animal
FROM `bigquery-public-data.pet_records.pets`
ORDER BY ID
```

结果如下：

**按文本列排序**
还可以按文本列排序，结果按字母顺序显示。

```SQL
SELECT ID, Name, Animal
FROM `bigquery-public-data.pet_records.pets`
ORDER BY Animal
```

**颠倒的顺序**
你可以使用**DESC**参数（降序的缩写）反转顺序。

此查询按**Animal**列对所选列进行排序，但将首先返回按字母顺序排列在最后的值。

```SQL
SELECT ID, Name, Animal
FROM `bigquery-public-data.pet_records.pets`
ORDER BY Animal DESC
```

## Dates

最后，咱们来讨论一下日期，因为他们在显示世界中的数据库中经常出现。

有两种方式可以将日期存储在**BigQuery**中：作为一个**日期**或**日期时间**。

**日期**的格式首先是年份，然后是月份，然后是日期。格式具体写成这个样子：
`YYYY-[M]M-[D]D`

* YYYY: Four-digit year
* [M]M: One or two digit month
* One or two digit day

所以**2019-01-10**被翻译成January 10, 2019。

**DATETIME**格式类似于日期格式....但是把时间加在最后。

通常你会想看日期的一部分，比如年份或日期。你可以用**EXTRACT**去查询。这个查询将返回一个列，其中只有日期的每一天在**column_with_timestamp**列中。

```SQL
SELECT EXTRACT(DAY FROM column_with_timestamp)
        FROM `bigquery-public-data.imaginary_dataset.imaginary_table`
```

SQL在日期方面很敏感，我们可以查看的信息不仅仅是提取单元格的一部分。例如，下面这个查询返回一个列。其中包含**column_with_timestamp**列中的年份（1到53之间）中每个日期的星期：

```SQL
 SELECT EXTRACT(WEEK FROM column_with_timestamp)
        FROM `bigquery-public-data.imaginary_dataset.imaginary_table`
```

你可以在** BigQuery**的“日期和时间函数”下找到所有可以和日期一起使用的函数。

## 例子：每周哪一天的交通事故最致命？

让我们使用美国交通事故记录数据库，它包含了美国至少一人死亡的交通事故信息。

首先，我们需要建立我们的环境。

### In [1]

```python
# import package with helper functions 
import bq_helper

# create a helper object for this dataset
accidents = bq_helper.BigQueryHelper(active_project="bigquery-public-data",
                                   dataset_name="nhtsa_traffic_fatalities")
```

```text
Using Kaggle's public dataset BigQuery integration.
```

现在，我们将计算每个事故的唯一id（在这个表中它们被称为"consecutive_number"）和星期几。然后对表进行排序，以便最先返回事故最多的日子。

### In [2]

```Python
# query to find out the number of accidents which 
# happen on each day of the week
query = """SELECT COUNT(consecutive_number) num_accidents, 
                  EXTRACT(DAYOFWEEK FROM timestamp_of_crash)
            FROM `bigquery-public-data.nhtsa_traffic_fatalities.accident_2015`
            GROUP BY EXTRACT(DAYOFWEEK FROM timestamp_of_crash)
            ORDER BY COUNT(consecutive_number) DESC
        """
```

像往常一样，我们运行如下：

### In [3]

```python
# the query_to_pandas_safe method will cancel the query if
# it would use too much of your quota, with the limit set 
# to 1 GB by default
accidents_by_day = accidents.query_to_pandas_safe(query)
```

这里给了一个熊猫数据帧。如果你已经知道matplotlib,你可以将结果绘制如下图：

### In [4]

```python
# library for plotting
import matplotlib.pyplot as plt

# make a plot to show that our data is, actually, sorted:
plt.plot(accidents_by_day.num_accidents)
plt.title("Number of Accidents by Rank of Day \n (Most to least dangerous)")
plt.show()
```

### In [5]

```SQL
print(accidents_by_day)
```

要将返回的日期（第二列）映射到实际的日期，你可以参考**DAYOFWEEK**函数上的**BigQuery**文档。它能够返回“一个介于1（星期日）和7（星期六）之间的整数”。所以，2015年大多数致命的交通事故发生在周日和周六，而周二发生的事故最少。

## Your Turn

Order by可以使你的结果更容易解释，你可以[自己试试](https://www.kaggle.com/kernels/fork/682087)。
