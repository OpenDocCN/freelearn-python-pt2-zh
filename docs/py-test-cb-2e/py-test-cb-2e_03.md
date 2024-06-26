# 第三章：使用 doctest 创建可测试的文档

在本章中，我们将介绍以下配方：

+   记录基础知识

+   捕获堆栈跟踪

+   从命令行运行 doctest

+   为 doctest 编写测试工具

+   过滤测试噪音

+   打印出所有文档，包括状态报告。

+   测试边缘情况

+   通过迭代测试边缘情况

+   使用 doctest 进行调试

+   更新项目级脚本以运行本章的 doctest

# 介绍

Python 提供了一种在函数内部嵌入注释的有用能力，可以从 Python shell 中访问。这些被称为**文档字符串**。

文档字符串不仅提供了嵌入信息的能力，还提供了可运行的代码示例。

有一句古谚说“*注释不是代码*”。这是因为注释不经过语法检查，通常不会被维护。因此，它们携带的信息随着时间的推移可能会失去其价值。`doctest`通过将注释转换为代码来解决这个问题，这可以有很多有用的用途。

在本章中，我们将探讨使用`doctest`开发测试、文档和项目支持的不同方法。不需要特殊设置，因为`doctest`是 Python 标准库的一部分。

# 记录基础知识

Python 提供了一种在代码中放置注释的开箱即用的能力，称为文档字符串。查看源代码和从 Python shell 交互检查代码时，可以阅读文档字符串。在本配方中，我们将演示如何使用这些交互式文档字符串作为可运行的测试。

这提供了什么？它为用户提供了易于阅读的代码示例。这些代码示例不仅易于阅读，而且可以运行，这意味着我们可以确保文档保持最新。

# 如何做...

通过以下步骤，我们将创建一个应用程序，其中包含可运行的文档字符串注释，并看看如何执行这些测试：

1.  创建一个名为`recipe16.py`的新文件，以放置我们为此配方编写的所有代码。

1.  创建一个函数，使用递归将十进制数转换为任何其他进制：

```py
def convert_to_basen(value, base):
    import math
    def _convert(remaining_value, base, exp):
        def stringify(value):
            if value > 9:
                return chr(value + ord('a')-10)
            else:
                return str(value)
        if remaining_value >= 0 and exp >= 0:
            factor = int(math.pow(base, exp))
            if factor <= remaining_value:
                multiple = remaining_value / factor
                return stringify(multiple) + \
                  _convert(remaining_value-multiple*factor, \
                    base, exp-1)
        else:
            return "0" + \
                _convert(remaining_value, base, exp-1)
        else:
            return ""
    return "%s/%s" % (_convert(value, base, \
                int(math.log(value, base))), base)
```

1.  在外部函数的下面添加一个文档字符串，如下面代码的突出部分所示。这个文档字符串声明包括使用该函数的几个示例：

```py
def convert_to_basen(value, base):
    """Convert a base10 number to basen
 >>> convert_to_basen(1, 2)
 '1/2'
 >>> convert_to_basen(2, 2)
 '10/2'
 >>> convert_to_basen(3, 2)
 '11/2'
 >>> convert_to_basen(4, 2)
 '100/2'
 >>> convert_to_basen(5, 2)
 '101/2'
 >>> convert_to_basen(6, 2)
 '110/2'
 >>> convert_to_basen(7, 2)
 '111/2'
 >>> convert_to_basen(1, 16)
 '1/16'
 >>> convert_to_basen(10, 16)
 'a/16'
 >>> convert_to_basen(15, 16)
 'f/16'
 >>> convert_to_basen(16, 16)
 '10/16'
 >>> convert_to_basen(31, 16)
 '1f/16'
 >>> convert_to_basen(32, 16)
 '20/16'
 """
    import math
```

1.  添加一个测试运行器块，调用 Python 的`doctest`模块：

```py
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

1.  从交互式 Python shell 导入配方并查看其文档。看看这个截图：

![](img/00036.jpeg)

1.  从命令行运行代码。在下面的截图中，请注意没有任何内容被打印出来。这就是当所有测试都通过时会发生的情况。看看这个截图：

![](img/00037.jpeg)

1.  从命令行运行代码，使用`-v`增加详细程度。在下面的截图中，我们看到了一部分输出，显示了运行的内容和预期的内容。在调试`doctest`时，这可能很有用：

![](img/00038.jpeg)

# 它是如何工作的...

`doctest`模块查找文档字符串中的 Python 代码块，并像真正的代码一样运行它。`>>>`是我们在使用交互式 Python shell 时看到的相同提示。`>>>`后面的行显示了预期的输出。`doctest`运行它看到的语句，然后将实际输出与预期输出进行比较。

在本章的后面，我们将看到如何捕获堆栈跟踪、错误，并添加额外的代码，相当于测试装置。

# 还有更多...

`doctest`在匹配预期输出和实际结果时非常挑剔：

+   多余的空格或制表符可能会导致出现问题。

+   诸如字典之类的结构很难测试，因为 Python 不能保证项目的顺序。在每次测试运行时，项目可能以不同的顺序存储。简单地打印出一个字典肯定会出错。

+   强烈建议不要在预期输出中包含对象引用。这些值每次运行测试时也会变化。

# 捕获堆栈跟踪

一个常见的谬论是我们只应该为成功的代码路径编写测试。我们还需要针对包括生成堆栈跟踪的错误条件编写代码。通过这个示例，我们将探讨如何在文档测试中模式匹配堆栈跟踪，从而允许我们确认预期的错误。

# 如何做...

通过以下步骤，我们将看到如何使用`doctest`来验证错误条件：

1.  为此示例中的所有代码创建一个名为`recipe17.py`的新文件。

1.  创建一个函数，使用递归将十进制数转换为任何其他进制：

```py
def convert_to_basen(value, base):
    import math
    def _convert(remaining_value, base, exp):
        def stringify(value):
            if value > 9:
                return chr(value + ord('a')-10)
            else:
                return str(value)
        if remaining_value >= 0 and exp >= 0:
            factor = int(math.pow(base, exp))
            if factor <= remaining_value:
                multiple = remaining_value / factor
                return stringify(multiple) + \
                    _convert(remaining_value-multiple*factor, \
                                base, exp-1)
            else:
                return "0" + \
                    _convert(remaining_value, base, exp-1)
        else:
            return ""
    return "%s/%s" % (_convert(value, base, \
                int(math.log(value, base))), base)
