# 为什么机器学习容易受到对抗性攻击以及如何解决这个问题

> 原文：[`www.kdnuggets.com/2019/06/machine-learning-adversarial-attacks.html`](https://www.kdnuggets.com/2019/06/machine-learning-adversarial-attacks.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

AI 安全辩论跟随领域内顶尖研究者及许多当今技术愿景者的脚步进行，例如支持[生命未来研究所](http://futureoflife.org/team)和[机器智能研究所](http://intelligence.org/team)的人士。通过媒体，这一对话可能看起来像是在担忧那些将消灭人类的未来机器人。

然而，我们在实际问题中观察到有关如何轻易失去对我们 AI 创作的掌控的真实迹象，这些问题与设计不良的机器学习系统的不良行为相关。潜在的“AI 事故”之一就是[对抗性技术](https://www.kdnuggets.com/2018/10/adversarial-examples-explained.html)的案例。这种方法以一个训练好的分类模型为例，该模型在识别输入时表现良好，与人类的分类方式相比。但随后出现一个新输入，其中包含微妙但恶意伪造的数据，这导致模型表现非常糟糕。令人担忧的是，这种糟糕的行为不是模型的*统计性能*下降。使用这样的操控输入，模型可能仍以非常高的信心分类对象，但分类结果*不再是*人类所预期的。

* * *

## 我们的前三个课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 加速进入网络安全职业的快车道

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析水平

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

表现优异并赢得比赛的图像分类器已经被[对抗性攻击](https://www.kdnuggets.com/2019/03/breaking-neural-networks-adversarial-attacks.html)所欺骗。如果你仔细查看这些场景，可能会发现一个有趣的发现。令人困扰的噪声基于模型的成本函数，这表明恶意攻击者需要手中掌握模型以弄清楚如何构建对抗输入。简单地保护模型免受攻击者的攻击可能是解决问题的直接方法。

不要急于行动。

2017 年，宾州州立大学的研究人员，包括 Nicolas Papernot 和 Goodfellow，[展示了](https://arxiv.org/pdf/1602.02697.pdf)远程对抗攻击可以对被视为“黑箱”的机器学习算法进行——换句话说，无需访问模型的训练数据或代码。这种方法使用了[可转移性](https://arxiv.org/pdf/1605.07277.pdf)的概念，即训练来误导一个模型的样本可能会误导另一个模型（即被攻击的目标模型）。

Papernot 的团队创建了局部图像分类器模型，并训练它们以查看修改样本如何导致错误分类。然后，他们将这些对抗样本（以人眼不可察觉的方式修改的停车标志图像）发送给通过亚马逊和谷歌的 API 访问的远程深度神经网络。这些棘手的图像也欺骗了远程模型。他们甚至报告了包括对抗性防御策略的误导性远程托管模型。

(想尝试用对抗样本破坏你的模型吗？试试由 Papernot 等人开发的[cleverhans](https://github.com/tensorflow/cleverhans)库。)

这一切都非常重要，[我们该如何应对](https://www.kdnuggets.com/2019/01/machine-learning-security.html)？如果攻击者能够控制（或混淆）例如，驾驶你自动驾驶车辆的计算机视觉模型的分类结果，而不需要直接访问代码，那么停车标志可能会被轻易地分类为让行标志。这样一来，准时到达工作地点或者安全到达就变得不那么可能。虽然这是一个 AI 开发者必须认识到的明确问题，但它仍然代表着一个*明确的挑战*，应该是可以处理和解决的。这只是人类巧妙地入侵其他*人类*系统以造成*人类*问题的一种方式，对抗这些问题依然需要*人类*的防御。

如果这种对抗样本的情况只是冰山一角呢？是否存在某种机器处理信息的内在机制？是否有超出人类感知的因素？

最近，麻省理工学院的一组研究人员发布了[一项研究](https://arxiv.org/pdf/1905.02175.pdf)，假设先前对机器学习易受对抗性技术影响的观察是标准数据集中固有模式的直接结果。虽然这些模式对人类来说难以理解，但它们作为自然特征存在，主要被监督学习算法使用。

在这篇论文中，Andrew Ilyas 等人描述了数据集中存在的稳健特征和非稳健特征的概念，这些特征*都*为分类提供了有用的信号。回顾起来，当我们记得分类器通常只被训练以最大化准确性时，这可能并不令人惊讶。模型使用哪些特征来获得最高准确性——由人类监督标记确定——留给模型用蛮力来处理。对我们来说像人鼻的输入图像，在算法的角度下，只是与下巴的另一个 0 和 1 的组合。

为了展示这些人类可感知和不可感知的特征如何支持准确分类，麻省理工学院团队构建了一个框架，将特征分离到不同的训练数据集中。两者都经过训练，并能按预期进行分类。如图 1 所示，模型被发现以*有意义的方式*使用非稳健特征，就像使用稳健特征一样，尽管这些特征对我们来说看起来像是乱码信息。

![](img/e3a143e102045c2966404c87fe678a73.png)

**图 1.** 一只青蛙的图像被分解为“稳健”的特征，用于训练分类器准确识别为青蛙，以及“非稳健”的特征，这些特征也用于训练分类器准确识别青蛙，但当输入中包含对抗性扰动时则不适用。模型使用的这些非稳健特征是*固有的*图像特征，并非攻击者操控或插入的 [来自 [Ilyas et al.](https://arxiv.org/pdf/1905.02175.pdf) 的许可]。

让我们再次明确这一点。

合法数据由对人类不可感知的特征组成，这些特征影响训练。这些看似与适当的人类分类无关的奇怪特征不是伪影或过拟合，而是自然发生的模式，*单独*就足以导致分类器的学习。

换句话说，算法可能会发现那些我们作为人类期望看到的有用分类模式。然而，*其他*模式也在被识别，它们对我们来说毫无意义——但它们仍然是支持算法进行良好分类的模式。

麻省理工学院的 Dimitris Tsipras 对他们的结果感到兴奋，称“它有力地证明了模型确实可以学习不可感知的良好泛化特征。”

虽然这些模式对我们看起来毫无意义，但机器学习分类器正在发现并利用它们。

面对对抗性输入（例如，新数据中的小扰动），这一套无意义的特征会崩溃，从而导致错误分类。现在，不仅仅是某个第三方攻击者将误导数据插入样本中，而是非稳健特征在真实数据中存在，使得分类器容易受到对手的挑衅。

回忆一下 Papernot 等人提到的可转移性概念，这些跨数据集存在的非鲁棒特性使得它们对其他模型训练出的对抗样本敏感。一个训练来攻破某个模型的对手很可能也会攻破另一个模型，因为它正干扰着两个模型中固有的非鲁棒特性。

因此，监督学习对抗对抗样本的脆弱性是算法如何看待数据的固有属性的直接结果。算法在处理任何特征模式时表现得很好，导向最佳准确率。如果一个对手干扰了它的分类结果，那么模型仍然认为它表现得相当好。结果与我们作为人类所感知的不一致，是我们的问题。

为了增加一点人性，Tsipras 说，“模型可能依赖于数据的各种特征，我们需要明确地对模型学习的内容施加先验。”

换句话说，如果我们希望创建在我们看来是*可解释的*模型，那么我们需要将我们期望的感知编码到训练过程中，而不是让算法依赖于其自身的非人类装置。

那么，如果算法看到的事物与我们人类*不同*，这有什么问题？这有什么意外之处吗？这令人害怕吗？这是我们不想看到的事情吗？

我们知道，机器学习处理世界的方式与我们不同。只需对比 NLP 中的基础概念，即单词的意义可以通过探索人类编写的文本中的邻近词汇（如 [Word2vec](https://www.tensorflow.org/tutorials/representation/word2vec)）来提取，而不是通过记忆词典（如我们人类所做的）。我们应该已经意识到，运行在硅片上的算法以不同于大脑中突触的方式看待信息。

然而，正是这种替代性、尽管同样有效的观点，可能使得 AI 在意图上走上与人类不同的道路。这可能让我们失去控制。

根据 Ilyas 等人的研究，一个关键的关注点应该是将人类偏见嵌入到机器学习算法中，以便始终将其引导到我们期望的*视角*中。Tsipras 说，他们正在积极利用鲁棒性作为工具，以将模型偏向学习对人类更有意义的特征。

在开发和训练过程中通过包括先验来使我们的模型更符合人类的期望，可能是确保我们不失去对未来 AI 控制的关键起点。

因此，在你的下一个机器学习项目中，考虑保留一点“人性”。

…

本文引用的资源：

1.  Amodei, D. 等. “AI 安全中的具体问题。” [arXiv.org](https://arxiv.org/abs/1606.06565), 2016 年 6 月。

1.  Tanay, T. “对抗样本的解释。” [KDnuggets](https://www.kdnuggets.com/2018/10/adversarial-examples-explained.html)，2018 年 10 月。

1.  Jain, A. “用对抗攻击破解神经网络。” [KDnuggets](https://www.kdnuggets.com/2019/03/breaking-neural-networks-adversarial-attacks.html)，2019 年 3 月。

1.  Papernot, N. 等. “针对机器学习的实际黑箱攻击。” [arXiv.org](https://arxiv.org/abs/1602.02697)，2016 年 2 月。

1.  Papernot, N. 等. “机器学习中的可迁移性：从现象到使用对抗样本的黑箱攻击。” [arXiv.org](https://arxiv.org/abs/1605.07277)，2016 年 5 月。

1.  Jost, Z. “机器学习安全性。” [KDnuggets](https://www.kdnuggets.com/2019/01/machine-learning-security.html)，2019 年 1 月。

1.  Ilyas, A. 等. “对抗样本不是错误，它们是特性。” [arXiv.org](https://arxiv.org/abs/1905.02175)，2019 年 5 月。

**相关：**

+   [大数据的 3 个重大问题及其解决方法](https://www.kdnuggets.com/2019/04/3-big-problems-big-data.html)

+   [机器学习与人类的相遇——来自 HUML 2016 的见解](https://www.kdnuggets.com/2017/01/machine-learning-humans-huml-2016.html)

+   [AI 会议 SF 第 2 天的关键要点：AI 与安全、对抗样本、创新](https://www.kdnuggets.com/2018/10/key-takeaways-aiconf-san-francisco-day2.html)

### 更多相关话题

+   [GPT-4 易受提示注入攻击导致误导信息](https://www.kdnuggets.com/2023/05/gpt4-vulnerable-prompt-injection-attacks-causing-misinformation.html)

+   [10 个最常见的数据质量问题及其解决方法](https://www.kdnuggets.com/2022/11/10-common-data-quality-issues-fix.html)

+   [什么是对抗性机器学习？](https://www.kdnuggets.com/2022/03/adversarial-machine-learning.html)

+   [机器学习算法——什么、为什么以及如何？](https://www.kdnuggets.com/2022/09/machine-learning-algorithms.html)

+   [为什么机器学习模型在沉默中消亡？](https://www.kdnuggets.com/2022/01/machine-learning-models-die-silence.html)

+   [机器学习没有为我的业务创造价值。为什么？](https://www.kdnuggets.com/2021/12/machine-learning-produce-value-business.html)
