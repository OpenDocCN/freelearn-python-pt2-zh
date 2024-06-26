# 第一章：介绍、代码格式化和工具

在本章中，我们将探讨与干净代码相关的第一个概念，从它是什么和意味着什么开始。本章的主要观点是要理解干净代码不仅仅是软件项目中的一件好事或奢侈品。这是必需的。没有质量的代码，项目将面临由于积累的技术债务而失败的危险。

沿着同样的思路，但更详细地讨论的是格式化和文档化代码的概念。这也可能听起来像是一个多余的要求或任务，但我们将发现它在保持代码库的可维护性和可操作性方面起着基本作用。

我们将分析采用良好的编码准则对该项目的重要性。意识到保持代码与参考一致是一项持续的任务，我们将看到如何从自动化工具中获得帮助，以简化我们的工作。因此，我们迅速讨论如何配置主要工具，以便它们作为构建的一部分自动运行在项目上。

阅读本章后，您将了解干净代码是什么，为什么它很重要，为什么格式化和文档化代码是关键任务，以及如何自动化这个过程。从中，您应该获得快速组织新项目结构的思维方式，以追求良好的代码质量。

阅读本章后，您将学到以下内容：

+   干净代码在软件构建中的真正意义远远重要于格式化

+   即使如此，拥有标准的格式化是软件项目中必须具备的关键组成部分，以确保其可维护性

+   如何通过使用 Python 提供的功能使代码自我记录

+   如何配置工具以帮助以一致的方式安排代码布局，以便团队成员可以专注于问题的本质。

# 干净代码的含义

没有干净代码的唯一或严格定义。此外，可能没有正式衡量干净代码的方法，因此您无法在存储库上运行工具，告诉您代码的好坏、可维护性或不可维护性。当然，您可以运行检查器、linter、静态分析器等工具。这些工具非常有帮助。它们是必需的，但不够。干净的代码不是机器或脚本可以告诉的东西（到目前为止），而是我们作为专业人士可以决定的东西。

几十年来，我们一直在使用编程语言这个术语，我们认为它们是用来向机器传达我们的想法，以便它可以运行我们的程序。我们错了。这不是真相，而是真相的一部分。编程语言背后的真正语言是将我们的想法传达给其他开发人员。

这就是干净代码的真正本质所在。它取决于其他工程师能够阅读和维护代码。因此，我们作为专业人士是唯一能够判断这一点的人。想想看；作为开发人员，我们花在阅读代码上的时间要比实际编写代码的时间多得多。每当我们想要进行更改或添加新功能时，我们首先必须阅读我们需要修改或扩展的代码周围的所有内容。语言（Python）是我们用来相互交流的工具。

因此，与其给您一个干净代码的定义（或我的定义），我邀请您阅读全书，了解有关惯用 Python 的所有内容，看到好代码和坏代码之间的区别，识别好代码和良好架构的特征，然后提出您自己的定义。阅读本书后，您将能够自行判断和分析代码，并对干净代码有更清晰的理解。您将知道它是什么和意味着什么，而不受任何给定的定义的影响。

# 拥有干净代码的重要性

清洁代码重要的原因有很多。其中大部分都围绕着可维护性、减少技术债务、有效地与敏捷开发合作以及管理成功的项目。

我想探讨的第一个想法是关于敏捷开发和持续交付。如果我们希望我们的项目能够以稳定和可预测的速度不断成功地交付功能，那么拥有一个良好且可维护的代码库是必不可少的。

想象一下，你正在驾驶一辆汽车沿着一条通往你想在某个时间点到达的目的地的道路行驶。你必须估计到达时间，这样你就可以告诉等待你的人。如果汽车运行良好，道路平坦完美，那么我不明白为什么你会大大地错过你的估计。现在，如果道路破损，你不得不下车移动路上的石头，或者避开裂缝，每隔几公里就停下来检查引擎，等等，那么你很可能不会确定你何时到达（或者是否会到达）。我认为这个类比很清楚；道路就是代码。如果你想以稳定、持续和可预测的速度前进，代码就需要是可维护和可读的。如果不是，每当产品管理要求新功能时，你都必须停下来重构和修复技术债务。

技术债务是指由于妥协和错误决策而导致软件中的问题的概念。在某种程度上，可以从现在到过去来思考技术债务。如果我们目前面临的问题是先前编写的糟糕代码的结果，那么怎么办？从现在到未来——如果我们决定现在走捷径，而不是投入时间进行适当的解决方案，那么我们将为自己创造什么问题？

