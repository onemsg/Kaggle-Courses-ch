# Select,From & Where

## 介绍

**SELECT**语句或查询是SQL最重要的部分。

我们使用关键字**SELECT**、**FORM**、**WHERE**根据指定的条件从特定列表获取数据。

### SELECT...FROM

最基本的SQL查询从一个表中选择一个列。要做到这一点，您需要在**SELECT**之后指定你想要的列，然后在**FROM**字之后指定要从哪个表中提取相应的列。

让我们在一个假想的小数据库`pet_record`中查看一个查询，其中只有一个名为`pets`的表。

因此，如果我们想要从pet_record数据库的pets表中选择`Name`列（如果该数据库可以作为Kaggle上的BigQuery数据集访问，但因为是我编造的，所以不能访问），我们将这样做：

 ```SQL
SELECT Name
FROM `bigquery-public-data.pet_records.pets`
WHERE Animal = 'Cat'
 ```

我将从这个数字中返回重要的数据。

注意，我们传递给**FORM**的参数不是以单个或双引号（'或者是"），而是反向形式的引号（`）。我们用它识别相关的BigQuery数据集。

>你是否需要大写**SELECT**和**FROM**？不，SQL不区分大小写。不过，一般情况下人们习惯使>用大写的SQL命令，这样会是查询更加容易阅读。

### WHERE...

BigQuery数据集很大。所以您通常只想返回满足特定条件的行。你可以使用WHERE语句：

这有一个例子：

```SQL
SELECT Name
FROM `bigquery-public-data.pet_records.pets`
WHERE Animal = 'Cat'
```

这个查询将只返回`Name`列，这些列所在的行的`Animal`列中含有`cat`这个词。这些就是以这种方法用蓝色突出显示的单元格。

### 示例：OpenAQ数据集中的所有美国城市是什么？

既然你已经掌握了基本知识，我们就从一个实际数据集的例子来运用刚才的知识吧。我们将使用空气质量的OpenAQ数据集。

首先，我们设置了我们需要运行查询和快速查看什么表在我们的数据库中。

### In [1]

```python
# import package with helper functions 
import bq_helper

# create a helper object for this dataset
open_aq = bq_helper.BigQueryHelper(active_project="bigquery-public-data",
                                   dataset_name="openaq")

# print all the tables in this dataset (there's only one!)
open_aq.list_tables()
```

### Out [1]

```python
Using Kaggle's public dataset BigQuery integration.
['global_air_quality']
```

我们可以查看前几行来观察这个数据集中的数据类型是什么。

### In [2]

```python
# print the first couple rows of the "global_air_quality" dataset
open_aq.head("global_air_quality")
```

### Out [2]

<table class="dataframe" border="1">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>location</th>
      <th>city</th>
      <th>country</th>
      <th>pollutant</th>
      <th>value</th>
      <th>timestamp</th>
      <th>unit</th>
      <th>source_name</th>
      <th>latitude</th>
      <th>longitude</th>
      <th>averaged_over_in_hours</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>BTM Layout, Bengaluru - KSPCB</td>
      <td>Bengaluru</td>
      <td>IN</td>
      <td>co</td>
      <td>910.00</td>
      <td>2018-02-22 03:00:00+00:00</td>
      <td>µg/m³</td>
      <td>CPCB</td>
      <td>12.912811</td>
      <td>77.60922</td>
      <td>0.25</td>
    </tr>
    <tr>
      <td>1</td>
      <td>BTM Layout, Bengaluru - KSPCB</td>
      <td>Bengaluru</td>
      <td>IN</td>
      <td>no2</td>
      <td>131.87</td>
      <td>2018-02-22 03:00:00+00:00</td>
      <td>µg/m³</td>
      <td>CPCB</td>
      <td>12.912811</td>
      <td>77.60922</td>
      <td>0.25</td>
    </tr>
    <tr>
      <td>2</td>
      <td>BTM Layout, Bengaluru - KSPCB</td>
      <td>Bengaluru</td>
      <td>IN</td>
      <td>o3</td>
      <td>15.57</td>
      <td>2018-02-22 03:00:00+00:00</td>
      <td>µg/m³</td>
      <td>CPCB</td>
      <td>12.912811</td>
      <td>77.60922</td>
      <td>0.25</td>
    </tr>
    <tr>
      <td>3</td>
      <td>BTM Layout, Bengaluru - KSPCB</td>
      <td>Bengaluru</td>
      <td>IN</td>
      <td>pm25</td>
      <td>45.62</td>
      <td>2018-02-22 03:00:00+00:00</td>
      <td>µg/m³</td>
      <td>CPCB</td>
      <td>12.912811</td>
      <td>77.60922</td>
      <td>0.25</td>
    </tr>
    <tr>
      <td>4</td>
      <td>BTM Layout, Bengaluru - KSPCB</td>
      <td>Bengaluru</td>
      <td>IN</td>
      <td>so2</td>
      <td>4.49</td>
      <td>2018-02-22 03:00:00+00:00</td>
      <td>µg/m³</td>
      <td>CPCB</td>
      <td>12.912811</td>
      <td>77.60922</td>
      <td>0.25</td>
    </tr>
  </tbody>
