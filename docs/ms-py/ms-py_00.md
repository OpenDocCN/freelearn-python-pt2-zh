# 前言

Python 是一种易于学习的语言，从一开始就非常强大和方便。然而，精通 Python 是一个完全不同的问题。

你将遇到的每个编程问题都至少有几种可能的解决方案和/或范式可以应用于 Python 的广泛可能性之内。本书不仅将说明一系列不同和新的技术，还将解释何时何地应该应用某种方法。

这本书不是 Python 3 的初学者指南。它是一本可以教你 Python 中更高级技术的书。具体针对 Python 3.5 及以上版本，还演示了一些 Python 3.5 独有的特性，比如 async def 和 await 语句。

作为一名有多年经验的 Python 程序员，我将尝试用相关背景信息来理性地解释本书中所做的选择。然而，这些理性化并不是严格的指导方针。其中几个案例归根结底是个人风格的问题。只需知道它们源自经验，并且在许多情况下是 Python 社区推荐的解决方案。

本书中的一些参考对你来说可能不明显，如果你不是蒙提·派森的粉丝。本书在代码示例中广泛使用 spam 和 eggs 而不是 foo 和 bar。为了提供一些背景信息，我建议观看蒙提·派森的“垃圾食品”小品。它非常愚蠢！

# 本书涵盖内容

《第一章》《入门-每个项目一个环境》介绍了使用 virtualenv 或 venv 来隔离 Python 项目中的包的虚拟 Python 环境。

《第二章》《Pythonic 语法，常见陷阱和风格指南》解释了 Pythonic 代码是什么，以及如何编写符合 Python 哲学的 Pythonic 代码。

《第三章》《容器和集合-正确存储数据》是我们使用 Python 捆绑的许多容器和集合来创建快速且可读的代码的地方。

《第四章》《函数式编程-可读性与简洁性》涵盖了 Python 中可用的列表/字典/集合推导和 lambda 语句等函数式编程技术。此外，它还说明了它们与涉及的数学原理的相似之处。

《第五章》《装饰器-通过装饰实现代码重用》不仅解释了如何创建自己的函数/类装饰器，还解释了内部装饰器（如 property，staticmethod 和 classmethod）的工作原理。

《第六章》《生成器和协程-无限，一步一步》展示了生成器和协程如何用于惰性评估无限大小的结构。

《第七章》《异步 IO-无需线程的多线程》演示了使用 async def 和 await 的异步函数的用法，以便外部资源不再阻塞 Python 进程。

《第八章》《元类-使类（而不是实例）更智能》深入探讨了类的创建以及如何完全修改类的行为。

第九章，“文档-如何使用 Sphinx 和 reStructuredText”，展示了如何使用 Sphinx 自动记录你的代码，几乎不费吹灰之力。此外，它还展示了如何使用 Napoleon 语法来记录函数参数，这种方式在代码和文档中都很清晰。

第十章，“测试和日志-为错误做准备”，解释了如何测试代码以及如何添加日志以便在以后出现错误时进行轻松调试。

第十一章，“调试-解决错误”，演示了使用跟踪、日志和交互式调试来追踪错误的几种方法。

第十二章，“性能-跟踪和减少内存和 CPU 使用”，展示了几种测量和改进 CPU 和内存使用的方法。

第十三章，“多处理-当单个 CPU 核心不够用时”，说明了多处理库可以用于执行代码，不仅可以在多个处理器上执行，甚至可以在多台机器上执行。

第十四章，“C/C++扩展、系统调用和 C/C++库”，涵盖了调用 C/C++函数以实现互操作性和性能的方法，使用 Ctypes、CFFI 和本地 C/C++。

第十五章，“打包-创建自己的库或应用程序”，演示了使用 setuptools 和 setup.py 在 Python 包索引（PyPI）上构建和部署软件包。

# 你需要这本书

这本书的唯一硬性要求是 Python 解释器。建议使用 Python 3.5 或更新的解释器，但许多代码示例也可以在旧版本的 Python 中运行，比如 2.7，在文件顶部添加一个简单的 from __future__ import print_statement。

此外，第十四章，“C/C++扩展、系统调用和 C/C++库”需要 C/C++编译器，如 GCC、Visual Studio 或 XCode。Linux 机器是执行 C/C++示例最简单的机器，但在 Windows 和 OS X 机器上也应该可以轻松执行。

# 这本书是为谁准备的

如果你已经超越了绝对的 Python 初学者水平，那么这本书适合你。即使你已经是一名专业的 Python 程序员，我保证你会在这本书中找到一些有用的技巧和见解。

至少，它将允许 Python 2 程序员更多地了解 Python 3 中引入的新功能，特别是 Python 3.5。

需要基本的 Python 熟练，因为 Python 解释器的安装和基本的 Python 语法没有涵盖。

# 约定

在这本书中，你会发现许多文本样式，用来区分不同类型的信息。以下是一些这些样式的例子和它们的含义解释。

文本中的代码单词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄显示如下：“应该注意，`type()`函数还有另一个用途。”

代码块设置如下：

```py
import abc
import importlib

class Plugins(abc.ABCMeta):
    plugins = dict()

    def __new__(metaclass, name, bases, namespace):
        cls = abc.ABCMeta.__new__(
            metaclass, name, bases, namespace)
```

任何命令行输入或输出都写成如下形式，其中>>>表示 Python 控制台，#表示常规的 Linux/Unix shell：

```py
>>> class Spam(object):
…     eggs = 'my eggs'

>>> Spam = type('Spam', (object,), dict(eggs='my eggs'))

```

### 注意

警告或重要提示以这样的方式出现在一个框中。

### 提示

提示和技巧看起来像这样。
