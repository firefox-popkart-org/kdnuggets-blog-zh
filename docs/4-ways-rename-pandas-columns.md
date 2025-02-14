# 4 种重命名 Pandas 列的方法

> 原文：[`www.kdnuggets.com/2022/11/4-ways-rename-pandas-columns.html`](https://www.kdnuggets.com/2022/11/4-ways-rename-pandas-columns.html)

![4 种重命名 Pandas 列的方法](img/519d378f8fcac370c0bd52bdb2897606.png)

图片由作者提供

Pandas DataFrame 现在已经成为主流。每个人都在使用它进行数据分析、机器学习、数据工程，甚至软件开发。学习重命名列是数据清洗的第一步，这是数据分析的核心部分。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你组织的 IT 工作

* * *

在这个小教程中，我们将回顾四种方法，帮助你重命名单个或多个列名。

+   **方法 1**：使用 rename() 函数。

+   **方法 2**：分配新列名的列表。

+   **方法 3**：替换列字符串。

+   **方法 4**：使用 set_axis() 函数。

# 创建 Pandas DataFrame

我们将首先创建一个简单的学生班级表现字典。它包括三列：“id”、“name”和“grade”，以及五行数据。

要将 Python 字典转换为 pandas DataFrame，我们将使用 pandas **DataFrame()** 函数，并通过 Deepnote（Cloud Jupyter Notebook）显示结果。

> **注意**：我们将多次使用 `student_dict` 字典为每种方法创建一个 DataFrame。

# 方法 1

第一种方法相当简单。我们将使用 pandas `rename()` 函数来重新标记列名。

## 重命名单个列

在这个例子中，我们将使用 `.rename()` 重命名单个列。你只需将 **旧** 和 **新** 列名的字典提供给 `columns` 参数。

**例如**：{“旧列名” : “新列名”}

正如我们所观察到的，我们已经成功地将“id”替换为“ID”。

```py
student_df_1.rename(columns={"id": "ID"}, inplace=True)

student_df_1
```

![4 种重命名 Pandas 列的方法](img/ac9f75094db66255a84adcde5048420b.png)

> **注意：** `inplace = True` 意味着我们正在对 DataFrame 应用更改。这类似于 `df = df.rename()`

## 重命名多个列

对于多个列，我们只需提供旧列名和新列名的字典，用逗号 **“，”** 分隔，它将自动替换列名。

新的列名为 "Student_ID", "First_Name", 和 "Average_Grade"。

```py
student_df_1.rename(
    columns={"ID": "Student_ID", "name": "First_Name", "grade": "Average_Grade"},
    inplace=True,
)

student_df_1
```

![4 种重命名 Pandas 列的方法](img/ab79b1d3206726bdc163f5c561ceba51.png)

# 方法 2

第二种方法很简单。我们将通过将新名称列表分配给 DataFrame 对象的 `columns` 属性来重命名列。

例如，我们使用字典创建了一个新的数据框，并通过提供字符串列表给列属性来重命名列。

```py
student_df_2 = pd.DataFrame(student_dict)
student_df_2.columns = ["Student_ID", "First_Name", "Average_Grade"]

student_df_2
```

![重命名 Pandas 列的 4 种方法](img/2de14c257b734139b58d732bd0b88d9a.png)

# 方法 3

第三种方法是 Python 生态系统中原生的，我们替换 `columns` 属性的字符串。

**例如**：`df = df.columns.str.replace("old_name", "new_name")`

我们已经成功将列名更改为“ID”、“Name”和“Grades”。

```py
student_df_3 = pd.DataFrame(student_dict)

student_df_3.columns = student_df_3.columns.str.replace("id", "ID")
student_df_3.columns = student_df_3.columns.str.replace("name", "Name")
student_df_3.columns = student_df_3.columns.str.replace("grade", "Grades")

student_df_3
```

![重命名 Pandas 列的 4 种方法](img/940f1cbef5e0255c5b64774f268b79ff.png)

# 方法 4

在第四种方法中，我们将使用 `set_axis()` 函数重命名列。你需要提供一个新名称的列表，并设置 `axis = ”columns”` 以重命名列而不是索引。

```py
student_df_4 = pd.DataFrame(student_dict)
student_df_4.set_axis(["A", "B", "C"], axis="columns", inplace=True)

student_df_4
```

![重命名 Pandas 列的 4 种方法](img/186608f880b6ae105d9dbcd751809edc.png)

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一名认证的数据科学专业人士，热爱构建机器学习模型。目前，他专注于内容创作，并撰写有关机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是利用图神经网络为心理健康问题困扰的学生开发一个 AI 产品。

### 相关主题

+   [如何使用 [ ], .loc, iloc, .at… 选择 Pandas 中的行和列](https://www.kdnuggets.com/2019/06/select-rows-columns-pandas.html)

+   [五种 Pandas 条件筛选的方法](https://www.kdnuggets.com/2022/12/five-ways-conditional-filtering-pandas.html)

+   [3 种合并 Pandas DataFrames 的方法](https://www.kdnuggets.com/2023/03/3-ways-merge-pandas-dataframes.html)

+   [3 种将行追加到 Pandas DataFrames 的方法](https://www.kdnuggets.com/2022/08/3-ways-append-rows-pandas-dataframes.html)

+   [5 种不同的 Python 数据加载方法](https://www.kdnuggets.com/2020/08/5-different-ways-load-data-python.html)

+   [5 种 AI 在供应链管理中的应用方式](https://www.kdnuggets.com/2022/02/5-ways-ai-supply-chain-management.html)
