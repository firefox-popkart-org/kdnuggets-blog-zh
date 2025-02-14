# 使用 Prefect 在 Python 中协调数据科学项目

> 原文：[`www.kdnuggets.com/2022/02/orchestrate-data-science-project-python-prefect.html`](https://www.kdnuggets.com/2022/02/orchestrate-data-science-project-python-prefect.html)

## 动机

作为数据科学家，为什么你应该关心优化你的数据科学工作流程？让我们以一个基本的数据科学项目为例来开始。

想象一下你在处理一个 Iris 数据集。你开始编写函数来处理你的数据。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速入门网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT 工作

* * *

定义了函数后，你需要执行它们。

你的代码运行正常，你没有看到输出中的任何问题，所以你认为工作流程已经足够好了。然而，像上述这样的线性工作流程可能存在许多缺点。

![使用 Prefect 在 Python 中协调数据科学项目](img/d127665d6694b5015ed667a0f432c449.png)

作者提供的图片

缺点包括：

+   如果函数`get_classes`出现错误，由函数`encode_categorical_columns`生成的输出将会丢失，工作流程将需要从头开始。这可能会很令人沮丧，特别是如果执行函数`encode_categorical_columns`需要很长时间的话。

![使用 Prefect 在 Python 中协调数据科学项目](img/12ce19049288c80b227ee4e6b75aa423.png)

作者提供的图片

+   由于函数`encode_categorical_columns`和`get_classes`之间没有相互依赖，因此可以同时执行它们以节省时间：

![使用 Prefect 在 Python 中协调数据科学项目](img/fa139968159d706d7b42e62f2ac6640e.png)

作者提供的图片

以这种方式运行函数还可以防止在不工作的函数上浪费不必要的时间。如果函数`get_classes`出现错误，工作流程将立即重新启动，而无需等待函数`encode_categorical_columns`完成。

![使用 Prefect 在 Python 中协调数据科学项目](img/cf9dda753060e43cfb10a577389c3543.png)

作者提供的图片

现在，你可能会同意我所说的，优化不同功能的工作流程是重要的。然而，手动管理工作流程可能需要大量的工作。

是否有一种方法可以通过仅添加几行代码来**自动优化工作流程**？这就是 Prefect 发挥作用的地方。

## 什么是 Prefect？

[Prefect](https://www.prefect.io/)是一个开源框架，用于在 Python 中构建工作流。Prefect 使得构建、运行和监控大规模数据管道变得容易。

要安装 Prefect，请输入：

```py
pip install prefect
```

## 使用 Prefect 构建你的工作流

要了解 Prefect 的工作原理，让我们用 Prefect 封装本文开头的工作流。

## 第一步 — 创建任务

一个`Task`是在 Prefect 流中的一个离散操作。首先，使用装饰器`prefect.task`将上述定义的函数转换为任务：

## 第二步 — 创建一个流

一个`Flow`表示通过管理任务之间的依赖关系来管理整个工作流。要创建一个流，只需将代码插入到`with Flow(...)`上下文管理器中运行你的函数即可。

请注意，上述代码运行时，这些任务都不会执行。Prefect 允许你立即运行流或安排以后执行。

让我们尝试立即使用`flow.run()`执行流：

运行上述代码将得到类似于以下内容的输出：

为了理解 Prefect 创建的工作流，让我们可视化整个工作流。

首先安装`prefect[viz]`：

```py
pip install "prefect[viz]"
```

然后将`visualize`方法添加到代码中：

你应该能看到如下所示的`data-engineer`工作流的可视化效果！

![使用 Prefect 在 Python 中编排数据科学项目](img/acfe31a541b6bd2197d85662e428e6dc.png)

作者提供的图像

请注意，Prefect 自动管理任务之间的执行顺序，以优化工作流。这对于增加几行代码来说相当酷！

## 第三步 — 添加参数

如果你发现自己经常尝试不同的变量值，最好将该变量转换为`Parameter`。

你可以将`Parameter`视为`Task`，只是它可以在每次运行流时接收用户输入。要将变量转换为参数，只需使用`task.Parameter`。

`Parameter`的第一个参数指定了参数的名称。`default`是一个可选参数，用于指定参数的默认值。

再次运行`flow.visualize`将得到如下输出：

![使用 Prefect 在 Python 中编排数据科学项目](img/44166d29185137f45ad8915794050701.png)

作者提供的图像

你可以通过以下方式覆盖每次运行的默认参数：

+   将参数`parameters`添加到`flow.run()`中：

+   或使用 Prefect CLI：

+   或使用 JSON 文件：

你的 JSON 文件应类似于此：

你还可以使用 Prefect Cloud 更改每次运行的参数，下一节将介绍此功能。

## 监控你的工作流

## 概述

Prefect 还允许你在 Prefect Cloud 中监控工作流。请按照[此说明](https://docs.prefect.io/orchestration/getting-started/set-up.html#server-or-cloud)安装 Prefect Cloud 所需的相关依赖项。

安装和设置所有依赖项后，首先通过运行以下命令创建 Prefect 项目：

```py
$ prefect create project "Iris Project"
```

接下来，启动本地代理，将我们的流本地部署到单台计算机上：

```py
$ prefect agent local start
```

然后添加：

...在文件的末尾。

运行文件后，您应该会看到类似以下的内容：

点击输出中的 URL，您将被重定向到概述页面。概述页面显示了您的流的版本、创建时间、流的运行历史和运行摘要。

![使用 Prefect 在 Python 中协调数据科学项目](img/27be5414bd9ac912bc16daba19138177.png)

作者提供的图片

您还可以查看其他运行的摘要、执行时间和配置。

![使用 Prefect 在 Python 中协调数据科学项目](img/900f750ad69f67d6a7de4c920241c509.png)

作者提供的图片

这些重要信息能够自动跟踪，真是太酷了！

## 使用默认参数运行工作流

请注意，工作流已注册到 Prefect Cloud，但尚未执行。要使用默认参数执行工作流，请点击右上角的“快速运行”。

![使用 Prefect 在 Python 中协调数据科学项目](img/b52b6e1a8515b1625fa18af129404492.png)

作者提供的图片

点击创建的运行。现在您将能够实时查看新流运行的活动！

![使用 Prefect 在 Python 中协调数据科学项目](img/53f3ed04303e6959f9622c1b148d7a6d.png)

作者提供的图片

## 使用自定义参数运行工作流

要使用自定义参数运行工作流，请点击“运行”标签，然后更改“输入”下的参数。

![使用 Prefect 在 Python 中协调数据科学项目](img/500f29953a51ba46520a7507a9297495.png)

作者提供的图片

当您对参数满意时，只需点击“运行”按钮即可开始运行。

## 查看工作流图表

点击“示意图”将显示整个工作流的图表。

![使用 Prefect 在 Python 中协调数据科学项目](img/8f95084821ddf3cb25a4b6e757a38a67.png)

作者提供的图片

## 其他功能

除了上述提到的一些基本功能外，Prefect 还提供了一些其他酷炫的功能，这些功能将显著提高您的工作流效率。

## 输入缓存

记得我们在文章开头提到的问题吗？通常，如果函数 `get_classes` 失败，则函数 `encode_categorical_columns` 创建的数据将被丢弃，整个工作流需要从头开始。

然而，使用 Prefect 时，`encode_categorical_columns` 的输出会被存储。下次重新运行工作流时，`encode_categorical_columns` 的输出将被下一个任务**无需重新运行** `encode_categorical_columns` 使用。

![使用 Prefect 在 Python 中协调数据科学项目](img/5837902e6ee1f5ecfb8f7c04207e0f27.png)

作者提供的图片

这可能会大幅减少运行工作流所需的时间。

## 持久化输出

有时，您可能希望将任务的数据导出到外部位置。这可以通过在任务函数中插入保存数据的代码来实现。

但是，这样做会使得测试函数变得困难。

Prefect 通过以下方式简化了每次运行任务的输出保存：

+   将检查点设置为 `True`

```py
$ export PREFECT__FLOWS__CHECKPOINTING=true
```

+   并将 `result = LocalResult(dir=...))` 添加到装饰器 `@task` 中。

现在任务 `split_data` 的输出将被保存到目录 `data/processed` 中！名称会类似于这样：

```py
prefect-result-2021-11-06t15-37-29-605869-00-00
```

如果你想自定义文件名称，可以在 `@task` 中添加 `target` 参数：

Prefect 还提供了其他 `Result` 类，比如 `GCSResult` 和 `S3Result`。你可以在[这里](https://docs.prefect.io/api/latest/engine/results.html)查看 API 文档。

## 在当前流中使用另一个流的输出

如果你正在处理多个流，例如 `data-engineer` 流和 `data-science` 流，你可能希望将 `data-engineer` 流的输出用于 `data-science` 流。

![用 Prefect 在 Python 中组织数据科学项目](img/e298c783aeb52920e3bee451ac6043ab.png)

作者提供的图片

在将 `data-engineer` 流的输出保存为文件后，你可以使用 `read` 方法读取该文件：

## 连接相关流

想象这种情况：你创建了两个互相依赖的流。流 `data-engineer` 需要在流 `data-science` 之前执行。

有人查看了你的工作流但没有理解这两个流之间的关系。结果，他们同时执行了流 `data-science` 和流 `data-engineer`，并遇到了错误！

![用 Prefect 在 Python 中组织数据科学项目](img/179c539b21ae399aa7928b8c2ce53252.png)

作者提供的图片

为了防止这种情况发生，我们应该指定流之间的关系。幸运的是，Prefect 使这变得更简单。

首先使用 `StartFlowRun` 获取两个不同的流。将 `wait=True` 添加到参数中，以确保下游流仅在上游流执行完毕后才执行。

接下来，在 `with Flow(...)` 上下文管理器下调用 `data_science_flow`。使用 `upstream_tasks` 指定将在 `data-science` 流执行前执行的任务/流。

现在两个流连接如下：

![用 Prefect 在 Python 中组织数据科学项目](img/5deddfed0f278e9a0dffe9340d89edf3.png)

作者提供的图片

相当酷！

## 调度你的流

Prefect 也使得在特定时间或特定间隔执行流变得无缝。

例如，要每分钟运行一次流，你可以初始化 `IntervalSchedule` 类，并将 `schedule` 添加到 `with Flow(...)` 上下文管理器中：

现在你的流将每分钟重新运行一次！

了解更多关于调度流的不同方式，请访问[这里](https://docs.prefect.io/core/concepts/schedules.html#overview)。

## 日志记录

你可以通过简单地将 `log_stdout=True` 添加到 `@task` 中来记录任务中的打印语句：

执行任务时，你应该会看到如下输出：

```py
[2021-11-06 11:41:16-0500] INFO - prefect.TaskRunner | Model accuracy on test set: 93.33
```

## 结论

恭喜！你刚刚了解了如何用几行 Python 代码优化你的数据科学工作流程。代码中的小优化在长远中可能会带来巨大的效率提升。

随意玩耍并 [在这里 fork 这篇文章的源代码](https://github.com/khuyentran1401/Data-science/tree/master/data_science_tools/prefect_example)。

我喜欢写关于基本数据科学概念的文章，并玩各种数据科学工具。你可以在 [LinkedIn](https://www.linkedin.com/in/khuyen-tran-1ab926151/) 和 [Twitter](https://twitter.com/KhuyenTran16) 上与我联系。

如果你想查看我撰写的所有文章的代码，请给[this repo](https://github.com/khuyentran1401/Data-science)打星。关注我的 Medium，获取最新的数据科学文章：

+   [**Great Expectations：始终了解你的数据期望**](https://towardsdatascience.com/great-expectations-always-know-what-to-expect-from-your-data-51214866c24)

+   [**Kedro — 用于可重复数据科学项目的 Python 框架**](https://towardsdatascience.com/kedro-a-python-framework-for-reproducible-data-science-project-4d44977d4f04)

+   [**Weight & Biases 介绍：用 3 行代码跟踪和可视化你的机器学习实验**](https://towardsdatascience.com/introduction-to-weight-biases-track-and-visualize-your-machine-learning-experiments-in-3-lines-9c9553b0f99d)

+   [**Datapane 介绍：构建互动报告的 Python 库**](https://towardsdatascience.com/introduction-to-datapane-a-python-library-to-build-interactive-reports-4593fd3cb9c8)

**[Khuyen Tran](https://www.linkedin.com/in/khuyen-tran-1401/)** 是一位多产的数据科学作家，她撰写了 [一系列令人印象深刻的有用数据科学话题，包括代码和文章](https://github.com/khuyentran1401/Data-science)。她在 **[Data Science Simplified](https://mathdatasimplified.com/)** 分享了超过 400 个关于数据科学和 Python 的技巧。订阅她的新闻通讯，接收她的每日提示。

[原文](https://towardsdatascience.com/orchestrate-a-data-science-project-in-python-with-prefect-e69c61a49074)。转载已获许可。

### 更多相关话题

+   [使用 Prefect 构建数据管道](https://www.kdnuggets.com/building-data-pipeline-with-prefect)

+   [免费 Python 项目编码课程](https://www.kdnuggets.com/2022/08/free-python-project-coding-course.html)

+   [从数据收集到模型部署：数据科学项目的 6 个阶段](https://www.kdnuggets.com/2023/01/data-collection-model-deployment-6-stages-data-science-project.html)

+   [KDnuggets 新闻，7 月 5 日：一个糟糕的数据科学项目 • 10 AI…](https://www.kdnuggets.com/2023/n24.html)

+   [在数据科学项目中停止硬编码 - 改用配置文件](https://www.kdnuggets.com/2023/06/stop-hard-coding-data-science-project-config-files-instead.html)

+   [19 个适合初学者的数据科学项目想法](https://www.kdnuggets.com/2021/11/19-data-science-project-ideas-beginners.html)
