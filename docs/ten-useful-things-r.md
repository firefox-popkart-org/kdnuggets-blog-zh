# R 中你可能不知道的十个随机有用的东西

> 原文：[`www.kdnuggets.com/2019/06/ten-useful-things-r.html`](https://www.kdnuggets.com/2019/06/ten-useful-things-r.html)

![c](img/3d9c022da2d331bb56691a9617b91b90.png) 评论

**由[Keith McNulty](https://www.linkedin.com/in/keith-mcnulty/)，麦肯锡公司**

我经常发现自己向同事和其他程序员介绍一些我在 R 中使用的简单方法，这些方法真的对我完成任务有很大帮助。这些方法从微不足道的快捷键，到鲜为人知的函数，再到实用的小技巧。

* * *

## 我们的三大课程推荐

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 1\. [谷歌网络安全证书](https://www.kdnuggets.com/google-cybersecurity) - 快速进入网络安全职业生涯。

![](img/e225c49c3c91745821c8c0368bf04711.png) 2\. [谷歌数据分析专业证书](https://www.kdnuggets.com/google-data-analytics) - 提升你的数据分析技能

![](img/0244c01ba9267c002ef39d4907e0b8fb.png) 3\. [谷歌 IT 支持专业证书](https://www.kdnuggets.com/google-itsupport) - 支持你的组织进行 IT 工作

* * *

因为 R 生态系统如此丰富且不断发展，人们常常会错过一些真正能帮助他们完成任务的东西。所以我经常从观众那里得到类似“我从未知道过这个！”的惊讶反应。

这里有十个让我在 R 工作时生活更轻松的东西。如果你已经知道这些内容，抱歉浪费了你的阅读时间，并请考虑添加一个评论，分享你认为对其他读者有用的东西。

### 1\. `switch`函数

我喜欢`switch()`。它基本上是`if`语句的一种便利缩写，能够根据另一个变量的值选择其值。我发现它在编写需要根据你做出的先前选择加载不同数据集的代码时特别有用。例如，如果你有一个叫`animal`的变量，并且你想根据`animal`是狗、猫还是兔子来加载不同的数据集，你可以这样写：

```py

data <- read.csv(
  switch(animal,
         "dog" = "dogdata.csv",
         "cat" = "catdata.csv",
         "rabbit" = "rabbitdata.csv")
)

```

这在 Shiny 应用中特别有用，你可能希望根据一个或多个输入菜单选择来加载不同的数据集甚至环境文件。

### 2\. RStudio 快捷键

这不仅仅是一个 R 的技巧，更关于 RStudio IDE，但常用命令的快捷键非常有用，可以节省很多输入时间。我最喜欢的两个是 Ctrl+Shift+M 用于管道操作符`%>%`和 Alt+-用于赋值操作符`<-`。如果你想查看这些绝妙的快捷键的完整列表，只需在 RStudio 中输入 Alt+Shift+K。

### 3\. flexdashboard 包

如果你想快速创建一个简洁的 Shiny 仪表盘，`flexdashboard` 包含了你所需的一切。它提供了简单的 HTML 快捷方式，使得侧边栏的构建和显示的行列组织变得容易。它还有一个超级灵活的标题栏，你可以将应用程序组织成不同的页面，并插入图标和指向 Github 代码或电子邮件地址等的链接。作为一个在 `RMarkdown` 中运行的包，它也允许你将所有应用程序保存在一个 `Rmd` 文件中，而无需像 `shinydashboard` 那样将其拆分为单独的服务器和 UI 文件。我在需要创建一个简单的仪表盘原型版本时总是使用 `flexdashboard`，然后再将其移到更高级的设计中。我通常可以在一个小时内使用 `flexdashboard` 搭建好仪表盘。

### 4\. R Shiny 中的 req 和 validate 函数

R Shiny 开发可能会很令人沮丧，尤其是当你收到那些无法帮助你理解问题所在的通用错误消息时。随着 Shiny 的发展，越来越多的验证和测试函数被添加进来，以帮助更好地诊断和提醒特定错误的发生。`req()` 函数允许你在环境中存在另一个变量时才执行某个操作，但它是静默的，不会显示错误。因此，你可以根据先前的操作使 UI 元素的显示变为条件。例如，参考我上面第 1 个例子：

```py

output$go_button <- shiny::renderUI({

  # only display button if an animal input has been chosen

  shiny::req(input$animal)

  # display button

  shiny::actionButton("go",
                      paste("Conduct", input$animal, "analysis!")
  )
})

```

`validate()` 在渲染输出之前进行检查，并使你能够返回一个量身定制的错误消息，如果某个条件未满足，例如用户上传了错误的文件：

```py

# get csv input file

inFile <- input$file1
data <- inFile$datapath

# render table only if it is dogs

shiny::renderTable({
  # check that it is the dog file, not cats or rabbits
  shiny::validate(
    need("Dog Name" %in% colnames(data)),
    "Dog Name column not found - did you load the right file?"
  )

  data
})

```

有关这些函数的更多信息，请参阅我其他的文章 [这里](https://towardsdatascience.com/in-r-shiny-when-is-an-error-really-an-error-702205ebb5d5)。

### 5\. 使用系统环境将所有凭证保留给自己

如果你分享的代码需要数据库等的登录凭证，你可以使用系统环境来避免将这些凭证发布到 Github 或其他可能存在风险的地方。你可以在 R 会话中将凭证作为命名的环境变量，例如：

```py

Sys.setenv(
  DSN = "database_name",
  UID = "User ID",
  PASS = "Password"
)

```

然后在你共享的脚本中，你可以使用这些环境变量进行登录。例如：

```py

db <- DBI::dbConnect(
  drv = odbc::odbc(),
  dsn = Sys.getenv("DSN"),
  uid = Sys.getenv("UID"),
  pwd = Sys.getenv("PASS")
)

```

更方便的是，如果这些凭证是你经常使用的，你可以将它们设置为操作系统中的环境变量，这样在你使用 R 时，它们将始终可用，而你无需在代码中显示它们。（只是要小心谁有权访问这些凭证！）

### 6\. 使用 styler 自动化 tidyverse 风格

这一天很艰难，你的任务繁重。你的代码没有你想要的那么整洁，你没有时间逐行编辑它。不要担心。`styler` 包提供了许多函数，可以自动重新样式化你的代码以匹配 tidyverse 风格。只需在你的凌乱脚本上运行 `styler::style_file()`，它将为你完成大量（但不是全部）的工作。

### 7. 参数化 R Markdown 文档

所以你写了一份很棒的 R Markdown 文档，分析了关于狗的大量事实。然后你被告知——‘不，我对猫更感兴趣’。不要担心。如果你将你的 R Markdown 文档参数化，你可以只用一个命令自动生成类似的猫的报告。

你可以通过在 R Markdown 文档的 YAML 头部定义参数，并给每个参数一个值来做到这一点。例如：

```py

---
title: "Animal Analysis"
author: "Keith McNulty"
date: "21 March 2019"
output:
  html_document:
    code_folding: "hide"
params:
  animal_name:
    value: Dog
    choices:
      - Dog
      - Cat
      - Rabbit
  years_of_study:
    input: slider
    min: 2000
    max: 2019
    step: 1
    round: 1
    sep: ''
    value: [2010, 2017]
---

```

现在你可以在文档中的 R 代码里写入这些变量，如 `params$animal_name` 和 `params$years_of_study`。如果你像平常一样编织文档，它将使用这些参数的默认值进行编织。然而，如果你在 RStudio 的 Knit 下拉菜单中选择参数编织（或使用 `knit_with_parameters()`），一个漂亮的菜单选项会出现，让你在编织文档前选择你的参数。太棒了！

![figure-name](img/f38e3447a7255a86c388f21bc976d70c.png)使用参数进行编织

### 8. revealjs

`revealjs` 是一个允许你创建漂亮 HTML 演示文稿的包，具有直观的幻灯片导航菜单，并嵌入 R 代码。它可以在 R Markdown 中使用，并具有非常直观的 HTML 快捷键，允许你创建一个嵌套的、逻辑结构的漂亮幻灯片，并有多种样式选项。演示文稿以 HTML 格式呈现，意味着人们可以在平板或手机上跟随你的讲解，这非常方便。你可以通过安装这个包并在你的 YAML 头部中调用它来设置 `revealjs` 演示文稿。这是我最近用 `revealjs` 做的一次演讲的 YAML 头部示例

```py

---
title: "Exporing the Edge of the People Analytics Universe"
author: "Keith McNulty"
output:
  revealjs::revealjs_presentation:
    center: yes
    template: starwars.html
    theme: black
date: "HR Analytics Meetup London - 18 March, 2019"
resource_files:
- darth.png
- deathstar.png
- hanchewy.png
- millenium.png
- r2d2-threepio.png
- starwars.html
- starwars.png
- stormtrooper.png
---

```

这是一个示例页面。你可以在 [这里](https://github.com/keithmcnulty/hr_meetup_london/blob/master/presentation.Rmd) 找到代码，在 [这里](http://rpubs.com/keithmcnulty/hr_meetup_london) 找到演示文稿。

![figure-name](img/a712d903c5655a3bf4e13ba3f5e3184a.png)使用 revealjs 进行轻松的在线演示

### 9. R Shiny 中的 HTML 标签（例如，在你的 Shiny 应用中播放音频）

大多数人没有充分利用 R Shiny 中可用的 HTML 标签。有 110 个标签提供了各种 HTML 格式和其他命令的快捷方式。最近我构建了一个 Shiny 应用，执行一个任务花了很长时间。知道用户可能在等待完成时会多任务处理，我使用了 `tags$audio` 来播放胜利的庆祝音乐，以在任务完成时提醒用户。

### 10. praise 包

极其简单却又很棒，`praise` 包向用户提供赞美。虽然这看起来像是无用的自我赞赏，但它实际上在编写 R 包时非常有用，比如当某个过程成功完成时，你可以给予赞美或鼓励。你也可以把它放在复杂脚本的末尾，以便在脚本成功运行时获得额外的幸福感。

![figure-name](img/d801e873d262298b6823aa52dca097ad.png)The praise package

*最初我是一名纯数学家，然后我成为了心理测量学家和数据科学家。我对将这些学科的严谨性应用于复杂的人际问题充满热情。我还是一个编码爱好者以及日本 RPG 的超级粉丝。可以在*[*LinkedIn*](https://www.linkedin.com/in/keith-mcnulty/)*或*[*Twitter*](https://twitter.com/dr_keithmcnulty)*上找到我。*

![figure-name](img/d0dd8df4c324cc9b838ee47baf90f354.png)

**个人简介: [Keith McNulty](https://www.linkedin.com/in/keith-mcnulty/)** 是麦肯锡公司的数据科学家。

[原文](https://towardsdatascience.com/ten-random-useful-things-in-r-that-you-might-not-know-about-54b2044a3868?sk=f6c2757ec829c9cfea19cbed8db2f395)。转载已获许可。

**相关:**

+   数据清理的顶级 R 包

+   2018 年数据科学和 AI 的 7 大 R 包

+   R 语言中的自动化网页抓取

### 更多相关内容

+   [你可能不知道的 5 个 Pandas 绘图函数](https://www.kdnuggets.com/2023/02/5-pandas-plotting-functions-might-know.html)

+   [你可能会在下一次面试中看到的 24 道 SQL 问题](https://www.kdnuggets.com/2022/06/24-sql-questions-might-see-next-interview.html)

+   [你不知道的 7 种低代码工具的使用方法](https://www.kdnuggets.com/2022/09/7-things-didnt-know-could-low-code-tool.html)

+   [十年 AI 回顾](https://www.kdnuggets.com/2023/06/ten-years-ai-review.html)

+   [在业务中实施推荐系统的十个关键经验教训](https://www.kdnuggets.com/2022/07/ten-key-lessons-implementing-recommendation-systems-business.html)

+   [你需要了解的数据管理及其重要性…](https://www.kdnuggets.com/2022/05/6-things-need-know-data-management-matters-computer-vision.html)