```

1.  在外部函数声明的下方添加一个文档字符串，其中包含两个预期生成堆栈跟踪的示例：

```py
def convert_to_basen(value, base):
    """Convert a base10 number to basen.

 >>> convert_to_basen(0, 2)
 Traceback (most recent call last):
 ...
 ValueError: math domain error

 >>> convert_to_basen(-1, 2)
 Traceback (most recent call last):
 ...
 ValueError: math domain error
 """
    import math
```

1.  添加一个测试运行器块，调用 Python 的`doctest`模块：

```py
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

1.  从命令行运行代码。在下面的截图中，请注意没有打印任何内容。这是当所有测试都通过时发生的情况：

![](img/00039.jpeg)

1.  使用`-v`从命令行运行代码以增加详细信息。在下面的截图中，我们可以看到`0`和`-1`生成了数学域错误。这是由于使用`math.log`来找到起始指数：

![](img/00040.jpeg)

# 它是如何工作的...

`doctest`模块查找文档字符串中的 Python 代码块，并像真正的代码一样运行它。`>>>`是我们在交互式 Python shell 中使用时看到的相同提示。`>>>`后面的行显示了预期的输出。`doctest`运行它看到的语句，然后将实际输出与预期输出进行比较。

关于堆栈跟踪，堆栈跟踪提供了大量详细信息。模式匹配整个跟踪是无效的。通过使用省略号，我们能够跳过堆栈跟踪的中间部分，只匹配区分部分：`ValueError: math domain error`。

这是有价值的，因为我们的用户不仅会看到它如何处理良好的值，还会观察到在提供坏值时可以期望什么错误。

# 从命令行运行`doctest`

我们已经看到了如何通过在文档字符串中嵌入可运行的代码片段来开发测试。但是对于这些测试中的每一个，我们都必须使模块可运行。如果我们想要从命令行运行除了我们的`doctest`之外的其他内容怎么办？我们将不得不摆脱`doctest.testmod()`语句！

好消息是，从 Python 2.6 开始，有一个命令行选项可以在不编写运行器的情况下运行特定模块的`doctest`。

`python -m doctest -v example.py`命令将导入`example.py`并通过`doctest.testmod()`运行它。根据文档，如果模块是包的一部分并导入其他子模块，则可能会失败。

# 如何做...

在以下步骤中，我们将创建一个简单的应用程序。我们将添加一些 doctests，然后从命令行运行它们，而无需编写特殊的测试运行器：

1.  创建一个名为`recipe18.py`的新文件，用于存储为此示例编写的代码。

1.  创建一个函数，使用递归将十进制数转换为任何其他进制：

```py
def convert_to_basen(value, base):
    import math
    def _convert(remaining_value, base, exp):
        def stringify(value):
            if value > 9:
                return chr(value + ord('a')-10)
            else:
                return str(value)
        if remaining_value >= 0 and exp >= 0:
            factor = int(math.pow(base, exp))
            if factor <= remaining_value:
                multiple = remaining_value / factor
                return stringify(multiple) + \
                  _convert(remaining_value-multiple*factor, \
                                base, exp-1)
            else:
                return "0" + \
                       _convert(remaining_value, base, exp-1)
        else:
            return ""
    return "%s/%s" % (_convert(value, base, \
                         int(math.log(value, base))), base)
```

1.  在外部函数声明的下方添加一个文档字符串，其中包含一些测试：

```py
def convert_to_basen(value, base):
    """Convert a base10 number to basen.
 >>> convert_to_basen(10, 2)
 '1010/2'
 >>> convert_to_basen(15, 16)
 'f/16'
 >>> convert_to_basen(0, 2)
 Traceback (most recent call last):
 ...
 ValueError: math domain error
 >>> convert_to_basen(-1, 2)
 Traceback (most recent call last):
 ...
 ValueError: math domain error
 """
    import math
```

1.  使用`-m doctest`从命令行运行代码。如下面的截图所示，没有输出表示所有测试都已通过：

![](img/00041.jpeg)

1.  使用`-v`从命令行运行代码以增加详细信息。如果我们忘记包含`-m doctest`会发生什么？使用`-v`选项可以帮助我们避免这种情况，因为它给我们一种温暖的感觉，让我们知道我们的测试正在工作。看一下这个截图：

![](img/00042.jpeg)

# 它是如何工作的...

在上一章中，我们正在使用模块的`__main__`块来运行其他测试套件。如果我们想在这里做同样的事情怎么办？我们必须选择`__main__`是用于单元测试、doctests 还是两者兼而有之！如果我们甚至不想通过`__main__`运行测试，而是运行我们的应用程序怎么办？

这就是为什么 Python 添加了使用 `-m doctest` 从命令行直接调用测试的选项。

