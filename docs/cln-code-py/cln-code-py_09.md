# 第九章：常见的设计模式

自从它们最初出现在著名的**四人帮**（**GoF**）的书籍*《设计模式：可复用面向对象软件的元素》*中以来，设计模式一直是软件工程中的一个广泛讨论的话题。设计模式有助于解决一些常见的问题，这些问题是针对特定场景的抽象。当它们被正确实现时，解决方案的一般设计可以从中受益。

在本章中，我们将从最常见的设计模式的角度来看，但不是从在特定条件下应用工具的角度（一旦设计出模式），而是分析设计模式如何有助于编写清晰的代码。在介绍实现设计模式的解决方案后，我们将分析最终实现与选择不同路径相比是如何更好的。

在这个分析的过程中，我们将看到如何在 Python 中具体实现设计模式。由此产生的结果是，我们将看到 Python 的动态特性意味着在实现上存在一些差异，与其他静态类型的语言相比，许多设计模式最初是针对这些语言而设计的。这意味着在涉及 Python 时，设计模式有一些特殊之处，你应该记住，有些情况下，试图应用一个设计模式，而它实际上并不适用于 Python，是不符合 Python 风格的。

在本章中，我们将涵盖以下主题：

+   常见的设计模式。

+   在 Python 中不适用的设计模式，以及应该遵循的惯用替代方案。

+   在 Python 中实现最常见的设计模式的 Python 风格。

+   理解良好的抽象是如何自然地演变成模式的。

# Python 中的设计模式考虑事项

面向对象的设计模式是软件构建的想法，在解决问题模型时出现在不同的场景中。因为它们是高层次的想法，很难将它们视为与特定编程语言相关联。相反，它们更多是关于对象在应用程序中如何交互的一般概念。当然，它们会有它们的实现细节，从语言到语言会有所不同，但这并不构成设计模式的本质。

这是设计模式的理论方面，它是一个关于解决方案中对象布局的抽象概念。关于面向对象设计和设计模式的其他书籍和资源有很多，所以在本书中，我们将专注于 Python 的这些实现细节。

鉴于 Python 的特性，一些经典的设计模式实际上并不需要。这意味着 Python 已经支持了使这些模式不再需要的功能。有人认为它们在 Python 中不存在，但请记住，不可见并不意味着不存在。它们确实存在，只是嵌入在 Python 本身中，所以我们可能甚至不会注意到它们。

其他的实现方式要简单得多，这要归功于语言的动态特性，其余的实现方式在其他平台上几乎是一样的，只有细微的差异。

无论如何，在 Python 中实现清晰的代码的重要目标是知道要实现哪些模式以及如何实现。这意味着要识别 Python 已经抽象出来的一些模式以及我们如何利用它们。例如，尝试实现迭代器模式的标准定义（就像我们在其他语言中所做的那样）是完全不符合 Python 风格的，因为（正如我们已经讨论过的）迭代在 Python 中已经深深嵌入，我们可以创建的对象可以直接在`for`循环中工作，这是正确的做法。

一些创建型模式也存在类似的情况。在 Python 中，类是常规对象，函数也是。正如我们之前在几个示例中看到的那样，它们可以被传递、装饰、重新分配等。这意味着无论我们想对对象进行什么样的定制，我们很可能可以在不需要任何特定的工厂类设置的情况下完成。此外，在 Python 中没有创建对象的特殊语法（例如没有 new 关键字）。这也是为什么大多数情况下，简单的函数调用就可以作为工厂。

其他模式仍然是必需的，我们将看到如何通过一些小的调整，使它们更符合 Python 的特点，充分利用语言提供的特性（魔术方法或标准库）。

在所有可用的模式中，并非所有模式都同样频繁，也不同样有用，因此我们将专注于主要的模式，那些我们期望在我们的应用程序中最常见的模式，并且我们将通过实用的方法来做到这一点。

# 设计模式的实际应用

作为 GoF 所写的这个主题的权威参考，介绍了 23 种设计模式，每一种都属于创建型、结构型和行为型中的一种。甚至还有更多的模式或现有模式的变体，但我们不应该死记所有这些模式，而是应该专注于牢记两件事。一些模式在 Python 中是看不见的，我们可能在不知不觉中使用它们。其次，并非所有模式都同样常见；其中一些模式非常有用，因此它们经常出现，而其他模式则更适用于特定情况。

在本节中，我们将重新审视最常见的模式，这些模式最有可能从我们的设计中出现。请注意这里使用了“出现”这个词。这很重要。我们不应该强制将设计模式应用于我们正在构建的解决方案，而是应该演变、重构和改进我们的解决方案，直到出现一个模式。

因此，设计模式并非是被发明出来的，而是被发现的。当我们的代码中反复出现的情况揭示出来时，类、对象和相关组件的一般和更抽象的布局以一种名称出现，我们通过这个名称来识别一个模式。

思考同样的事情，但现在是向后看，我们意识到设计模式的名称包含了许多概念。这可能是设计模式最好的地方；它们提供了一种语言。通过设计模式，更容易有效地传达设计思想。当两个或更多的软件工程师共享相同的词汇时，其中一个提到构建器，其他人可以立即想到所有的类，它们之间的关系，它们的机制等，而无需再次重复这个解释。

读者会注意到，本章中显示的代码与所讨论的设计模式的规范或原始构想不同。这有不止一个原因。第一个原因是示例采用了更加务实的方法，旨在解决特定场景的问题，而不是探索一般的设计理论。第二个原因是这些模式是根据 Python 的特点实现的，在某些情况下可能非常微妙，但在其他情况下，差异是明显的，通常简化了代码。

# 创建型模式

在软件工程中，创建型模式处理对象实例化，试图抽象掉大部分复杂性（比如确定初始化对象的参数，可能需要的所有相关对象等），以便为用户留下一个更简单、更安全的接口。对象创建的基本形式可能导致设计问题或增加设计的复杂性。创建型设计模式通过某种方式控制对象的创建来解决这个问题。

创建对象的五种模式中，我们将主要讨论用于避免单例模式并用 Borg 模式替代的变体（在 Python 应用程序中最常用），讨论它们的区别和优势。

# 工厂

正如在介绍中提到的，Python 的一个核心特性是一切都是对象，因此它们都可以平等对待。这意味着对类、函数或自定义对象没有特殊的区分。它们都可以作为参数传递、赋值等。

正因为如此，许多工厂模式实际上并不是真正需要的。我们只需简单地定义一个函数来构造一组对象，甚至可以通过参数传递要创建的类。

# 单例和共享状态（单态）