“债务”这个词是一个很好的选择。这是一种债务，因为将来更改代码将比现在更改代码更困难。这种产生的成本就是债务的利息。技术债务意味着明天，代码将比今天更难、更昂贵（甚至可能测量）；后天更昂贵，以此类推。

每当团队无法按时交付某些东西并不得不停下来修复和重构代码时，就是在支付技术债务的代价。

技术债务最糟糕的地方在于它代表了一个长期和潜在的问题。这不是引起高度警报的事情。相反，它是一个潜在的问题，分散在项目的所有部分，某一天，在某个特定的时间，它将醒来并成为一个停工的原因。

# 代码格式在清洁代码中的作用

清洁的代码是关于根据一些标准（例如 PEP-8，或项目指南定义的自定义标准）格式化和构造代码吗？简短的答案是否定的。

清洁的代码是另一回事，远远超出了编码标准、格式化、linting 工具和其他有关代码布局的检查。清洁的代码是关于实现高质量的软件，构建一个健壮、可维护的系统，避免技术债务。一段代码或整个软件组件可以符合 PEP-8（或任何其他指南），但仍然不能满足这些要求。

然而，不关注代码结构也有一些危险。因此，我们将首先分析糟糕的代码结构问题，如何解决这些问题，然后我们将看到如何配置和使用 Python 项目工具，以便自动检查和纠正问题。

总之，我们可以说，清洁的代码与 PEP-8 或编码风格之类的东西无关。它远远超出了那些，对代码的可维护性和软件质量意味着更有意义的东西。然而，正如我们将看到的，正确格式化代码对于高效工作是重要的。

# - 遵循项目的编码风格指南

编码指南是项目应该具备的最低标准，以便被认为是按照质量标准开发的项目。在本节中，我们将探讨其中的原因，以便在接下来的章节中，我们可以开始探讨通过工具自动强制执行这一点的方法。

当我试图在代码布局中找到好的特点时，我脑海中首先想到的是一致性。我希望代码能够一致地结构化，以便更容易阅读和理解。如果代码不正确或结构不一致，并且团队中的每个人都按照自己的方式做事，那么我们最终会得到需要额外努力和专注才能正确遵循的代码。它将容易出错，误导人，并且错误或微妙之处可能很容易被忽略。

我们要避免这种情况。我们想要的正是相反的情况——我们能够在一瞥之间尽快阅读和理解的代码。

如果开发团队的所有成员都同意以标准化的方式结构化代码，那么最终的代码看起来会更加熟悉。因此，你将很快识别出模式（稍后会详细介绍），并且有了这些模式，理解事物和发现错误将变得更加容易。例如，当出现问题时，你会注意到你习惯看到的模式中有些地方不对劲，这会引起你的注意。你会仔细看一看，很可能会发现错误！

正如经典著作《代码大全》中所述，对此进行了有趣的分析，即在题为《国际象棋中的感知》（1973）的论文中进行了一项实验，以确定不同人如何理解或记忆不同的国际象棋局面。实验涉及各个水平的玩家（初学者、中级和国际象棋大师），以及棋盘上的不同国际象棋局面。他们发现，当局面是随机的时，初学者和国际象棋大师的表现一样好；这只是一个任何人都可以在相当同等水平上完成的记忆练习。当局面遵循可能在真实比赛中出现的逻辑顺序时（再次强调，一致性，遵循模式），国际象棋大师的表现要远远好于其他人。

现在想象一下，将这种情况应用到软件中。作为 Python 软件工程师专家，我们就像前面例子中的国际象棋大师。当代码结构随意，没有遵循任何逻辑或标准时，我们很难像初学者开发人员那样发现错误。另一方面，如果我们习惯以结构化的方式阅读代码，并且已经学会通过遵循模式快速理解代码的思想，那么我们就处于相当大的优势。

特别是对于 Python，你应该遵循的编码风格是 PEP-8。你可以扩展它或采用它的一些部分来适应你正在工作的项目的特殊情况（例如，行的长度，关于字符串的注释等）。然而，我建议无论你是只使用 PEP-8 还是扩展它，你都应该坚持使用它，而不是试图从头开始制定另一个不同的标准。

