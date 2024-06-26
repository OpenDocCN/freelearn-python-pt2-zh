# 第十章：保护您的应用程序

在关于应用程序性能和可扩展性的讨论中，以及确保应用程序在企业环境中稳定的最佳实践中，我们已经涵盖了很多内容。我们了解到用户体验对于使应用程序在企业内部成功非常重要。但是你认为我们在这里漏掉了什么吗？

假设我们拥有构建成功企业应用程序的所有组件，并且能够使其扩展，同时为用户提供符合预期行为的响应时间。然而，任何人都可以轻易访问我们应用程序的记录。如果存在漏洞允许用户在不进行登录的情况下从应用程序中获取敏感数据怎么办？是的，这就是缺失的环节：应用程序安全。在企业内部，应用程序的安全性是一个非常重要的因素。一个不安全的应用程序可能会向未预期的方面泄露敏感和机密数据，并且还可能给组织带来法律上的麻烦。

应用程序安全是一个大课题，即使是一本 500 页的书也可能不足以深入涵盖这个主题。但在本章的过程中，我们将快速介绍如何处理应用程序安全，让我们的用户在使用我们的应用程序时感到安全。

作为读者，在本章结束时，您将学到以下内容：

+   企业应用程序安全的重要性

+   用于突破应用程序安全的不同类型攻击向量

+   导致泄露的应用程序开发常见错误

+   确保您的应用程序安全

# 技术要求

对于本章，我们期望用户具有基本的配置 Web 服务器和网络通信基础知识。

# 企业应用程序安全

应用程序安全是一个如此重要的课题，您可能会讨论如何防止机密数据泄露，以及使应用程序足够强大以应对篡改攻击。

在企业中，这个话题变得更加严肃。这是因为大多数企业处理大量个人数据，其中可能包括可用于识别个人用户或与其财务详情相关的信息，例如信用卡号码、CVV 码或支付记录。

大多数企业都会花费大量资金来提高业务安全性，因为他们无法承受链条中的薄弱环节可能导致机密信息泄露的风险。一旦发生泄露，对组织的影响将从对未能维护机密数据安全的组织处以罚款开始，一直延伸到失去信任可能导致组织破产。

安全性不是闹着玩的，也没有一种解决方案适用于所有情况。相反，为了使事情变得更加复杂，用于突破组织安全防线的攻击变得越来越复杂，更难以建立保护措施。如果我们回顾一下网络安全漏洞的历史，我们可以找到一些例子，展示了网络安全问题有多么严重。例如，近年来，我们看到了一些涉及主要组织的泄露事件，其中一家组织的用户账户泄露超过 30 亿个；在另一次攻击中，一个游戏网络遭受了安全漏洞，并且停机了大约一个月，给组织造成了巨大的财务损失。

网络安全领域清楚地表明了一件事：这是一个不断发展的领域，每天都会发现新的攻击类型，并且正在研究新的缓解措施以及及时地克服它们。

现在，让我们来了解为什么企业应用安全是一个重要的话题，不应该被 compromise。

# 企业安全的重要性

大多数企业，无论其规模大小，都处理大量用户数据。这些数据可能涉及用户的一些公开可用的信息，也可能涉及机密数据。一旦这些数据进入组织的存储系统，组织就有责任保护数据的机密性，以防止未经许可的任何未经授权的人访问。

为了实现这一点，大多数企业加强了他们的网络安全，并建立了多重屏障，以防止未经授权的访问其用户数据系统。因此，让我们来看看企业安全如此重要的一些原因：

+   **数据的机密性：**许多组织...

# 系统安全面临的挑战

信息技术领域正在以快速的速度增长，每天都会出现新的技术。两方之间的通信方式也在不断发展，提供更有效的远程通信。但这种演变也带来了一系列关于系统安全的挑战。让我们来看看使得组织的系统安全变得困难的挑战。