你难道不想*知道*你的测试是否正在运行或工作吗？测试套件是否真的在做它承诺的事情？使用其他工具，通常我们必须嵌入打印语句，或者故意失败，只是为了知道事情被正确地捕获了。看起来`doctest`中的`-v`选项提供了一个方便的快速浏览正在发生的事情的方式，不是吗？

# 为`doctest`编写一个测试工具

到目前为止，我们编写的测试非常简单，因为我们正在测试的函数很简单。有两个输入和一个输出，没有副作用。不需要创建对象。这对我们来说并不是最常见的用例。通常，我们有与其他对象交互的对象。

`doctest` 模块支持创建对象、调用方法和检查结果。通过这个示例，我们将更详细地探讨这个问题。

`doctest`的一个重要方面是它找到文档字符串的各个实例，并在本地上下文中运行它们。在一个文档字符串中声明的变量不能在另一个文档字符串中使用。

# 如何做...

1.  创建一个名为`recipe19.py`的新文件，包含这个示例的代码。

1.  编写一个简单的购物车应用程序：

```py
class ShoppingCart(object):
    def __init__(self):
        self.items = []
    def add(self, item, price):
        self.items.append(Item(item, price))
        return self
    def item(self, index):
        return self.items[index-1].item
    def price(self, index):
        return self.items[index-1].price
    def total(self, sales_tax):
        sum_price = sum([item.price for item in self.items])
        return sum_price*(1.0 + sales_tax/100.0)
    def __len__(self):
        return len(self.items)
class Item(object):
    def __init__(self, item, price):
        self.item = item
        self.price = price
```

1.  在`ShoppingCart`类声明之前，在模块顶部插入一个文档字符串：

```py
"""
This is documentation for the this entire recipe.
With it, we can demonstrate usage of the code.

>>> cart = ShoppingCart().add("tuna sandwich", 15.0)
>>> len(cart)
1
>>> cart.item(1)
'tuna sandwich'
>>> cart.price(1)
15.0
>>> print (round(cart.total(9.25), 2))
16.39
"""
class ShoppingCart(object):
...
```

1.  使用`-m doctest`和`-v`进行运行：

![](img/00043.jpeg)

1.  将我们刚刚从`recipe19.py`中编写的所有代码复制到一个名为`recipe19b.py`的新文件中。

1.  在`recipe19b.py`中，在模块顶部定义`cart`变量后添加另一个文档字符串：

```py
def item(self, index):
    """
    >>> cart.item(1)
    'tuna sandwich'
    """
    return self.items[index-1].item
```

1.  运行这个示例的变体。为什么它失败了？`cart`不是在之前的文档字符串中声明的吗？看一下这个截图：

![](img/00044.jpeg)

# 工作原理...

`doctest`模块查找每个文档字符串。对于它找到的每个文档字符串，它都会创建模块全局变量的浅拷贝，然后运行代码并检查结果。除此之外，每个创建的变量都是局部作用域的，当测试完成时会被清除。这意味着我们稍后添加的第二个文档字符串无法看到我们在第一个文档字符串中创建的`cart`。这就是为什么第二次运行失败的原因。

与一些 unittest 示例中使用的`setUp`方法相比，`doctest`没有等价的方法。如果`doctest`没有`setUp`选项，那么这个示例有什么价值呢？它突显了所有开发人员在使用之前必须了解的`doctest`的一个关键限制。

# 还有更多...

`doctest` 模块提供了一种非常方便的方式来为我们的文档添加可测试性。但这并不能替代完整的测试框架，比如 unittest。正如前面所述，没有`setUp`的等价物。在文档字符串中嵌入的 Python 代码也没有语法检查。

将 `doctest` 的正确级别与 unittest（或者我们可能选择的任何其他测试框架）混合在一起是一个判断的问题。

# 过滤测试噪音

各种选项帮助`doctest`忽略噪音，比如在测试用例中的空白。这是有用的，因为它允许我们更好地结构化预期的结果，以便用户更容易阅读。

我们还可以标记一些可以跳过的测试。这可以用在我们想要记录已知问题，但尚未修补系统的地方。

当我们试图进行全面测试但专注于系统的其他部分时，这两种情况都很容易被解释为噪音。在这个示例中，我们将深入研究如何放宽`doctest`的严格检查。我们还将看看如何忽略整个测试，无论是临时的还是永久的。

# 如何做...

通过以下步骤，我们将尝试过滤测试结果并放宽`doctest`的某些限制：

1.  为这个示例的代码创建一个名为`recipe20.py`的新文件。

1.  创建一个递归函数，将十进制数转换为其他进制：

```py
def convert_to_basen(value, base):
    import math
    def _convert(remaining_value, base, exp):
        def stringify(value):
            if value > 9:
                return chr(value + ord('a')-10)
            else:
                return str(value)

        if remaining_value >= 0 and exp >= 0:
            factor = int(math.pow(base, exp))
            if factor <= remaining_value:
                multiple = remaining_value / factor
                return stringify(multiple) + \
                  _convert(remaining_value-multiple*factor, \
                                base, exp-1)
            else:
                return "0" + \
                       _convert(remaining_value, base, exp-1)
        else:
            return ""
    return "%s/%s" % (_convert(value, base, \
                         int(math.log(value, base))), base)
```

1.  添加一个包含一系列值的测试来练习的文档字符串，以及记录一个尚未实现的未来功能：

```py
def convert_to_basen(value, base):
    """Convert a base10 number to basen.

 >>> [convert_to_basen(i, 16) for i in range(1,16)] #doctest:
+NORMALIZE_WHITESPACE
 ['1/16', '2/16', '3/16', '4/16', '5/16', '6/16', '7/16', '8/16',
 '9/16',  'a/16', 'b/16', 'c/16', 'd/16', 'e/16', 'f/16']

 FUTURE: Binary may support 2's complement in the future, but not
now.
 >>> convert_to_basen(-10, 2) #doctest: +SKIP
 '0110/2'
 """
    import math
```

