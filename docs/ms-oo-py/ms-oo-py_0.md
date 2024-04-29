# 前言

本书将向您介绍 Python 编程语言的更高级特性。重点是创建尽可能高质量的 Python 程序。这通常意味着创建具有最高性能或最易维护性的程序。这意味着探索设计替代方案，并确定哪种设计在解决的问题中提供了最佳性能。

本书大部分内容将探讨给定设计的多种替代方案。有些性能更好。有些看起来更简单，或者对于问题领域来说是更好的解决方案。找到最佳算法和最佳数据结构，以最少的计算机处理来创造最大价值是至关重要的。时间就是金钱，节省时间的程序将为其用户创造更多价值。

Python 使许多内部特性直接可用于我们的应用程序。这意味着我们的程序可以与现有的 Python 功能紧密集成。通过确保我们的 OO 设计良好地集成，我们可以利用许多 Python 功能。

我们经常专注于一个特定的问题，并检查该问题的几种不同解决方案。当我们研究不同的算法和数据结构时，我们将看到不同的内存和性能替代方案。通过研究替代解决方案，以便正确优化最终应用程序，这是一项重要的 OO 设计技能。

本书的一个更重要的主题是，对于任何问题，都没有单一的*最佳*方法。有许多不同属性的替代方法。

在编程*风格*方面，风格的主题引起了令人惊讶的兴趣。敏锐的读者会注意到，示例并没有在每一个细节上都严格遵循 PEP-8 的名称选择或标点符号。

随着我们朝着掌握面向对象的 Python 的目标迈进，我们将花费大量时间阅读来自各种来源的 Python 代码。我们将观察到即使在 Python 标准库模块中也存在广泛的差异。与其呈现所有完全一致的示例，我们选择了一些不一致，这种不一致将更好地符合在野外遇到的各种开源项目中所见的代码。

# 本书涵盖的内容

我们将在一系列章节中涵盖三个高级 Python 主题，深入探讨细节。

+   *一些准备工作*，涵盖了一些初步主题，如 unittest，doctest，docstrings 和一些特殊方法名称。

第一部分，*通过特殊方法创建 Pythonic 类*：本部分更深入地探讨了面向对象编程技术，以及如何更紧密地将应用程序的类定义与 Python 的内置功能集成。它包括以下九章：

+   第一章方法")，*__init__()方法*，为我们提供了对`__init__()`方法的详细描述和实现。我们将研究简单对象的不同初始化形式。从中，我们可以研究涉及集合和容器的更复杂对象。

+   第二章，*与 Python 基本特殊方法无缝集成*，将详细解释如何扩展简单的类定义以添加特殊方法。我们需要查看从对象继承的默认行为，以便了解何时需要覆盖以及何时实际上需要覆盖。

+   第三章，*属性访问、属性和描述符*，详细介绍了默认处理的工作原理。我们需要决定何时以及在哪里覆盖默认行为。我们还将探讨描述符，并更深入地了解 Python 内部的工作原理。

+   第四章，一致设计的抽象基类，总体上看抽象基类在`collections.abc`模块中的抽象基类。我们将研究我们可能想要修改或扩展的各种容器和集合背后的一般概念。同样，我们将研究我们可能想要实现的数字背后的概念。

+   第五章，使用可调用对象和上下文，通过`contextlib`中的工具来创建上下文管理器的几种方法。我们将展示一些可调用对象的变体设计。这将向您展示为什么有状态的可调用对象有时比简单函数更有用。我们还将研究如何使用一些现有的 Python 上下文管理器，然后再深入编写我们自己的上下文管理器。

+   第六章，创建容器和集合，着重介绍容器类的基础知识。我们将回顾涉及成为容器并提供容器所提供的各种特性的各种特殊方法。我们将讨论扩展内置容器以添加功能。我们还将研究包装内置容器并通过包装器委托方法到底层容器。

+   第七章，创建数字，涵盖了这些基本算术运算符：`+`，`-`，`*`，`/`，`//`，`%`和`**`。我们还将研究这些比较运算符：`<`，`>`，`<=`，`>=`，`==`和`!=`。最后，我们将总结一些扩展或创建新数字时涉及的设计考虑。

+   第八章，装饰器和混入-横切面，涵盖了简单的函数装饰器，带参数的函数装饰器，类装饰器和方法装饰器。

第二部分，持久性和序列化：持久对象已经序列化到存储介质。也许它被转换为 JSON 并写入文件系统。ORM 层可以将对象存储在数据库中。这部分将研究处理持久性的替代方案。本节包括以下五章：