+   **数据量的增加：**大多数组织正在构建他们的系统，利用人工智能和机器学习为用户提供更个性化的体验，他们也正在收集大量关于用户的信息，以改进推荐。这大量的数据存储使得数据的安全性更难以维护，因为现在越来越多的机密信息被保留，使得系统对攻击者更具吸引力。

+   **数据分布在公共服务提供商之上：**许多企业现在正在削减其存储基础设施，并且越来越依赖第三方公共存储提供商，这些提供商以更低的成本提供相同数量的存储空间，以及降低的维护成本。这也会使企业的安全性面临风险，因为现在数据受第三方服务提供商的安全策略管辖，数据所有者对数据的安全策略几乎没有控制权。存储服务提供商的一次违规行为可能会暴露不同组织的多个用户的数据。

+   **连接到互联网的设备数量不断增加：**随着越来越多的设备加入互联网，攻击面也在增加。即使是单个设备内部存在弱点，无论是加密标准还是未实施适当的访问控制，整个系统的安全性都很容易被破坏。

+   **复杂的攻击：**攻击变得越来越复杂，攻击者现在利用系统中的零日漏洞，甚至利用组织尚未发现的漏洞。这些攻击危害了大量数据，并对整个系统构成了巨大的安全风险。更加复杂的是，由于漏洞是新的，它们没有即时解决方案，导致延迟的响应，甚至有时延迟识别攻击发生。

+   **国家赞助攻击的增加：**随着全球信息技术驱动的通信和流程不断增加，战争的背景也在改变。以往的战争是在地面上进行的，现在它们正在网络上进行，这导致了国家赞助的攻击。这些攻击通常针对企业，要么是为了收集情报，要么是为了造成重大破坏。国家赞助的攻击问题在于这些攻击本质上是高度复杂的，并利用了大量资源，这使得它们难以克服。

有了这些，我们现在知道了不同因素使企业难以提高其系统的安全性。这就是为什么网络安全总是在进行追赶，企业正在改进其安全性，以抵御攻击者利用不断变化的攻击向量攻击 IT 系统。

现在，有了这些知识，是时候让我们了解到底是什么影响了应用程序的安全性。只有了解了不同的攻击向量，我们才能继续前进，使我们的应用程序免受攻击。因此，让我们开始这段旅程。

# 看一下攻击向量

每次侵犯系统安全或使其崩溃的攻击，都会利用系统应用程序运行的一个或另一个漏洞。这些漏洞对每种类型的应用程序都是不同的。为系统本地构建的应用程序可能具有不同的攻击向量，而为网络开发的应用程序可能具有不同的攻击向量。

为了充分保护应用程序免受攻击，我们需要了解针对不同应用程序类型使用的不同攻击向量。

从现在开始，我们将简要介绍两种最常见的应用程序类型和可能用于针对这些应用程序的攻击向量。

# 本地应用程序的安全问题

本地应用程序是专门为其运行的平台构建的应用程序。这些应用程序利用所提供的库和功能，以充分利用平台功能。这些应用程序可能遇到的安全问题通常是影响这些应用程序运行的基础平台的安全问题，或者是由应用程序开发人员留下的漏洞造成的。因此，让我们来看看影响本地应用程序安全性的一些问题：

+   **基础平台的漏洞：**当应用程序在平台上运行时，其功能受基础平台公开的内容所支配。如果基础平台容易受到安全问题的影响，那么在平台上运行的应用程序也会容易受到影响，除非它们在应用程序级别实施适当的措施来减轻这些漏洞。这类问题可能涉及硬件问题，比如最近影响 x86 平台的 Spectre 和 Meltdown 漏洞。

+   **使用第三方库：**一些使用第三方库的应用程序，特别是用于在应用程序内部实现安全性的库，如果开发人员停止维护这些库，或者存在一些未修复的漏洞，确实会使应用程序更容易受到安全漏洞的影响。通常，更好的选择是至少针对在应用程序中实现安全性的用例使用平台本身提供的库，而不是使用未记录的平台 API，这可能对应用程序的使用具有未解释的安全影响。

