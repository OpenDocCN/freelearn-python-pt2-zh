# 前言

Python 很棒！

从 20 世纪 80 年代末的最早版本到当前版本，它以相同的理念发展：提供一种多范式编程语言，考虑可读性和生产力。

人们过去常常将 Python 视为又一种脚本语言，并不认为它适合用来构建大型系统。然而，多年来，多亏了一些先锋公司，显而易见的是 Python 可以用来构建几乎任何类型的系统。

事实上，许多来自其他语言的开发人员都被 Python 所吸引，并将其作为自己的首选语言。

这是您可能已经知道的内容，所以无需进一步说服您使用这种语言的优点。

本书是根据多年使用 Python 构建各种应用程序的经验而写成的，从几小时内完成的小型系统脚本到由数十名开发人员在数年内编写的非常大型应用程序。

它描述了开发人员在使用 Python 时使用的最佳实践。

本书涵盖了一些不专注于语言本身的主题，而是关于用于处理它的工具和技术。

换句话说，这本书描述了高级 Python 开发人员每天的工作。

# 本书涵盖的内容

第一章，Python 的当前状态，展示了 Python 语言及其社区的当前状态。它展示了 Python 是如何不断变化的，为什么它在变化，以及为什么这些事实对任何想要称自己为 Python 专业人士的人都很重要。本章还介绍了在 Python 中工作的最流行和规范的方式——流行的生产工具和现在已经成为事实标准的约定。

第二章，语法最佳实践——类级别以下，以高级方式介绍了迭代器、生成器、描述符等。它还涵盖了关于 Python 成语和内部 CPython 类型实现的有用注释，以及它们的计算复杂性作为展示成语的理由。

第三章，语法最佳实践——类级别以上，解释了语法最佳实践，但侧重于类级别以上。它涵盖了 Python 中可用的更高级的面向对象的概念和机制。为了理解本章的最后一部分，需要掌握这些知识，该部分介绍了 Python 中元编程的不同方法。

第四章，选择良好的名称，涉及选择良好的名称。它是对 PEP 8 的扩展，包括命名最佳实践，还提供了有关设计良好 API 的建议。

第五章，编写包，解释了如何创建 Python 包以及使用哪些工具来正确在官方 Python 包索引或任何其他包存储库上分发它。关于包的信息还附有一个简要回顾，介绍了允许您从 Python 源代码创建独立可执行文件的工具。

第六章，部署代码，主要面向 Python 网络开发人员和后端工程师，因为它涉及代码部署。它解释了 Python 应用程序应该如何构建，以便轻松部署到远程服务器，以及您可以使用哪些工具来自动化该过程。这一章与第五章，编写包，相呼应，因为它展示了如何使用包和私有包存储库来简化应用程序部署。

第七章，“其他语言中的 Python 扩展”，解释了为什么有时为 Python 编写 C 扩展可能是一个好的解决方案。它还表明只要使用适当的工具，这并不像看起来那么困难。

第八章，“管理代码”，提供了一些关于如何管理项目代码库的见解，并解释了如何设置各种持续开发流程。

第九章，“记录您的项目”，涵盖了文档编写，并提供了有关技术写作以及如何记录 Python 项目的建议。

第十章，“测试驱动开发”，解释了测试驱动开发的基本原则以及可以在这种开发方法中使用的工具。

第十一章，“优化-一般原则和分析技术”，解释了优化。它提供了分析技术和优化策略指南。

第十二章，“优化-一些强大的技术”，扩展了第十一章，“优化-一般原则和分析技术”，提供了一些常见的解决方案，用于解决 Python 程序中经常出现的性能问题。

第十三章，“并发”，介绍了 Python 中广泛的并发主题。它解释了并发是什么，何时可能需要编写并发应用程序，以及 Python 程序员的并发主要方法。

第十四章，“有用的设计模式”，通过一组有用的设计模式和 Python 中的示例实现来结束本书。

# 您需要为本书做好准备

这本书是为那些在任何 Python 3 可用的操作系统下工作的开发人员编写的。

这不是一本面向初学者的书，所以我假设您已经在您的环境中安装了 Python，或者知道如何安装它。无论如何，这本书考虑到并非每个人都需要完全了解最新的 Python 功能或官方推荐的工具。这就是为什么第一章提供了常见实用工具（如虚拟环境和 pip）的回顾，这些现在被认为是专业 Python 开发人员的标准工具。

# 这本书是为谁写的

这本书是为希望在掌握 Python 方面更进一步的 Python 开发人员编写的。而且我所说的开发人员主要是专业人士，所以是以 Python 为生写软件的程序员。这是因为它主要关注的是对于在 Python 中创建高性能、可靠和可维护软件至关重要的工具和实践。

这并不意味着爱好者找不到有趣的东西。这本书对于任何对学习 Python 高级概念感兴趣的人来说都应该很棒。任何具有基本 Python 技能的人都应该能够理解本书的内容，尽管对于经验较少的程序员来说可能需要额外的努力。对于那些仍然使用 Python 2.7 或更早版本的人来说，这也应该是 Python 3.5 的一个很好的介绍。

最终，应该从阅读本书中受益最多的群体是 Web 开发人员和后端工程师。这是因为本书中有两个特别重要的主题：可靠的代码部署和并发。

# 惯例

在本书中，您将找到一些区分不同信息类型的文本样式。以下是这些样式的一些示例及其含义的解释。

文本中的代码单词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄显示如下：“使用`str.encode(encoding, errors)`方法，该方法使用注册的编码器对字符串进行编码。”

一块代码设置如下：

```py
[print("hello world")
print "goodbye python2"
```

当我们希望引起您对代码块的特定部分的注意时，相关行或项目以粗体设置：

```py
cdef long long fibonacci_cc(unsigned int n) nogil:
    if n < 2:
        return n
    else:
        return fibonacci_cc(n - 1) + fibonacci_cc(n - 2)
```

任何命令行输入或输出都以以下方式编写：

```py
$ pip show pip
---
Metadata-Version: 2.0
Name: pip
Version: 7.1.2
Summary: The PyPA recommended tool for installing Python packages.
Home-page: https://pip.pypa.io/
Author: The pip developers
Author-email: python-virtualenv@groups.google.com
License: MIT
Location: /usr/lib/python2.7/site-packages
Requires:

```

**新术语**和**重要单词**以粗体显示。您在屏幕上看到的单词，例如在菜单或对话框中出现在文本中，就像这样：“单击**下一步**按钮将您移动到下一个屏幕。”

### 注意

警告或重要说明显示在这样的框中。

### 提示

提示和技巧显示如下。