原因是这个文档已经考虑了 Python 语法的许多特殊情况（这些特殊情况通常不适用于其他语言），而且它是由实际为 Python 语法做出贡献的核心 Python 开发人员创建的。因此，很难想象 PEP-8 的准确性可以被否定，更不用说改进了。

特别是，PEP-8 在处理代码时具有一些特点，可以带来其他良好的改进，例如：

+   **可搜索性**：这是在代码中搜索标记的能力；也就是说，在特定文件中（以及这些文件的哪个部分）搜索我们正在寻找的特定字符串。这个标准引入的其中一项内容是区分变量赋值的方式和传递给函数的关键字参数的方式。

为了更好地理解这一点，让我们举个例子。假设我们正在调试，我们需要找到将值传递给名为`location`的参数的地方。我们可以运行以下`grep`命令，结果将告诉我们我们正在寻找的文件和行：

```py
$ grep -nr "location=" . 
./core.py:13: location=current_location,
```

现在，我们想知道这个变量是在哪里被赋予这个值，下面的命令也会给我们提供我们正在寻找的信息：

```py
$ grep -nr "location =" .
./core.py:10: current_location = get_location()
```

PEP-8 规定，当通过关键字将参数传递给函数时，我们不使用空格，但在赋值变量时使用。因此，我们可以调整我们的搜索标准（在第一次搜索时不使用`=`周围的空格，在第二次搜索时使用一个空格），并且在我们的搜索中更加高效。这是遵循约定的优势之一。

+   **一致性**：如果代码看起来像一个统一的格式，那么阅读它将更容易。这对于入职非常重要，如果你想欢迎新的开发人员加入你的项目，或者雇佣新的（可能经验不足）程序员加入你的团队，并且他们需要熟悉代码（甚至可能由多个存储库组成）。如果他们打开的所有文件中的代码布局、文档、命名约定等都是相同的，那么他们的生活将变得更加轻松。

+   **代码质量**：通过以结构化的方式查看代码，你将更加熟练地一览无余地理解它（再次，就像*国际象棋中的感知*），并且更容易地发现错误和错误。除此之外，检查代码质量的工具也会提示潜在的错误。对代码进行静态分析可能有助于减少每行代码的错误比率。

# 文档字符串和注释

这一部分是关于在 Python 中从代码内部对代码进行文档化。良好的代码是自解释的，但也要有良好的文档。解释它应该做什么（而不是如何）是一个好主意。

一个重要的区别；对代码进行文档化并不等同于在代码上添加注释。注释是不好的，应该避免。通过文档化，我们指的是解释数据类型，提供它们的示例，并对变量进行注释。

这在 Python 中很重要，因为它是动态类型的，可能很容易在函数和方法之间的变量或对象的值上迷失。因此，陈述这些信息将使未来的代码读者更容易理解。

还有另一个特别与注释相关的原因。它们还可以帮助运行一些自动检查，比如通过 Mypy 这样的工具进行类型提示。最终，我们会发现，添加注释是值得的。

# 文档字符串

简单来说，我们可以说文档字符串基本上是嵌入在源代码中的文档。**文档字符串**基本上是一个字面字符串，放在代码的某个地方，目的是对该逻辑部分进行文档化。

注意对**文档**一词的强调。这种微妙之处很重要，因为它的意图是代表解释，而不是证明。文档字符串不是注释；它们是文档。

在代码中添加注释是一个不好的做法，原因有多个。首先，注释代表了我们未能用代码表达我们的想法。如果我们实际上必须解释为什么或如何做某事，那么该代码可能还不够好。首先，它没有足够的自解释性。其次，它可能会误导。在阅读复杂部分时，最糟糕的情况是阅读一个注释，说明它应该如何工作，然后发现代码实际上做了不同的事情。人们往往会忘记在更改代码时更新注释，因此刚刚更改的行旁边的注释将过时，导致危险的错误方向。

有时，在极少数情况下，我们无法避免添加注释。也许第三方库上有一个错误，我们必须规避。在这些情况下，放置一个小但描述性的注释可能是可以接受的。

然而，文档字符串的情况不同。再次强调，它们不代表注释，而是代码中特定组件（模块、类、方法或函数）的文档。它们的使用不仅被接受，而且被鼓励。尽可能添加文档字符串是一个好习惯。