1.  添加一个测试运行程序：

```py
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

1.  以详细模式运行测试用例，如此截图所示：

![](img/00045.jpeg)

1.  将`recipe20.py`中的代码复制到一个名为`recipe20b.py`的新文件中。

1.  通过更新文档字符串来编辑`recipe20b.py`，包括另一个测试，显示我们的函数不会转换`0`：

```py
def convert_to_basen(value, base):
    """Convert a base10 number to basen.
    >>> [convert_to_basen(i, 16) for i in range(1,16)] #doctest:
+NORMALIZE_WHITESPACE
    ['1/16', '2/16', '3/16', '4/16', '5/16', '6/16', '7/16', '8/16',
    '9/16',  'a/16', 'b/16', 'c/16', 'd/16', 'e/16', 'f/16']
    FUTURE: Binary may support 2's complement in the future, but not
now.
    >>> convert_to_basen(-10, 2) #doctest: +SKIP
    '0110/2'
    BUG: Discovered that this algorithm doesn't handle 0\. Need to patch
it.
 TODO: Renable this when patched.
 >>> convert_to_basen(0, 2)
 '0/2'
 """
    import math
```

1.  运行测试用例。注意这个版本的配方有什么不同之处，以及为什么它失败了？看一下这个截图：

![](img/00046.jpeg)

1.  将`recipe20b.py`中的代码复制到一个名为`recipe20c.py`的新文件中。

1.  编辑`recipe20c.py`并更新文档字符串，指示我们现在将跳过测试：

```py
def convert_to_basen(value, base): 
    """Convert a base10 number to basen. 

    >>> [convert_to_basen(i, 16) for i in range(1,16)] #doctest: +NORMALIZE_WHITESPACE 
    ['1/16', '2/16', '3/16', '4/16', '5/16', '6/16', '7/16', '8/16', 
    '9/16',  'a/16', 'b/16', 'c/16', 'd/16', 'e/16', 'f/16'] 

    FUTURE: Binary may support 2's complement in the future, but not now. 
    >>> convert_to_basen(-10, 2) #doctest: +SKIP 
    '0110/2' 

    BUG: Discovered that this algorithm doesn't handle 0\. Need to patch it. 
    TODO: Renable this when patched. 
    >>> convert_to_basen(0, 2) #doctest: +SKIP 
    '0/2' 
    """ 
    import math
```

1.  运行测试用例。看一下这个截图：

![](img/00047.jpeg)

# 它是如何工作的...

在这个配方中，我们重新审视了从十进制转换为任意进制数字的函数。第一个测试显示它在一个范围内运行。通常，Python 会将这个结果数组放在一行上。为了使其更易读，我们将输出分布在两行上。我们还在值之间放了一些任意的空格，以使列更好地对齐。

这是`doctest`绝对不会支持的事情，因为它严格的模式匹配性质。通过使用`#doctest: +NORMALIZE_WHITESPACE`，我们能够要求`doctest`放宽这个限制。仍然有约束。例如，预期数组中的第一个值不能有任何空格在它前面（*相信我，我试过了，为了最大的可读性！*）但是将数组包装到下一行不再破坏测试。

我们还有一个测试用例，实际上只是作为文档。它指示了一个未来的要求，显示了我们的函数如何处理负二进制值。通过添加`#doctest: +SKIP`，我们能够命令`doctest`跳过这个特定的实例。

最后，我们看到了一个情景，我们发现我们的代码不能处理`0`。由于算法通过取对数得到最高指数，存在一个数学问题。我们通过一个测试来捕获这个边缘情况。然后我们确认代码以经典的**测试驱动设计**（**TDD**）方式失败。最后一步将是修复代码以处理这个边缘情况。但我们决定，以一种有点牵强的方式，我们没有足够的时间在当前的迭代中修复代码。为了避免破坏我们的**持续集成**（**CI**）服务器，我们用一个`TO-DO`语句标记测试，并添加`#doctest: +SKIP`。

# 还有更多...

我们用`#doctest: +SKIP`标记的两种情况都是最终我们希望移除`SKIP`标记并让它们运行的情况。可能还有其他情况我们永远不会移除`SKIP`。代码演示可能有很大的波动，可能无法轻易测试而不使其难以阅读。例如，返回字典的函数更难测试，因为结果的顺序会变化。我们可以弯曲它以通过测试，但我们可能会失去文档的价值，以便呈现给读者。

# 打印出所有的文档，包括状态报告

由于本章涉及文档和测试，让我们构建一个脚本，它接受一组模块并打印出一个完整的报告，显示所有文档以及运行任何给定的测试。

这是一个有价值的配方，因为它向我们展示了如何使用 Python 的 API 来收集一个基于代码的可运行报告。这意味着文档是准确的，也是最新的，反映了我们代码的当前状态。

# 如何做...

在接下来的步骤中，我们将编写一个应用程序和一些`doctests`。然后我们将构建一个脚本来收集一个有用的报告：

1.  创建一个名为`recipe21_report.py`的新文件，用于包含收集报告的脚本。

1.  通过导入 Python 的`inspect`库来创建一个脚本，作为深入模块的基础：`from inspect import*`。

1.  添加一个函数，专注于打印出一个项目的`__doc__`字符串或打印出未找到文档的消息：