</table>

所有的地方看起来都很棒！让我们一起查询吧。我想从"country"列是US（代表"UnitedStates"）的全部行中筛选出"city"列里的所有值。

>三重引号（"""）有什么用？这些告诉 Python，它们内部的所有内容都是单个字符串，即使我们其中有换行符。换行符不是必需的，但它们使阅读查询变得更加容易。

### In [3]

```python
 # query to select all the items from the "city" column where the
# "country" column is "us"
query = """SELECT city
            FROM `bigquery-public-data.openaq.global_air_quality`
            WHERE country = 'US'
        """
```

现在我可以使用这个查询从我们的open_aq数据集中获取信息。我正在使用`BigQueryHelper.query_to_pandas_safe()`这个方法，因为如果这个数据集太大的话这个查询不能正常进行。很快我们就会有更多相关这方面的说明。

### In [4]

```python
# the query_to_pandas_safe will only return a result if it's less
# than one gigabyte (by default)
us_cities = open_aq.query_to_pandas_safe(query)
```

现在我有一个熊猫数据模型叫us_cities，我可以像使用其他数据模型一样使用：

### In [5]

```python
# What five cities have the most measurements taken there?
us_cities.city.value_counts().head()
```

### Out [5]

```text
Phoenix-Mesa-Scottsdale                     87
Houston                                     80
New York-Northern New Jersey-Long Island    60
Los Angeles-Long Beach-Santa Ana            60
Riverside-San Bernardino-Ontario            59
Name: city, dtype: int64
```

如果你想要多个列，你可以在名称之间选择一列。

### In [6]

```SQL
query = """SELECT city, country
            FROM `bigquery-public-data.openaq.global_air_quality`
            WHERE country = 'US'
        """
```

你可以用 `*` 选择所有列的数据，像下面这样：

### In [7]

```python
query = """SELECT *
            FROM `bigquery-public-data.openaq.global_air_quality`
            WHERE country = 'US'
        """
```

## 处理大数据集

BigQuery数据集可以是巨大的。我们允许你免费做大量计算，但是每个人都是有限制的。

**每位Kaggle用户每30天可以免费扫描5TB。一旦你达到了这个极限，你将不得不等待它它重置。**

[Kaggle上当前最大的数据集](https://www.kaggle.com/github/github-repos)是3tb,所以如果不小心的话，可以通过几个查询来完成30天的限制。

不过不要担心：如果使用`query_to_pandas_safe`,你就不会一次提取太多的数据并运行超过你的限制。

另一种小心靠谱的方法就是：在实际查询之前估计查询的大小。你可以使用`BigQueryHelper.estimate_query_size()`方法来实现这一点。

这远远要比依赖于你对查询大小的直觉要好，因为你的配额是基于扫描的数据，而不是返回的数据量。而且，要知道一个数据库需要“扫描”多少数据才能重新调整结果是很棘手的，即使你很清楚结果会有多大。

下面是一个使用大数据集的工作流程示例：

### In [8]

```python
# this query looks in the full table in the hacker_news
# dataset, then gets the score column from every row where 
# the type column has "job" in it.
query = """SELECT score
            FROM `bigquery-public-data.hacker_news.full`
            WHERE type = "job" """

# check how big this query will be
hacker_news.estimate_query_size(query)
```

```text
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-8-78578bf56016> in <module>
      7 
      8 # check how big this query will be
----> 9 hacker_news.estimate_query_size(query)

NameError: name 'hacker_news' is not defined
```

`query_to_pandas_safe`有一个可选参数，用于指定你愿意为任何特定查询保留多少数据。

### In [9]

```python
# only run this query if it's less than 100 MB
hacker_news.query_to_pandas_safe(query, max_gb_scanned=0.1)
```

下面是一个同样的查询返回一个数据帧的例子。

### In [10]

```python
# check out the scores of job postings (if the 
# query is smaller than 1 gig)
job_post_scores = hacker_news.query_to_pandas_safe(query)
```

```text
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-10-f9fbfaffa5d5> in <module>
      1 # check out the scores of job postings (if the
      2 # query is smaller than 1 gig)
----> 3 job_post_scores = hacker_news.query_to_pandas_safe(query)

NameError: name 'hacker_news' is not defined
```

我们可以像处理其他任何数据帧一样处理生成的数据帧。例如，我们可以得到列的均值：

### In [11]

```python
# average score for job posts
job_post_scores.score.mean()
```

```text
---------------------------------------------------------------------------
NameError                                 Traceback (most recent call last)
<ipython-input-11-e2f61ed8efd9> in <module>
      1 # average score for job posts
----> 2 job_post_scores.score.mean()

NameError: name 'job_post_scores' is not defined
```

## Your turn

编写SELECT语句是使用SQL的关键，所以[尝试一下你的新技能](https://www.kaggle.com/kernels/fork/681989)。