另一方面，单例模式并不是完全被 Python 抽象化的东西。事实上，大多数情况下，这种模式要么不是真正需要的，要么是一个糟糕的选择。单例存在很多问题（毕竟，它们实际上是面向对象软件的全局变量形式，因此是一种不好的实践）。它们很难进行单元测试，它们可能随时被任何对象修改，这使得它们很难预测，它们的副作用可能会带来真正的问题。

作为一个一般原则，我们应该尽量避免使用单例。如果在某些极端情况下需要它们，Python 中最简单的实现方式是使用模块。我们可以在一个模块中创建一个对象，一旦在那里，它将从模块的每个部分中可用。Python 本身确保模块已经是单例，无论它们被导入多少次，从多少地方导入，始终是相同的模块将被加载到`sys.modules`中。

# 共享状态

与其强制设计为只创建一个实例的单例，无论对象如何被调用、构造或初始化，还不如在多个实例之间复制数据。

单态模式（SNGMONO）的想法是我们可以有许多实例，它们只是普通对象，而不必关心它们是否是单例（因为它们只是对象）。这种模式的好处是这些对象的信息将以完全透明的方式同步，而无需我们担心这是如何在内部工作的。

这使得这种模式成为一个更好的选择，不仅因为它的便利性，而且因为它更少受到单例的缺点的影响（关于它们的可测试性、创建派生类等）。

我们可以在许多级别上使用这种模式，这取决于我们需要同步多少信息。

在最简单的形式中，我们可以假设只需要一个属性在所有实例中反映。如果是这种情况，实现就像使用一个类变量一样简单，我们只需要确保提供正确的接口来更新和检索属性的值。

假设我们有一个对象，必须通过最新的“标签”从 Git 存储库中拉取代码的版本。可能会有多个此对象的实例，当每个客户端调用获取代码的方法时，此对象将使用其属性中的“标签”版本。在任何时候，此“标签”都可以更新为更新版本，我们希望任何其他实例（新创建的或已创建的）在调用“获取”操作时使用这个新分支，如下面的代码所示：

```py
class GitFetcher:
    _current_tag = None

    def __init__(self, tag):
        self.current_tag = tag

    @property
    def current_tag(self):
        if self._current_tag is None:
            raise AttributeError("tag was never set")
        return self._current_tag

    @current_tag.setter
    def current_tag(self, new_tag):
        self.__class__._current_tag = new_tag

    def pull(self):
        logger.info("pulling from %s", self.current_tag)
        return self.current_tag
```

读者只需验证创建具有不同版本的`GitFetcher`类型的多个对象将导致所有对象在任何时候都设置为最新版本，如下面的代码所示：

```py
>>> f1 = GitFetcher(0.1)
>>> f2 = GitFetcher(0.2)
>>> f1.current_tag = 0.3
>>> f2.pull()
0.3
>>> f1.pull()
0.3
```

如果我们需要更多属性，或者希望更好地封装共享属性，使设计更清晰，我们可以使用描述符。

像下面代码中所示的描述符解决了问题，虽然它需要更多的代码，但它也封装了更具体的责任，部分代码实际上从我们的原始类中移开，使它们中的任何一个更具凝聚性和符合单一责任原则：

```py
class SharedAttribute:
    def __init__(self, initial_value=None):
        self.value = initial_value
        self._name = None

    def __get__(self, instance, owner):
        if instance is None:
            return self
        if self.value is None:
            raise AttributeError(f"{self._name} was never set")
        return self.value

    def __set__(self, instance, new_value):
        self.value = new_value

    def __set_name__(self, owner, name):
        self._name = name
```

除了这些考虑因素外，模式现在更具可重用性。如果我们想重复这个逻辑，我们只需创建一个新的描述符对象，它将起作用（符合 DRY 原则）。

如果我们现在想做同样的事情，但是针对当前的分支，我们创建这个新的类属性，而类的其余部分保持不变，同时仍然具有所需的逻辑，如下面的代码所示：

```py
class GitFetcher:
    current_tag = SharedAttribute()
    current_branch = SharedAttribute()

    def __init__(self, tag, branch=None):
        self.current_tag = tag
        self.current_branch = branch

    def pull(self):
        logger.info("pulling from %s", self.current_tag)
        return self.current_tag
```

这种新方法的平衡和权衡现在应该是清楚的。这种新实现使用了更多的代码，但它是可重用的，因此从长远来看它节省了代码行数（和重复的逻辑）。再次参考三个或更多实例的规则，以决定是否应该创建这样的抽象。

这种解决方案的另一个重要好处是它还减少了单元测试的重复。在这里重用代码将使我们对解决方案的整体质量更有信心，因为现在我们只需要为描述符对象编写单元测试，而不是为使用它的所有类编写单元测试（只要单元测试证明描述符是正确的，我们就可以安全地假设它们是正确的）。

# borg 模式

前面的解决方案对大多数情况都适用，但如果我们真的必须使用单例（这必须是一个非常好的例外情况），那么还有一个更好的替代方案，尽管这是一个更有风险的选择。

这是实际的单态模式，在 Python 中被称为 borg 模式。其思想是创建一个对象，能够在同一类的所有实例之间复制其所有属性。绝对复制每个属性的事实必须警示我们要注意不良副作用。尽管如此，这种模式比单例模式有很多优点。

在这种情况下，我们将之前的对象分成两个部分——一个用于 Git 标签，另一个用于分支。我们使用的代码将使 borg 模式起作用：

```py
class BaseFetcher:
    def __init__(self, source):
        self.source = source

class TagFetcher(BaseFetcher):
    _attributes = {}

    def __init__(self, source):
        self.__dict__ = self.__class__._attributes
        super().__init__(source)

    def pull(self):
        logger.info("pulling from tag %s", self.source)
        return f"Tag = {self.source}"

class BranchFetcher(BaseFetcher):
    _attributes = {}

    def __init__(self, source):
        self.__dict__ = self.__class__._attributes
        super().__init__(source)

    def pull(self):
        logger.info("pulling from branch %s", self.source)
        return f"Branch = {self.source}"
```

这两个对象都有一个基类，共享它们的初始化方法。但是它们必须再次实现它，以使 borg 逻辑起作用。其思想是使用一个类属性，它是一个字典，用于存储属性，然后使每个对象的字典（在初始化时）使用这个完全相同的字典。这意味着对一个对象的字典的任何更新都会反映在类中，因为它们的类是相同的，而字典是可变对象，是作为引用传递的。换句话说，当我们创建这种类型的新对象时，它们都将使用相同的字典，并且这个字典会不断更新。

请注意，我们不能将字典的逻辑放在基类上，因为这将使不同类的对象混合值，这不是我们想要的。这种样板解决方案会让许多人认为这实际上是一种习惯用语，而不是一种模式。

