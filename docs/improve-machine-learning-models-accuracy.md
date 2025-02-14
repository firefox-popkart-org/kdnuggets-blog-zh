# 如何将我的机器学习模型准确率从 80%持续提高到 90%以上

> 原文：[`www.kdnuggets.com/2020/09/improve-machine-learning-models-accuracy.html`](https://www.kdnuggets.com/2020/09/improve-machine-learning-models-accuracy.html)

评论

![](img/13a446e223542c95af0d8db39d2b365b.png)

*照片由 [Ricardo Arce](https://unsplash.com/@jrarce?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，发布在 [Unsplash](https://unsplash.com/s/photos/target?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上。*

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业道路。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT 需求

* * *

如果你完成了一些数据科学项目，那么你可能已经意识到，80%的准确率还算不错！但在现实世界中，80%是不够的。实际上，我工作过的大多数公司都期望最低准确率（或他们关注的其他指标）达到 90%以上。

因此，我将讨论 5 个可以显著提高准确性的做法。**我强烈建议你仔细阅读这五点**，因为我包含了许多初学者不知道的细节。

到了这一步，你应该明白影响机器学习模型表现的变量远比你想象的要多。

话虽如此，下面是 5 件你可以做的事情来改善你的机器学习模型！

### 1\. 处理缺失值

我看到的一个最大错误是人们如何处理缺失值，这不一定是他们的错。很多网上的资料说，你通常通过**均值填补**来处理缺失值，即用给定特征的均值替换空值，但这通常不是最佳方法。

例如，假设我们有一个显示年龄和健身分数的表格，并且假设一个 80 岁的人缺少健身分数。如果我们从 15 岁到 80 岁的年龄范围中计算平均健身分数，那么 80 岁的人看起来会有比实际更高的健身分数。

因此，你首先要问自己的是**为什么**数据最初会缺失。

接下来，考虑处理缺失数据的其他方法，而不仅仅是均值/中位数填补：

+   **特征预测建模**：回到我关于年龄和健身评分的例子，我们可以建模年龄和健身评分之间的关系，然后使用该模型找到给定年龄的预期健身评分。这可以通过回归、ANOVA 等多种技术实现。

+   **K 最近邻插补**：使用 KNN 插补，缺失的数据会用另一个类似样本的值来填补，对于那些不知道的人，KNN 中的相似性是通过距离函数（即欧几里得距离）来确定的。

+   **删除行**：最后，你可以删除行。通常不推荐这样做，但当你开始时有**大量**数据时，这是可以接受的。

### 2\. 特征工程

你可以显著提升机器学习模型的第二种方式是通过特征工程。特征工程是将原始数据转化为更好地表示所要解决的潜在问题的特征的过程。对于这一步骤没有特定的方法，这使得数据科学既是一门艺术，也是一门科学。话虽如此，以下是一些你可以考虑的事项：

+   将日期时间变量转换以提取仅仅是星期几、月份等信息……

+   为变量创建分箱或桶。（例如，对于身高变量，可以有 100–149 cm、150–199 cm、200–249 cm 等）

+   结合多个特征和/或值以创建一个新的特征。例如，在泰坦尼克挑战赛中，最准确的模型之一工程了一个名为“Is_women_or_child”的新变量，如果该人是女性或儿童则为 True，否则为 False。

### 3\. 特征选择

你可以大幅提高模型准确性的第三个领域是特征选择，即选择数据集中最相关/有价值的特征。特征过多可能导致算法过拟合，而特征过少则可能导致算法欠拟合。

我喜欢使用的两种主要方法可以帮助你选择特征：

+   **特征重要性**：一些算法，如随机森林或 XGBoost，允许你确定哪些特征在预测目标变量的值时最为“重要”。通过快速创建这些模型并进行特征重要性分析，你将理解哪些变量比其他变量更有用。

+   **降维**：其中一种最常见的降维技术，主成分分析（PCA），将大量特征通过线性代数方法减少到更少的特征。

### 4\. 集成学习算法

改善机器学习模型的最简单方法之一是选择更好的机器学习算法。如果你还不知道什么是集成学习算法，现在是学习它的时候了！

**集成学习**是一种将多个学习算法结合使用的方法。这样做的目的是使你能够实现比单独使用某个算法更高的预测性能。

常见的集成学习算法包括随机森林、XGBoost、梯度提升和 AdaBoost。为了说明为什么集成学习算法如此强大，我将以随机森林为例：

随机森林涉及使用原始数据的自助采样数据集创建多个决策树。然后，模型选择每棵决策树所有预测的众数（多数）。这样做的意义是什么？通过依赖“多数获胜”模型，它减少了来自单个树的错误风险。

![](img/46ab45c9b19ee0edd64b4766bba258a5.png)

例如，如果我们创建了一个决策树，即第三棵，它会预测 0。但是如果我们依赖所有 4 棵决策树的众数，预测值将是 1。这就是集成学习的力量！

### 5\. 调整超参数

最后，还有一个不常被提及但仍然非常重要的方面，那就是调整模型的超参数。在这方面，你必须清楚理解你所使用的机器学习模型。否则，很难理解每个超参数的作用。

查看随机森林的所有超参数：

```py
class sklearn.ensemble.RandomForestClassifier(n_estimators=100, *, criterion='gini', max_depth=None, 
min_samples_split=2, min_samples_leaf=1, min_weight_fraction_leaf=0.0, max_features='auto', 
max_leaf_nodes=None, min_impurity_decrease=0.0, min_impurity_split=None, bootstrap=True, 
oob_score=False, n_jobs=None, random_state=None, verbose=0, warm_start=False, 
class_weight=None, ccp_alpha=0.0, max_samples=None

```

例如，了解`min_impurity_decrease`是什么，可能会是个好主意，这样当你希望你的机器学习模型更加宽容时，你可以调整这个参数！ ;)

[原文](https://towardsdatascience.com/how-i-consistently-improve-my-machine-learning-models-from-80-to-over-90-accuracy-6097063e1c9a)。已获得许可重新发布。

**相关内容：**

+   [理解集成学习技术](https://www.kdnuggets.com/2020/03/making-sense-ensemble-learning-techniques.html)

+   [如何评估机器学习模型的性能](https://www.kdnuggets.com/2020/09/performance-machine-learning-model.html)

+   [高级特征工程和预处理的 4 个技巧](https://www.kdnuggets.com/2019/08/4-tips-advanced-feature-engineering-preprocessing.html)

### 更多相关话题

+   [停止学习数据科学以寻找目标，寻找目标以…](https://www.kdnuggets.com/2021/12/stop-learning-data-science-find-purpose.html)

+   [数据科学统计学习的顶级资源](https://www.kdnuggets.com/2021/12/springboard-top-resources-learn-data-science-statistics.html)

+   [一个 90 亿美元的 AI 失败案例分析](https://www.kdnuggets.com/2021/12/9b-ai-failure-examined.html)

+   [成功数据科学家的 5 个特征](https://www.kdnuggets.com/2021/12/5-characteristics-successful-data-scientist.html)

+   [是什么让 Python 成为初创公司的理想编程语言](https://www.kdnuggets.com/2021/12/makes-python-ideal-programming-language-startups.html)

+   [每个数据科学家都应该了解的三个 R 库（即使你使用 Python）](https://www.kdnuggets.com/2021/12/three-r-libraries-every-data-scientist-know-even-python.html)
