## Python 是什么？

### 它是一种编程语言！

那么 Python 是什么？简单地说，Python 是一种编程语言。它最初是由 Guido van Rossum 在 20 世纪 80 年代末在荷兰开发的。Guido 继续积极参与指导语言的发展和演变，以至于他被赋予了“终身仁慈独裁者”的称号，或者更常见的*BDFL*。Python 是作为一个开源项目开发的，可以自由下载和使用。非营利性的[Python 软件基金会](https://www.python.org/psf/)管理 Python 的知识产权，在推广语言方面发挥着重要作用，并在某些情况下资助其发展。

在技术层面上，Python 是一种强类型语言。这意味着语言中的每个对象都有一个确定的类型，通常没有办法规避该类型。与此同时，Python 是动态类型的，这意味着在运行代码之前没有对代码进行类型检查。这与 C++或 Java 等静态类型语言形成对比，编译器会为您进行大量的类型检查，拒绝错误使用对象的程序。最终，对 Python 类型系统的最佳描述是它使用*鸭子类型*，其中对象在运行时才确定其适用于上下文。我们将在第八章中更详细地介绍这一点。

Python 是一种通用编程语言。它并不是用于任何特定领域或环境，而是可以丰富地用于各种任务。当然，也有一些领域不太适合它 - 例如在极端时间敏感或内存受限的环境中 - 但大多数情况下，Python 像许多现代编程语言一样灵活和适应性强，比大多数编程语言更灵活。

Python 是一种解释型语言。从技术上讲，这有点错误，因为 Python 在执行之前通常会被编译成一种字节码形式。然而，这种编译是隐形的，使用 Python 的体验通常是立即执行代码，没有明显的编译阶段。编辑和运行之间的中断缺失是使用 Python 的一大乐趣之一。

Python 的语法旨在清晰、可读和富有表现力。与许多流行的语言不同，Python 使用空格来界定代码块，并在这个过程中摒弃了大量不必要的括号，同时强制执行统一的布局。这意味着所有 Python 代码在重要方面看起来都是相似的，你可以很快学会阅读 Python。与此同时，Python 富有表现力的语法意味着你可以在一行代码中表达很多含义。这种富有表现力、高度可读的代码意味着 Python 的维护相对容易。

Python 语言有多种实现。最初 - 也是迄今为止最常见的 - 实现是用 C 编写的。这个版本通常被称为*CPython*。当有人谈论“运行 Python”时，通常可以安全地假设他们在谈论 CPython，这也是我们在本书中将使用的实现。

Python 的其他实现包括：

+   [Jython](http://www.jython.org/)，编写以针对 Java 虚拟机

+   [IronPython](http://ironpython.net/)，编写以针对.NET 平台

+   [PyPy](http://pypy.org/)，用一种称为 RPython 的语言编写（有点循环），该语言旨在开发像 Python 这样的动态语言

这些实现通常落后于 CPython，后者被认为是该语言的“标准”。本书中学到的大部分内容都适用于所有这些实现。

#### Python 语言的版本

Python 语言目前有两个重要的常用版本：Python 2 和 Python 3。这两个版本代表了语言中一些关键元素的变化，除非你采取特殊预防措施，否则为其中一个版本编写的代码通常不会适用于另一个版本。Python 2 比 Python 3 更老，更为成熟，但 Python 3 解决了较老版本中的一些已知缺陷。Python 3 是 Python 的明确未来，如果可能的话，你应该使用它。

虽然 Python 2 和 3 之间存在一些关键差异，但这两个版本的大部分基础知识是相同的。如果你学会了其中一个，大部分知识都可以顺利转移到另一个版本。在本书中，我们将教授 Python 3，但在必要时我们会指出版本之间的重要差异。

### 这是一个标准库！

除了作为一种编程语言外，Python 还附带一个强大而广泛的标准库。Python 哲学的一部分是“电池包含”，这意味着你可以直接使用 Python 来处理许多复杂的现实任务，无需安装第三方软件包。这不仅非常方便，而且意味着通过使用有趣、引人入胜的示例来学习 Python 更容易 - 这也是我们在本书中的目标！

“电池包含”方法的另一个重要影响是，这意味着许多脚本 - 即使是非平凡的脚本 - 可以立即在任何 Python 安装上运行。这消除了在安装软件时可能面临的其他语言的常见烦人障碍。

标准库有相当高水平的良好文档。API 有很好的文档，模块通常有良好的叙述描述，包括快速入门指南、最佳实践信息等。[标准库文档始终可在线获取](https://docs.python.org/3/library/index.html)，如果需要，你也可以在本地安装它。

由于标准库是 Python 的重要组成部分，我们将在本书中涵盖其中的部分内容。即便如此，我们也不会涵盖其中的一小部分，因此鼓励你自行探索。

### 这是一种哲学

最后，没有描述 Python 的内容是完整的，没有提到对许多人来说，Python 代表了编写代码的一种哲学。清晰和可读性的原则是编写正确或*pythonic*代码的一部分。在所有情况下，*pythonic*的含义并不总是清晰，有时可能没有单一的正确写法。但 Python 社区关注简单、可读性和明确性的事项意味着 Python 代码往往更…嗯…美丽！

Python 的许多原则体现在所谓的“Python 之禅”中。这个“禅”不是一套严格的规则，而是一套在编码时牢记的指导原则或准则。当你试图在几种行动方案之间做出决定时，这些原则通常可以给你一个正确的方向。我们将在本书中突出显示“Python 之禅”的元素。