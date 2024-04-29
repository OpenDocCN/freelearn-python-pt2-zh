# 前言

区块链被视为所有加密货币交易的公共账本的主要技术解决方案。本书是开发完全成熟的使用 Python 与区块链应用程序的各种构建模块进行交互的去中心化应用程序的实用指南。

*面向 Python 开发人员的区块链实战*首先演示了区块链技术和加密货币哈希是如何工作的。您将了解智能合约的基本原理和好处，比如抗审查和交易准确性。随着您的稳步进展，您将开始使用类似 Python 的 Vyper 构建智能合约。这种经验将进一步帮助您揭开智能合约的其他好处，比如可靠的存储和备份，以及效率。您还将使用 web3.py 与智能合约进行交互，并利用 web3.py 和 Populus 框架的力量构建提供安全性和与加密货币无缝集成的去中心化应用程序。随着您探索后续章节，您将学习如何在以太坊上创建自己的代币，并使用 PySide2 库构建一个可以处理以太坊和以太坊请求的评论（ERC-20）代币的加密货币钱包图形用户界面（GUI）。这将使用户能够无缝地存储、发送和接收数字货币。最后，您将在您的去中心化应用程序中实现星际文件系统（IPFS）技术，以提供一个可以存储和公开媒体的点对点文件系统。

通过本书，您将精通区块链编程，并能够使用 Python 在各种领域构建端到端的去中心化应用程序。

# 这本书是为谁准备的

如果您是一名想要进入区块链世界的 Python 开发人员，*面向 Python 开发人员的区块链实战*适合您。本书将成为您精通区块链生态系统并使用 Python 和库支持构建自己的去中心化应用程序的指南。

# 这本书涵盖了什么

第一章，*区块链编程简介*，讲述了比特币的故事以及比特币的价值所在。您将了解赋予比特币力量的基础技术，即区块链技术。此外，您还将了解以太坊最初的创建目的。

第二章，*智能合约基础*，展示了传统程序和智能合约之间的区别。您将了解传统程序存在哪些缺陷，以及为什么智能合约有潜力克服这些缺陷。您还将看到智能合约现在正在被应用在哪些领域。

第三章，*使用 Vyper 实现智能合约*，教您如何使用类似 Python 的 Vyper 编程语言编写智能合约。您将学习 Vyper 编程语言的许多重要特性。

第四章，*使用 Web3 与智能合约交互*，向您展示如何安装`web3.py`库，如何与智能合约交互，以及如何部署智能合约。

第五章，*Populus 开发框架*，向您展示如何使用 Populus 开发框架，并认识到它对开发人员的价值。

第六章，*构建实用的去中心化应用程序*，教您如何使用`web3.py`库和 Populus 开发框架构建去中心化应用程序。

第七章，*前端去中心化应用程序*，向您展示如何使用桌面前端构建类似 Twitter 的去中心化应用程序。

第八章，*在以太坊中创建代币*，教你如何创建自己的代币。这是一个关于如何启动自己的加密货币的实践学习指南。

第九章，*加密货币钱包*，向您展示如何使用桌面前端构建以太坊钱包。

第十章，*星际文件系统-一个全新的文件系统*，是对星际文件系统的介绍，人们可以在其中存储分布式文件。在区块链中，存储是昂贵的。在区块链上存储图像文件（更不用说视频文件了）已经是不可行的。IPFS 是一种新技术，旨在解决这个问题。您将了解 IPFS 是什么，以及这项技术目前的状态。

第十一章，*使用 ipfsapi 与 IPFS 交互*，教你如何使用 Python 库连接到 IPFS 节点。

第十二章，*使用 IPFS 实现去中心化应用*，向您展示如何实现一个利用 IPFS 技术的去中心化视频分享应用。

# 充分利用本书

必须具备 Python 的基本知识。本书适用于希望从事区块链开发的 Python 开发人员。

# 下载示例代码文件

您可以从[www.packt.com](http://www.packt.com)的帐户中下载本书的示例代码文件。如果您在其他地方购买了本书，您可以访问[www.packt.com/support](http://www.packt.com/support)并注册，文件将直接发送到您的邮箱。

您可以按照以下步骤下载代码文件：

1.  登录或注册[www.packt.com](http://www.packt.com)。

1.  选择 SUPPORT 选项卡。

1.  点击“代码下载和勘误”。

1.  在搜索框中输入书名，然后按照屏幕上的说明操作。

文件下载后，请确保使用最新版本的解压缩或提取文件夹：

+   WinRAR/7-Zip for Windows

+   Zipeg/iZip/UnRarX for Mac

+   7-Zip/PeaZip for Linux

该书的代码包也托管在 GitHub 上，网址为[`github.com/PacktPublishing/Hands-On-Blockchain-for-Python-Developers`](https://github.com/PacktPublishing/Hands-On-Blockchain-for-Python-Developers)。如果代码有更新，将在现有的 GitHub 存储库上进行更新。

我们还有来自我们丰富的书籍和视频目录的其他代码包，可在[`github.com/PacktPublishing/`](https://github.com/PacktPublishing/)上找到。去看看吧！

# 下载彩色图像

我们还提供了一个 PDF 文件，其中包含本书中使用的屏幕截图/图表的彩色图像。您可以在这里下载：[`www.packtpub.com/sites/default/files/downloads/9781788627856_ColorImages.pdf`](https://www.packtpub.com/sites/default/files/downloads/9781788627856_ColorImages.pdf)。

# 使用的约定

本书中使用了许多文本约定。

`CodeInText`：表示文本中的代码词、数据库表名、文件夹名、文件名、文件扩展名、路径名、虚拟 URL、用户输入和 Twitter 句柄。例如："如果你在 Linux 平台上，你将下载这个文件：`qt-unified-linux-x64-3.0.5-online.run`。"

代码块设置如下：

```py
"compilation": {
    "backend": {
      "class": "populus.compilation.backends.VyperBackend"
    },
    "contract_source_dirs": [
      "./contracts"
    ],
    "import_remappings": []
},
```

任何命令行输入或输出都是这样写的：

```py
$ python3.6 -m venv qt-venv $ source qt-venv/bin/activate (qt-venv) $ pip install PySide2
```

**粗体**：表示一个新术语、一个重要单词或者屏幕上看到的单词。例如，菜单中的单词或对话框中的单词会出现在文本中，就像这样。例如："点击下一步。然后你会看到一个登录屏幕。"

警告或重要说明会出现在这样的地方。提示和技巧会出现在这样的地方。