```py
def print_doc(name, item):
    if item.__doc__:
        print "Documentation for %s" % name
        print "-------------------------------"
        print item.doc
        print "-------------------------------"
    else:
        print "Documentation for %s - None" % name
```

1.  添加一个函数，根据给定模块打印出文档。确保这个函数查找类、方法和函数，并打印出它们的文档：

```py
def print_docstrings(m, prefix=""):
    print_doc(prefix + "module %s" % m.__name__, m)

    for (name, value) in getmembers(m, isclass):
        if name == '__class__': continue
        print_docstrings(value, prefix=name + ".")
    for (name, value) in getmembers(m, ismethod):
        print_doc("%s%s()" % (prefix, name), value)
    for (name, value) in getmembers(m, isfunction):
        print_doc("%s%s()" % (prefix, name), value)
```

1.  添加一个解析命令行字符串并迭代每个提供的模块的运行器：

```py
if __name__ == "__main__":
    import sys
    import doctest

    for arg in sys.argv[1:]:
        if arg.startswith("-"): continue
        print "==============================="
        print "== Processing module %s" % arg
        print "==============================="
        m = __import__(arg)
        print_docstrings(m)
        print "Running doctests for %s" % arg
        print "-------------------------------"
        doctest.testmod(m)
```

1.  创建一个新文件`recipe21.py`，其中包含一个我们将对之前的脚本运行的应用程序和测试。

1.  在`recipe21.py`中，创建一个购物车应用程序，并填充它的文档字符串和`doctests`。这是整个食谱的文档。有了它，我们可以演示代码的用法：

```py
>>> cart = ShoppingCart().add("tuna sandwich", 15.0)
>>> len(cart)
1
>>> cart.item(1)
'tuna sandwich'
>>> cart.price(1)
15.0
>>> print round(cart.total(9.25), 2)
16.39
"""

class ShoppingCart(object):
    """
    This object is used to store the goods.
    It conveniently calculates total cost including
    tax.
    """
    def __init__(self):
        self.items = []
    def add(self, item, price):
        "Add an item to the internal list."
        self.items.append(Item(item, price))
        return self
    def item(self, index):
        "Look up the item. The cart is a 1-based index."
        return self.items[index-1].item
    def price(self, index):
        "Look up the price. The cart is a 1-based index."
        return self.items[index-1].price
    def total(self, sales_tax):
        "Add up all costs, and then apply a sales tax."
        sum_price = sum([item.price for item in self.items])
        return sum_price*(1.0 + sales_tax/100.0)
    def __len__(self):
        "Support len(cart) operation."
        return len(self.items)

class Item(object):
    def __init__(self, item, price):
        self.item = item
        self.price = price
```

1.  使用`-v`对这个模块运行报告脚本，并查看屏幕输出：

```py
===============================
== Processing module recipe21
===============================
Documentation for module recipe21
-------------------------------
This is documentation for the this entire recipe.
With it, we can demonstrate usage of the code.
>>> cart = ShoppingCart().add("tuna sandwich", 15.0)
>>> len(cart)
1
>>> cart.item(1)
'tuna sandwich'
>>> cart.price(1)
15.0
>>> print round(cart.total(9.25), 2)
16.39
-------------------------------
Documentation for Item.module Item - None
Documentation for Item.__init__() - None
Documentation for ShoppingCart.module ShoppingCart
-------------------------------
 This object is used to store the goods.
 It conveniently calculates total cost including
 tax.
…
Running doctests for recipe21
-------------------------------
Trying:
 cart = ShoppingCart().add("tuna sandwich", 15.0)
Expecting nothing
ok
Trying:
 len(cart)
Expecting:
 1
ok
5 tests in 10 items.
5 passed and 0 failed.
Test passed.
```

# 它是如何工作的...

这个脚本很小，但它收集了很多有用的信息。

通过使用 Python 的标准`inspect`模块，我们能够从模块级别开始深入研究。查找文档字符串的反射方式是通过访问对象的`__doc__`属性。它包含在模块、类、方法和函数中。它们存在于其他地方，但我们在这个食谱中限制了我们的重点。

我们以详细模式运行它，以显示测试实际上被执行。我们手动解析了命令行选项，但`doctest`自动查找`-v`来决定是否打开详细输出。为了防止我们的模块处理器捕捉到这一点并尝试将其处理为另一个模块，我们添加了一行来跳过任何`-xyz`风格的标志：

```py
 if arg.startswith("-"): continue 
```

# 还有更多...

我们可以花更多时间来增强这个脚本。例如，我们可以使用 HTML 标记将其导出，使其可以在 Web 浏览器中查看。我们还可以找到第三方库以其他方式导出它。

我们还可以在哪里寻找文档字符串以及如何处理它们上进行改进。在我们的情况下，我们只是将它们打印到屏幕上。一个更可重用的方法是返回包含所有信息的某种结构。然后，调用者可以决定是打印到屏幕上，将其编码为 HTML，还是生成 PDF 文档。

这并不是必要的，因为这个食谱的重点是看如何将 Python 提供的这些强大的开箱即用选项混合到一个快速和有用的工具中。

# 测试边缘

测试需要在我们的代码边界上进行练习，直到超出范围限制。在这个食谱中，我们将深入定义和测试使用`doctest`的边缘。

# 如何做...

通过以下步骤，我们将看到如何编写测试软件边缘的代码：

1.  创建一个名为`recipe22.py`的新文件，并使用它来放置这个食谱的所有代码。

1.  创建一个将十进制数转换为 2 进制到 36 进制之间任何进制的函数：