它们在代码中是一个好东西的原因（或者甚至可能是必需的，这取决于项目的标准）是因为 Python 是动态类型的。这意味着，例如，函数可以将任何东西作为其参数的值。Python 不会强制执行或检查任何这样的事情。因此，想象一下，在代码中找到一个您知道必须修改的函数。您甚至很幸运，该函数有一个描述性的名称，它的参数也是如此。它可能仍然不太清楚应该传递什么类型。即使是这种情况，它们应该如何使用？

这就是一个好的文档字符串可能有所帮助的地方。记录函数的预期输入和输出是一个好习惯，将帮助该函数的读者理解它应该如何工作。

考虑一下标准库中的这个很好的例子：

```py
In [1]: dict.update??
Docstring:
D.update([E, ]**F) -> None. Update D from dict/iterable E and F.
If E is present and has a .keys() method, then does: for k in E: D[k] = E[k]
If E is present and lacks a .keys() method, then does: for k, v in E: D[k] = v
In either case, this is followed by: for k in F: D[k] = F[k]
Type: method_descriptor
```

在这里，字典上`update`方法的文档字符串为我们提供了有用的信息，并告诉我们可以以不同的方式使用它：

1.  我们可以传递一个具有`.keys()`方法的对象（例如，另一个字典），它将使用传递的对象的键更新原始字典：

```py
>>> d = {}
>>> d.update({1: "one", 2: "two"})
>>> d
{1: 'one', 2: 'two'}
```

1.  我们可以传递一个键和值的对的可迭代对象，并将它们解包到`update`中：

```py
>>> d.update([(3, "three"), (4, "four")])
>>> d
{1: 'one', 2: 'two', 3: 'three', 4: 'four'}
```

在任何情况下，字典将使用传递给它的其余关键字参数进行更新。

这对于必须学习和理解新函数如何工作以及如何利用它的人来说是至关重要的信息。

请注意，在第一个示例中，我们通过在其上使用双问号（`dict.update??`）获得了函数的文档字符串。这是 IPython 交互式解释器的一个特性。调用它时，它将打印您期望的对象的文档字符串。现在，想象一下，以同样的方式，我们从标准库的这个函数中获得帮助；如果您在编写的函数上放置文档字符串，以便其他人可以以同样的方式理解它们的工作原理，那么您可以使您的读者（代码的用户）的生活变得更加轻松多少？

文档字符串不是与代码分离或孤立的东西。它成为代码的一部分，您可以访问它。当对象有定义的文档字符串时，这通过其`__doc__`属性成为其一部分：

```py
>>> def my_function():
 ... """Run some computation"""
 ... return None
 ...
 >>> my_function.__doc__
 'Run some computation'
```

这意味着甚至可以在运行时访问它，甚至可以从源代码生成或编译文档。实际上，有工具可以做到这一点。如果运行 Sphinx，它将为项目的文档创建基本的框架。特别是使用`autodoc`扩展（`sphinx.ext.autodoc`），该工具将从代码中获取文档字符串，并将其放置在记录函数的页面中。

一旦你有了构建文档的工具，就将其公开，使其成为项目本身的一部分。对于开源项目，你可以使用 read the docs，它将根据分支或版本（可配置）自动生成文档。对于公司或项目，你可以使用相同的工具或在本地配置这些服务，但无论做出何种决定，重要的是文档应该准备好并对团队的所有成员可用。

不幸的是，文档字符串也有一个缺点，就是和所有文档一样，它需要手动和持续的维护。随着代码的变化，文档也需要更新。另一个问题是，为了使文档字符串真正有用，它们必须详细，这就需要多行。

维护适当的文档是我们无法逃避的软件工程挑战。这也是有道理的。如果你仔细想想，文档需要手动编写的原因是因为它是打算让其他人阅读的。如果它是自动化的，可能就没有太大的用处。为了使文档有价值，团队中的每个人都必须同意它是需要手动干预的东西，因此需要付出努力。关键是要理解软件不仅仅是代码。随之而来的文档也是交付内容的一部分。因此，当有人对一个函数进行更改时，同样重要的是更新文档的相应部分，无论是维基、用户手册、README 文件还是多个文档字符串。

# 注解

PEP-3107 引入了注解的概念。它们的基本想法是向代码的读者提示函数参数的预期值。使用“提示”这个词并非偶然；注解使类型提示成为可能，我们将在本章后面讨论这个问题，首先介绍注解。