实现 DRY 原则的一种可能的抽象方式是创建一个 mixin 类，如下面的代码所示：

```py
class SharedAllMixin:
    def __init__(self, *args, **kwargs):
        try:
            self.__class__._attributes
        except AttributeError:
            self.__class__._attributes = {}

        self.__dict__ = self.__class__._attributes
        super().__init__(*args, **kwargs)

class BaseFetcher:
    def __init__(self, source):
        self.source = source

class TagFetcher(SharedAllMixin, BaseFetcher):
    def pull(self):
        logger.info("pulling from tag %s", self.source)
        return f"Tag = {self.source}"

class BranchFetcher(SharedAllMixin, BaseFetcher):
    def pull(self):
        logger.info("pulling from branch %s", self.source)
        return f"Branch = {self.source}"
```

这一次，我们使用 mixin 类在每个类中创建具有属性的字典（如果它尚不存在），然后继续相同的逻辑。

这种实现在继承方面不应该有任何主要问题，因此这是一个更可行的替代方案。

# 建造者

建造者模式是一个有趣的模式，它抽象了对象的所有复杂初始化。这种模式不依赖于语言的任何特性，因此在 Python 中同样适用于任何其他语言。

虽然它解决了一个有效的情况，但通常也是一个更可能出现在框架、库或 API 设计中的复杂情况。与描述符的建议类似，我们应该将这种实现保留给我们期望公开的 API 将被多个用户使用的情况。

这种模式的高层思想是，我们需要创建一个复杂的对象，这个对象也需要许多其他对象一起工作。我们不希望让用户创建所有这些辅助对象，然后将它们分配给主要对象，而是希望创建一个抽象，允许所有这些在一个步骤中完成。为了实现这一点，我们将有一个构建对象，它知道如何创建所有部分并将它们链接在一起，给用户一个接口（可能是一个类方法），以参数化有关所得对象应该看起来像什么的所有信息。

# 结构模式

结构模式对于需要创建更简单的接口或通过扩展功能而使对象更强大而不增加接口复杂性的情况非常有用。

这些模式最好的地方在于我们可以创建更有趣的对象，具有增强的功能，并且可以以一种干净的方式实现这一点；也就是说，通过组合多个单一对象（这个最清晰的例子就是组合模式），或者通过收集许多简单而紧密的接口。

# 适配器

适配器模式可能是最简单的设计模式之一，同时也是最有用的设计模式之一。也被称为包装器，这种模式解决了两个或多个不兼容对象的接口适配问题。

通常情况下，我们的代码的一部分与一个模型或一组类一起工作，这些类在某个方法方面是多态的。例如，如果有多个对象用于检索具有`fetch（）`方法的数据，那么我们希望保持这个接口，这样我们就不必对我们的代码进行重大更改。

但是，当我们需要添加一个新的数据源时，这个数据源却没有`fetch（）`方法。更糟糕的是，这种类型的对象不仅不兼容，而且也不是我们控制的（也许是另一个团队决定了 API，我们无法修改代码）。

我们不直接使用这个对象，而是将其接口采用到我们需要的接口上。有两种方法可以做到这一点。

第一种方法是创建一个从我们想要使用的类继承的类，并为该方法创建一个别名（如果需要，还必须调整参数和签名）。

通过继承，我们导入外部类并创建一个新类，该类将定义新方法，调用具有不同名称的方法。在这个例子中，假设外部依赖项有一个名为`search（）`的方法，它只接受一个参数进行搜索，因为它以不同的方式查询，所以我们的`adapter`方法不仅调用外部方法，而且还相应地转换参数，如下面的代码所示：

```py
from _adapter_base import UsernameLookup

class UserSource(UsernameLookup):
    def fetch(self, user_id, username):
        user_namespace = self._adapt_arguments(user_id, username)
        return self.search(user_namespace)

    @staticmethod
    def _adapt_arguments(user_id, username):
        return f"{user_id}:{username}"
```

也许我们的类已经从另一个类派生出来了，在这种情况下，这将成为多重继承的情况，Python 支持这一点，所以这不应该是一个问题。然而，正如我们以前多次看到的那样，继承会带来更多的耦合（谁知道有多少其他方法是从外部库中继承而来的？），而且它是不灵活的。从概念上讲，这也不是正确的选择，因为我们将继承保留给规范的情况（一种**是一个**的关系），在这种情况下，我们完全不清楚我们的对象是否必须是第三方库提供的那种对象之一（特别是因为我们并不完全理解那个对象）。

因此，更好的方法是使用组合。假设我们可以为我们的对象提供一个`UsernameLookup`的实例，那么代码就会变得很简单，只需在采用参数之前重定向请求，如下面的代码所示：

```py
class UserSource:
    ...
    def fetch(self, user_id, username):
        user_namespace = self._adapt_arguments(user_id, username)
        return self.username_lookup.search(user_namespace)
```

如果我们需要采用多种方法，并且我们可以想出一种通用的方法来调整它们的签名，那么使用`__getattr__()`魔术方法将请求重定向到包装对象可能是值得的，但是像所有通用实现一样，我们应该小心不要给解决方案增加更多的复杂性。

# 组合

我们的程序中将有一些部分需要我们处理由其他对象组成的对象。我们有基本对象，具有明确定义的逻辑，然后我们将有其他容器对象，将一堆基本对象分组，挑战在于我们希望处理这两种对象（基本对象和容器对象）而不会注意到任何差异。

对象按树形层次结构组织，基本对象将是树的叶子，组合对象是中间节点。客户可能希望调用它们中的任何一个来获得调用的方法的结果。然而，组合对象将充当客户端；它也将传递这个请求以及它包含的所有对象，无论它们是叶子还是其他中间节点，直到它们都被处理。

想象一个在线商店的简化版本，在这个商店里我们有产品。假设我们提供了将这些产品分组的可能性，并且我们给顾客每组产品提供折扣。产品有一个价格，当顾客来付款时，就会要求这个价格。但是一组分组的产品也有一个必须计算的价格。我们将有一个代表这个包含产品的组的对象，并且将责任委托给每个特定产品询问价格（这个产品也可能是另一组产品），等等，直到没有其他东西需要计算。这个实现如下代码所示：

```py
class Product:
    def __init__(self, name, price):
        self._name = name
        self._price = price

    @property
    def price(self):
        return self._price

class ProductBundle:
    def __init__(
        self,
        name,
        perc_discount,
        *products: Iterable[Union[Product, "ProductBundle"]]
    ) -> None:
        self._name = name
        self._perc_discount = perc_discount
        self._products = products

    @property
    def price(self):
        total = sum(p.price for p in self._products)
        return total * (1 - self._perc_discount)
```

