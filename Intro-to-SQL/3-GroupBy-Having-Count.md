# Group By, Having & Count

## 介绍

既然你可以选择原始数据，你已经准备好学习如何对数据进行分组并在这些组中计数。这可以帮助你解决以下类似问题：

* 我们商店每种水果卖了多少？
* 兽医诊所治疗过多少种类的动物？

想要要做到这一点，你将学习这三个新技巧：**GROUP BY** ,**HAVING**和**COUNT()**。再一次，我们将使用这个虚构的关于宠物的信息表。

## COUNT()

**COUNT()**,顾名思义，返回一些列的数。如果将列的名称传递给它，它将返回该列中的条目数。

例如，如果我们在**pets**表中**选择****ID**列的**COUNT()**，它将返回4，因为表中有4个ID.

COUNT()是一个**聚合函数**的一个例子，它接受许多值并返回一个值。（聚合函数的其他示例包括**SUM()**, **AVG()**, **MIN()**, and **MAX()**）正如你在上面的图中所注意到的，聚合函数引入了奇怪的列名（如**f0__**）。在本教程的后面，你将学习如何将名称更改为更具描述性的名称。

## GROUP BY

GROUP BY接受一个或多个列的名称，当应用COUNT()之类的聚合函数时，将该列中具有相同值的所有行视为一个单独的组。

例如，如果我们想知道在**pets**表中每种动物的数量，我们可以使用**GROUP BY**把**Animal**列中具有相同的值的行归为一类放在一起，通过使用**COUNT()**找出多少**ID**在每一个组中。

它返回一个包含三行的表（每一行对应一个不同的动物）。我们可以看到**pets**表包含1只兔子、1只狗和2只猫。

## GROUP BY ... HAVING

**HAVING**与**GROUP BY**结合使用，用于忽略不满足特定条件的组。所以这种查询，例如，将只包含多个ID的组。由于只有一个组满足指定的条件，查询将返回一个只有一行的表。

## 例子：哪条黑客新闻评论引发的讨论最多？

准备好查看真实数据集中的示例了马？黑客新闻数据集中包含来自黑客新闻社交网站的故事和评论信息。

我们将使用**comments**表，首先打印前几行。（我们已经隐藏了相应的代码。来看一眼，点击下面的**Code**按钮。）

### In [1]

```python
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()

# Construct a reference to the "hacker_news" dataset
dataset_ref = client.dataset("hacker_news", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)

# Construct a reference to the "comments" table
table_ref = dataset_ref.table("comments")

# API request - fetch the table
table = client.get_table(table_ref)

# Preview the first five lines of the "comments" table
client.list_rows(table, max_results=5).to_dataframe()
```

```text
Using Kaggle's public dataset BigQuery integration.
```

