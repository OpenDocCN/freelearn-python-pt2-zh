# 调试和故障排除

“如果调试是消除软件错误的过程，那么编程一定是引入错误的过程。”- Edsger W. Dijkstra

在专业程序员的生活中，调试和故障排除占据了相当大的时间。即使你在人类编写的最美丽的代码库上工作，仍然会有错误；这是肯定的。

在我的观点中，一个优秀的软件开发人员是一个即使在阅读没有报告错误或错误的代码时也能保持高度关注的人。

能够高效快速地调试代码是每个程序员都需要不断提高的技能。有些人认为因为他们已经阅读了手册，所以没问题，但现实是，游戏中的变量数量如此之大，以至于没有手册。有一些指导方针可以遵循，但没有一本魔法书会教你所有你需要知道的东西，以便成为这方面的专家。

在这个特定的主题上，我觉得我从同事那里学到了最多。观察一个非常熟练的人攻击问题让我感到惊讶。我喜欢看到他们采取的步骤，验证排除可能的原因，以及他们考虑嫌疑人的方式，最终导致他们找到解决方案。

我们与之合作的每个同事都可以教给我们一些东西，或者用一个最终证明是正确的奇妙猜测让我们感到惊讶。当这种情况发生时，不要只停留在惊讶中（或者更糟糕的是嫉妒），而是抓住这一刻，问问他们是如何猜到的，以及为什么。答案将让你看到是否有一些东西你可以后来深入研究，也许下一次，你就是那个发现问题的人。

有些错误很容易发现。它们是由粗心的错误造成的，一旦你看到这些错误的影响，很容易找到解决问题的方法。

但还有其他一些错误要微妙得多，更加难以捉摸，需要真正的专业知识，以及大量的创造力和超越常规的思维来处理。

对我来说，最糟糕的是那些不确定的错误。有时会发生，有时不会。有些只在环境 A 中发生，但在环境 B 中却没有，尽管 A 和 B 应该是完全相同的。这些错误是真正邪恶的，它们会让你发疯。

当然，错误不仅仅发生在沙盒中，对吧？当你的老板告诉你，“别担心！花点时间解决这个问题。先吃午饭！”的时候，不。它们发生在星期五下午五点半，当你的大脑已经烧坏，你只想回家的时候。就在那些每个人都在瞬间变得沮丧的时刻，当你的老板在你身边喘着气的时候，你必须能够保持冷静。我是认真的。如果你让自己的大脑感到紧张，那么创造性思维、逻辑推理以及你在那一刻所需要的一切都会消失。所以深呼吸，端正坐姿，集中注意力。

在这一章中，我将尝试演示一些有用的技术，根据错误的严重程度，以及一些建议，希望能够增强你对错误和问题的解决能力。

具体来说，我们将看一下以下内容：

+   调试技术

+   性能分析

+   断言

+   故障排除指南

# 调试技术

在这部分，我将向你介绍最常见的技术，我经常使用的技术；但是，请不要认为这个列表是详尽无遗的。

# 使用打印进行调试

这可能是所有技术中最简单的技术。它并不是非常有效，不能在所有地方使用，需要同时访问源代码和一个能运行它的终端（因此显示`print`函数调用结果）。

然而，在许多情况下，这仍然是一种快速和有用的调试方式。例如，如果你正在开发一个 Django 网站，页面上发生的情况与你的预期不符，你可以在视图中填充打印，并在重新加载页面时留意控制台。当你在代码中散布调用`print`时，通常会出现这样的情况，你会重复大量的调试代码，要么是因为你正在打印时间戳（就像我们在测量列表推导和生成器的速度时所做的那样），要么是因为你不得不以某种方式构建一个你想要显示的字符串。

另一个问题是，在你的代码中很容易忘记调用`print`。

因此，出于这些原因，我有时候更喜欢编写自定义函数，而不是直接调用`print`。让我们看看如何做。

