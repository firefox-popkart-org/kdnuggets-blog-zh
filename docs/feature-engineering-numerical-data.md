# 数值数据的特征工程

> 原文：[`www.kdnuggets.com/2020/09/feature-engineering-numerical-data.html`](https://www.kdnuggets.com/2020/09/feature-engineering-numerical-data.html)

评论

数值数据几乎是一个福音。为什么说几乎呢？因为它已经是机器学习模型可以处理的格式。然而，如果我们将其翻译成易于理解的术语，仅仅因为一本博士级教材是用英语写的——我会说、读和写英语——并不意味着我能够充分理解这本教材以得出有用的见解。使教材对我有用的，应该是它以一种考虑到我心理模型假设的方式总结最重要的信息，比如“数学是一种神话”（顺便说一下，我现在已经不再这样看待了，因为我真的开始享受数学了）。同样，一个好的特征应该能够代表数据的显著方面，并且符合机器学习模型所做的假设。

![数据工程](img/efe8223e7c92f882a8d32fd9e5e37cb1.png)

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业的轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 部门

* * *

[特征工程](https://www.kdnuggets.com/2018/12/feature-engineering-explained.html)是从原始数据中提取特征并将其转换为机器学习模型可以处理的格式的过程。通常需要进行转换以降低建模难度并提升模型结果。因此，工程化数值数据的技术是数据科学家（机器学习工程师也一样）必须掌握的基本工具。

> “数据就像是机器学习的*原油*，这意味着它必须被提炼成*特征*——预测变量——才能对训练模型有用。” — [Will Koehrsen](https://medium.com/u/e2f299e30cb9?source=post_page-----e20167ec18----------------------)

在追求精通的过程中，重要的是要注意，仅仅知道一个机制如何工作以及它能做什么是不够的。精通意味着知道某事如何完成，对基本原理有直觉，并具备在面对挑战时快速选择正确工具的神经连接。这不会仅仅通过阅读这篇文章来获得，而是通过有意识的练习来实现，本文将为你提供这些技术背后的直觉，以便你理解如何以及何时应用它们。

“数据中的特征将直接影响你使用的预测模型以及你能取得的结果。” — [杰森·布朗利](https://medium.com/u/f374d0159316?source=post_page-----e20167ec18----------------------)

*注意：你可以在我的 [Github 页面](https://github.com/kurtispykes/demo/tree/master) 找到用于本文的代码。*

可能会有数据收集到一个特征上，这个特征是累积的，因此具有无限的上限。这种类型的连续数据的例子可能是一个跟踪系统，监控我所有博客帖子每日收到的访问次数。这种数据容易吸引异常值，因为可能会有一些不可预测的事件影响我文章的总流量。例如，有一天，人们可能决定他们想进行数据分析，因此我关于 [有效数据可视化](https://towardsdatascience.com/effective-data-visualization-ef30ae560961) 的文章当天可能会出现激增。换句话说，当数据可以快速且大量收集时，那么它很可能包含一些需要工程处理的极端值。

处理这种情况的一些方法包括：

### 量化

这种方法通过将数据值分组到区间中来包含数据的尺度。因此，量化将连续值映射到离散值，从概念上讲，可以将其视为有序的区间序列。为了实现这一点，我们必须考虑创建区间的宽度，这些解决方案分为两类：固定宽度区间或自适应区间。

> *注意：这对线性模型特别有用。在基于树的模型中，这并不有用，因为基于树的模型会自行进行分割。*

在固定宽度的情况下，值被自动或自定义设计为将数据分割成离散的区间——这些区间也可以是线性缩放或指数缩放的。一个常见的例子是将人的年龄按十年间隔分区，例如区间 1 包含 0–9 岁，区间 2 包含 10–19 岁，等等。

注意，如果值跨越了很大的数值范围，那么一个更好的方法可能是将值分组为常数的幂，例如 10 的幂：0–9、10–99、100–999、1000–9999。注意分箱宽度呈指数增长，因此在 1000–9999 的情况下，分箱宽度为 O(10000)，而 0–9 为 O(10)。取计数的对数以将计数映射到数据的分箱中。

```py
import numpy as np 

#15 random integers from the "discrete uniform" distribution
ages = np.random.randint(0, 100, 15)

#evenly spaced bins
ages_binned = np.floor_divide(ages, 10)

print(f"Ages: {ages} \nAges Binned: {ages_binned} \n")
>>> Ages: [97 56 43 73 89 68 67 15 18 36  4 97 72 20 35]
Ages Binned: [9 5 4 7 8 6 6 1 1 3 0 9 7 2 3]

#numbers spanning several magnitudes
views = [300, 5936, 2, 350, 10000, 743, 2854, 9113, 25, 20000, 160, 683, 7245, 224]

#map count -> exponential width bins
views_exponential_bins = np.floor(np.log10(views))

print(f"Views: {views} \nViews Binned: {views_exponential_bins}")
>>> Views: [300, 5936, 2, 350, 10000, 743, 2854, 9113, 25, 20000, 160, 683, 7245, 224]
Views Binned: [2\. 3\. 0\. 2\. 4\. 2\. 3\. 3\. 1\. 4\. 2\. 2\. 3\. 2.]

```

当计数之间存在较大间隙时，自适应分箱更合适。当计数值之间存在较大差距时，一些固定宽度的分箱可能会为空。

要进行自适应分箱，我们可以利用数据的分位数——将数据划分为相等部分的值，例如中位数。

```py
import pandas as pd

#map the counts to quantiles (adaptive binning)
views_adaptive_bin = pd.qcut(views, 5, labels=False)

print(f"Adaptive bins: {views_adaptive_bin}")
>>> Adaptive bins: [1 3 0 1 4 2 3 4 0 4 0 2 3 1]

```

### 幂变换

我们已经看到一个例子：对数变换是方差稳定变换家族的一部分，这些变换被称为幂变换。维基百科将幂变换描述为*“用于稳定方差，使数据更接近正态分布，提高变量之间的皮尔逊相关系数等关联测量的有效性，并用于其他数据稳定化过程的技术。”*

为什么我们要将数据转换为符合正态分布？这是个好问题！你可能希望使用参数模型——一种对数据做出假设的模型——而不是非参数模型。当数据呈正态分布时，参数模型非常强大。然而，在某些情况下，我们的数据可能需要一些帮助，以展现正态分布的美丽钟形曲线。例如，数据可能存在偏斜，因此我们应用幂变换以帮助特征看起来更像高斯分布。

下面的代码利用了数据科学框架，如 pandas、scipy 和 numpy，来演示幂变换，并使用 Plotly.py 框架进行交互式图表的可视化。使用的数据集是[*House Prices: Advanced regression techniques*](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data)来自 Kaggle，你可以轻松下载（[点击此处访问数据](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data)）。

```py
import numpy as np
import pandas as pd
from scipy import stats
import plotly.graph_objects as go
from plotly.subplots import make_subplots

df = pd.read_csv("../data/raw/train.csv")

# applying various transformations
x_log = np.log(df["GrLivArea"].copy()) # log 
x_square_root = np.sqrt(df["GrLivArea"].copy()) # square root x_boxcox, _ = stats.boxcox(df["GrLivArea"].copy()) # boxcox
x = df["GrLivArea"].copy() # original data

# creating the figures
fig = make_subplots(rows=2, cols=2,
                    horizontal_spacing=0.125,
                    vertical_spacing=0.125,
                    subplot_titles=("Original Data",
                                    "Log Transformation",
                                    "Square root transformation",
                                    "Boxcox Transformation")
                    )

# drawing the plots
fig.add_traces([
                go.Histogram(x=x,
                             hoverinfo="x",
                             showlegend=False),
                go.Histogram(x=x_log,
                             hoverinfo="x",
                             showlegend=False),
                go.Histogram(x=x_square_root,
                             hoverinfo="x",
                             showlegend=False),
                go.Histogram(x=x_boxcox,
                             hoverinfo="x",
                             showlegend=False),
               ],
               rows=[1, 1, 2, 2],
               cols=[1, 2, 1, 2]
)

fig.update_layout(
    title=dict(
               text="GrLivArea with various Power Transforms",
               font=dict(
                         family="Arial",
                         size=20)),
    showlegend=False,
    width=800,
    height=500)

fig.show() # display figure

```

![](img/0128765b007865d69c64281134a0a389.png)

*图 1：可视化原始数据和各种幂变换。*

*注意：Box-Cox 变换仅在数据为非负数时有效*

> “这些中哪个最好？你无法事先知道。你必须尝试它们并评估结果，以实现你的算法和性能指标。” — [杰森·布朗利](https://medium.com/u/f374d0159316?source=post_page-----e20167ec18----------------------)

### 特征缩放

顾名思义，特征缩放（也称为特征归一化）涉及到改变特征的尺度。当数据集中各特征的尺度差异很大时，敏感于输入特征尺度的模型（如线性回归、逻辑回归、神经网络）将会受到影响。确保特征在相似的尺度内是至关重要的。而像树模型（如决策树、随机森林、梯度提升）这样的模型不关心尺度。

缩放特征的常见方法包括最小-最大缩放、标准化和 L²归一化。以下是简要介绍及其在 Python 中的实现。

**最小-最大缩放** - 特征被缩放到一个固定范围（通常在 0 到 1 之间），这意味着我们将减少标准差，从而抑制异常值对特征的影响。其中 x 是实例的个体值（如，个人 1，特征 2），max(x)，min(x)是特征的最大值和最小值 — 见图 2。有关更多信息，请参见[sklearn 文档](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)。

![](img/157fdaa503f0d1e9470d4abdfecbed6d.png)

*图 2：最小-最大缩放的公式。*

**标准化** - 特征值将被重新缩放，以符合正态分布的性质，其中均值为 0，标准差为 1。为此，我们从特征实例值中减去特征的均值（在所有实例上计算），然后除以方差 — 见图 3。有关标准化，请参阅[sklearn 文档](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html)。

![](img/eb10ee1da49f806d2f3c13d7c09bfc86.png)

*图 3：标准化的公式。*

**L² 归一化** - 该技术将原始特征值除以 l²范数（也称为欧几里得距离）— 图 4 中的第二个公式。L²范数取所有实例中特征集值的平方和。有关 L²范数，请参阅 sklearn 的[文档](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.Normalizer.html#sklearn.preprocessing.Normalizer)（注意，还可以通过将 norm 参数设置为"l1"来进行 L¹归一化）。

![](img/7152bbf318af813b1bb26af628e98fb2.png)

*图 4：L² 归一化的公式。*

> *特征缩放效果的可视化将更好地展示发生了什么。为此，我使用了可以从 sklearn 数据集中导入的葡萄酒数据集。*

```py
import pandas as pd
from sklearn.datasets import load_wine
from sklearn.preprocessing import StandardScaler, MinMaxScaler, Normalizer
import plotly.graph_objects as go

wine_json= load_wine() # load in dataset

df = pd.DataFrame(data=wine_json["data"], columns=wine_json["feature_names"]) # create pandas dataframe

df["Target"] = wine_json["target"] # created new column and added target labels

# standardization
std_scaler = StandardScaler().fit(df[["alcohol", "malic_acid"]])
df_std = std_scaler.transform(df[["alcohol", "malic_acid"]])

# minmax scaling
minmax_scaler = MinMaxScaler().fit(df[["alcohol", "malic_acid"]])
df_minmax = minmax_scaler.transform(df[["alcohol", "malic_acid"]])

# l2 normalization
l2norm = Normalizer().fit(df[["alcohol", "malic_acid"]])
df_l2norm = l2norm.transform(df[["alcohol", "malic_acid"]])

# creating traces
trace1 = go.Scatter(x= df_std[:, 0],
                    y= df_std[:, 1],
                    mode= "markers",
                    name= "Standardized Scale")

trace2 = go.Scatter(x= df_minmax[:, 0],
                    y= df_minmax[:, 1],
                    mode= "markers",
                    name= "MinMax Scale")

trace3 = go.Scatter(x= df_l2norm[:, 0],
                    y= df_l2norm[:, 1],
                    mode= "markers",
                    name= "L2 Norm Scale")

trace4 = go.Scatter(x= df["alcohol"],
                    y= df["malic_acid"],
                    mode= "markers",
                    name= "Original Scale")

layout = go.Layout(
         title= "Effects of Feature scaling",
         xaxis=dict(title= "Alcohol"),
         yaxis=dict(title= "Malic Acid")
         )

data = [trace1, trace2, trace3, trace4]
fig = go.Figure(data=data, layout=layout)
fig.show()

```

![](img/89d92631e43a8a4e64462d398d69b551.png)

*图 5：原始特征和各种缩放实现的图示。*

### 特征交互

我们可以通过使用特征之间的配对交互的乘积来创建逻辑与函数。在基于树的模型中，这些交互是隐式发生的，但在假设特征独立的模型中，我们可以显式地声明特征之间的交互，以提高模型的输出。

设想一个简单的线性模型，该模型使用输入特征的线性组合来预测输出 y：

![](img/bfcaba2049dd879a6c88b78eb18ed3ef.png)

*图 6：线性模型的公式。*

我们可以扩展线性模型以捕捉特征之间发生的交互。

![](img/72d7a01b39fe9a925af4ba2d08d110f7.png)

*图 7：扩展线性模型。*

*注意：线性函数的使用代价较高，线性模型的配对交互的评分和训练复杂度从 O(n) 增加到 O(n²)。然而，你可以进行特征提取来克服这个问题（特征提取超出了本文的范围，但我将在未来的文章中讨论）。*

让我们在 Python 中编写代码，我将利用 scikit-learn 的*PolynomialFeatures*类，你可以在[文档](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.PolynomialFeatures.html)中阅读更多内容：

```py
import numpy as np
from sklearn.preprocessing import PolynomialFeatures

# creating dummy dataset
X = np.arange(10).reshape(5, 2)
X.shape
>>> (5, 2)

# interactions between features only
interactions = PolynomialFeatures(interaction_only=True)
X_interactions= interactions.fit_transform(X)
X_interactions.shape
>>> (5, 4)

# polynomial features 
polynomial = PolynomialFeatures(5)
X_poly = polynomial.fit_transform(X)
X_poly.shape
>>> (5, 6)

```

*这篇文章深受书籍[《机器学习特征工程：数据科学家的原则与技术》](https://www.amazon.co.uk/Feature-Engineering-Machine-Learning-Principles-ebook/dp/B07BNX4MWC)的启发，我强烈推荐阅读。尽管该书于 2016 年出版，但即便是没有数学背景的人也能从中获得非常有用的信息和清晰的解释。*

### 结论

就这样。在这篇文章中，我们讨论了处理数值特征的技术，如量化、幂变换、特征缩放以及交互特征（这些技术可以应用于各种数据类型）。这绝不是特征工程的全部内容，我们每天都可以学到更多。特征工程是一门艺术，需要练习，所以现在你有了直觉，就可以开始实践了。

[原文](https://towardsdatascience.com/feature-engineering-for-numerical-data-e20167ec18)。转载已获许可。

**简历：** [Kurtis Pykes](https://www.linkedin.com/in/kurtispykes/) 是 Codehouse 的机器学习工程实习生。他热衷于利用机器学习和数据科学的力量来帮助人们变得更高效和有效。

**相关：**

+   [SQL 和 Python 中的特征工程：混合方法](https://www.kdnuggets.com/2020/07/feature-engineering-sql-python-hybrid-approach.html)

+   [数据转换：标准化与归一化](https://www.kdnuggets.com/2020/04/data-transformation-standardization-normalization.html)

+   [高级特征工程和预处理的 4 个技巧](https://www.kdnuggets.com/2019/08/4-tips-advanced-feature-engineering-preprocessing.html)

### 了解更多相关主题

+   [2022 特征商店峰会：关于特征工程的免费会议](https://www.kdnuggets.com/2022/10/hopsworks-feature-store-summit-2022-free-conference-feature-engineering.html)

+   [为多变量时间序列构建可处理的特征工程管道](https://www.kdnuggets.com/2022/03/building-tractable-feature-engineering-pipeline-multivariate-time-series.html)

+   [使用 RAPIDS cuDF 在特征工程中利用 GPU](https://www.kdnuggets.com/2023/06/rapids-cudf-leverage-gpu-feature-engineering.html)

+   [机器学习中特征工程的实用方法](https://www.kdnuggets.com/2023/07/practical-approach-feature-engineering-machine-learning.html)

+   [初学者的特征工程](https://www.kdnuggets.com/feature-engineering-for-beginners)

+   [特征选择：科学与艺术的交汇点](https://www.kdnuggets.com/2021/12/feature-selection-science-meets-art.html)
