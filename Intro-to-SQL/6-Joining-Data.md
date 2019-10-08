# Joining Data

## 介绍

你拥有从某个表中以任何格式获取数据的工具。但是，如果你想要的数据分布在多个表中呢？

这就是**JOIN**的作用！**JOIN**在实际工作流中非常重要。开始我们的学习吧。

## 例子

我们将使用假想的**pets**表，它有三列：

* ID-宠物的ID号码
* Name-宠物的名字
* Animal-宠物的类型
  
我们还将添加另一个表，称为**owners**。它也有三列：

* ID-主人的ID号（与宠物的ID号不同）
* Name-主人的名字
* Pet_ID-属于所有者的宠物的ID号（与**pets**表中宠物的ID号匹配）
  
为了获取适用于特定宠物的信息，我们将**pets**表中的**ID**列与**owners**表中的**Pet_ID**列匹配。

例如，

* **pets**表显示，Dr. Harris Bonkers是ID为1的宠物。
* **owners**表显示，Aubrey Little是ID为1的宠物的所有者。

把这两个事实放在一起，Dr. Harris Bonkers是归Aubrey Little所拥有。

幸运的是，我们不需要手工来确定哪个主人和哪个宠物和哪个主人一起。在接下来的一部分，你将学习如何使用**JOIN**创建一个新表，将**pets**表和**owners**表中的信息组合在一起。

## JOIN

使用**JOIN**，我们可以编写一个查询来创建一个只有两列的表：名称为pet和名称为owners。

我们通过匹配**pets**表中的**ID**列与**owners**表中的**Pet_ID**列匹配的行来组合来自两个表的信息。

在查询中，**ON**确定每个表中的哪一列用于组合表。注意，由于**ID**列存在于这两个表中，我们必须明确使用哪个列。我们使用**p.ID**来引用**pets**表中的**ID**列，以及**o.Pet_ID**引用**owners**表中的**Pet_ID**列。

通常情况下，在连接表时，指定每个列来自哪个表是个好习惯。这样的话，你就不必每次返回读取查询时都打开模式。

我们今天使用的**连接**类型称为**内部连接**。这意味着，只有当用于组合它们的列中的值出现在要连接的两个表中时，才会将一行放入最终的输出表中。例如，如果在**pets**表中不存在Tom的ID数字4，那么我们只会从这个查询中返回3行，还有其他类型的**连接**，但是**内部连接**使用非常广泛，所有这是一个很好的开始。

## 例子：每种类型的软件许可证涵盖多少个文件？

**GitHub**是最流行的软件项目协作平台。一个GitHub存储库（或repo）是与特定项目关联的文件集合。

GitHub上的大多数repos都是在特定的法律许可下共享的，这就决定了如何使用它们的法律限制。对于我们的示例，我们将查看在每个许可证下发布了多少个不同的文件。

我们将使用数据库中的两个表。第一个表是**licenses**表，它提供了每个GitHub repo的名称（在**repo_name**列中）及其相应的许可证。这是前五行的视图。

### In [1]

```SQL
from google.cloud import bigquery

# Create a "Client" object
client = bigquery.Client()

# Construct a reference to the "github_repos" dataset
dataset_ref = client.dataset("github_repos", project="bigquery-public-data")

# API request - fetch the dataset
dataset = client.get_dataset(dataset_ref)

# Construct a reference to the "licenses" table
licenses_ref = dataset_ref.table("licenses")

# API request - fetch the table
licenses_table = client.get_table(licenses_ref)

# Preview the first five lines of the "licenses" table
client.list_rows(licenses_table, max_results=5).to_dataframe()
```

```text
Using Kaggle's public dataset BigQuery integration.
```

### Out [1]

<table class="dataframe" border="1">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>repo_name</th>
      <th>license</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>azuredream/chat_server-client</td>
      <td>artistic-2.0</td>
    </tr>
    <tr>
      <td>1</td>
      <td>Egyptian19/JemCraft</td>
      <td>artistic-2.0</td>
    </tr>
    <tr>
      <td>2</td>
      <td>ZioRiP/cookie</td>
      <td>artistic-2.0</td>
    </tr>
    <tr>
      <td>3</td>
      <td>ajs/perl6-log</td>
      <td>artistic-2.0</td>
    </tr>
    <tr>
      <td>4</td>
      <td>JohanPotgieter/Internet</td>
      <td>artistic-2.0</td>
    </tr>
  </tbody>
</table>

第二个表是**sample_files**表，其中提供了每个文件所属的GitHub repo（在**repo_name**列中）。下面打印了该表的前几行。

### In [2]

```python
# Construct a reference to the "sample_files" table
files_ref = dataset_ref.table("sample_files")

# API request - fetch the table
files_table = client.get_table(files_ref)

# Preview the first five lines of the "sample_files" table
client.list_rows(files_table, max_results=5).to_dataframe()
```

### Out [2]