```py
def convert_to_basen(value, base):
    if base < 2 or base > 36:
        raise Exception("Only support bases 2-36")

    import math
    def _convert(remaining_value, base, exp):
        def stringify(value):
            if value > 9:
                return chr(value + ord('a')-10)
            else:
                return str(value)

        if remaining_value >= 0 and exp >= 0:
            factor = int(math.pow(base, exp))
            if factor <= remaining_value:
                multiple = remaining_value / factor
                return stringify(multiple) + \
                  _convert(remaining_value-multiple*factor, \
                                base, exp-1)
            else:
                return "0" + \
                       _convert(remaining_value, base, exp-1)
        else:
            return ""

    return "%s/%s" % (_convert(value, base, \
                         int(math.log(value, base))), base)
```

1.  在我们的函数声明下面添加一个文档字符串，其中包括显示 2 进制边缘、36 进制边缘和无效的 37 进制的测试：

```py
def convert_to_basen(value, base):
    """Convert a base10 number to basen.

    These show the edges for base 2.
    >>> convert_to_basen(1, 2)
    '1/2'
    >>> convert_to_basen(2, 2)
    '10/2'
    >>> convert_to_basen(0, 2)
    Traceback (most recent call last):
       ...
    ValueError: math domain error

    These show the edges for base 36.
    >>> convert_to_basen(1, 36)
    '1/36'
    >>> convert_to_basen(35, 36)
    'z/36'
    >>> convert_to_basen(36, 36)
    '10/36'
    >>> convert_to_basen(0, 36)
    Traceback (most recent call last):
       ...
    ValueError: math domain error

    These show the edges for base 37.
    >>> convert_to_basen(1, 37)
    Traceback (most recent call last):
       ...
    Exception: Only support bases 2-36
    >>> convert_to_basen(36, 37)
    Traceback (most recent call last):
       ...
    Exception: Only support bases 2-36
    >>> convert_to_basen(37, 37)
    Traceback (most recent call last):
       ...
    Exception: Only support bases 2-36
    >>> convert_to_basen(0, 37)   
    Traceback (most recent call last):
       ...
    Exception: Only support bases 2-36
    """
    if base < 2 or base > 36:
```

1.  添加一个测试运行器：

```py
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

1.  按照这个屏幕截图展示的方式运行这个食谱：

![](img/00048.jpeg)

# 它是如何工作的...

这个版本有一个处理 2 进制到 36 进制的限制。

对于 36 进制，它使用`a`到`z`。这与使用`a`到`f`的 16 进制进行比较。在 10 进制中，`35`表示为 36 进制中的`z`。

我们包括了几个测试，包括 2 进制和 36 进制的`1`。我们还测试了在回卷之前的最大值和下一个值，以显示回卷。对于 2 进制，这是`1`和`2`。对于 36 进制，这是`35`和`36`。

正如我们还包括了测试 0 来显示我们的函数不处理任何基数，我们还测试了无效的 36 进制。

# 还有更多...

对于有效的输入，我们的软件能够正常工作是很重要的。同样重要的是，我们的软件对于无效的输入能够按预期工作。我们有文档可以在用户使用我们的软件时查看，记录了这些边缘情况。而且，由于 Python 的`doctest`模块，我们可以测试它，确保我们的软件表现正确。

# 另请参阅

在第一章中提到的*测试边缘*部分，*使用 Unittest 开发基本测试*。

# 通过迭代测试边缘情况

随着我们继续开发我们的代码，边缘情况将会出现。通过在可迭代列表中捕获边缘情况，我们需要编写的代码更少，以捕获另一个测试场景。这可以提高我们测试新场景的效率。

# 如何做...

1.  创建一个名为`recipe23.py`的新文件，并用它来存储这个配方的所有代码。

1.  创建一个将十进制转换为任何其他进制的函数：

```py
def convert_to_basen(value, base):
    import math

    def _convert(remaining_value, base, exp):
        def stringify(value):
            if value > 9:
                return chr(value + ord('a')-10)
            else:
                return str(value)

        if remaining_value >= 0 and exp >= 0:
            factor = int(math.pow(base, exp))
            if factor <= remaining_value:
                multiple = remaining_value / factor
                return stringify(multiple) + \
                  _convert(remaining_value-multiple*factor, \
                                base, exp-1)
            else:
                return "0" + \
                       _convert(remaining_value, base, exp-1)
        else:
            return ""

    return "%s/%s" % (_convert(value, base, \
                         int(math.log(value, base))), base)
```

1.  添加一些包含一系列输入值以生成一系列预期输出的`doctest`实例。包括一个失败的实例：

```py
def convert_to_basen(value, base):
    """Convert a base10 number to basen.

    Base 2
    >>> inputs = [(1,2,'1/2'), (2,2,'11/2')]
    >>> for value,base,expected in inputs:
    ...     actual = convert_to_basen(value,base)
    ...     assert actual == expected, 'expected: %s actual: %s' %
(expected, actual)

    >>> convert_to_basen(0, 2)
    Traceback (most recent call last):
       ...
    ValueError: math domain error

    Base 36.
    >>> inputs = [(1,36,'1/36'), (35,36,'z/36'), (36,36,'10/36')]
    >>> for value,base,expected in inputs:
    ...     actual = convert_to_basen(value,base)
    ...     assert actual == expected, 'expected: %s actual: %s' %
(expected, value)

    >>> convert_to_basen(0, 36)
    Traceback (most recent call last):
       ...
    ValueError: math domain error
    """
    import math
```

1.  添加一个测试运行器：

```py
if __name__ == "__main__":
    import doctest
    doctest.testmod()