+   **未加密的数据存储：**如果一个应用程序涉及存储和检索数据，以未加密的格式存储数据可能导致数据被不受信任的来源访问，并使数据容易被滥用。应用程序应确保其存储的数据是以加密形式存储的。

+   **与第三方的未加密通信：**如今，许多应用程序依赖于第三方服务来实现特定功能。即使在企业网络内部，应用程序可能也会调用网络内部的第三方身份验证服务器来验证用户的身份。如果这些应用程序之间的通信是未加密的，可能会导致攻击，如中间人攻击。

+   **避免边界检查：**那些实施自己的内存管理技术的本地应用程序，如果应用程序的开发人员忽略了可能的边界检查，可能会变得容易受到攻击者访问应用程序边界之外的数据的攻击。这可能会导致系统安全性的严重破坏，不仅受影响的应用程序的数据暴露，其他应用程序的数据也会暴露。

这是一个非详尽的可能影响本地应用程序安全性的问题列表。其中一些问题可以很容易地修复，而其他问题则需要应用程序开发人员和平台提供商付出大量努力来减轻可能的安全漏洞。

现在，了解可能影响本地应用程序的攻击向量的知识后，是时候让我们了解可能影响 Web 应用程序的攻击向量了。

# Web 应用程序的安全问题

Web 应用程序的使用量一直在不断增加。随着互联网的日益普及，越来越多的组织正在将日常办公工作转移到帮助不同地理位置的办公室之间建立联系的 Web 应用程序上。但是，这些优势也伴随着安全方面的成本。

由于 Web 应用程序可能发生的攻击方式之多，Web 应用程序的安全一直是一个具有挑战性的领域。因此，让我们来看看困扰 Web 应用程序安全的问题：

+   **SQL 注入：**由于使用 SQL 数据库支持的 Web 应用程序的常见攻击之一是使用 SQL 注入。

# 安全反模式

现在，我们需要了解通常会使应用程序处于安全漏洞区域的实践。可能有很多事情会导致应用程序遭受安全问题，当我们通过本节时，我们将看一些通常会使应用程序容易受到安全漏洞的错误。所以，让我们逐个看一下。

# 不过滤用户输入

作为应用程序开发人员，我们希望用户信任我们的应用程序。这是我们确保用户会使用我们的应用程序的唯一方式。但是，同样地，我们是否也应该信任我们的用户，并期望他们不会做任何错误的事情？具体来说，信任他们通过我们的应用程序向他们提供输入的输入机制。

以下代码片段展示了一个简单的例子，未过滤用户提供的输入：

```py
username = request.args.get('username')email = request.args.get('email')password = request.args.get('password')user_record = User(username=username, email=email, password=password) #Let's create an object to store in database ...
```

# 未加密存储敏感数据

现在，作为应用程序开发人员，我们喜欢在应用程序代码库中保持简单，以便以后可以轻松维护应用程序。在考虑这种简单性的同时，我们认为我们的应用程序已经在一个良好的防火墙后面运行，并且每次访问都经过了彻底检查，那么为什么不只是以明文形式在数据库中存储用户的密码呢？这将帮助我们轻松匹配它们，也将帮助我们节省大量的 CPU 周期。

有一天，当应用程序在生产中运行时，攻击者能够破坏数据库的安全性，并不知何故能够从用户表中获取详细信息。现在，我们面临的情况是用户的登录凭据不仅泄露了，而且还以明文格式可用。根据一般心理学，许多人会在许多服务上重复使用相同的密码。在这种情况下，我们不仅危及了我们应用程序用户的凭据，还危及了用户可能正在使用的其他应用程序的凭据。

这种试图在没有任何强大加密的情况下存储安全敏感数据的做法不仅使应用程序面临可能随时发生的安全问题，而且还使其用户面临风险。

# 忽略边界检查

与缺少边界检查相关的安全问题在软件应用程序中是相当常见的情况。这种情况发生在开发人员意外地忘记在他们正在实现的数据结构中实施边界检查时。