# 使用自定义函数进行调试

在一个片段中有一个自定义函数，你可以快速抓取并粘贴到代码中，然后用于调试，这是非常有用的。如果你很快，你总是可以即兴编写一个。重要的是以一种不会在最终删除调用和定义时留下东西的方式编写它。因此*以一种完全自包含的方式编写它是很重要的*。这个要求的另一个很好的理由是它将避免与代码的其余部分潜在的名称冲突。

让我们看一个这样的函数的例子：

```py
# custom.py
def debug(*msg, print_separator=True):
    print(*msg)
    if print_separator:
        print('-' * 40)

debug('Data is ...')
debug('Different', 'Strings', 'Are not a problem')
debug('After while loop', print_separator=False)
```

在这种情况下，我使用了一个仅限关键字的参数，以便能够打印一个分隔符，这是一个由`40`个破折号组成的行。

这个函数非常简单。我只是将`msg`中的任何内容重定向到对`print`的调用，如果`print_separator`为`True`，我会打印一条分隔线。运行代码将显示以下内容：

```py
$ python custom.py
Data is ...
----------------------------------------
Different Strings Are not a problem
----------------------------------------
After while loop
```

正如你所看到的，最后一行后面没有分隔符。

这只是一种简单的方法，以某种方式增强对`print`函数的简单调用。让我们看看如何利用 Python 的一个棘手特性来计算调用之间的时间差：

```py
# custom_timestamp.py
from time import sleep

def debug(*msg, timestamp=[None]):
    print(*msg)
    from time import time  # local import
    if timestamp[0] is None:
        timestamp[0] = time()  #1
    else:
        now = time()
        print(
            ' Time elapsed: {:.3f}s'.format(now - timestamp[0])
        )
        timestamp[0] = now  #2

debug('Entering nasty piece of code...')
sleep(.3)
debug('First step done.')
sleep(.5)
debug('Second step done.')
```

这有点棘手，但仍然相当简单。首先，注意我们从`debug`函数内部的`time`模块中导入`time`函数。这使我们避免了在函数外部添加该导入，也许会忘记在那里添加。

看一下我是如何定义`timestamp`的。当然，它是一个列表，但这里重要的是它是一个**可变**对象。这意味着当 Python 解析函数时，它将被设置，并且在不同的调用中保留其值。因此，如果我们在每次调用后都放一个时间戳，我们就可以跟踪时间，而不必使用外部全局变量。我从我的**闭包**研究中借鉴了这个技巧，我鼓励你去了解一下，因为它非常有趣。

好了，所以，在打印出我们必须打印的任何消息和一些导入时间之后，我们检查`timestamp`中的唯一项的内容。如果它是`None`，我们没有先前的引用，因此我们将值设置为当前时间（`#1`）。

另一方面，如果我们有一个先前的引用，我们可以计算一个差值（我们很好地格式化为三个小数位），然后我们最终再次将当前时间放入`timestamp`（`#2`）。这是一个很好的技巧，不是吗？

运行这段代码会显示以下结果：

```py
$ python custom_timestamp.py
Entering nasty piece of code...
First step done.
 Time elapsed: 0.304s
Second step done.
 Time elapsed: 0.505s
```

无论你的情况如何，拥有一个像这样的自包含函数可能非常有用。

# 检查回溯

我们在第八章中简要讨论了回溯，*测试、分析和处理异常*，当我们看到了几种不同类型的异常。回溯提供了关于应用程序出了什么问题的信息。阅读它是有帮助的，所以让我们看一个小例子：

```py
# traceback_simple.py
d = {'some': 'key'}
key = 'some-other'
print(d[key])
```

我们有一个字典，我们尝试访问其中不存在的键。你应该记住这将引发一个`KeyError`异常。让我们运行代码：

```py
$ python traceback_simple.py
Traceback (most recent call last):
 File "traceback_simple.py", line 3, in <module>
 print(d[key])
KeyError: 'some-other'
```