```

1.  运行这个配方：

![](img/00049.jpeg)

在前面的截图中，关键信息在这一行上：`AssertionError: expected: 11/2 actual: 10/2`。这个测试失败有点牵强吗？当然是。但是看到一个显示有用输出的测试用例并不是。重要的是要验证我们的测试是否给了我们足够的信息来修复测试或代码。

# 它是如何工作的...

我们创建了一个数组，每个条目都包含输入数据和预期输出。这为我们提供了一种简单的方式来查看一组测试用例。

然后，我们遍历了每个测试用例，计算了实际值，并通过 Python 的`assert`运行了它。一个需要的重要部分是自定义消息`'expected: %s actual: %s'`。没有它，我们将永远得不到告诉我们哪个测试用例失败的信息。

如果一个测试用例失败会怎么样？

如果数组中的一个测试失败了，那么该代码块将退出并跳过其余的测试。这是为了拥有更简洁的一组测试而进行的权衡。

# 这种类型的测试更适合于 doctest 还是 unittest？

以下是一些标准，可以帮助您决定是否值得将这些测试放入`doctest`中：

+   代码一目了然吗？

+   当用户查看文档字符串时，是否有清晰、简洁、有用的信息？

如果在文档中没有这个的价值，而且它会使代码混乱，那么这是一个明显的提示，表明这个测试块属于一个单独的测试模块。

# 另请参阅

在第一章的*通过迭代测试边缘情况*部分，*使用 Unittest 开发基本测试*

# 用 doctest 变得爱管闲事

到目前为止，我们要么是用测试运行器附加模块，要么是在命令行上输入`python -m doctest <module>`来执行我们的测试。

在上一章中，我们介绍了强大的`nose`库（有关详细信息，请参阅[`somethingaboutorange.com/mrl/projects/nose`](http://somethingaboutorange.com/mrl/projects/nose)）。

简要回顾一下，nose 具有以下功能：

+   为我们提供了方便的测试发现工具`nosetests`

+   可插拔，有大量的插件可用

+   包括一个针对查找 doctests 并运行它们的内置插件

# 准备工作

我们需要激活我们的虚拟环境（`virtualenv`），然后为这个配方安装 nose：

1.  创建一个虚拟环境，激活它，并验证工具是否正常工作。看一下这个截图：

![](img/00050.jpeg)

1.  使用`pip`，按照截图中显示的方式安装`nose`：

![](img/00051.jpeg)这个配方假设您已经构建了本章中的所有先前的配方。如果您只构建了其中一些，您的结果可能会有所不同。

# 如何做...

1.  对这个文件夹中的所有模块运行`nosetests -with-doctest`。您可能会注意到它打印了一个非常简短的`.....F.F...F`，表示有三个测试失败了。

1.  运行`nosetests -with-doctest -v`以获得更详细的输出。在下面的截图中，注意到失败的测试与本章前面的示例中失败的测试是相同的。还有一个有价值的地方是看到了`<module>.<method>`格式，要么是`ok`要么是`FAIL`：

![](img/00052.jpeg)

1.  按照屏幕截图中显示的方式，对`recipe19.py`文件以及`recipe19`模块运行`nosetests -with-doctest`，以不同的组合方式进行测试：

![](img/00053.jpeg)

# 它是如何工作的...

`nosetests`旨在发现测试用例，然后运行它们。使用这个插件时，当它发现一个文档字符串时，它会使用`doctest`库来进行程序化测试。

`doctest`插件是基于这样的假设构建的，即 doctests 不在与其他测试（如 unittest）相同的包中。这意味着它只会运行从非测试包中找到的 doctests。

`nosetests`并不复杂，也不难使用，`nosetests`旨在成为一个方便使用的工具，让测试触手可及。在这个示例中，我们已经看到了如何使用`nosetests`来获取到目前为止在本章中构建的所有 doctest。

# 更新项目级别的脚本以运行本章的 doctests

这个示例将帮助我们探索构建一个项目级别的脚本，允许我们运行不同的测试套件。我们还将专注于如何在我们的`doctest`中运行它。

# 如何做...

通过以下步骤，我们将创建一个命令行脚本，以允许我们管理一个包括运行`doctest`的项目：

1.  创建一个名为`recipe25.py`的新文件，以放置本示例的所有代码。

1.  添加代码，使用 Python 的`getopt`库解析一组选项：

```py
import getopt
import glob
import logging
import nose
import os
import os.path
import re
import sys
def usage():
    print ()
    print ("Usage: python recipe25.py [command]")
    print ()
    print ("\t--help")
    print ("\t--doctest")
    print ("\t--suite [suite]")
    print ("\t--debug-level [info|debug]")
    print ("\t--package")
    print ("\t--publish")
    print ("\t--register")
    print ()

try:
    optlist, args = getopt.getopt(sys.argv[1:],
            "h",
           ["help", "doctest", "suite=", \
            "debug-level=", "package", \
            "publish", "register"])
except getopt.GetoptError:
    # print help information and exit:
    print "Invalid command found in %s" % sys.argv
    usage()
    sys.exit(2)