我们通过一个属性公开公共接口，并将`price`作为私有属性。`ProductBundle`类使用这个属性来计算值，并首先添加它包含的所有产品的价格。

这些对象之间唯一的差异是它们是用不同的参数创建的。为了完全兼容，我们应该尝试模仿相同的接口，然后添加额外的方法来向包中添加产品，但使用一个允许创建完整对象的接口。不需要这些额外的步骤是一个可以证明这个小差异的优势。

# 装饰器

不要将装饰器模式与我们在第五章中介绍的 Python 装饰器的概念混淆，*使用装饰器改进我们的代码*。它们有一些相似之处，但设计模式的想法是完全不同的。

这种模式允许我们动态扩展一些对象的功能，而不需要继承。这是创建更灵活对象的多重继承的一个很好的替代方案。

我们将创建一个结构，让用户定义要应用于对象的一组操作（装饰），并且我们将看到每个步骤按指定顺序进行。

以下代码示例是一个以参数形式从传递给它的参数构造字典形式的查询的对象的简化版本（例如，它可能是我们用于运行到 elasticsearch 的查询的对象，但代码省略了分散注意力的实现细节，以便专注于模式的概念）。

在其最基本的形式中，查询只返回创建时提供的数据的字典。客户端期望使用此对象的`render()`方法。

```py
class DictQuery:
    def __init__(self, **kwargs):
        self._raw_query = kwargs

    def render(self) -> dict:
        return self._raw_query
```

现在我们想通过对数据应用转换来以不同的方式呈现查询（过滤值，标准化等）。我们可以创建装饰器并将它们应用于`render`方法，但这不够灵活，如果我们想在运行时更改它们怎么办？或者如果我们只想选择其中一些，而不选择其他一些呢？

设计是创建另一个对象，具有相同的接口和通过多个步骤增强（装饰）原始结果的能力，但可以组合。这些对象被链接在一起，每个对象都会做最初应该做的事情，再加上其他一些东西。这些其他东西就是特定的装饰步骤。

由于 Python 具有鸭子类型，我们不需要创建一个新的基类，并使这些新对象成为该层次结构的一部分，以及`DictQuery`。只需创建一个具有`render()`方法的新类就足够了（再次，多态性不应该需要继承）。这个过程在下面的代码中显示：

```py
class QueryEnhancer:
    def __init__(self, query: DictQuery):
        self.decorated = query

    def render(self):
        return self.decorated.render()

class RemoveEmpty(QueryEnhancer):
    def render(self):
        original = super().render()
        return {k: v for k, v in original.items() if v}

class CaseInsensitive(QueryEnhancer):
    def render(self):
        original = super().render()
        return {k: v.lower() for k, v in original.items()}
```

`QueryEnhancer`短语具有与`DictQuery`的客户端期望的接口兼容的接口，因此它们是可互换的。这个对象被设计为接收一个装饰过的对象。它将从中获取值并将其转换，返回代码的修改版本。

如果我们想要删除所有评估为`False`的值并将它们标准化以形成我们的原始查询，我们将不得不使用以下模式：

```py
>>> original = DictQuery(key="value", empty="", none=None, upper="UPPERCASE", title="Title")
>>> new_query = CaseInsensitive(RemoveEmpty(original))
>>> original.render()
{'key': 'value', 'empty': '', 'none': None, 'upper': 'UPPERCASE', 'title': 'Title'}
>>> new_query.render()
{'key': 'value', 'upper': 'uppercase', 'title': 'title'}
```

这是一个我们也可以以不同方式实现的模式，利用 Python 的动态特性和函数是对象的事实。我们可以使用提供给基本装饰器对象（`QueryEnhancer`）的函数来实现这种模式，并将每个装饰步骤定义为一个函数，如下面的代码所示：

```py
class QueryEnhancer:
    def __init__(
        self,
        query: DictQuery,
        *decorators: Iterable[Callable[[Dict[str, str]], Dict[str, str]]]
    ) -> None:
        self._decorated = query
        self._decorators = decorators

    def render(self):
        current_result = self._decorated.render()
        for deco in self._decorators:
            current_result = deco(current_result)
        return current_result
```

就客户端而言，由于这个类通过其`render()`方法保持兼容性，因此没有改变。但在内部，这个对象的使用方式略有不同，如下面的代码所示：

```py
>>> query = DictQuery(foo="bar", empty="", none=None, upper="UPPERCASE", title="Title")
>>> QueryEnhancer(query, remove_empty, case_insensitive).render()
{'foo': 'bar', 'upper': 'uppercase', 'title': 'title'}
```

在前面的代码中，`remove_empty`和`case_insensitive`只是转换字典的常规函数。

在这个例子中，基于函数的方法似乎更容易理解。可能存在更复杂的规则，这些规则依赖于被装饰对象的数据（不仅仅是其结果），在这种情况下，可能值得采用面向对象的方法，特别是如果我们真的想要创建一个对象层次结构，其中每个类实际上代表了我们想要在设计中明确表示的某些知识。

# 外观

Facade 是一个很好的模式。它在许多情况下都很有用，当我们想要简化对象之间的交互时。该模式适用于多个对象之间存在多对多关系，并且我们希望它们进行交互。我们不是创建所有这些连接，而是在它们前面放置一个作为外观的中间对象。

门面在这个布局中充当一个中心或单一的参考点。每当一个新对象想要连接到另一个对象时，它不需要为所有可能连接到的*N*个对象拥有*N*个接口，而是只需与门面交谈，门面会相应地重定向请求。门面后面的一切对外部对象完全不透明。

除了主要和明显的好处（对象的解耦），这种模式还鼓励更简单的设计，更少的接口和更好的封装。

这是一个我们不仅可以用来改进我们领域问题的代码，还可以用来创建更好的 API 的模式。如果我们使用这种模式并提供一个单一的接口，作为我们代码的单一真相点或入口点，那么我们的用户与暴露的功能交互将会更容易。不仅如此，通过暴露一个功能并隐藏一切在接口后面，我们可以自由地改变或重构底层代码，因为只要它在门面后面，它就不会破坏向后兼容性，我们的用户也不会受到影响。

注意，使用门面的这个想法不仅仅局限于对象和类，还适用于包（在技术上，包在 Python 中也是对象，但仍然）。我们可以使用门面的这个想法来决定包的布局；即，对用户可见和可导入的内容，以及内部的内容，不应该直接导入。

