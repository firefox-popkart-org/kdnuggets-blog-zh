# 优化数据分析：在 Databricks 中集成 GitHub Copilot

> 原文：[`www.kdnuggets.com/optimizing-data-analytics-integrating-github-copilot-in-databricks`](https://www.kdnuggets.com/optimizing-data-analytics-integrating-github-copilot-in-databricks)

# 介绍

* * *

## 我们的前三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业轨道。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织 IT

* * *

GitHub Copilot 是 GitHub 和 OpenAI 合作开发的人工智能驱动的代码补全助手，利用 ChatGPT 模型。它旨在帮助开发者加速编码过程，同时减少错误。该模型在 GitHub 自有代码库和公开可用代码的混合数据上进行训练，赋予其广泛的编程范式理解。

另一方面，Databricks 是一个由 Apache Spark 原始创建者创建的开源分析和云平台，帮助组织无缝构建数据分析和机器学习管道，从而加速创新。此外，它促进了用户之间的协作。

将 GitHub Copilot 与 Databricks 集成，使数据分析和机器学习工程师能够高效且及时地部署解决方案。此集成促进了更顺畅的代码开发，提高了代码质量和标准化，提升了跨语言效率，加快了原型开发，并有助于文档编写，从而提高了工程师的生产力和效率。

**GitHub Copilot 和 Databricks 集成的前提条件**：

Databricks 账户 [设置](https://www.databricks.com/try-databricks#account)。

设置 [GitHub Copilot](https://github.com/features/copilot)。

下载并安装 [Visual Studio Code](https://code.visualstudio.com/download)。

# 集成步骤

在 Visual Studio Code 市场中安装 Databricks 插件。

![优化数据分析：在 Databricks 中集成 GitHub Copilot](img/d31ea6fc9358f1bb689892a2f2ac1864.png)

在 Visual Studio Code 中配置 Databricks 插件。如果你之前使用过 Databricks CLI，那么它已经在 databrickscfg 文件中为你本地配置好。如果没有，请在 ~/.databrickscfg 文件中创建以下内容。

```py
[DEFAULT]
host = https://xxx
token = <token>
jobs-api-version = 2.0
```

点击“配置 Databricks”选项，然后从下拉菜单中选择第一个选项，该选项显示在上述步骤中配置的主机名，并继续使用“DEFAULT”配置文件。

![优化数据分析：在 Databricks 中集成 GitHub Copilot](img/912f16d4e2d9549ec9c8dec4690477fa.png)

配置完成后，Databricks 连接会与 Visual Studio Code 建立。点击 Databricks 插件时，可以查看工作区和集群配置详细信息。

用户完成 GitHub Copilot 账户设置后，确保您可以访问 GitHub Copilot。通过 Marketplace 在 VSCode 中安装 GitHub Copilot 和 GitHub Copilot Chat 插件。

![优化数据分析：在 Databricks 中集成 GitHub Copilot](img/436a913cf1a7742d6fd029d54d0b753d.png)

一旦用户安装了 GitHub Copilot 和 Copilot Chat 插件，将提示通过 Visual Studio IDE 登录 GitHub Copilot。如果没有提示进行授权，请点击 Visual Studio 代码 IDE 底部面板中的铃铛图标。

![优化数据分析：在 Databricks 中集成 GitHub Copilot](img/c21cc7b4faf53a62797778fa84c2fb33.png)

现在，是时候使用 GitHub Copilot 开发了

# 开发数据工程管道

数据工程师可以利用 GitHub Copilot 快速编写数据工程管道，包括文档，几乎无需时间。以下是使用提示技术创建简单数据工程管道的步骤。

使用 Python 和 Spark 框架从 S3 存储桶中读取文件。

![优化数据分析：在 Databricks 中集成 GitHub Copilot](img/ab6bbdc52bee8e3d3fa4dcc83d9f5bc2.png)

使用 Python 和 Spark 框架将数据框写入 S3 存储桶

![优化数据分析：在 Databricks 中集成 GitHub Copilot](img/a91adbe7a4b8609248e6c8c36e6b27fb.png)

通过主方法执行函数：在提示中表示相同，并通过代码执行步骤得到结果

![优化数据分析：在 Databricks 中集成 GitHub Copilot](img/3608acb375dc7c3c5cf19667e51563a9.png)

# 在 Databricks 中使用 GitHub Copilot 进行数据工程和机器学习的好处

+   优秀的 AI 配对编程工具，可快速提供合理建议并生成模板代码。

+   顶级建议，以优化代码和运行时间。

+   更好的文档和逻辑步骤的 ASCII 表示。

+   更快的数据管道实现，错误最小。

+   详细解释现有的简单/复杂功能，并建议智能代码重构技术。

# 备忘单

+   打开一个 Co-pilot 文本/搜索栏，您可以在其中输入您的提示。![优化数据分析：在 Databricks 中集成 GitHub Copilot](img/4c8d39ce04bdf3644241067870538c9d.png)

    Windows: [Cltr] + [I]

    Mac: Command + [I]

+   在右侧打开一个单独的窗口，显示前 10 个代码建议。

    Windows: [Cltr] + [Enter]

    Mac: [control] + [return]

![优化数据分析：在 Databricks 中集成 GitHub Copilot](img/3c36000105f16c30f4ae467360e42a1f.png)

+   在左侧打开一个单独的 Copilot 聊天窗口。

    Windows: [Cltr] + [Alt] + [I]

    Mac: [Control] + [Command] + [I]

+   驳回内联建议。

    Windows/Mac: Esc

+   接受建议。

    Windows/Mac: Tab

+   参考以前的建议。

    Windows: [Alt] + [

    Mac: [option] + [

+   检查下一个建议

    Windows: [Alt] + ]

    Mac: [option] + ]

# 结论

人工智能配对编程工具与集成开发环境的集成，帮助开发人员通过实时代码建议加速开发，减少查阅文档以获取模板代码和语法的时间，并使开发人员能够专注于创新和业务问题解决方案。

## 进一步资源

+   [`app.pluralsight.com/library/courses/getting-started-prompt-engineering-generative-ai/table-of-contents`](https://app.pluralsight.com/library/courses/getting-started-prompt-engineering-generative-ai/table-of-contents)

+   [`docs.github.com/en/copilot/quickstart`](https://docs.github.com/en/copilot/quickstart)

**[](http://www.linkedin.com/in/naresh-vurukonda-a23861124)**[Naresh Vurukonda](http://www.linkedin.com/in/naresh-vurukonda-a23861124)** 是一位首席架构师，在医疗保健、生命科学和媒体网络组织中拥有超过 10 年的数据工程和机器学习项目经验。

### 更多相关主题

+   [GitHub Copilot 开源替代方案](https://www.kdnuggets.com/2021/07/github-copilot-open-source-alternatives-code-generation.html)

+   [将 ChatGPT 集成到数据科学工作流中：提示和最佳实践](https://www.kdnuggets.com/2023/05/integrating-chatgpt-data-science-workflows-tips-best-practices.html)

+   [通过集成 Jupyter 和 KNIME 减少实施时间](https://www.kdnuggets.com/2021/12/cutting-implementation-time-integrating-jupyter-knime.html)

+   [将生成性人工智能集成到内容创作中](https://www.kdnuggets.com/integrating-generative-ai-in-content-creation)

+   [优化数据存储：探讨 SQL 中的数据类型和规范化](https://www.kdnuggets.com/optimizing-data-storage-exploring-data-types-and-normalization-in-sql)

+   [使用遗传算法优化基因](https://www.kdnuggets.com/2022/04/optimizing-genes-genetic-algorithm.html)
