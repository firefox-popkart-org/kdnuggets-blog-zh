# AutoXGB 的简单 AutoML

> 原文：[`www.kdnuggets.com/2022/02/no-brainer-automl-autoxgb.html`](https://www.kdnuggets.com/2022/02/no-brainer-automl-autoxgb.html)

![AutoXGB 的简单 AutoML](img/02ab161ce6b93042832d676525e34fed.png)

作者提供的图片

自动化机器学习（AutoML）会自动运行各种机器学习过程，并优化错误指标以生成最佳模型。这些过程包括：数据预处理、编码、缩放、超参数优化、模型训练、生成工件和结果列表。自动化机器学习过程使得开发 AI 解决方案快速、用户友好，并且通常可以产生准确的低代码结果 - [TechTarget](https://www.techtarget.com/searchenterpriseai/definition/automated-machine-learning-AutoML)。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [Google 网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全领域。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [Google 数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析能力

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [Google IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织的 IT

* * *

**流行的 AutoML 库：**

+   [LightAutoML](https://github.com/sberbank-ai-lab/LightAutoML)

+   [MLJar](https://pypi.org/project/mljar-supervised/)

+   [EvalML](https://github.com/alteryx/evalml)

+   [FLAML](https://github.com/microsoft/FLAML)

+   [PyCaret](https://pycaret.org/)

+   [AutoGluon](https://auto.gluon.ai/stable/index.html)

+   [H2O 3](https://docs.h2o.ai/h2o/latest-stable/h2o-docs/index.html)

在本教程中，我们将使用 1994 年人口普查收入数据预测一个人是否年收入超过 50K 美元。这是一个经典的二分类问题，我们将使用 Kaggle 数据集 [成人人口普查收入](https://www.kaggle.com/uciml/adult-census-income)，该数据集采用 [CC0: 公共领域](https://creativecommons.org/publicdomain/zero/1.0/) 许可证。它由 Ronny Kohavi 和 Barry Becker（数据挖掘与可视化，硅谷图形公司）从 [1994 年人口普查局数据库](http://www.census.gov/en.html) 提取。我们不会深入数据分析或模型工作原理。相反，我们将用几行代码构建一个优化的机器学习模型，并通过 FastAPI 服务器访问它。

## AutoXGB

[AutoXGB](https://github.com/abhishekkrthakur/autoxgb) 是一个简单但有效的 AutoML 工具，可以直接从 CSV 文件中训练模型表格数据集。AutoXGB 使用 [XGBoost](https://xgboost.readthedocs.io/en/stable/) 训练模型，使用 [Optuna](https://optuna.org/) 进行超参数优化，并使用 [FastAPI](https://fastapi.tiangolo.com/) 提供模型推理 API 形式。

让我们开始安装 autoxgb。如果在运行服务器时遇到错误，请安装 fastapi 和 unvicorn。

```py
pip install autoxgb
```

## 初始化中

我们将深入探讨 AutoXGB 函数的特性以及这些参数如何用于改善结果或减少训练时间。

+   **train_filename**: 训练数据的路径

+   **output**: 存储工件的输出文件夹路径

+   **test_filename**: 测试数据的路径。如果未指定，仅保存 OOF 预测

+   **task**: 如果未指定，任务将自动推断

    +   task = "classification"

    +   task = "regression"

+   **idx**: 如果未指定，id 列将自动生成，名称为 id

+   **targets**: 如果未指定，目标列将被假定为名为 target，并且问题将被视为二分类、多分类或单列回归中的一种

    +   targets = ["target"]

    +   targets = ["target1", "target2"]

+   **features**: 如果未指定，除了 id、targets 和 kfold 列外，将使用所有列

    +   features = ["col1", "col2"]

+   **categorical_features**: 如果未指定，将自动推断类别列

    +   categorical_features = ["col1", "col2"]

+   **use_gpu**: 如果未指定，则不使用 GPU

    +   use_gpu = True

    +   use_gpu = False

+   **num_folds**: 用于交叉验证的折数

+   **seed**: 用于可重复性的随机种子

+   **num_trials**: 要运行的 Optuna 尝试次数

    +   默认值为 1000

    +   num_trials = 1000

+   **time_limit**: Optuna 尝试的时间限制（秒）

    +   如果未指定，将运行所有尝试

    +   time_limit = None

+   **fast**: 如果 fast 设置为 True，则超参数调整将仅使用一个折，这样可以减少优化时间。之后它将对其余的折进行训练，并生成 OOF 和测试预测。

在我们的情况下，除了*train_filename, output, target, num_folds, seed, num_trails,* 和 *time_limit*，我们将大部分值设置为默认值。

```py
from autoxgb import AutoXGB

train_filename = "binary_classification.csv"
output = "output"
test_filename = None
task = None
idx = None
targets = ["income"]
features = None
categorical_features = None
use_gpu = False
num_folds = 5
seed = 42
num_trials = 100
time_limit = 360
fast = False
```

## 训练与优化

是时候定义一个模型使用*AutoXGB()*，并将先前定义的参数添加到模型中。最后，我们将使用*axgb.train()*开始训练过程。它将运行 XGBoost、Optuna，并输出工件（**model, predication, results, config, params, encoders**）。

```py
axgb = AutoXGB(
    train_filename=train_filename,
    output=output,
    test_filename=test_filename,
    task=task,
    idx=idx,
    targets=targets,
    features=features,
    categorical_features=categorical_features,
    use_gpu=use_gpu,
    num_folds=num_folds,
    seed=seed,
    num_trials=num_trials,
    time_limit=time_limit,
    fast=fast,
)
axgb.train()
```

训练过程花费了 10-12 分钟，我们可以在下面看到最佳结果。我认为我们可以通过增加时间限制来提高**F1**分数。我们还可以调整其他超参数以改善模型性能。

```py
2022-02-09 18:11:27.163 | INFO     | autoxgb.utils:predict_model:336 - Metrics: {'auc': 0.851585935958628, 'logloss': 0.3868651767621002, 'f1': 0.5351485750859325, 'accuracy': 0.8230396087432015, 'precision': 0.7282822005864846, 'recall': 0.42303153575005525}
```

### **使用 CLI 进行训练**

要在终端/bash 中训练模型，我们将使用**autoxgb** **train**。我们将只设置*train_filename*和*output* 文件夹。

```py
autoxgb train \
 --train_filename binary_classification.csv \
 --output output \
```

## Web API

通过在终端运行**autoxgb serve**，我们可以在本地运行 FastAPI 服务器。

![无需脑力的 AutoML 与 AutoXGB](img/24954fac438a81a6ff390025c0994129.png)

## AutoXGB 服务参数

+   **model_path** -> 模型路径。在我们的案例中，这是输出文件夹。

+   **port** -> 服务端口 8080

+   **host** -> 用于服务的主机，IP 地址：0.0.0.0

+   **workers** -> 工作人员数量或同时请求的数量。

+   **debug** -> 显示错误和成功的日志

### Deepnote 公共服务器

为了在云端运行服务器，[Deepnote](https://deepnote.com/) 使用 **ngrok** 创建公共 URL。我们只需开启该选项并使用端口 **8080**。如果你在本地运行，则无需遵循此步骤，直接使用 “http://0.0.0.0:8080” 访问 API 即可。

![无脑 AutoML 与 AutoXGB](img/2aa8fd7dff333dd6014552e3db831a3f.png)

我们提供了 **model path**、**host ip** 和 **port number** 来运行服务器。

```py
!autoxgb serve --model_path /work/output --host 0.0.0.0 --port 8080 --debug
```

我们的 API 运行顺利，你可以通过 “https://8d3ae411-c6bc-4cad-8a14-732f8e3f13b7.deepnoteproject.com” 访问它。

```py
INFO:     Will watch for changes in these directories: ['/work']
INFO:     Uvicorn running on http://0.0.0.0:8080 (Press CTRL+C to quit)
INFO:     Started reloader process [153] using watchgod
INFO:     Started server process [163]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     172.3.161.55:40628 - "GET /docs HTTP/1.1" 200 OK
INFO:     172.3.188.123:38788 - "GET /openapi.json HTTP/1.1" 200 OK
INFO:     172.3.167.43:48326 - "GET /docs HTTP/1.1" 200 OK
INFO:     172.3.161.55:47018 - "GET /openapi.json HTTP/1.1" 200 OK
```

## 预测

我们可以添加随机输入来预测一个人的收入是否超过 $50k。在这个例子中，我们使用 FastAPI 的 **/docs** 选项来访问用户界面。

### 输入

我们将使用 FastAPI GUI 通过在链接末尾添加 ***/docs*** 来运行模型预测。例如 “172.3.167.43:39118/docs”

+   **workclass**: "私营"

+   **education**: "高中毕业"

+   **marital.status**: "丧偶"

+   **occupation**: "运输搬运"

+   **relationship**: "未婚"

+   **race**: "白人"

+   **sex**: "男性"

+   **native.country**: "美国"

+   **age**: 20

+   **fnlwgt**: 313986

+   **education.num**: 9

+   **capital.gain**: 0

+   **capital.loss**: 0

+   **hours.per.week**: 40

![无脑 AutoML 与 AutoXGB](img/b472bb19afa5e39dda93694edb062195.png)

### 结果

结果为 **<50k**，置信度为 97.6%，**>50k** 的置信度为 2.3%。

![无脑 AutoML 与 AutoXGB](img/310b95a9c11395c61f0d017f5303ba78.png)

## 使用请求进行测试

你也可以使用 Python 中的 **requests** 测试 API。只需将参数以字典形式推送，并以 JSON 格式获取输出。

```py
import requests

params = {
    "workclass": "Private",
    "education": "HS-grad",
    "marital.status": "Widowed",
    "occupation": "Transport-moving",
    "relationship": "Unmarried",
    "race": "White",
    "sex": "Male",
    "native.country": "United-States",
    "age": 20,
    "fnlwgt": 313986,
    "education.num": 9,
    "capital.gain": 0,
    "capital.loss": 0,
    "hours.per.week": 40,
}

article = requests.post(
    f"https://8d3ae411-c6bc-4cad-8a14-732f8e3f13b7.deepnoteproject.com/predict",
    json=params,
)

data_dict = article.json()
print(data_dict)
## {'id': 0, '<=50K': 0.9762147068977356, '>50K': 0.023785298690199852}
```

## 项目

代码和示例可以在以下地址找到：

+   [Deepnote](https://deepnote.com/@abid/AutoXGB-jTrkEca8TK2KFHMvjj8Ttw)

+   [GitHub](https://github.com/kingabzpro/Intro-to-AutoXGB)

+   [DAGsHub](https://dagshub.com/kingabzpro/Intro-to-AutoXGB)

## 结论

我使用 AutoML 在 Kaggle 竞赛中获得优势，并为机器学习项目开发模型基线。你有时可以获得快速且准确的结果，但如果你想创建最先进的解决方案，你需要手动实验各种机器学习过程。

在本教程中，我们了解了 AutoXGB 的各种功能。我们可以使用 AutoXGB 进行数据预处理，训练 XGboost 模型，使用 Optuna 优化模型，并使用 FastAPI 运行 Web 服务器。总之，AutoXGB 提供了一个端到端的解决方案来处理你的日常表格数据问题。如果你对实现或使用我提到的其他 AutoML 有疑问，请在评论区留言。我很乐意回答你的所有问题。

**[Abid Ali Awan](https://www.polywork.com/kingabzpro)** ([@1abidaliawan](https://twitter.com/1abidaliawan)) 是一位认证的数据科学专业人士，热衷于构建机器学习模型。目前，他专注于内容创作，并撰写关于机器学习和数据科学技术的技术博客。Abid 拥有技术管理硕士学位和电信工程学士学位。他的愿景是使用图神经网络构建一个 AI 产品，帮助那些在心理健康方面面临困扰的学生。

### 更多相关话题

+   [Nota AI 发布 NetPresso 模型搜索的测试版，...] (https://www.kdnuggets.com/2022/04/nota-ai-releases-beta-version-netpresso-model-search-hardwareaware-automl-tool.html)

+   [2023 年你应该考虑的顶级 AutoML 框架](https://www.kdnuggets.com/2023/05/best-automl-frameworks-2023.html)