当我们创建一个目录来构建一个包时，我们将`__init__.py`文件与其余文件放在一起。这是模块的根，一种门面。其余的文件定义要导出的对象，但它们不应该被客户端直接导入。`init`文件应该导入它们，然后客户端应该从那里获取它们。这样创建了一个更好的接口，因为用户只需要知道一个单一的入口点来获取对象，更重要的是，包（其余的文件）可以根据需要进行重构或重新排列，只要`init`文件上的主要 API 得到维护，这不会影响客户端。牢记这样的原则是非常重要的，以构建可维护的软件。

Python 本身就有一个例子，使用`os`模块。这个模块将操作系统的功能分组在一起，但在底层，它使用`posix`模块来处理**可移植操作系统接口**（**POSIX**）操作系统（在 Windows 平台上称为`nt`）。这个想法是，出于可移植性的原因，我们不应该直接导入`posix`模块，而应该始终导入`os`模块。这个模块要确定它被调用的平台，并公开相应的功能。

# 行为模式

行为模式旨在解决对象应该如何合作，它们应该如何通信，以及运行时它们的接口应该是什么的问题。

我们主要讨论以下行为模式：

+   责任链

+   模板方法

+   命令

+   状态

这可以通过继承静态地实现，也可以通过组合动态地实现。无论模式使用什么，我们将在接下来的例子中看到，这些模式的共同之处在于，最终的代码在某种重要的方式上更好，无论是因为它避免了重复，还是因为它创建了良好的抽象，封装了相应的行为，并解耦了我们的模型。

# 责任链

现在我们要再次审视我们的事件系统。我们想要从日志行（例如从我们的 HTTP 应用服务器转储的文本文件）中解析系统上发生的事件的信息，并以一种方便的方式提取这些信息。

在我们先前的实现中，我们实现了一个有趣的解决方案，符合开闭原则，并依赖于使用`__subclasses__()`魔术方法来发现所有可能的事件类型，并使用正确的事件处理数据，通过每个类上封装的方法解决责任。

这个解决方案对我们的目的是有效的，并且它是相当可扩展的，但正如我们将看到的，这种设计模式将带来额外的好处。

这里的想法是，我们将以稍微不同的方式创建事件。每个事件仍然具有确定是否可以处理特定日志行的逻辑，但它还将具有一个后继者。这个后继者是一个新的事件，是行中的下一个事件，它将继续处理文本行，以防第一个事件无法这样做。逻辑很简单——我们链接这些事件，每个事件都尝试处理数据。如果可以，它就返回结果。如果不能，它将把它传递给它的后继者并重复，如下所示的代码：

```py
import re

class Event:
    pattern = None

    def __init__(self, next_event=None):
        self.successor = next_event

    def process(self, logline: str):
        if self.can_process(logline):
            return self._process(logline)

        if self.successor is not None:
            return self.successor.process(logline)

    def _process(self, logline: str) -> dict:
        parsed_data = self._parse_data(logline)
        return {
            "type": self.__class__.__name__,
            "id": parsed_data["id"],
            "value": parsed_data["value"],
        }

    @classmethod
    def can_process(cls, logline: str) -> bool:
        return cls.pattern.match(logline) is not None

    @classmethod
    def _parse_data(cls, logline: str) -> dict:
        return cls.pattern.match(logline).groupdict()

class LoginEvent(Event):
    pattern = re.compile(r"(?P<id>\d+):\s+login\s+(?P<value>\S+)")

class LogoutEvent(Event):
    pattern = re.compile(r"(?P<id>\d+):\s+logout\s+(?P<value>\S+)")
```

通过这种实现，我们创建了`event`对象，并按照它们将被处理的特定顺序排列它们。由于它们都有一个`process()`方法，它们对于这个消息是多态的，所以它们被排列的顺序对于客户端来说是完全透明的，它们中的任何一个也是透明的。不仅如此，`process()`方法也具有相同的逻辑；它尝试提取信息，如果提供的数据对于处理它的对象类型是正确的，如果不是，它就继续到下一个对象。

这样，我们可以按以下方式`process`登录事件：

```py
>>> chain = LogoutEvent(LoginEvent())
>>> chain.process("567: login User")
{'type': 'LoginEvent', 'id': '567', 'value': 'User'}
```

注意`LogoutEvent`作为其后继者接收了`LoginEvent`，当它被要求处理无法处理的内容时，它会重定向到正确的对象。从字典的`type`键上可以看出，`LoginEvent`实际上是创建了该字典的对象。

这个解决方案足够灵活，并且与我们先前的解决方案共享一个有趣的特性——所有条件都是互斥的。只要没有冲突，没有数据有多个处理程序，以任何顺序处理事件都不会成为问题。

但是如果我们不能做出这样的假设呢？通过先前的实现，我们仍然可以将`__subclasses__()`调用更改为根据我们的标准制作的列表，这样也可以正常工作。如果我们希望优先顺序在运行时（例如由用户或客户端）确定呢？那将是一个缺点。

有了新的解决方案，我们可以实现这样的要求，因为我们在运行时组装链条，所以我们可以根据需要动态地操纵它。

例如，现在我们添加了一个通用类型，将登录和注销会话事件分组，如下所示的代码：

```py
class SessionEvent(Event):
    pattern = re.compile(r"(?P<id>\d+):\s+log(in|out)\s+(?P<value>\S+)")
```

如果由于某种原因，在应用程序的某个部分，我们希望在登录事件之前捕获这个，可以通过以下`chain`来实现：

```py
chain = SessionEvent(LoginEvent(LogoutEvent()))
```

通过改变顺序，我们可以，例如，说一个通用会话事件比登录事件具有更高的优先级，但不是注销事件，依此类推。

这种模式与对象一起工作的事实使它相对于我们先前的依赖于类的实现更加灵活（虽然它们在 Python 中仍然是对象，但它们并不排除一定程度的刚性）。

# 模板方法

`template`方法是一种在正确实现时产生重要好处的模式。主要是，它允许我们重用代码，而且还使我们的对象更灵活，更容易改变，同时保持多态性。

这个想法是，有一个类层次结构，定义了一些行为，比如说它的公共接口中的一个重要方法。层次结构中的所有类共享一个公共模板，并且可能只需要更改其中的某些元素。因此，这个想法是将这个通用逻辑放在父类的公共方法中，该方法将在内部调用所有其他（私有）方法，而这些方法是派生类将要修改的方法；因此，模板中的所有逻辑都被重用。

热心的读者可能已经注意到，我们在上一节中已经实现了这种模式（作为责任链示例的一部分）。请注意，从`Event`派生的类只实现了它们特定的模式。对于其余的逻辑，模板在`Event`类中。`process`事件是通用的，并依赖于两个辅助方法`can_process()`和`process()`（后者又调用`_parse_data()`）。