注解允许您指定已定义的某些变量的预期类型。实际上，这不仅仅是关于类型，还有任何可以帮助您更好地了解该变量实际代表的元数据。

考虑以下示例：

```py
class Point:
    def __init__(self, lat, long):
        self.lat = lat
        self.long = long

def locate(latitude: float, longitude: float) -> Point:
    """Find an object in the map by its coordinates"""
```

在这里，我们使用`float`来指示`latitude`和`longitude`的预期类型。这仅仅是为了让函数的读者了解这些预期类型。Python 不会检查这些类型，也不会强制执行它们。

我们还可以指定函数返回值的预期类型。在这种情况下，`Point`是一个用户定义的类，这意味着无论返回什么都将是`Point`的一个实例。

然而，类型或内置类型并不是我们可以用作注解的唯一类型。基本上，任何在当前 Python 解释器范围内有效的东西都可以放在那里。例如，解释变量意图的字符串，可用作回调或验证函数的可调用对象等。

随着注解的引入，还包括了一个新的特殊属性，即`__annotations__`。这将使我们能够访问一个字典，将注解的名称（作为字典中的键）与它们的对应值进行映射，这些值是我们为它们定义的。在我们的示例中，这将如下所示：

```py
>>> locate.__annotations__
 {'latitude': float, 'longitue': float, 'return': __main__.Point}
```

如果我们认为有必要，我们可以使用这些来生成文档，运行验证，或者在我们的代码中强制检查。

说到通过注解检查代码，这就是 PEP-484 发挥作用的时候。这个 PEP 指定了类型提示的基础，即通过注解检查我们函数的类型。再次明确一下，引用 PEP-484 本身：

Python 将保持为一种动态类型的语言，作者们也不希望通过约定来强制类型提示成为必须。

类型提示的想法是提供额外的工具（与解释器无关），以检查和评估代码中类型的正确使用，并在检测到任何不兼容性时提示用户。运行这些检查的工具 Mypy 将在后面的章节中详细解释，我们将讨论如何为项目使用和配置这些工具。现在，您可以将其视为一种检查代码中使用的类型语义的 linter。这有时有助于在运行测试和检查时尽早发现错误。因此，将 Mypy 配置到项目中并将其与其他静态分析工具放在同一级别是一个好主意。

然而，类型提示意味着不仅仅是代码中类型检查的工具。从 Python 3.5 开始，引入了新的 typing 模块，这显著改进了我们在 Python 代码中定义类型和注释的方式。

其基本思想是，现在语义扩展到更有意义的概念，使我们（人类）更容易理解代码的含义，或者在给定点处期望的内容。例如，您可以有一个函数，它的一个参数是列表或元组，并且您可以将这两种类型之一作为注释，甚至是一个解释字符串。但是使用这个模块，可以告诉 Python 它期望一个可迭代对象或序列。甚至可以识别类型或其中的值；例如，它需要一个整数序列。

在编写本书时，关于注释方面进行了一项额外的改进，那就是从 Python 3.6 开始，可以直接注释变量，而不仅仅是函数参数和返回类型。这是在 PEP-526 中引入的，其想法是可以声明一些变量的类型，而不一定给它们赋值，如下面的清单所示：

```py
class Point:
    lat: float
    long: float

>>> Point.__annotations__
{'lat': <class 'float'>, 'long': <class 'float'>} 
```

# 注释是否取代了 docstrings？

这是一个合理的问题，因为在 Python 的旧版本中，在引入注释之前很久，函数或属性的参数类型的文档方式是通过在它们上面放置 docstrings 来完成的。甚至有一些关于如何构造 docstrings 的格式的约定，以包括函数的基本信息，包括每个参数的类型和含义，结果的类型和含义，以及函数可能引发的异常。

大部分内容已经通过注释以更紧凑的方式进行了处理，因此人们可能会想知道是否真的值得使用 docstrings。答案是肯定的，因为它们互补。

确实，以前包含在 docstring 中的一部分信息现在可以移动到注释中。但这只应该为 docstring 提供更好的文档空间。特别是对于动态和嵌套数据类型，提供预期数据的示例总是一个好主意，这样我们就可以更好地了解我们正在处理的内容。

考虑以下示例。假设我们有一个函数，它期望一个字典来验证一些数据：

```py
def data_from_response(response: dict) -> dict:
    if response["status"] != 200:
        raise ValueError
    return {"data": response["payload"]}
```