当程序尝试访问分配给它的内存区域之外的内存区域时，会导致程序发生缓冲区溢出。

例如，考虑以下代码片段：

```py
arr = [None] * 10for i in range(0,15):    arr[i] = i
```

当执行此程序时，程序会尝试更改实际上并非由其管理的内存内容。如果底层平台没有引发任何内存保护，此程序将成功地能够覆盖...

# 不保持库的更新

大多数生产应用程序依赖于第三方库来启用一些功能集。保持这些库过时可以节省一些额外的千字节的更新或维护软件，以便它继续使用更新的库。然而，这也可能导致应用程序存在未修复的安全漏洞，攻击者以后可能利用这些漏洞非法访问您的应用程序和应用程序管理的数据。

# 将数据库的完整权限赋予单个用户

许多应用程序实际上会给应用程序的单个用户完整的数据库权限。有时，这些权限足以让您的应用程序数据库用户具有与数据库的根用户相同的权限集。

现在，这种实现方式在解决验证某个用户是否具有执行数据库操作的特定权限的问题上有很大帮助，同时也为应用程序带来了巨大的漏洞。

想象一下，如果某个数据库用户的凭据不知何故泄露了。攻击者现在将完全访问您的数据库，这使他们...

# 改进应用程序的安全性

如果我们遵循软件安全的一些基本规则并且在应用程序的开发和生产周期中严格实施这些规则，就可以保持应用程序的安全性：

+   **永远不要相信用户输入：** 作为应用程序的开发人员，我们应该确保不相信任何用户输入。在应用程序存储或任何其他可能导致提供的输入被执行的操作之前，用户端可能提供的一切都应该得到适当的过滤。

+   **加密敏感数据：** 任何敏感数据都应具有强大的加密，以支持其存储和检索。在生成数据的加密版本时具有一定程度的随机性可以帮助防止攻击者从数据中获取有用信息，即使他们以某种方式获得了对数据的访问权限。

+   **妥善保护基础设施：** 用于运行应用程序的基础设施应该得到妥善保护，防火墙配置应限制对内部网络或节点的任何未经授权的访问。

+   **实施端到端加密：**两个服务之间发生的任何通信都应该进行端到端加密，以避免中间人攻击或信息被窃取。

+   **谨慎实施边界检查：**如果您的应用程序使用任何类型的数据结构，请确保适当的边界检查已经就位，以避免漏洞，比如缓冲区溢出，这可能允许恶意代码被执行。

+   **限制用户权限：**没有应用程序应该有一个拥有所有权限的单个用户。用户权限应该受到限制，以定义用户执行操作的边界。遵循这种建议可以帮助限制较低权限用户的凭据被泄露时可能造成的损害。

+   **保持依赖项更新：**应用程序的依赖项应该保持更新，以确保依赖项没有已知的安全漏洞。

遵循这些指南可以在改善应用程序安全性方面起到很大作用，并确保应用程序和数据都得到保护，从而保持用户信任和数据安全。

# 总结

随着我们在本章的进展，我们了解了管理软件应用程序开发和运营的不同安全原则。我们谈到了需要在企业应用程序方面保持高安全标准，以及应用程序安全被破坏后会发生什么。然后，我们了解了系统安全面临的挑战。然后，我们转向了用于危害应用程序安全的常见攻击向量。

一旦我们了解了攻击向量，我们就看了一些常见的安全反模式，这些反模式会危及您的应用程序以及与应用程序相关的数据的安全性。一旦我们了解了这些反模式，...

# 问题

1.  是什么不同的问题使应用程序安全变得困难？

1.  什么是 XSS 攻击？

1.  我们如何防止 DoS 攻击？

1.  一些危害应用程序安全的错误是什么？

# 进一步阅读

如果您觉得应用程序安全是一个有趣的话题，并且想要了解如何使用 Python 来提高应用程序的安全性，请看一下这个由 Manish Saini 撰写、Packt 制作的视频系列《Python 持续交付和应用程序安全》。