这些额外的方法依赖于类属性模式。因此，为了用新类型的对象扩展这个模式，我们只需要创建一个新的派生类并放置正则表达式。之后，其余的逻辑将继承这个新属性的变化。这样做可以重用大量的代码，因为处理日志行的逻辑只在父类中定义一次。

这使得设计变得灵活，因为保持多态性也很容易实现。如果我们需要一个新的事件类型，由于某种原因需要以不同的方式解析数据，我们只需要在子类中覆盖这个私有方法，兼容性将得到保持，只要它返回与原始类型相同的类型（符合 Liskov 的替换和开闭原则）。这是因为是父类调用派生类的方法。

如果我们正在设计自己的库或框架，这种模式也很有用。通过这种方式安排逻辑，我们使用户能够相当容易地改变其中一个类的行为。他们只需要创建一个子类并覆盖特定的私有方法，结果将是一个具有新行为的新对象，保证与原始对象的调用者兼容。

# 命令

命令模式为我们提供了将需要执行的操作与请求执行的时刻分开的能力。更重要的是，它还可以将客户端发出的原始请求与接收者分开，接收者可能是一个不同的对象。在本节中，我们将主要关注模式的第一个方面；我们可以将命令如何运行与它实际执行的时刻分开。

我们知道我们可以通过实现`__call__()`魔术方法来创建可调用对象，因此我们可以初始化对象，然后以后再调用它。事实上，如果这是唯一的要求，我们甚至可以通过一个嵌套函数来实现这一点，通过闭包创建另一个函数来实现延迟执行的效果。但是这种模式可以扩展到不那么容易实现的地方。

这个想法是命令在定义后也可以被修改。这意味着客户端指定要运行的命令，然后可能更改一些参数，添加更多选项等，直到最终有人决定执行这个动作。

这种情况的例子可以在与数据库交互的库中找到。例如，在`psycopg2`（一个 PostgreSQL 客户端库）中，我们建立一个连接。从这个连接中，我们得到一个游标，然后我们可以向这个游标传递要运行的 SQL 语句。当我们调用`execute`方法时，对象的内部表示会发生变化，但实际上并没有在数据库中运行任何东西。只有当我们调用`fetchall()`（或类似的方法）时，数据才会被查询并在游标中可用。

在流行的**对象关系映射 SQLAlchemy**（**ORM SQLAlchemy**）中也是如此。查询是通过几个步骤定义的，一旦我们有了`query`对象，我们仍然可以与之交互（添加或删除过滤器，更改条件，申请排序等），直到我们决定要查询的结果。在调用每个方法之后，`query`对象会改变其内部属性并返回`self`（它自己）。

这些都是类似我们想要实现的行为的示例。创建这种结构的一个非常简单的方法是拥有一个对象，该对象存储要运行的命令的参数。之后，它还必须提供与这些参数交互的方法（添加或删除过滤器等）。可选地，我们可以向该对象添加跟踪或日志记录功能，以审计已经发生的操作。最后，我们需要提供一个实际执行操作的方法。这个方法可以是`__call__()`或自定义的方法。让我们称之为`do()`。

# 状态

状态模式是软件设计中具体化的一个明显例子，使我们的领域问题的概念成为一个显式对象，而不仅仅是一个边值。

在第八章中，*单元测试和重构*，我们有一个代表合并请求的对象，并且它有一个与之关联的状态（打开、关闭等）。我们使用枚举来表示这些状态，因为在那时，它们只是保存特定状态的字符串表示的数据。如果它们需要有一些行为，或者整个合并请求需要根据其状态和转换执行一些操作，这是不够的。

我们正在向代码的一部分添加行为，一个运行时结构，这让我们必须以对象的方式思考，因为毕竟这就是对象应该做的。这就是具体化的意义——现在状态不能只是一个带有字符串的枚举；它需要是一个对象。

想象一下，我们必须向合并请求添加一些规则，比如说，当它从打开状态变为关闭状态时，所有的批准都被移除（他们将不得不重新审查代码）——当合并请求刚刚打开时，批准的数量被设置为零（无论是重新打开的还是全新的合并请求）。另一个规则可能是，当合并请求被合并时，我们希望删除源分支，当然，我们还希望禁止用户执行无效的转换（例如，关闭的合并请求不能被合并等）。

如果我们把所有这些逻辑都放在一个地方，即`MergeRequest`类中，我们最终会得到一个责任很多（设计不佳）、可能有很多方法和非常多的`if`语句的类。很难跟踪代码并理解哪一部分应该代表哪个业务规则。

最好将这些分布到更小的对象中，每个对象负责更少的责任，状态对象是一个很好的地方。我们为要表示的每种状态创建一个对象，并在它们的方法中放置与上述规则的转换逻辑。然后，`MergeRequest`对象将有一个状态协作者，而这个协作者也将了解`MergeRequest`（需要双重分派机制来在`MergeRequest`上运行适当的操作并处理转换）。

我们定义一个基本的抽象类，其中包含要实现的方法集，然后为我们要表示的每种特定`state`创建一个子类。然后，`MergeRequest`对象将所有操作委托给`state`，如下面的代码所示：

```py
class InvalidTransitionError(Exception):
    """Raised when trying to move to a target state from an unreachable 
    source
    state.
    """

class MergeRequestState(abc.ABC):
    def __init__(self, merge_request):
        self._merge_request = merge_request

    @abc.abstractmethod
    def open(self):
        ...

    @abc.abstractmethod
    def close(self):
        ...

    @abc.abstractmethod
    def merge(self):
        ...

    def __str__(self):
        return self.__class__.__name__

class Open(MergeRequestState):
    def open(self):
        self._merge_request.approvals = 0

    def close(self):
        self._merge_request.approvals = 0
        self._merge_request.state = Closed

    def merge(self):
        logger.info("merging %s", self._merge_request)
        logger.info("deleting branch %s", 
        self._merge_request.source_branch)
        self._merge_request.state = Merged

class Closed(MergeRequestState):
    def open(self):
        logger.info("reopening closed merge request %s", 
         self._merge_request)
        self._merge_request.state = Open

    def close(self):
        pass

    def merge(self):
        raise InvalidTransitionError("can't merge a closed request")

class Merged(MergeRequestState):
    def open(self):
        raise InvalidTransitionError("already merged request")

    def close(self):
        raise InvalidTransitionError("already merged request")

    def merge(self):
        pass

class MergeRequest:
    def __init__(self, source_branch: str, target_branch: str) -> None:
        self.source_branch = source_branch
        self.target_branch = target_branch
        self._state = None
        self.approvals = 0
        self.state = Open

    @property
    def state(self):
        return self._state

    @state.setter
    def state(self, new_state_cls):
        self._state = new_state_cls(self)

    def open(self):
        return self.state.open()

    def close(self):
        return self.state.close()

    def merge(self):
        return self.state.merge()

    def __str__(self):
        return f"{self.target_branch}:{self.source_branch}"
```