```

1.  创建一个映射到`-test`的函数：

```py
def test(test_suite, debug_level):
    logger = logging.getLogger("recipe25")
    loggingLevel = debug_level
    logger.setLevel(loggingLevel)
    ch = logging.StreamHandler()
    ch.setLevel(loggingLevel)
    formatter = logging.Formatter("%(asctime)s - %(name)s - 
(levelname)s - %(message)s")
    ch.setFormatter(formatter)
    logger.addHandler(ch)

    nose.run(argv=["", test_suite, "--verbosity=2"])
```

1.  创建一个映射到`-doctest`的函数：

```py
def doctest(test_suite=None):
    args = ["", "--with-doctest"]
    if test_suite is not None:
        print ("Running doctest suite %s" % test_suite)
        args.extend(test_suite.split(','))
        nose.run(argv=args)
    else:
        nose.run(argv=args)
```

1.  创建支持`package`、`publish`和`register`的存根函数：

```py
def package(): 
    print ("This is where we can plug in code to run " + \
          "setup.py to generate a bundle.")

def publish():
    print ("This is where we can plug in code to upload " + \
          "our tarball to S3 or some other download site.")

def register():
    print ("setup.py has a built in function to " + \
          "'register' a release to PyPI. It's " + \
          "convenient to put a hook in here.")
    # os.system("%s setup.py register" % sys.executable)
```

1.  添加一些代码来检测选项列表是否为空。如果是，让它打印出帮助菜单并退出脚本：

```py
if len(optlist) == 0:
    usage()
    sys.exit(1)
```

1.  添加一些代码来定义调试级别，然后解析选项以允许用户进行覆盖：

```py
debug_levels = {"info":logging.INFO, "debug":logging.DEBUG}
# Default debug level is INFO
debug_level = debug_levels["info"]

for option in optlist:
    if option[0] in ("--debug-level"):
        # Override with a user-supplied debug level
        debug_level = debug_levels[option[1]]
```

1.  添加一些代码，扫描命令行选项以查找`-help`，如果找到，则退出脚本：

```py
# Check for help requests, which cause all other
# options to be ignored.
for option in optlist:
    if option[0] in ("--help", "-h"):
    usage()
    sys.exit(1)
```

1.  添加代码来检查是否选择了`--doctest`。如果是，让它专门扫描`--suite`并通过`doctest()`方法运行它。否则，通过`-suite`运行`test()`方法：

```py
ran_doctests = False
for option in optlist:
    # If --doctest is picked, then --suite is a
    # suboption.
    if option[0] in ("--doctest"):
        suite = None
        for suboption in optlist:
            if suboption[0] in ("--suite"):
                suite = suboption[1]
        print "Running doctests..."
        doctest(suite)
        ran_doctests = True

if not ran_doctests:
    for option in optlist:
        if option[0] in ("--suite"):
            print "Running test suite %s..." % option[1]
            test(option[1], debug_level)
```

1.  通过迭代每个命令行选项来完成，并根据所选的选项调用其他函数：

```py
# Parse the arguments, in order
for option in optlist:
    if option[0] in ("--package"):
        package()

    if option[0] in ("--publish"):
        publish()

    if option[0] in ("--register"):
        register()
```

1.  按照屏幕截图中显示的方式使用`--help`运行脚本：

![](img/00054.jpeg)

1.  使用`--doctest`运行脚本。注意以下屏幕截图中的前几行输出。它显示了测试的通过和失败以及详细的输出。看一下这个屏幕截图：

![](img/00055.jpeg)输出要长得多。为了简洁起见，已经对其进行了修剪。

1.  按照屏幕截图中显示的方式，使用`-doctest -suite=recipe16,recipe17.py`运行脚本：

![](img/00056.jpeg)我们故意使用`recipe16.py`和`recipe17.py`来演示它是如何与模块名和文件名一起工作的。

# 它是如何工作的...

这个脚本使用了 Python 的`getopt`库，它是模仿`getopt()`函数的（有关更多详细信息，请参阅[`docs.python.org/library/getopt.html`](http://docs.python.org/library/getopt.html)）。

我们已经连接了以下函数：

+   `Usage`：提供帮助给用户的函数。

+   `Key`：关键选项定义包括在以下块中：

```py
optlist, args = getopt.getopt(sys.argv[1:],
        "h",
       ["help", "doctest", "suite=", \
        "debug-level=", "package", \
        "publish", "register"])
```

+   +   我们解析除第一个外的所有参数，第一个是可执行文件。

+   `"h"`定义了短选项：`-h`。

+   列表定义了长选项。带有`"="`的选项接受一个参数。没有参数的是标志。

+   如果收到的选项不在列表中，就会抛出异常，我们打印出`usage()`，然后退出。

+   `doctest`：它使用`-with-doctest`通过 nose 运行模块。

+   `package`、`pubilsh`和`register`：这些与上一章中描述的函数类似。

定义了这些函数后，我们现在可以迭代解析的选项。对于这个脚本，有一个顺序：

1.  检查是否有调试覆盖。我们默认为`logging.INFO`，但我们提供切换到`logging.DEBUG`的能力。

1.  检查是否调用了`-h`或`-help`。如果是，打印出`usage()`信息，然后退出，不再解析。

1.  因为`-suite`可以单独用于运行 unittest 测试，或作为`-doctest`的子选项，我们必须解析一下，并弄清楚是否使用了`-doctest`。

1.  最后，迭代选项，并调用它们对应的函数。

为了练习，我们首先用`-help`选项调用这个脚本，打印出我们的命令选择。

然后我们用`-doctest`调用它，看它如何找到这个文件夹中的所有 doctests。在我们的例子中，我们找到了本章的所有配方，包括三个测试失败。

最后，我们用`-doctest -suite=recipe16,recipe17.py`调用脚本。这显示了我们如何选择由逗号分隔的测试子集。通过这个例子，我们看到 nose 可以通过模块名（`recipe16.py`）或文件名（`recipe17.py`）来处理。

# 还有更多...

这个脚本提供的功能可以很容易地通过已经构建的命令来处理。我们在本章前面看过`nosetests`和`doctest`，并看到它如何接受参数来灵活地选择测试。

在 Python 社区中，使用`setup.py`生成 tarballs 和注册发布也是一个常用的功能。

那么为什么要编写这个脚本呢？因为我们可以利用一个命令来利用所有这些功能。