在这里，我们可以看到一个接受字典并返回另一个字典的函数。可能会在键`"status"`下的值不是预期值时引发异常。但是，我们对此了解不多。例如，`response`对象的正确实例是什么样的？`result`的实例会是什么样的？为了回答这两个问题，最好是记录预期由参数传入并由此函数返回的数据的示例。

让我们看看是否可以通过 docstring 更好地解释这一点：

```py
def data_from_response(response: dict) -> dict:
    """If the response is OK, return its payload.

    - response: A dict like::

    {
        "status": 200, # <int>
        "timestamp": "....", # ISO format string of the current
        date time
        "payload": { ... } # dict with the returned data
    }

    - Returns a dictionary like::

    {"data": { .. } }

    - Raises:
    - ValueError if the HTTP status is != 200
    """
    if response["status"] != 200:
        raise ValueError
    return {"data": response["payload"]}
```

现在，我们对这个函数预期接收和返回的内容有了更好的了解。文档不仅作为理解和了解传递内容的宝贵输入，还是单元测试的宝贵来源。我们可以从中获取数据作为输入，并知道测试时应该使用的正确和不正确的值。实际上，测试也可以作为我们代码的可执行文档，但这将在更详细地解释。

好处是现在我们知道键的可能值以及它们的类型，我们对数据的外观有了更具体的解释。成本是，正如我们之前提到的，它占用了很多行，并且需要冗长和详细才能有效。

# 配置工具以强制执行基本质量门

在这一部分，我们将探讨如何配置一些基本工具，并自动运行代码检查，目的是利用重复性验证检查的一部分。

这是一个重要的观点：记住，代码是为了我们人类理解的，所以只有我们才能确定什么是好的或坏的代码。我们应该在代码审查上投入时间，思考什么是好的代码，以及它的可读性和可理解性。当查看同事编写的代码时，你应该问这样的问题：

+   这段代码对其他程序员来说易于理解和遵循吗？

+   它是否以问题域的术语来表达？

+   一个新加入团队的人能够理解并有效地使用它吗？

正如我们之前所看到的，代码格式化、一致的布局和适当的缩进是必需的，但不足以成为代码库中具有的特征。此外，作为具有高质量意识的工程师，我们会认为这是理所当然的，因此我们会读写代码，远远超出其布局的基本概念。因此，我们不愿意浪费时间审查这些种类的项目，因此我们可以通过查看代码中的实际模式来更有效地投入时间，以理解其真正含义并提供有价值的结果。

所有这些检查都应该是自动化的。它们应该是测试或清单的一部分，反过来又应该是持续集成构建的一部分。如果这些检查未通过，构建将失败。这是确保代码结构始终连续的唯一方法。它也作为团队的客观参数参考。与其让一些工程师或团队领导在代码审查中总是说相同的关于 PEP-8 的评论，构建将自动失败，使其成为客观的事实。

# Mypy 的类型提示