以下列表概述了一些关于实现细节和设计决策的澄清：

+   状态是一个属性，因此不仅是公共的，而且有一个单一的地方定义了如何为合并请求创建状态，将`self`作为参数传递。

+   抽象基类并不是严格需要的，但拥有它也有好处。首先，它使我们正在处理的对象类型更加明确。其次，它强制每个子状态实现接口的所有方法。对此有两种替代方案：

+   我们本可以不放置方法，让`AttributeError`在尝试执行无效操作时引发，但这是不正确的，也不能表达发生了什么。

+   与这一点相关的是，我们本可以只使用一个简单的基类并留下那些方法为空，但是这样做的默认行为并不清楚应该发生什么。如果子类中的某个方法应该什么都不做（如合并的情况），那么最好让空方法保持原样，并明确表示对于特定情况，不应该做任何事情，而不是强制所有对象都遵循这个逻辑。

+   `MergeRequest`和`MergeRequestState`彼此之间有链接。一旦进行转换，前一个对象将不再有额外的引用，应该被垃圾回收，因此这种关系应该始终是 1:1。在一些小而更详细的考虑中，可以使用弱引用。

以下代码显示了如何使用对象的一些示例：

```py
>>> mr = MergeRequest("develop", "master") 
>>> mr.open()
>>> mr.approvals
0
>>> mr.approvals = 3
>>> mr.close()
>>> mr.approvals
0
>>> mr.open()
INFO:log:reopening closed merge request master:develop
>>> mr.merge()
INFO:log:merging master:develop
INFO:log:deleting branch develop
>>> mr.close()
Traceback (most recent call last):
...
InvalidTransitionError: already merged request
```

状态转换的操作被委托给`MergeRequest`始终持有的`state`对象（这可以是`ABC`的任何子类）。它们都知道如何以不同的方式响应相同的消息，因此这些对象将根据每个转换采取相应的操作（删除分支、引发异常等），然后将`MergeRequest`移动到下一个状态。

由于`MergeRequest`将所有操作委托给其`state`对象，我们会发现每次需要执行的操作都是`self.state.open()`这种形式。我们能否删除一些样板代码？

我们可以通过`__getattr__()`来实现，如下面的代码所示：

```py
class MergeRequest:
    def __init__(self, source_branch: str, target_branch: str) -> None:
        self.source_branch = source_branch
        self.target_branch = target_branch
        self._state: MergeRequestState
        self.approvals = 0
        self.state = Open

    @property
    def state(self):
        return self._state

    @state.setter
    def state(self, new_state_cls):
        self._state = new_state_cls(self)

    @property
    def status(self):
        return str(self.state)

    def __getattr__(self, method):
        return getattr(self.state, method)

    def __str__(self):
        return f"{self.target_branch}:{self.source_branch}"
```

一方面，我们重用一些代码并删除重复的行是好事。这使得抽象基类更有意义。在某个地方，我们希望将所有可能的操作记录下来，列在一个地方。那个地方过去是`MergeRequest`类，但现在这些方法都消失了，所以唯一剩下的真相来源是`MergeRequestState`。幸运的是，`state`属性上的类型注解对用户来说非常有帮助，可以知道在哪里查找接口定义。

用户可以简单地查看并了解`MergeRequest`没有的所有内容都将要求其`state`属性具有。从`init`定义中，注解会告诉我们这是`MergeRequestState`类型的对象，并通过查看此接口，我们将看到我们可以安全地要求其`open()`、`close()`和`merge()`方法。

# 空对象模式

空对象模式是与本书前几章提到的良好实践相关的一个想法。在这里，我们正在正式化它们，并为这个想法提供更多的背景和分析。

原则相当简单——函数或方法必须返回一致类型的对象。如果这得到保证，那么我们代码的客户端可以使用返回的对象进行多态，而无需对它们进行额外的检查。

在之前的例子中，我们探讨了 Python 的动态特性如何使大多数设计模式变得更容易。在某些情况下，它们完全消失，在其他情况下，它们更容易实现。设计模式最初的目标是，方法或函数不应该明确命名它们需要的对象的类。因此，它们提出了创建接口和重新排列对象的方法，使它们适应这些接口以修改设计。但在 Python 中大多数情况下，这是不需要的，我们可以只传递不同的对象，只要它们遵守必须具有的方法，解决方案就会起作用。

另一方面，对象不一定要遵守接口的事实要求我们更加小心，以确保从这些方法和函数返回的东西。就像我们的函数没有对它们接收到的东西做出任何假设一样，可以合理地假设我们代码的客户也不会做出任何假设（我们有责任提供兼容的对象）。这可以通过契约式设计来强制执行或验证。在这里，我们将探讨一种简单的模式，可以帮助我们避免这些问题。

考虑在前一节中探讨的责任链设计模式。我们看到了它有多么灵活以及它的许多优点，比如将责任解耦为更小的对象。它存在的问题之一是，我们实际上永远不知道哪个对象最终会处理消息，如果有的话。特别是在我们的例子中，如果没有合适的对象来处理日志行，那么该方法将简单地返回`None`。

我们不知道用户将如何使用我们传递的数据，但我们知道他们期望得到一个字典。因此，可能会发生以下错误：

```py
AttributeError: 'NoneType' object has no attribute 'keys'
```

在这种情况下，修复方法相当简单——`process()`方法的默认值应该是一个空字典，而不是`None`。

确保返回一致类型的对象。

但是，如果该方法没有返回字典，而是我们领域的自定义对象呢？

为了解决这个问题，我们应该有一个代表该对象的空状态的类并返回它。如果我们有一个代表系统中用户的类，并且有一个按 ID 查询用户的函数，那么在找不到用户的情况下，它应该执行以下两种操作之一：

+   引发异常

+   返回一个`UserUnknown`类型的对象

但在任何情况下，它都不应该返回`None`。短语`None`并不代表刚刚发生的事情，调用者可能会合理地尝试向其请求方法，但会因为`AttributeError`而失败。

我们之前讨论过异常及其利弊，所以我们应该提到这个`null`对象应该只有与原始用户相同的方法，并对每个方法都不做任何操作。

使用这种结构的优势不仅在于我们在运行时避免了错误，而且这个对象可能是有用的。它可以使代码更容易测试，甚至可以帮助调试（也许我们可以在方法中放置日志以了解为什么达到了这种状态，提供了什么数据等）。