<table class="dataframe" border="1">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id</th>
      <th>by</th>
      <th>author</th>
      <th>time</th>
      <th>time_ts</th>
      <th>text</th>
      <th>parent</th>
      <th>deleted</th>
      <th>dead</th>
      <th>ranking</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>2701393</td>
      <td>5l</td>
      <td>5l</td>
      <td>1309184881</td>
      <td>2011-06-27 14:28:01+00:00</td>
      <td>And the glazier who fixed all the broken windo...</td>
      <td>2701243</td>
      <td>None</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>5811403</td>
      <td>99</td>
      <td>99</td>
      <td>1370234048</td>
      <td>2013-06-03 04:34:08+00:00</td>
      <td>Does canada have the equivalent of H1B/Green c...</td>
      <td>5804452</td>
      <td>None</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>21623</td>
      <td>AF</td>
      <td>AF</td>
      <td>1178992400</td>
      <td>2007-05-12 17:53:20+00:00</td>
      <td>Speaking of Rails, there are other options in ...</td>
      <td>21611</td>
      <td>None</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>10159727</td>
      <td>EA</td>
      <td>EA</td>
      <td>1441206574</td>
      <td>2015-09-02 15:09:34+00:00</td>
      <td>Humans and large livestock (and maybe even pet...</td>
      <td>10159396</td>
      <td>None</td>
      <td>None</td>
      <td>0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2988424</td>
      <td>Iv</td>
      <td>Iv</td>
      <td>1315853580</td>
      <td>2011-09-12 18:53:00+00:00</td>
      <td>I must say I reacted in the same way when I re...</td>
      <td>2988179</td>
      <td>None</td>
      <td>None</td>
      <td>0</td>
    </tr>
  </tbody>
</table>

### Out [1]

输出输出输出输出

让我们使用这个表来查看哪些评论产生了最多的回复。即：

- **parent**列表示被回复的评论

- **id**列有唯一的ID用于标识每个评论。

我们可以根据**parent**列进行**分组**，并**计算**相应的**ID**列，以便计算作为对特定注释的响应而做出的注释的数量。（这可能不会马上说得通———在这点浪费点时间确保一切都是清楚的）

此外，由于我们只对流行评论感兴趣，所以我们将查看超过10条回复的评论。因此，我们只返回ID大于10的组。

### In [2]

```python
# Query to select comments that received more than 10 replies
query_popular = """
                SELECT parent, COUNT(id)
                FROM `bigquery-public-data.hacker_news.comments`
                GROUP BY parent
                HAVING COUNT(id) > 10
                """
```

既然我们的查询已经准备好了，让我们运行它并将结果存储在一个熊猫 数据帧中。

### In [3]

```python
# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 10 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
query_job = client.query(query_popular, job_config=safe_config)

# API request - run the query, and convert the results to a pandas DataFrame
popular_comments = query_job.to_dataframe()

# Print the first five rows of the DataFrame
popular_comments.head()
```

### Out [3]

<table class="dataframe" border="1">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>parent</th>
      <th>f0_</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>1822253</td>
      <td>174</td>
    </tr>
    <tr>
      <td>1</td>
      <td>9680982</td>
      <td>77</td>
    </tr>
    <tr>
      <td>2</td>
      <td>6739074</td>
      <td>41</td>
    </tr>
    <tr>
      <td>3</td>
      <td>6640430</td>
      <td>62</td>
    </tr>
    <tr>
      <td>4</td>
      <td>2665090</td>
      <td>61</td>
    </tr>
  </tbody>
</table>

**popular_comments**数据帧中的每一行对应一条接收超过10条的回复评论。例如，ID为**801208**的评论收到了**56**条回复。

## 混叠和其他改进

这里有几个提示让你的查询更好：

* **COUNT(id)**产生的列称为**f0__**.这不是一个描述性很强的名字。在你指定相应的聚合之后，你可以通过添加**AS NumPosts**来更改名字。这叫做**混叠**，在 下一节课中会详细地介绍。
* 如果你不确定要在 **COUNT()**中放入什么，你可以使用**COUNT(1)**来计算每个组中的行数。大多数人觉得它特别易读，因为我们知道它不关注其他列。与提供列名相比，它还扫描更少的数据（使其更快且使用更少的数据访问配额）。

使用这些技巧，我们可以重写我们的查询：

### In [4]

```SQL
# Improved version of earlier query, now with aliasing & improved readability
query_improved = """
                 SELECT parent, COUNT(1) AS NumPosts
                 FROM `bigquery-public-data.hacker_news.comments`
                 GROUP BY parent
                 HAVING COUNT(1) > 10
                 """

safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
query_job = client.query(query_improved, job_config=safe_config)

# API request - run the query, and convert the results to a pandas DataFrame
improved_df = query_job.to_dataframe()

# Print the first five rows of the DataFrame
improved_df.head()
```

### Out [4]

<table class="dataframe" border="1">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>parent</th>
      <th>NumPosts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>4684384</td>
      <td>87</td>
    </tr>
    <tr>
      <td>1</td>
      <td>6584683</td>
      <td>47</td>
    </tr>
    <tr>
      <td>2</td>
      <td>9616946</td>
      <td>78</td>
    </tr>
    <tr>
      <td>3</td>
      <td>7750036</td>
      <td>57</td>
    </tr>
    <tr>
      <td>4</td>
      <td>8185461</td>
      <td>63</td>
    </tr>
  </tbody>
</table>

现在你有了想要的数据，它有描述性的名字。这是很好的风格。

## 使用GROUP BY的注意事项

注意，因为它告诉SQL应用聚合函数（像**COUNT（）**），所以在没有聚合函数的情况下使用**GROUP BY**是没有意义的。同样地，如果你有任何**GROUP BY**子句，那么所有变量都必须传递给任意一个。

1. 按命令分组
2. 一个聚合函数

思考下面的查询：

### In [5]

```SQL
query_good = """
             SELECT parent, COUNT(id)
             FROM `bigquery-public-data.hacker_news.comments`
             GROUP BY parent
             """
```

注意有两个变量：**parent**和**id**变量。

* **parent**通过命令传递给组（在**GROUP BY parent**中）
* **id**传递给聚合函数（在**COUNT(id**）中）

### In [6]

```python
query_bad = """
            SELECT author, parent, COUNT(id)
            FROM `bigquery-public-data.hacker_news.comments`
            GROUP BY parent
            """
```

如果发生此错误，你将得到错误消息**SELECT list expression references column (column's name) which is neither grouped nor aggregated at**。

## Your turn

这些聚合使你可以编写更有趣地查询，用这些编码试着[自己练习练习](https://www.kaggle.com/kernels/fork/682058)。