<table class="dataframe" border="1">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>repo_name</th>
      <th>ref</th>
      <th>path</th>
      <th>mode</th>
      <th>id</th>
      <th>symlink_target</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>git/git</td>
      <td>refs/heads/master</td>
      <td>RelNotes</td>
      <td>40960</td>
      <td>62615ffa4e97803da96aefbc798ab50f949a8db7</td>
      <td>Documentation/RelNotes/2.10.0.txt</td>
    </tr>
    <tr>
      <td>1</td>
      <td>np/ling</td>
      <td>refs/heads/master</td>
      <td>tests/success/plug_compose.t/plug_compose.ll</td>
      <td>40960</td>
      <td>0c1605e4b447158085656487dc477f7670c4bac1</td>
      <td>../../../fixtures/all/plug_compose.ll</td>
    </tr>
    <tr>
      <td>2</td>
      <td>np/ling</td>
      <td>refs/heads/master</td>
      <td>fixtures/strict-par-success/parallel_assoc_lef...</td>
      <td>40960</td>
      <td>b59bff84ec03d12fabd3b51a27ed7e39a180097e</td>
      <td>../all/parallel_assoc_left.ll</td>
    </tr>
    <tr>
      <td>3</td>
      <td>np/ling</td>
      <td>refs/heads/master</td>
      <td>fixtures/sequence/parallel_assoc_2tensor2_left.ll</td>
      <td>40960</td>
      <td>f29523e3fb65702d99478e429eac6f801f32152b</td>
      <td>../all/parallel_assoc_2tensor2_left.ll</td>
    </tr>
    <tr>
      <td>4</td>
      <td>np/ling</td>
      <td>refs/heads/master</td>
      <td>fixtures/success/my_dual.ll</td>
      <td>40960</td>
      <td>38a3af095088f90dfc956cb990e893909c3ab286</td>
      <td>../all/my_dual.ll</td>
    </tr>
  </tbody>
</table>

接下来我们编写一个查询，它使用两个表中的信息来确定每个许可证中释放了多少文件。

### In [3]

```python
# Query to determine the number of files per license, sorted by number of files
query = """
        SELECT L.license, COUNT(1) AS number_of_files
        FROM `bigquery-public-data.github_repos.sample_files` AS sf
        INNER JOIN `bigquery-public-data.github_repos.licenses` AS L 
            ON sf.repo_name = L.repo_name
        GROUP BY L.license
        ORDER BY number_of_files DESC
        """

# Set up the query (cancel the query if it would use too much of 
# your quota, with the limit set to 10 GB)
safe_config = bigquery.QueryJobConfig(maximum_bytes_billed=10**10)
query_job = client.query(query, job_config=safe_config)

# API request - run the query, and convert the results to a pandas DataFrame
file_count_by_license = query_job.to_dataframe()
```

这是一个很大的查询，所以我们将分别研究每个部分。

我们将从**JOIN**（上面用蓝色高亮显示）开始。这指定了数据的来源以及如何连接它们。我们使用**ON**指定通过匹配表中的**repo_name**列中的值来组合表。

接下来我们将讨论**SELECT**和**GROUP BY**（用黄色突出显示）。在统计**sample_files**表中对应于每个许可证的行数之前，**GROUP BY**将每个许可证的数据分成不同的组。（记住，可以使用**COUNT(1)**计算行数。）

最后，**ORDER BY**（用紫色突出显示）对结果进行排序，以便先处理包含更多文件的许可证。

这是一个很大的查询，但它给了我们一个很好的表格，总结了每个协议下允许下提交多少文件：

### In [4]

```python
# Print the DataFrame
file_count_by_license
```

### Out [4]

<table class="dataframe" border="1">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>license</th>
      <th>number_of_files</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>mit</td>
      <td>20615554</td>
    </tr>
    <tr>
      <td>1</td>
      <td>gpl-2.0</td>
      <td>17112844</td>
    </tr>
    <tr>
      <td>2</td>
      <td>apache-2.0</td>
      <td>7225328</td>
    </tr>
    <tr>
      <td>3</td>
      <td>gpl-3.0</td>
      <td>4954512</td>
    </tr>
    <tr>
      <td>4</td>
      <td>bsd-3-clause</td>
      <td>2945518</td>
    </tr>
    <tr>
      <td>5</td>
      <td>agpl-3.0</td>
      <td>1297463</td>
    </tr>
    <tr>
      <td>6</td>
      <td>lgpl-2.1</td>
      <td>800634</td>
    </tr>
    <tr>
      <td>7</td>
      <td>bsd-2-clause</td>
      <td>700337</td>
    </tr>
    <tr>
      <td>8</td>
      <td>lgpl-3.0</td>
      <td>567189</td>
    </tr>
    <tr>
      <td>9</td>
      <td>mpl-2.0</td>
      <td>469300</td>
    </tr>
    <tr>
      <td>10</td>
      <td>cc0-1.0</td>
      <td>404796</td>
    </tr>
    <tr>
      <td>11</td>
      <td>epl-1.0</td>
      <td>322484</td>
    </tr>
    <tr>
      <td>12</td>
      <td>unlicense</td>
      <td>209289</td>
    </tr>
    <tr>
      <td>13</td>
      <td>artistic-2.0</td>
      <td>148414</td>
    </tr>
    <tr>
      <td>14</td>
      <td>isc</td>
      <td>117807</td>
    </tr>
  </tbody>
</table>

你将会经常使用**JOIN**子句，并且随着你的实验，你将会非常高效地使用它们。

## Your turn

现在你就剩最后一步了。通过解决[这些练习](https://www.kaggle.com/kernels/fork/682118)来完成它。