通过利用 Python 的几乎所有魔术方法，可以创建一个绝对什么都不做的通用`null`对象，无论如何调用它，但几乎可以从任何客户端调用。这样的对象略微类似于`Mock`对象。不建议走这条路，因为有以下原因：

+   它失去了与领域问题的意义。回到我们的例子中，拥有`UnknownUser`类型的对象是有意义的，并且让调用者清楚地知道查询出了问题。

+   它不尊重原始接口。这是有问题的。请记住，`UnknownUser`是一个用户，因此它必须具有相同的方法。如果调用者意外地要求不存在的方法，那么在这种情况下，它应该引发`AttributeError`异常，这是好的。使用通用的`null`对象，它可以做任何事情并对任何事情做出响应，我们将丢失这些信息，可能会引入错误。如果我们选择创建一个带有`spec=User`的`Mock`对象，那么这种异常将被捕获，但再次使用`Mock`对象来表示实际上是一个空状态的东西会损害代码的意图。

这种模式是一个很好的实践，它允许我们在对象中保持多态性。

# 关于设计模式的最终想法

我们已经在 Python 中看到了设计模式的世界，并且在这样做时，我们找到了解决常见问题的解决方案，以及更多的技术，这些技术将帮助我们实现一个清晰的设计。

这些听起来都不错，但问题是，设计模式有多好呢？有人认为它们弊大于利，认为它们是为了那些类型系统有限（和缺乏一流函数）的语言而创建的，这些语言无法完成我们通常在 Python 中完成的事情。还有人声称设计模式强制了设计解决方案，产生了一些限制本来会出现的设计的偏见，而且本来会更好。让我们依次看看这些观点。

# 设计对设计的影响

设计模式，就像软件工程中的任何其他主题一样，本身并不是好坏之分，而是取决于它的实现方式。在某些情况下，实际上并不需要设计模式，一个更简单的解决方案就可以。试图在不适合的地方强行使用模式是一种过度设计的情况，这显然是不好的，但这并不意味着设计模式有问题，而且在这些情况下，问题很可能根本与模式无关。有些人试图过度设计一切，因为他们不理解灵活和适应性软件的真正含义。正如我们在本书中之前提到的，制作好的软件并不是关于预测未来的需求（进行未来学是没有意义的），而只是解决我们目前手头的问题，以一种不会阻止我们在将来对其进行更改的方式。它不必现在就处理这些变化；它只需要足够灵活，以便将来可以进行修改。当未来到来时，我们仍然必须记住三个或更多相同问题的实例才能提出通用解决方案或适当的抽象。

这通常是设计模式应该出现的时候，一旦我们正确识别了问题并能够识别模式并相应地抽象出来。

让我们回到模式与语言适应性的话题。正如我们在本章的介绍中所说，设计模式是高层次的想法。它们通常指的是对象及其相互作用的关系。很难想象这些东西会从一种语言消失到另一种语言。有些模式实际上是在 Python 中手动实现的，比如迭代器模式（正如本书前面大量讨论的那样，在 Python 中内置了迭代器模式），或者策略（因为我们可以像传递其他常规对象一样传递函数；我们不需要将策略方法封装到一个对象中，函数本身就是一个对象）。

但其他模式实际上是需要的，它们确实解决了问题，比如装饰器和组合模式。在其他情况下，Python 本身实现了设计模式，我们并不总是看到它们，就像我们在`os`部分讨论的外观模式一样。

至于我们的设计模式是否会导致我们的解决方案走向错误方向，我们在这里必须小心。再次强调，最好的做法是从领域问题的角度开始设计解决方案，创建正确的抽象，然后再看是否有设计模式从该设计中出现。假设确实有。那是一件坏事吗？已经有一个解决我们正在尝试解决的问题的解决方案这个事实不能是一件坏事。重复造轮子是坏事，这在我们的领域中经常发生。此外，应用一种已经被证明和验证的模式，应该让我们对我们正在构建的东西的质量更有信心。

# 我们模型中的名称

我们在代码中是否应该提到我们正在使用设计模式？

如果设计良好，代码干净，它应该自说明。不建议您根据您使用的设计模式来命名事物，原因有几个：

+   我们代码的用户和其他开发人员不需要知道代码背后的设计模式，只要它按预期工作即可。

+   说明设计模式会破坏意图揭示原则。在类名中添加设计模式的名称会使其失去部分原始含义。如果一个类代表一个查询，它应该被命名为`Query`或`EnhancedQuery`，以显示该对象应该执行的意图。 `EnhancedQueryDecorator`没有任何有意义的含义，`Decorator`后缀会带来更多混乱而不是清晰。

在文档字符串中提到设计模式可能是可以接受的，因为它们作为文档，并且在我们的设计中表达设计思想（再次交流）是一件好事。然而，这并不是必要的。大多数情况下，我们不需要知道设计模式在那里。

最好的设计是那些设计模式对用户完全透明的设计。一个例子是外观模式如何出现在标准库中，使用户完全透明地访问`os`模块。更优雅的例子是迭代器设计模式如何被语言完全抽象化，以至于我们甚至不必考虑它。

# 总结

设计模式一直被视为常见问题的成熟解决方案。这是一个正确的评估，但在本章中，我们从良好设计技术的角度探讨了它们，这些模式利用了干净的代码。在大多数情况下，我们看到它们如何提供了保留多态性、减少耦合和创建正确的抽象以封装所需细节的良好解决方案。所有这些特征都与第八章中探讨的概念相关，即“单元测试和重构”。

然而，设计模式最好的地方不是我们可以从应用它们中获得的干净设计，而是扩展的词汇。作为一种交流工具，我们可以使用它们的名称来表达我们设计的意图。有时，我们不需要应用整个模式，而是可能需要从我们的解决方案中采用特定的想法（例如子结构），在这里，它们也被证明是更有效地交流的一种方式。

当我们通过模式思考解决问题时，我们是在更一般的层面上解决问题。以设计模式思考，使我们更接近更高级别的设计。我们可以慢慢“放大”并更多地考虑架构。现在我们正在解决更一般的问题，是时候开始考虑系统如何在长期内发展和维护（如何扩展、改变、适应等）。

要使软件项目在这些目标中取得成功，它需要以干净的代码为核心，但架构也必须是干净的，这是我们将在下一章中讨论的内容。

# 参考资料

这里是一些你可以参考的信息列表：

+   *GoF*：由 Erich Gamma、Richard Helm、Ralph Johnson 和 John Vlissides 撰写的书籍，名为*设计模式：可复用面向对象软件的元素*

+   *SNGMONO*：一篇由 Robert C. Martin 于 2002 年撰写的文章，名为*SINGLETON and MONOSTATE*

+   《空对象模式》，作者 Bobby Woolf