您可以看到我们获得了所有需要的信息：模块名称，导致错误的行（数字和指令），以及错误本身。有了这些信息，您可以返回到源代码并尝试理解发生了什么。

现在让我们创建一个更有趣的例子，基于此构建，并练习 Python 3 中才有的一个特性。假设我们正在验证一个字典，处理必填字段，因此我们希望它们存在。如果没有，我们需要引发一个自定义的`ValidationError`，我们将在运行验证器的过程中进一步捕获它（这里没有显示，所以它可能是任何东西）。应该是这样的：

```py
# traceback_validator.py
class ValidatorError(Exception):
    """Raised when accessing a dict results in KeyError. """

d = {'some': 'key'}
mandatory_key = 'some-other'
try:
    print(d[mandatory_key])
except KeyError as err:
    raise ValidatorError(
        f'`{mandatory_key}` not found in d.'
    ) from err
```

我们定义了一个自定义异常，当必需的键不存在时会引发该异常。请注意，它的主体由其文档字符串组成，因此我们不需要添加任何其他语句。

非常简单，我们定义了一个虚拟字典，并尝试使用`mandatory_key`访问它。当发生`KeyError`时，我们捕获并引发`ValidatorError`。我们通过使用 Python 3 中由 PEP 3134（[`www.python.org/dev/peps/pep-3134/`](https://www.python.org/dev/peps/pep-3134/)）引入的`raise ... from ...`语法来实现这一点，以链接异常。这样做的目的是，我们可能还想在其他情况下引发`ValidatorError`，不一定是由于缺少必需的键而引起的。这种技术允许我们在一个简单的`try`/`except`中运行验证，只关心`ValidatorError`。

如果不能链接异常，我们将丢失关于`KeyError`的信息。代码产生了这个结果：

```py
$ python traceback_validator.py
Traceback (most recent call last):
 File "traceback_validator.py", line 7, in <module>
 print(d[mandatory_key])
KeyError: 'some-other'

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
 File "traceback_validator.py", line 10, in <module>
 '`{}` not found in d.'.format(mandatory_key)) from err
__main__.ValidatorError: `some-other` not found in d.
```

这很棒，因为我们可以看到导致我们引发`ValidationError`的异常的回溯，以及`ValidationError`本身的回溯。

我和我的一位审阅者就`pip`安装程序产生的回溯进行了很好的讨论。他在设置一切以便审查第十三章 *数据科学*的代码时遇到了麻烦。他的新的 Ubuntu 安装缺少一些`pip`软件包所需的库，以便正确运行。

他被阻止的原因是，他试图修复回溯中显示的错误，从顶部开始。我建议他从底部开始，然后修复。原因是，如果安装程序已经到达最后一行，我猜在那之前，无论发生了什么错误，仍然有可能从中恢复。只有在最后一行之后，`pip`决定无法继续下去，因此我开始修复那个错误。一旦安装了修复该错误所需的库，其他一切都顺利进行。

阅读回溯可能会很棘手，我的朋友缺乏解决这个问题所需的经验。因此，如果您也遇到了同样的情况。不要灰心，试着摇动一下，不要想当然。

Python 有一个庞大而美妙的社区，很少有可能当您遇到问题时，您是第一个遇到它的人，所以打开浏览器并搜索。通过这样做，您的搜索技能也会得到提高，因为您将不得不将错误减少到最小但必要的详细信息集，以使您的搜索有效。

如果您想更好地玩耍和理解回溯，标准库中有一个模块可以使用，惊喜惊喜，名为`traceback`。它提供了一个标准接口，用于提取、格式化和打印 Python 程序的堆栈跟踪，模仿 Python 解释器在打印堆栈跟踪时的行为。

# 使用 Python 调试器

调试 Python 的另一个非常有效的方法是使用 Python 调试器：`pdb`。不过，您应该绝对检查`pdbpp`库，而不是直接使用它。`pdbpp`通过提供一些方便的工具来增强标准的`pdb`接口，其中我最喜欢的是**粘性模式**，它允许您在逐步执行其指令时查看整个函数。

有几种不同的使用调试器的方法（无论哪个版本，都不重要），但最常见的一种方法是简单地设置一个断点并运行代码。当 Python 达到断点时，执行将被暂停，并且您可以访问该点的控制台，以便您可以检查所有名称等。您还可以即时更改数据以改变程序的流程。

作为一个玩具示例，假设我们有一个解析器，因为字典中缺少一个键而引发`KeyError`。字典来自我们无法控制的 JSON 有效负载，我们只是想暂时欺骗并通过控制，因为我们对之后发生的事情感兴趣。让我们看看我们如何能拦截这一刻，检查数据，修复它，并深入了解，使用`pdbpp`：

```py
# pdebugger.py
# d comes from a JSON payload we don't control
d = {'first': 'v1', 'second': 'v2', 'fourth': 'v4'}
# keys also comes from a JSON payload we don't control
keys = ('first', 'second', 'third', 'fourth')

def do_something_with_value(value):
    print(value)

for key in keys:
    do_something_with_value(d[key])

print('Validation done.')
```

正如您所看到的，当`key`获得`'third'`值时，代码将中断，这个值在字典中缺失。请记住，我们假装`d`和`keys`都是动态来自我们无法控制的 JSON 有效负载，因此我们需要检查它们以修复`d`并通过`for`循环。如果我们按原样运行代码，我们会得到以下结果：

```py
$ python pdebugger.py
v1
v2
Traceback (most recent call last):
 File "pdebugger.py", line 10, in <module>
 do_something_with_value(d[key])
KeyError: 'third'
```

所以我们看到字典中缺少`key`，但由于每次运行此代码时我们可能会得到不同的字典或`keys`元组，这些信息并不能真正帮助我们。让我们在`for`循环之前注入一个`pdb`调用。您有两个选择：

```py
import pdb
pdb.set_trace()
```

这是最常见的方法。您导入`pdb`并调用其`set_trace`方法。许多开发人员在其编辑器中有宏，可以通过键盘快捷键添加此行。不过，从 Python 3.7 开始，我们甚至可以进一步简化事情，变成这样：

```py
breakpoint()
```

新的`breakpoint`内置函数在底层调用`sys.breakpointhook()`，默认情况下编程为调用`pdb.set_trace()`。但是，您可以重新编程`sys.breakpointhook()`来调用任何您想要的东西，因此`breakpoint`也将指向那个东西，这非常方便。

此示例的代码位于`pdebugger_pdb.py`模块中。如果我们现在运行此代码，事情变得有趣起来（请注意，您的输出可能会有所不同，本输出中的所有注释都是我添加的）：

```py
$ python pdebugger_pdb.py
(Pdb++) l
 16
 17 -> for key in keys:  # breakpoint comes in
 18 do_something_with_value(d[key])
 19

(Pdb++) keys  # inspecting the keys tuple
('first', 'second', 'third', 'fourth')
(Pdb++) d.keys()  # inspecting keys of `d`
dict_keys(['first', 'second', 'fourth'])
(Pdb++) d['third'] = 'placeholder'  # add tmp placeholder
(Pdb++) c  # continue
v1
v2
placeholder
v4
Validation done.
```

首先，请注意，当您达到断点时，会收到一个控制台，告诉您您所在的位置（Python 模块）以及下一行要执行的行。在这一点上，您可以执行一系列的探索性操作，比如检查下一行之前和之后的代码，打印堆栈跟踪，并与对象交互。请参考官方 Python 文档（[`docs.python.org/3.7/library/pdb.html`](https://docs.python.org/3.7/library/pdb.html)）上的`pdb`，了解更多信息。在我们的例子中，我们首先检查`keys`元组。之后，我们检查`d`的键。我们发现`'third'`缺失了，所以我们自己放进去（这可能危险—想一想）。最后，现在所有的键都在了，我们输入`c`，表示（*c*）继续。

`pdb`还可以让您逐行执行代码，使用（*n*）下一步，深入分析函数，或使用（*b*）断点处理。有关命令的完整列表，请参考文档或在控制台中输入（*h*）帮助。

您可以看到，从前面的运行输出中，我们最终可以到达验证的结尾。

`pdb`（或`pdbpp`）是我每天都使用的宝贵工具。所以，去玩耍吧，设置一个断点，尝试检查它，按照官方文档尝试在您的代码中使用命令，看看它们的效果并好好学习。

请注意，在此示例中，我假设您已安装了`pdbpp`。如果不是这样，那么您可能会发现一些命令在`pdb`中不起作用。一个例子是字母`d`，在`pdb`中会被解释为*down*命令。为了解决这个问题，您需要在`d`前面加上`!`，告诉`pdb`它应该被字面解释，而不是作为命令。

# 检查日志文件

调试一个行为异常的应用程序的另一种方法是检查其日志文件。**日志文件**是特殊的文件，应用程序会在其中记录各种事情，通常与其内部发生的事情有关。如果重要的过程开始了，我通常期望在日志中有相应的记录。当它结束时也是一样，可能还有它内部发生的事情。

错误需要被记录下来，这样当出现问题时，我们可以通过查看日志文件中的信息来检查出错的原因。

在 Python 中有许多不同的设置记录器的方法。日志记录非常灵活，可以进行配置。简而言之，通常有四个角色：记录器、处理程序、过滤器和格式化程序：

+   **记录器**：公开应用程序代码直接使用的接口

+   **处理程序**：将日志记录（由记录器创建）发送到适当的目的地

+   **过滤器**：提供了一个更精细的设施，用于确定要输出哪些日志记录

+   **格式化程序**：指定最终输出中日志记录的布局

记录是通过调用`Logger`类的实例的方法来执行的。您记录的每一行都有一个级别。通常使用的级别有：`DEBUG`、`INFO`、`WARNING`、`ERROR`和`CRITICAL`。您可以从`logging`模块中导入它们。它们按严重程度排序，正确使用它们非常重要，因为它们将帮助您根据您要搜索的内容过滤日志文件的内容。日志文件通常变得非常庞大，因此将其中的信息正确地写入非常重要，这样在需要时您可以快速找到它。

您可以记录到文件，也可以记录到网络位置，队列，控制台等。一般来说，如果您的架构部署在一台机器上，记录到文件是可以接受的，但当您的架构跨越多台机器（比如面向服务或微服务架构的情况下），实现一个集中的日志记录解决方案非常有用，这样每个服务产生的所有日志消息都可以存储和调查在一个地方。否则，尝试从几个不同来源的巨大文件中找出问题发生了什么可能会变得非常具有挑战性。

**面向服务的架构**（SOA）是软件设计中的一种架构模式，其中应用程序组件通过通信协议向其他组件提供服务，通常通过网络。这个系统的美妙之处在于，当编写正确时，每个服务都可以用最合适的语言来实现其目的。唯一重要的是与其他服务的通信，这需要通过一个共同的格式进行，以便进行数据交换。

**微服务架构**是 SOA 的演变，但遵循一组不同的架构模式。

在这里，我将向您介绍一个非常简单的日志记录示例。我们将向文件记录一些消息：

```py
# log.py
import logging

logging.basicConfig(
    filename='ch11.log',
    level=logging.DEBUG,  # minimum level capture in the file
    format='[%(asctime)s] %(levelname)s: %(message)s',
    datefmt='%m/%d/%Y %I:%M:%S %p')

mylist = [1, 2, 3]
logging.info('Starting to process `mylist`...')

for position in range(4):
    try:
        logging.debug(
            'Value at position %s is %s', position, mylist[position]
        )
    except IndexError:
        logging.exception('Faulty position: %s', position)

logging.info('Done parsing `mylist`.')
```

让我们逐行进行。首先，我们导入`logging`模块，然后设置基本配置。一般来说，生产日志配置比这复杂得多，但我想尽可能简单。我们指定一个文件名，我们想要在文件中捕获的最低日志级别，以及消息格式。我们将记录日期和时间信息、级别和消息。

我将从记录一个告诉我我们即将处理列表的`info`消息开始。然后，我将记录（这次使用`DEBUG`级别，使用`debug`函数）某个位置的值。我在这里使用`debug`，因为我希望能够在将来过滤这些日志（通过将最低级别设置为`logging.INFO`或更高），因为我可能必须处理非常大的列表，而我不想记录所有的值。

如果我们得到`IndexError`（我们确实得到了，因为我正在循环遍历`range(4)`），我们调用`logging.exception()`，它与`logging.error()`相同，但还会打印出回溯。

在代码的结尾，我记录了另一个`info`消息，说我们已经完成了。结果是这样的：

```py
# ch11.log
[05/06/2018 11:13:48 AM] INFO:Starting to process `mylist`...
[05/06/2018 11:13:48 AM] DEBUG:Value at position 0 is 1
[05/06/2018 11:13:48 AM] DEBUG:Value at position 1 is 2
[05/06/2018 11:13:48 AM] DEBUG:Value at position 2 is 3
[05/06/2018 11:13:48 AM] ERROR:Faulty position: 3
Traceback (most recent call last):
  File "log.py", line 15, in <module>
    position, mylist[position]))
IndexError: list index out of range
[05/06/2018 11:13:48 AM] INFO:Done parsing `mylist`.
```

这正是我们需要的，可以调试在服务器上运行而不是在我们的控制台上运行的应用程序。我们可以看到发生了什么，引发的任何异常的回溯等等。

这里介绍的示例只是日志记录的皮毛。要获得更深入的解释，您可以在官方 Python 文档的*Python HOWTOs*部分找到信息：*日志记录 HOWTO*和*日志记录 Cookbook*。

日志记录是一门艺术。您需要在记录所有内容和不记录任何内容之间找到一个良好的平衡。理想情况下，您应该记录任何需要确保应用程序正常工作的内容，以及可能的所有错误或异常。

# 其他技术

在这最后一节中，我想简要演示一些您可能会发现有用的技术。

# 分析

我们在第八章中讨论了分析，*测试、分析和处理异常*，我在这里提到它只是因为分析有时可以解释由于组件过慢而导致的奇怪错误。特别是涉及网络时，了解应用程序需要经历的时间和延迟非常重要，以便在出现问题时了解可能发生了什么，因此我建议您熟悉分析技术，也从故障排除的角度来看。

# 断言

断言是确保代码验证您的假设的一种好方法。如果是，一切都会正常进行，但如果不是，您会得到一个很好的异常，可以处理。有时，与其检查，不如在代码中放置一些断言来排除可能性更快。让我们看一个例子：

```py
# assertions.py
mylist = [1, 2, 3]  # this ideally comes from some place
assert 4 == len(mylist)  # this will break
for position in range(4):
    print(mylist[position])
```

这段代码模拟了一个情况，即`mylist`并非由我们定义，但我们假设它有四个元素。因此我们在那里放置了一个断言，结果是这样的：

```py
$ python assertions.py
Traceback (most recent call last):
 File "assertions.py", line 3, in <module>
 assert 4 == len(mylist)  # this will break
AssertionError
```

这告诉我们问题出在哪里。

# 查找信息的位置

在 Python 官方文档中，有一个专门介绍调试和分析的部分，您可以在那里了解`bdb`调试器框架，以及诸如`faulthandler`、`timeit`、`trace`、`tracemallock`和当然`pdb`等模块。只需转到文档中的标准库部分，您就可以非常容易地找到所有这些信息。

# 故障排除指南

在这个简短的部分中，我想给您一些建议，这些建议来自我的故障排除经验。

# 使用控制台编辑器

首先，要熟练使用**Vim**或**nano**作为编辑器，并学习控制台的基础知识。当事情出错时，您就没有您的编辑器带来的所有便利了。您必须连接到服务器并从那里工作。因此，熟练使用控制台命令浏览生产环境，并能够使用基于控制台的编辑器编辑文件，比如 vi、Vim 或 nano，是一个非常好的主意。不要让您通常的开发环境宠坏了您。

# 检查的位置

我的第二个建议涉及在哪里放置调试断点。无论您使用`print`、自定义函数还是`pdb`，您仍然必须选择在哪里放置提供信息的调用，对吧？

有些地方比其他地方更好，有些处理调试进展的方法比其他方法更好。

我通常不会在`if`子句中设置断点，因为如果该子句没有执行，我就失去了获取所需信息的机会。有时很难或很快到达断点，所以在设置断点之前请仔细考虑。

另一件重要的事情是从哪里开始。想象一下，您有 100 行代码来处理您的数据。数据从第 1 行进入，但在第 100 行出现错误。您不知道错误在哪里，那么该怎么办呢？您可以在第 1 行设置断点，耐心地检查所有行，检查您的数据。在最坏的情况下，99 行（和许多杯咖啡）后，您找到了错误。因此，请考虑使用不同的方法。

您从第 50 行开始，然后进行检查。如果数据正常，这意味着错误发生在后面，这种情况下，您将在第 75 行设置下一个断点。如果第 50 行的数据已经出错，您将在第 25 行设置断点。然后，您重复这个过程。每次，您要么向后移动，要么向前移动，跳过上次的一半。

在最坏的情况下，您的调试将从 1、2、3、...、99 以线性方式进行，变成一系列跳跃，如 50、75、87、93、96、...、99，速度要快得多。事实上，这是对数的。这种搜索技术称为**二分搜索**，它基于分而治之的方法，非常有效，因此请尽量掌握它。

# 使用测试进行调试

您还记得第八章吗，*测试、性能分析和处理异常*，关于测试？如果我们有一个错误，而所有测试都通过了，这意味着我们的测试代码库中有问题或遗漏。因此，一种方法是修改测试，以便它们适应已经发现的新边缘情况，然后逐步检查代码。这种方法非常有益，因为它确保在修复错误时，您的错误将被测试覆盖。

# 监控

监控也非常重要。软件应用程序可能会在遇到边缘情况时变得完全疯狂，并且在网络中断、队列已满或外部组件无响应等情况下出现非确定性的故障。在这些情况下，重要的是要了解问题发生时的整体情况，并能够以微妙、甚至神秘的方式将其与相关的内容联系起来。

您可以监视 API 端点、进程、网页可用性和加载时间，基本上几乎可以监视您可以编码的所有内容。一般来说，从头开始设计应用程序时，考虑如何监视它可能非常有用。

# 总结

在这个简短的章节中，我们探讨了不同的调试和故障排除技术和建议。调试是软件开发人员工作中始终存在的活动，因此擅长调试非常重要。

如果以正确的态度对待，调试可以是有趣和有益的。

我们探讨了检查我们的代码库的技术，包括函数、日志记录、调试器、回溯信息、性能分析和断言。我们看到了它们大部分的简单示例，我们还谈到了一套指导方针，将在面对困难时提供帮助。

只要记住始终保持*冷静和专注*，调试就会变得更容易。这也是一种需要学习的技能，也是最重要的。激动和紧张的心态无法正常、逻辑和创造性地工作，因此，如果您不加强它，很难将所有知识充分利用。

在下一章中，我们将探讨 GUI 和脚本，从更常见的 Web 应用程序场景中进行有趣的偏离。