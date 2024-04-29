# 第六章：构建一个实用的去中心化应用程序

在本章中，我们将在区块链上编写一个流行的应用程序，这将是一个由区块链驱动的安全投票应用程序。您有所有开发此应用程序的工具，即 populus 和`web3.py`。

以下是本章将涵盖的主题：

+   开发一个简单的投票应用

+   了解智能合约中的事件

+   开发一个商业投票应用程序

+   开发基于令牌的投票应用

+   讨论另一种类型的投票应用程序

# 开发一个简单的投票应用

首先，我们将构建最简单的投票应用程序，比 Vyper 软件源代码中提供的投票应用程序示例还要简单。让我们设置我们的 Populus 项目目录：

[PRE0]

然后，通过将键编译的值更改为以下内容，将 Vyper 支持添加到`project.json`中：

[PRE1]

Vyper 的最新版本是 0.1.0b6，这破坏了 Populus。开发人员需要一些时间来解决这个问题。如果到您阅读本书时错误仍未被修复，您可以自行修补 Populus。

首先，使用以下命令检查错误是否已修复：

[PRE2]

在我们的案例中，错误尚未被修复。因此，让我们修补 Populus 以修复错误。确保您仍然在相同的目录（`voting-venv/src/populus`）中：

[PRE3]

现在，在`contracts`目录中创建一个简单的投票智能合约。命名为`SimpleVoting.vy`。*请参考以下 GitLab 链接获取完整代码 - [`gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/contracts/SimpleVoting.vy`](https://gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/contracts/SimpleVoting.vy)：

[PRE4]

让我们讨论这个简单的投票智能合约。它受到 Vyper 源代码中投票示例的启发，但这个示例甚至更简化。原始示例具有委托功能，这将使事情难以理解。我们从结构数据类型变量声明开始：

[PRE5]

数据结构是一个具有复合数据类型的变量，其中包含提案的名称和提案的金额。`Proposal`结构中的`vote_count`数据类型为`int128`，而`Proposal`结构中的`name`数据类型为`bytes32`。您也可以使用`uint256`而不是`int128`数据类型来表示`Proposal`结构中的`vote_count`。不过，这不会有任何区别。但是，`bytes32`是一个新的数据类型。正如您可能还记得的那样，如果要在 Vyper 中使用字符串（或字节数组）数据类型，如果该字符串的长度小于 20，则使用`bytes[20]`。

bytes32 是另一种类似于`bytes[32]`的字符串数据类型，但有一个特殊之处；如果您将`b'messi'`字符串设置为具有`bytes[32]`类型的变量，并使用`web3`检索它，您将得到`b'messi'`。但是，如果您将`b'messi'`字符串设置为具有`bytes32`类型的变量，并使用`web3`检索它，您将得到`b'messi\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'`。此字符串将填充直到达到 32 字节。默认情况下，您应该使用`bytes[20]`或`bytes[256]`作为字符串数据类型，而不是使用`bytes32`。那么为什么我在这个智能合约中使用`bytes32`？我有一个很好的理由这样做，但我们需要先继续到构造函数以了解我为什么使用`bytes32`来保存提案的名称：

[PRE6]

这是我们第一次在智能合约中使用事件。`event`是 Vyper 中的一个关键字，用于创建事件。事件是智能合约内发生的事情，我们的客户端（`web3`程序）希望订阅。在这个语句中，`Voting`是事件的名称，有两个参数。第一个参数是`_from`，类型为`address`。`indexed`用于使用`_from`作为过滤器进行事件过滤。第二个参数是`_proposal`，类型为`int128`。记住，`int128`是一个 128 位整数。当我们在客户端程序中订阅它时，这个事件将变得更清晰。现在，让我们继续下一个：

[PRE7]

这个变量是一个映射数据类型变量，将一个`int128`数据类型变量映射到一个`Proposal`结构变量。基本上，这是一个提案列表：

[PRE8]

这是一个辅助变量，用来计算这个智能合约中有多少提案：

[PRE9]

这用于检查一个账户是否已经投票。我们不希望一个账户对同一个提案投票多次。记住，这是一个映射数据类型。默认情况下，不存在的值指向一个空值。在`int128`的上下文中，空值是`0`：

[PRE10]

这个构造函数有一个参数，是一个`bytes32`数组。在构造函数内部，它将迭代两次（我们将提案的数量硬编码为两个）。每次迭代都会将一个新成员设置到`proposals`映射变量中。`name`是从参数中设置的，`vote_count`初始化为 0。然后，对于每次迭代，`proposals_count`都会增加 1。

我之所以使用`bytes32`作为提案名称的数据类型，是因为如果我使用`bytes[128]`作为提案名称的数据类型，我无法将其作为参数发送。

Vyper 编程语言中的智能合约方法不能接受嵌套数组，比如`bytes[128][2]`作为参数（至少在 Vyper 的最新版本中是这样）：

[PRE11]

这是投票的函数。它接受一个名为`proposal`的参数。在这里，用户用一个整数为提案投票。所以，如果用户使用`0`这样的参数调用`vote`方法，比如`vote(0)`，这意味着用户为第一个提案投票。当你设计自己的投票智能合约时，当然你也可以使用字符串来投票，比如`vote(b'proposal1')`。在这里，我使用整数来简化事情。

在这个函数中，我们断言选民还没有使用这个语句投票：`assert self.voters_voted[msg.sender] == 0`。投票后，我们将`voters_voted`的值设置为选民的地址作为键的`1`：`self.voters_voted[msg.sender] = 1`。我们还通过检查投票的值是否小于提案的数量（即`2`）来验证投票是否有效。这个函数的关键是以下语句：`self.proposals[proposal].vote_count += 1`。在这个函数的结尾，我们的`Voting`事件在这个语句中被使用：`log.Voting(msg.sender, proposal)`。这类似于广播发生了重要的事情——嘿，世界！有一个`Voting`事件，有两个参数，`msg.sender`作为`address`参数，`proposal`作为`int128`参数。然后，任何订阅了这个事件的人都会收到通知。事件的订阅发生在客户端，使用`web3`库，如下面的代码所示：

[PRE12]

这个私有函数设计用来检查哪个提案获得了最多的投票：

[PRE13]

这个`public`函数设计用来获取获得最多票数的提案的名称。这个函数使用了前面描述的私有函数。

这个智能合约很简单，但并不完美，因为存在一个错误。例如，在`vote`函数中，我们没有处理投票的负值。此外，提案的数量是硬编码为 2。但是，它可以完成工作。

然后，你可以像通常一样编译智能合约的代码：

[PRE14]

作为一个好公民，让我们为这个智能合约编写一个测试。在`tests`目录中创建一个名为`test_simple_voting_app.py`的文件。参考以下 GitLab 链接获取以下代码块的完整代码：[`gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/tests/test_simple_voting_app.py`](https://gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/tests/test_simple_voting_app.py)：

[PRE15]

让我们逐个讨论这个测试：

[PRE16]

因为我们简单的投票智能合约的构造函数需要一个参数，所以我们需要在测试中使用一个 fixture，如第五章*Populus 开发框架*中所讨论的那样。然后，我们的 fixture 可以作为测试方法的参数使用：

[PRE17]

这是为了检查部署后智能合约的状态。这里有一件非常独特的事情；在提案变量内的结构数据的名称变量的长度是`32`，即使我们将其设置为值`b'messi'`，这就是`bytes32`数据类型的特殊之处。这就是为什么我们要切片变量以获取我们想要的内容。然后，对于下一个测试方法，我们使用`chain`参数以及`voting`参数：

[PRE18]

这用于测试`vote`函数。我们测试`vote`函数是否确实改变了`proposals`变量的`vote_count`属性：

[PRE19]

这确保了我们不能使用同一个账户投票超过一次。正如我们在`第五章`*Populus 开发框架*中学到的那样，您可以使用`pytest.raises with`语句将失败的情况包装起来。最后一个测试用例是检查获胜的提案：

[PRE20]

在这个测试中，您可以使用`t.get_accounts`辅助方法使用三个账户。

# 部署一个构造函数中带有参数的智能合约

让我们将这个智能合约部署到以太坊区块链。然而，我们首先需要意识到有一些事情会使情况复杂化。首先，`event`在 Ganache 中不起作用，因此我们必须将其部署到 Rinkeby 网络或私有以太坊区块链。其次，我们的智能合约在构造函数中有一个参数。要部署带有参数的智能合约，我们需要使用不同的方法；我们不能像在`第五章`*Populus 开发框架*中演示的那样使用普通方法。在[第五章](https://cdp.packtpub.com/hands_on_blockchain_for_python_developers/wp-admin/_wp_link_placeholder)*Populus 开发框架*中，我们使用 Populus 以这种方式部署了一个智能合约[:](https://cdp.packtpub.com/hands_on_blockchain_for_python_developers/wp-admin/post.php?post=29&action=edit#post_28)[`populus deploy --chain localblock Donation`.](https://cdp.packtpub.com/hands_on_blockchain_for_python_developers/wp-admin/post.php?post=29&action=edit#post_28)

Populus 方法只能部署一个没有参数的构造函数的智能合约。让我们一一克服这些障碍。我们需要做的第一件事是将其部署到私有以太坊区块链，如下所示：

1.  在`voting_project`目录中，运行以下命令：

[PRE21]

1.  然后，使用`init_chain.sh`脚本初始化私有链：

[PRE22]

1.  编辑`chains/localblock/run_chain.sh`并将`--ipcpath`标志的值更改为`/tmp/geth.ipc`。然后，运行区块链：

[PRE23]

1.  现在，编辑`project.json`文件。`chains`键有一个对象，其中有 4 个键：`tester`，`temp`，`ropsten`和`mainnet`。在这个对象中添加一个名为`localblock`的键及其值：

[PRE24]

运行区块链需要一个专用的终端。因此，打开一个新的终端，执行一个虚拟环境脚本，然后进入`voting_project`目录。创建这个文件并命名为`deploy_SmartVoting.py`：

[PRE25]

现在，让我们讨论一下这个程序的作用：

[PRE26]

我们从`populus`库中导入工具，`Project`代表`project.json`配置文件。`wait_for_transaction_receipt`是一个等待我们的交易在以太坊区块链中确认的函数：

[PRE27]

在`main`函数中，我们初始化了一个`Project`实例，然后获取了`localblock`链：

[PRE28]

`chain`对象现在代表`project.json`文件中的这个`json`对象。

我们从`build/contracts.json`中获取`SimpleVoting`智能合约工厂：

[PRE29]

然后，我们将我们的智能合约部署到私有以太坊区块链上：

[PRE30]

它接收两个关键字参数，`transaction`和`args`。`transaction`参数是一个交易字典。在这里，我们设置了`from`参数。`chain.web3.eth.coinbase`是我们的默认账户，在`testing/development`场景中很常见。在这里，我们使用默认账户而不使用私钥。在这个交易对象中，我们还可以设置`gas`、`gasPrice`和其他交易参数。`args`关键字参数允许我们向智能合约的构造函数发送参数。它是一个嵌套数组，`[[b'Messi', b'Ronaldo']]`，因为内部数组是智能合约构造函数中的`_proposalNames`参数。

外部数组被设计用来封装构造函数中的其他参数，但在这种情况下我们只有一个参数：

[PRE31]

我们等待交易确认。然后，我们从部署过程中获取智能合约的地址：

[PRE32]

`receipt`对象是区块链中描述交易确认的对象。在这种情况下，我们关心的是地址，也就是`receipt`对象中的`contractAddress`键：

[PRE33]

这是为了执行`main`函数而设计的。

不像 Ganache，那里有 10 个账户（每个账户都有 100 个以太币），在 Populus 的默认设置下的私有以太坊区块链中，你只有一个账户，配备了 1 万亿以太币！以下脚本允许你查看默认账户有多少以太币：

[PRE34]

在这个智能合约中，我们想要使用多个账户来玩我们的智能合约。所以让我们在这个以太坊私有区块链中创建 10 个账户。在`voting_project`目录中创建一个新文件，命名为`create_10_accounts_on_private_chain.py`。

[PRE35]

我们将新账户的地址写入文件，以便以后重复使用。你需要注意的函数是`w3.personal.newAccount('password123')`。这将给你公共地址。私钥将使用`password123`进行加密。这将保存在`chains/localblock/chain_data/keystore`目录中。加密文件的名称类似于`UTC—2018-10-26T13-13-25.731124692Z—36461a003a03f857d60f5bd0b8e8a64aab4e4535`。文件名的结尾部分是`public`地址。在这个文件名的示例中，`public`地址是`36461a003a03f857d60f5bd0b8e8a64aab4e4535`。执行这个脚本。10 个账户的`public`地址将被写入`10_accounts.txt`文件。

如果你查看`chains/localblock/chain_data/keystore`目录，你会看到至少 11 个文件。

这 10 个新账户中的每一个都配备了 0 个以太币。要在我们的智能合约中投票，你不能有空余的余额。那么，为什么我们不把默认账户的钱分发给这 10 个账户呢？在`voting_project`目录下创建一个文件，命名为`distribute_money.py`。参考以下 GitLab 链接中的代码文件获取完整的代码：[`gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/distribute_money.py`](https://gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/distribute_money.py)：

[PRE36]

现在，让我们逐行讨论这个脚本：

[PRE37]

您已经了解了`Web3`，`IPCProvider`*，*和`wait_for_transaction_receipt`。`glob`来自 Python 标准库。它的目的是从目录中过滤文件：

[PRE38]

我们使用套接字连接到以太坊节点：

[PRE39]

这是我们默认账户的地址。您怎么知道？您可以在连接到这个私有以太坊区块链的脚本中使用`w3.eth.coinbase`找到它，或者您可以查看`chains/localblock/chain_data/keystore`目录中的文件名。在初始化和运行私有以太坊区块链后，只有一个文件名。现在，在您初始化另外 10 个账户后，文件的数量自然会变成 11：

[PRE40]

解锁默认账户的密码存储在`chains/localblock/password`的纯文本文件中：

[PRE41]

找到这个之后，我们使用`w3.eth.account.decrypt`方法解密加密文件：

[PRE42]

这是为了避免在创建交易时提供`from`参数的义务：

[PRE43]

我们打开了`10_accounts.txt`，里面包含了我们拥有的所有新账户，然后我们一个一个地迭代这些账户：

[PRE44]

在将其提供给交易对象之前，我们使用`w3.eth.getTransactionCount`检查最新的 nonce 值。交易对象有`to`，`value`，`gas`和`gasPrice`，以及`nonce`键。在这里，我们想给每个账户发送 10 个以太币：

[PRE45]

我们用我们的私钥对交易进行签名，然后使用`w3.eth.sendRawTransaction`方法将交易广播给矿工：

[PRE46]

这很重要。如果您只向一个账户发送资金，您可以跳过它。但是，由于我们按顺序广播了 10 笔交易，您必须在广播下一笔交易之前等待每笔交易先确认。

这样想：您广播了一个 nonce 为 3 的发送 10 个以太币的交易，然后矿工需要时间来确认这笔交易。但是，在短时间内，您广播了一个新的 nonce 为 4 的交易。得到这笔交易的矿工会向您抱怨，因为您试图从 nonce 2 跳到 nonce 4。请记住，nonce 3 的交易需要时间来确认。

执行文件后，您可以检查您的 10 个账户每个都有 10 个以太币。

让我们基于智能合约创建我们的简单去中心化投票应用。离开`voting_project`，创建一个新目录来包含我们的应用。创建目录后，输入以下内容：

[PRE47]

让我们创建一个订阅`Voting`事件的程序。将此文件命名为`watch_simple_voting.py`：

[PRE48]

现在，让我们逐行讨论这个程序：

[PRE49]

我们需要`abi`来连接到一个智能合约。您可以从智能合约的编译中获得这个。由于`abi`是一个`json`对象，其中有一个布尔值设置为`true`和`false`，而 Python 的布尔值是`True`和`False`（注意大写），我们需要调整它：

[PRE50]

要连接到一个智能合约，您需要一个地址。这是部署脚本中的地址。您也可以将地址设置为代码中硬编码的地址，如下所示：

[PRE51]

但是，我更喜欢把它放在一个外部文件中：

[PRE52]

这是为了创建一个订阅`SimpleVoting`智能合约的`Voting`事件。语法如下：

[PRE53]

`fromBlock`是历史指针。块越低，历史越早：

[PRE54]

然后，我们订阅投票事件。您会得到类似于这样的东西：

[PRE55]

让这个脚本运行。不要退出应用程序。打开一个新的终端，执行我们的虚拟环境脚本，并进入`voting_dapp`项目。这样做后，创建一个新的脚本，并将其命名为`simple_voting_client.py`。参考以下 GitLab 链接中的代码文件获取完整的代码：[`gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_dapp/simple_voting_client.py`](https://gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_dapp/simple_voting_client.py)：

[PRE56]

现在，让我们逐行讨论这个。我们从脚本的顶部开始：

[PRE57]

这里的逻辑与之前的脚本相同。您首先使用`password123`打开加密文件。然后在`client_address.txt`文件中设置选民的账户地址，以使此脚本灵活。您可以在脚本中硬编码选民的账户地址：

[PRE58]

在这里，您以通常的方式从智能合约编译中设置`abi`：

[PRE59]

请记住，在这个脚本中有两个地址。第一个是选民或客户的地址。第二个是智能合约的地址。然后，您需要获取 nonce：

[PRE60]

在构建交易时，您使用此 nonce：

[PRE61]

这是`vote`函数。在这里，我们为索引为`0`的提案投票，即`b'messi'`。您提交`gas`、`gasPrice`和`nonce`，并且省略`from`，因为您已经设置了`w3.eth.defaultAccount`：

[PRE62]

最后几行是用于签名和广播交易的。

执行脚本，然后转到您运行`watch_simple_voting.py`脚本的终端。然后您会得到类似于这样的东西：

[PRE63]

就是这样。在实际应用中，此事件可用于在分散式应用程序中提供通知。然后，您可以更新投票的排名或其他任何您喜欢的内容。

您还可以从一开始获取所有事件。还记得获取事件的代码吗？如下所示：

[PRE64]

您可以使用`get_all_entries`而不是`get_new_entries`来检索从一开始的所有事件，如下所示：

[PRE65]

# 开发商业投票应用程序

让我们将我们的智能合约升级为商业智能合约。为了投票，选民需要支付一小笔钱。这类似于美国偶像，人们通过短信投票来决定谁获胜。

返回`voting_project`目录，打开`contracts`目录中的新文件，命名为`CommercialVoting.vy`。有关此代码块的完整代码，请参考以下 GitLab 链接中的代码文件：[`gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/contracts/CommercialVoting.vy`](https://gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/contracts/CommercialVoting.vy)：

[PRE66]

这个智能合约类似于`SimpleVoting.vy`，但具有额外的支付功能。我们不会逐行讨论它，但我们将看一下之前的智能合约和这个之间的区别：

[PRE67]

在这个构造函数中，我们保存了启动智能合约的账户的地址：

[PRE68]

在这个`vote`函数中，我们添加了`@payable`装饰器，以便人们在想要投票时可以发送资金。除此之外，我们要求最低支付为`0.01`以太币，使用此语句：`assert msg.value >= as_wei_value(0.01, "ether")`：

[PRE69]

当然，我们必须创建一个从智能合约中提取以太币的函数。在这里，我们将以太币发送到经理账户。

现在，让我们继续测试智能合约。在`tests`目录中创建测试文件，命名为`test_commercial_voting.py`。有关完整代码，请参考以下 GitLab 链接中的代码文件：[`gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/tests/test_commercial_voting.py`](https://gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/tests/test_commercial_voting.py)：

[PRE70]

让我们逐个讨论测试函数：

[PRE71]

这是为了测试经理变量是否指向启动智能合约的账户。请记住，`web3.eth.coinbase`是默认账户。测试投票是否需要一定数量的以太币和账户，我们可以从`t.get_accounts()`中获取：

[PRE72]

这是为了测试您可以在`vote`函数中发送以太币。您还测试了在智能合约中累积的以太币的余额：

[PRE73]

这是为了测试在您想要投票时，您需要发送至少`0.01`以太币：

[PRE74]

这是这个智能合约中最重要的测试之一。它旨在测试您是否可以正确地从智能合约中提取以太币。您可以在提取前后检查余额，并确保差额大约为 1 个以太币（因为您需要支付燃气费）。

# 开发基于代币的投票应用

现在，让我们在区块链上开发一个基于代币的投票应用。我所说的基于代币的投票是指，为了投票，您必须拥有在智能合约中创建的代币。如果您用这个代币投票，那么这个代币就会被销毁，这意味着您不能投两次票。在这个智能合约中，代币的数量也是有限的，不像之前的投票应用程序，无限的账户可以投票。让我们在`contracts`目录中编写一个智能合约，并将文件命名为`TokenBasedVoting.vy`。请参考以下 GitLab 链接中的代码文件获取完整代码：[`gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/contracts/TokenBasedVoting.vy`](https://gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/contracts/TokenBasedVoting.vy)：

[PRE75]

让我们逐行讨论这个脚本：

[PRE76]

您已经熟悉了`proposals`变量，它与之前的投票应用具有相同的目的。`token`是一个新变量，旨在跟踪代币的所有者。`index`和`maximum_token`是用来计算我们分配了多少个代币的变量。请记住，我们希望限制代币的数量。`manager`是启动智能合约的人：

[PRE77]

在构造函数中，在设置`proposals`变量之后，我们将`index`初始化为`0`，`maximum_token`初始化为`8`。在这个智能合约中只有`8`个代币可用，这意味着只能尝试`8`次投票。`manager`变量初始化为启动智能合约的变量：

[PRE78]

在这个函数中，所有者可以将代币分配给任何账户。为了指示代币的所有者，我们将`true`值设置给`token`变量，并将其键指向`target`。`index`增加了一，所以以后我们不能创建超过`maximum_token`变量的代币：

[PRE79]

在这个`vote`函数中，我们通过将`token`映射变量设置为投票者的地址键的`false`来销毁代币。但首先，我们必须确保投票者是代币的有效所有者，使用这个语句：`assert self.token[msg.sender]`。我们还必须确保在分配了所有代币之后人们可以投票。当然，就像之前的投票应用程序一样，我们增加了投票者投票的提案的计数。

让我们为基于代币的投票应用创建一个测试。为此，在`tests`目录中创建一个名为`test_token_based_voting.py`的文件。请参考以下 GitLab 链接中的代码文件获取完整代码：[`gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/tests/test_token_based_voting.py`](https://gitlab.com/arjunaskykok/hands-on-blockchain-for-python-developers/blob/master/chapter_06/voting_project/tests/test_token_based_voting.py)。将以下代码添加到新文件中：

[PRE80]

让我们逐行讨论这个脚本。我们从`fixture`函数开始：

[PRE81]

像往常一样，我们通过手动部署智能合约来创建这个智能合约的`fixture`：

[PRE82]

这是一个为不同账户分配`8`个代币的`helper`函数：

[PRE83]

这个`test`函数旨在检查`assign_token`函数是否可以将代币分配给目标地址：

[PRE84]

这个`test`函数旨在确保只有代币的所有者可以在这个智能合约中投票：

[PRE85]

这个`test`函数旨在确保代币的所有者可以成功为提案投票。

让我解释一下为什么基于代币的投票非常了不起。只有`8`个可用的代币，这些代币可以用来在这个智能合约中投票。编写和部署这个智能合约的程序员甚至在这个智能合约上线后也无法改变规则。选民可以通过要求从程序员那里获取智能合约的源代码，并验证编译的字节码是否与智能合约地址中的字节码相同来验证规则是否公平。要从智能合约地址获取字节码，你可以这样做：

[PRE86]

然后，你编译作者的智能合约源代码并进行比较。它们一样吗？如果是，那么你可以审计智能合约，确保没有作弊。如果不是，那么你可以向作者投诉或决定不参与他们的智能合约。

在传统的网络应用程序中实现这种透明度并不容易。在 GitHub/GitLab 中验证代码并不意味着太多，因为开发者可能在他们的服务器上部署不同的代码。你可以被授予在他们服务器上的访客会话来验证代码的透明性，但是，开发者可能会部署一种复杂的方式来欺骗你。你可以每秒监视前端的网络应用程序，并部署一种监视策略，无论是手动还是借助 ML 来检测可疑活动。例如，你突然注意到一个评论突然被修改了，但后来没有被编辑的迹象，所以你可以肯定作弊发生在应用程序内部。然而，指责开发者并不容易，因为这是你的话对他们的话。你可能会被指控制造虚假证据。

有效的方法是雇佣一个可信赖和称职的审计员来做这项工作。审计员可以访问他们的网络应用程序，并有足够的权限读取数据库日志和服务器日志，以确保没有作弊发生。这只有在审计员无法被贿赂并且足够称职以避免被开发者欺骗的情况下才能实现。或者，你可以使用区块链。

投票是一个广泛的主题。我们在这个投票应用程序中还没有实现委托功能。我所说的委托类似于许多国家的民主制度。在一些民主国家，人们不直接选择他们的总理或总统。他们选择众议院的成员。在这些成员当选后，他们将选择总理。你可以创建一个实现委托系统的投票智能合约。如果你想进一步研究这个问题，请参考*进一步阅读*部分。

最后，我们的投票智能合约非常透明。这可能是好事，也可能是坏事，这取决于情况。透明度是好的，特别是在金融交易中，因为你可以审计日志以发现洗钱案件。然而，当涉及到投票，特别是在政治方面，保密性是一个可取的特性。如果选民没有保密性，他们可能会害怕被其他人迫害。智能合约中的投票保密性仍处于研究阶段。

# 总结

在本章中，你已经学会了如何创建一个区块链技术可以发挥作用的真实应用程序。这个真实应用程序是一个投票应用程序。从每个账户都可以投票的简单投票智能合约开始，我们逐渐创建了一个只有特定账户可以使用代币系统投票的投票应用程序。在构建这个投票智能合约时，我们还学习了如何编写一个脚本来部署带有构造函数的智能合约。在部署智能合约后，我们还学习了智能合约的一个特性，即事件。在`web3`脚本中，我们订阅这个事件来了解我们感兴趣的事情。最后，我们创建了辅助脚本来创建许多账户，并向其他账户发送资金以进行开发目的。

在下一章中，您将为您的`web3`脚本创建一个前端。您将构建一个适当的去中心化应用程序，以桌面应用程序的形式。

# 更多阅读

+   [`www.ethereum.org/dao`](https://www.ethereum.org/dao)
