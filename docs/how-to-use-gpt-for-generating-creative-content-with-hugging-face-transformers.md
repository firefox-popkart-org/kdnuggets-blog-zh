# 使用 GPT 生成创意内容的方法

> 原文：[`www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers`](https://www.kdnuggets.com/how-to-use-gpt-for-generating-creative-content-with-hugging-face-transformers)

![如何使用 GPT 生成创意内容与 Hugging Face Transformers](img/7c2590f4db8e141e3008c7f98ba82fe5.png)

## 介绍

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持组织的 IT 工作

* * *

GPT，即生成预训练变换器，是一类基于变换器的语言模型。作为一种早期基于变换器的模型，它能够生成连贯的文本，OpenAI 的 GPT-2 是此类模型的初期成功案例之一，可用于各种应用，包括以更具创意的方式撰写内容。Hugging Face Transformers 库是一个预训练模型库，它简化了与这些复杂语言模型的交互。

创意内容的生成可能会很有价值，例如，在数据科学和机器学习的世界中，它可以用于各种方式来美化枯燥的报告、创建合成数据，或简单地帮助讲述更有趣的故事。本教程将指导你如何使用 GPT-2 和 Hugging Face Transformers 库来生成创意内容。请注意，我们在这里使用 GPT-2 模型是由于其简便和适中的大小，但替换为其他生成模型将遵循相同的步骤。

## 设置环境

在开始之前，我们需要设置我们的环境。这将涉及安装和导入必要的库及所需的包。

安装必要的库：

```py
pip install transformers torch
```

导入所需的包：

```py
from transformers import AutoModelForCausalLM, AutoTokenizer
import torch
```

你可以在 [这里](https://huggingface.co/docs/transformers/model_doc/auto) 了解 Hugging Face 的自动类和自动模型。继续阅读。

## 加载模型和分词器

接下来，我们将在脚本中加载模型和分词器。在这个案例中，模型是 GPT-2，而分词器负责将文本转换为模型可以理解的格式。

```py
model_name = "gpt2"
model = AutoModelForCausalLM.from_pretrained(model_name)
tokenizer = AutoTokenizer.from_pretrained(model_name)
```

注意，更改上面的 model_name 可以切换不同的 Hugging Face 语言模型。

## 为生成准备输入文本

为了让我们的模型生成文本，我们需要提供模型一个初始输入或提示。这个提示将被分词器进行分词处理。

```py
prompt = "Once upon a time in Detroit, "
input_ids = tokenizer(prompt, return_tensors="pt").input_ids
```

请注意，`return_tensors='pt'` 参数确保返回 PyTorch 张量。

## 生成创造性内容

一旦输入文本被标记化并准备好输入模型，我们就可以使用模型生成创造性内容。

```py
gen_tokens = model.generate(input_ids, do_sample=True, max_length=100, pad_token_id=tokenizer.eos_token_id)
gen_text = tokenizer.batch_decode(gen_tokens)[0]
print(gen_text)
```

## 使用高级设置自定义生成

为了增加创造性，我们可以调整温度，并使用 top-k 采样和 top-p（核）采样。

调整温度：

```py
gen_tokens = model.generate(input_ids, 
                            do_sample=True, 
                            max_length=100, 
                            temperature=0.7, 
                            pad_token_id=tokenizer.eos_token_id)
gen_text = tokenizer.batch_decode(gen_tokens)[0]
print(gen_text)
```

使用 top-k 采样和 top-p 采样：

```py
gen_tokens = model.generate(input_ids, 
                            do_sample=True, 
                            max_length=100, 
                            top_k=50, 
                            top_p=0.95, 
                            pad_token_id=tokenizer.eos_token_id)
gen_text = tokenizer.batch_decode(gen_tokens)[0]
print(gen_text)
```

## 创造性内容生成的实际示例

这里是一些使用 GPT-2 生成创造性内容的实际示例。

```py
# Example: Generating story beginnings
story_prompt = "In a world where AI contgrols everything, "
input_ids = tokenizer(story_prompt, return_tensors="pt").input_ids
gen_tokens = model.generate(input_ids, 
                            do_sample=True, 
                            max_length=150, 
                            temperature=0.4, 
                            top_k=50, 
                            top_p=0.95, 
                            pad_token_id=tokenizer.eos_token_id)
story_text = tokenizer.batch_decode(gen_tokens)[0]
print(story_text)

# Example: Creating poetry lines
poetry_prompt = "Glimmers of hope rise from the ashes of forgotten tales, "
input_ids = tokenizer(poetry_prompt, return_tensors="pt").input_ids
gen_tokens = model.generate(input_ids, 
                            do_sample=True, 
                            max_length=50, 
                            temperature=0.7, 
                            pad_token_id=tokenizer.eos_token_id)
poetry_text = tokenizer.batch_decode(gen_tokens)[0]
print(poetry_text)
```

## 总结

通过尝试不同的参数和设置可以显著影响生成内容的质量和创造性。GPT，特别是我们都熟知的较新版本，在创造性领域具有巨大的潜力，使数据科学家能够生成引人入胜的叙述、合成数据等。进一步阅读可考虑探索 Hugging Face 文档及其他资源，以加深理解和扩展技能。

遵循本指南后，你现在应该能够利用 GPT-3 和 Hugging Face Transformers 的强大功能，为数据科学及其他领域生成创造性内容。

欲了解更多相关信息，请查看以下资源：

+   [Hugging Face Transformers 文档](https://huggingface.co/transformers/)

+   [PyTorch 文档](https://pytorch.org/docs/stable/index.html)

+   [生成预训练变换器（维基百科）](https://en.wikipedia.org/wiki/Generative_pre-trained_transformer)

[](https://www.linkedin.com/in/mattmayo13/)****[Matthew Mayo](https://www.kdnuggets.com/wp-content/uploads/./profile-pic.jpg)**** ([**@mattmayo13**](https://twitter.com/mattmayo13)) 拥有计算机科学硕士学位和数据挖掘研究生文凭。作为 [KDnuggets](https://www.kdnuggets.com/) 和 [Statology](https://www.statology.org/) 的主编，以及 [Machine Learning Mastery](https://machinelearningmastery.com/) 的贡献编辑，Matthew 旨在使复杂的数据科学概念变得易于理解。他的职业兴趣包括自然语言处理、语言模型、机器学习算法以及探索新兴的人工智能。他致力于在数据科学社区中普及知识。Matthew 从 6 岁起便开始编程。

### 更多相关主题

+   [如何使用 Hugging Face Transformers 微调 BERT 进行情感分析](https://www.kdnuggets.com/how-to-fine-tune-bert-sentiment-analysis-hugging-face-transformers)

+   [使用 Hugging Face Transformers 构建推荐系统](https://www.kdnuggets.com/building-a-recommendation-system-with-hugging-face-transformers)

+   [使用 Hugging Face Transformers 进行文本情感检测](https://www.kdnuggets.com/using-hugging-face-transformers-for-emotion-detection-in-text)

+   [如何从头开始构建和训练一个 Transformer 模型…](https://www.kdnuggets.com/how-to-build-and-train-a-transformer-model-from-scratch-with-hugging-face-transformers)

+   [如何使用 MarianMT 和 Hugging Face Transformers 进行语言翻译](https://www.kdnuggets.com/how-to-translate-languages-with-marianmt-and-hugging-face-transformers)

+   [认识 Gorilla：加州大学伯克利分校和微软的 API 增强型 LLM…](https://www.kdnuggets.com/2023/06/meet-gorilla-uc-berkeley-microsoft-apiaugmented-llm-outperforms-gpt4-chatgpt-claude.html)