+   第九章，序列化和保存-JSON，YAML，Pickle，CSV 和 XML，涵盖了使用专注于各种数据表示的库进行简单持久化，如 JSON，YAML，pickle，XML 和 CSV。

+   第十章，通过 Shelve 存储和检索对象，解释了使用 Python 模块进行基本数据库操作，比如`shelve`（和`dbm`）。

+   第十一章，通过 SQLite 存储和检索对象，转向更复杂的 SQL 和关系数据库世界。因为 SQL 特性与面向对象编程特性不匹配，我们有一个**阻抗不匹配**问题。一个常见的解决方案是使用 ORM 允许我们持久化大量对象域。

+   第十二章，传输和共享对象，看一下 HTTP 协议，JSON，YAML 和 XML 表示来传输对象。

+   第十三章，配置文件和持久性，涵盖了 Python 应用程序可以处理配置文件的各种方式。

第三部分, *测试、调试、部署和维护*：我们将向您展示如何收集数据以支持和调试高性能程序。这将包括有关创建最佳文档以减少支持的混乱和复杂性的信息。本节包含最后五章，如下所示：

+   第十四章, *日志和警告模块*，介绍了使用`logging`和`warning`模块创建审计信息以及调试。我们将迈出使用`print()`函数的重要一步。

+   第十五章, *可测试性设计*，涵盖了可测试性设计以及我们如何使用`unittest`和`doctest`。

+   第十六章, *处理命令行*，介绍了使用`argparse`模块解析选项和参数。我们将进一步使用命令设计模式来创建可以组合和扩展的程序组件，而无需编写 shell 脚本。

+   第十七章, *模块和包设计*，涵盖了模块和包设计。这是一组更高级的考虑。我们将研究模块中的相关类和包中的相关模块。

+   第十八章, *质量和文档*，介绍了我们如何记录我们的设计，以建立对我们的软件正确性和正确实施的信任。

# 本书所需的内容

为了编译和运行本书中提到的示例，您需要以下软件：

+   Python 版本 3.2 或更高版本，带有标准库套件。我们将专注于 Python 3.3，但与 3.2 的差异很小。

+   我们将介绍一些额外的软件包。这些包括 PyYaml、SQLAlchemy 和 Jinja2。

+   [`pyyaml.org`](http://pyyaml.org)

+   [`www.sqlalchemy.org`](http://www.sqlalchemy.org)。在构建此内容时，请查看安装指南，[`docs.sqlalchemy.org/en/rel_0_9/intro.html#installation`](http://docs.sqlalchemy.org/en/rel_0_9/intro.html#installation)。使用`--without-cextensions`选项可以简化安装。

+   [`jinja.pocoo.org/`](http://jinja.pocoo.org/)

+   可选地，您可能希望将 Sphinx 或 Docutils 添加到您的环境中，因为我们也将涵盖它们。

+   [`sphinx-doc.org`](http://sphinx-doc.org)

+   [`docutils.sourceforge.net`](http://docutils.sourceforge.net)

# 本书的受众

这是高级 Python。您需要对 Python 3 非常熟悉。您还将受益于解决较大或复杂的问题。

如果您是其他语言的熟练程序员，并且想要转换到 Python，您可能会发现本书有所帮助。本书不介绍语法或其他基础概念。

高级 Python 2 程序员在切换到 Python 3 时可能会发现这很有帮助。我们不会涵盖任何转换实用程序（例如从版本 2 到 3）或任何共存库（例如 six）。本书侧重于完全在 Python 3 中发生的新开发。

# 约定

在本书中，您将找到一些区分不同类型信息的文本样式。以下是一些示例及其含义的解释。

文本中的代码单词显示如下：“我们可以通过使用`import`语句访问其他 Python 模块。”

代码块设置如下：

```py
   class Friend(Contact):
       def __init__(self, name, email, phone):
           self.name = name
           self.email = email
           self.phone = phone
```

当我们希望引起您对代码块的特定部分的注意时，相关的行或项目以粗体显示：

```py
   class Friend(Contact):
       def __init__(self, name, email, phone):
           self.name = name
           **self.email = email**
           self.phone = phone
```

任何命令行输入或输出都以以下形式编写：

```py
**>>> e = EmailableContact("John Smith", "jsmith@example.net")**
**>>> Contact.all_contacts**

```

**新术语**和**重要单词**以粗体显示。屏幕上看到的单词，比如菜单或对话框中的单词，会以这样的方式出现在文本中："我们使用这个功能来每次点击**Roll!**按钮时更新标签为一个新的随机值"。

### 注意

警告或重要说明会显示在这样的框中。

### 提示

提示和技巧会显示为这样。