Mypy ([`mypy-lang.org/`](http://mypy-lang.org/)) 是 Python 中可选的静态类型检查的主要工具。其想法是，一旦安装，它将分析项目中的所有文件，检查类型的使用是否一致。这是有用的，因为大多数时候，它会及早检测到实际的错误，但有时它可能会产生误报。

你可以使用 `pip` 安装它，并建议将其包含在项目的设置文件中作为依赖项：

```py
$ pip install mypy
```

一旦它安装在虚拟环境中，你只需运行上述命令，它将报告类型检查的所有发现。尽量遵循它的报告，因为大多数时候，它提供的见解有助于避免可能会滑入生产中的错误。然而，该工具并不完美，所以如果你认为它报告了一个误报，你可以用以下标记忽略该行作为注释：

```py
type_to_ignore = "something" # type: ignore
```

# 使用 Pylint 检查代码

在 Python 中，有许多用于检查代码结构的工具（基本上是符合 PEP-8 的），例如 pycodestyle（以前称为 PEP-8）、Flake8 等。它们都是可配置的，并且像运行它们提供的命令一样容易使用。在所有这些工具中，我发现 Pylint 是最完整（也是最严格）的。它也是可配置的。

再次强调，您只需在虚拟环境中使用`pip`安装它：

```py
$ pip install pylint
```

然后，只需运行`pylint`命令就足以检查代码。

可以通过名为`pylintrc`的配置文件来配置 Pylint。

在这个文件中，您可以决定要启用或禁用的规则，并对其他规则进行参数化（例如，更改列的最大长度）。

# 自动检查设置

在 Unix 开发环境中，最常见的工作方式是通过 makefile。**Makefile**是强大的工具，让我们可以配置在项目中运行的命令，主要用于编译、运行等。除此之外，我们还可以在项目的根目录中使用一个 makefile，其中配置了一些命令来自动运行代码的格式和约定检查。

一个好的方法是为测试设置目标，每个特定的测试，然后再设置一个将所有测试一起运行的目标。例如：

```py
typehint:
mypy src/ tests/

test:
pytest tests/

lint:
pylint src/ tests/

checklist: lint typehint test

.PHONY: typehint test lint checklist
```

在这里，我们应该运行的命令（无论是在我们的开发机器上还是在持续集成环境构建中）是以下命令：

```py
make checklist
```

这将按以下步骤运行所有内容：

1.  它将首先检查是否符合编码指南（例如 PEP-8）

1.  然后它将检查代码中类型的使用

1.  最后，它将运行测试

如果这些步骤中的任何一个失败，都要将整个过程视为失败。

除了在构建中自动配置这些检查之外，如果团队采用了一种约定和自动化的代码结构方法，那也是一个好主意。诸如 Black（[`github.com/ambv/black`](https://github.com/ambv/black)）之类的工具会自动格式化代码。有许多工具可以自动编辑代码，但 Black 的有趣之处在于它以独特的方式进行。它是一种主观和确定性的工具，因此代码最终总是以相同的方式排列。

例如，Black 字符串总是双引号，参数的顺序总是遵循相同的结构。这可能听起来很严格，但这是确保代码差异最小的唯一方法。如果代码始终遵守相同的结构，代码中的更改将只显示在实际进行更改的拉取请求中，而不会有额外的美观修改。它比 PEP-8 更严格，但也更方便，因为通过工具直接格式化代码，我们不必真正担心这个问题，可以专注于手头问题的关键。

在撰写本书时，唯一可以配置的是行的长度。其他一切都由项目的标准来纠正。

以下代码符合 PEP-8 规范，但不符合`black`的约定：

```py
def my_function(name):
 """
 >>> my_function('black')
 'received Black'
 """
 return 'received {0}'.format(name.title())
```

现在，我们可以运行以下命令来格式化文件：

```py
black -l 79 *.py
```

现在，我们可以看到工具写了什么：

```py
def my_function(name):
 """
 >>> my_function('black')
 'received Black'
 """
 return "received {0}".format(name.title())
```

在更复杂的代码中，会有更多的变化（尾随逗号等），但这个想法可以清楚地看到。再次强调，这是一种主观看法，但对于我们来说，拥有一个处理细节的工具也是一个好主意。这也是 Golang 社区很久以前就学会的东西，以至于有一个标准的工具库`got fmt`，可以根据语言的约定自动格式化代码。Python 现在也有这样的东西，这是件好事。

这些工具（Black、Pylint、Mypy 等）可以与您选择的编辑器或 IDE 集成，使事情变得更加容易。将编辑器配置为在保存文件时或通过快捷键进行这些修改是一个不错的投资。

# 总结

现在我们对清晰的代码有了第一个概念，以及一个可行的解释，这将成为本书其余部分的参考点。

更重要的是，我们明白了清晰的代码比代码的结构和布局更重要得多。我们必须关注代码中的思想是如何表示的，以确定它们是否正确。清晰的代码是关于代码的可读性、可维护性，将技术债务降到最低，并有效地将我们的想法传达到代码中，以便他人能够理解我们最初打算写的东西。

然而，我们讨论了遵循编码风格或指南的重要性，有多种原因。我们一致认为这是一个必要的条件，但并不充分，因为它是每个扎实项目都应该遵守的最低要求，很明显，这是我们最好留给工具的事情。因此，自动化所有这些检查变得至关重要，在这方面，我们必须记住如何配置诸如 Mypy、Pylint 等工具。

接下来的章节将更加专注于特定于 Python 的代码，以及如何用 Python 的习惯用法表达我们的想法。我们将探讨 Python 中使代码更简洁高效的习惯用法。在这个分析中，我们将看到，总的来说，Python 有不同的想法或不同的方法来完成任务，与其他语言相比。
