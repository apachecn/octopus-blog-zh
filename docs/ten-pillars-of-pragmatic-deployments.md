# 务实部署的十大支柱-八达通部署

> 原文：<https://octopus.com/blog/ten-pillars-of-pragmatic-deployments>

[![The ten pillars of pragmatic deployments](img/4c1dbb64e25f9ca827fef8ffb3e74c87.png)](#)

正如你可能想象的那样，在 Octopus，我们花了大量时间考虑部署。Octopus 的诞生是为了管理现实世界的部署流程，并通过与客户的对话、支持请求、我们自己的内部部署要求以及许多其他关于我们认为好的部署的讨论而不断形成。

这些知识已经被提炼为实用部署的 10 个支柱。

我们相当谨慎地使用“务实”一词。这个列表不是记分卡、金字塔、清单或固定的需求列表。在许多情况下，无论是作为产品还是作为公司，八达通都没有实现这些支柱。开发这些支柱的主要目标之一是找到我们自己的过程中的差距，以便形成我们产品的特性和理念，并继续正在进行的关于部署软件意味着什么的讨论。

记住这一点，第 0 个支柱，也是列表中所有其他支柱的先决条件，就是*做对你有用的事情*。

我们希望您喜欢这次讨论，并期待通过您的反馈塑造务实部署的未来。

## 十大支柱

## 支柱 1。可重复部署

在各种环境中推进部署的主要原因之一是获得越来越多的信心，相信您正在为最终用户提供一个可行的解决方案。这种信心可以通过测试(手动和自动)、手动签署、内部使用你自己的软件(喝你自己的香槟)、测试用户的早期发布，或者任何数量的允许问题在影响到你的最终用户之前被识别的其他过程来建立。

但是，只有当您部署到生产环境中的内容尽可能接近您在非生产环境中验证的内容时，您才能获得这种信心。

通过采用可重复部署，您可以确保您的最终用户在生产环境中使用的是您在非生产环境中测试、验证并获得信任的内容。

### 一般部署概念

为了理解可重复的部署，我们需要理解什么是部署，以及在典型的构建和部署管道中的哪个点发生部署。

#### 持续集成、持续交付和持续部署

术语持续集成和持续交付或部署(CI/CD)经常被用来描述从源代码到可公开访问的应用程序的进展。

我们认为持续集成(CI)是在源代码更新时编译、测试、打包和发布应用程序的过程。

持续交付和持续部署(都缩写为 CD)有着微妙的不同含义。我们将这两个术语视为将应用程序交付到其目的地的一系列自动化步骤。区别在于这些自动化步骤是否将应用程序直接交付给最终用户，而无需人工干预或决策。

连续部署没有人工干预。当您的 CI 服务器对您的源代码进行编译、测试和打包，然后部署、测试并向最终用户公开时，您就实现了连续部署。这个过程的每个阶段的成功或失败都是自动的，从而产生一个向消费者承诺的工作流。

连续交付涉及到一个人做出向最终用户进行部署的决策。这种发展通常表现为通过一系列环境的提升。环境进展的一个典型例子是将应用程序部署到非生产开发和测试环境，最后部署到生产环境。

开发环境可以很好地配置为连续部署，CI 服务器成功构建的每个提交都是自动部署的，无需人工干预。当开发人员对他们的更改适合更广泛的受众感到满意时，就可以将部署提升到测试环境中。

在测试环境中，质量保证(QA)人员验证变更，产品所有者确保功能满足他们的需求，安全团队探查漏洞等。当每个人都对变更满足他们的需求感到满意时，部署就可以升级到生产环境了。

生产环境是部署的最终目的地，也是应用程序向最终用户公开的地方。

我们从大多数客户那里了解到，持续交付*对他们来说很有效*。因此，虽然大多数支柱同样适用于持续交付和持续部署，但我们将从持续交付的角度来看待它们。

#### 什么是环境？

环境代表单个应用程序副本或整个应用程序堆栈及其支持基础架构之间的界限。

所有环境的配置应该尽可能相似，以确保应用程序无论在哪个环境中都能保持一致的行为。

环境通常代表从具有高部署频率和低稳定性的初始环境到具有低部署频率和高稳定性的最终环境的进展。

部署在整个环境中进行，以获得越来越大的信心，相信可以向最终用户交付工作解决方案。

标准的环境集被称为开发、测试和生产。下表描述了这些环境的特征:

| 环境 | 描述 | 部署频率 | 稳定性/信心 |
| --- | --- | --- | --- |
| 发展 | 开发人员使用它来测试实现的单个更改。 | 高的 | 低的 |
| 试验 | 由开发人员、QA 和非技术人员用来验证变更是否满足需求。 | 中等 | 中等 |
| 生产 | 供最终用户访问，以使用公开可用的应用程序实例。 | 低的 | 高的 |

尽管您可以自由地拥有任意数量、任意名称的环境，但是这组环境将在示例中使用。

#### 什么是部署？

我们已经讨论过将“应用程序”部署到环境中，这通常是我们描述部署的方式。但是为了理解可重复部署是如何实现的，我们首先需要明确我们实际部署了什么。

有三样东西可以部署到环境中:

1.  为特定环境配置的已编译应用程序。这些被称为包。
2.  定义如何配置应用程序的变量，通常有一小部分特定于特定环境。
3.  内联编写的脚本和配置文件(即，不作为文件保存在包中)，用于支持或配置环境中的应用程序及其相关基础架构。

软件包版本、变量值和脚本或配置文件被捕获为一个版本。

然后将该版本部署到环境中。这样，包、变量、脚本和配置文件的一致捆绑从一个环境提升到下一个环境。只有一小部分特定于环境的设置会因环境而异。

## 支柱 2。可核实的部署

可重复部署支柱描述了如何在环境中推广版本，从而提高对交付的解决方案的信心。我们讨论了频繁的开发环境部署如何使开发人员能够测试他们的变更，而不太频繁的测试环境部署则允许其他方验证潜在的产品发布。当每个人都满意时，生产环境就更新了，向最终用户公开了这些变化。

可验证部署的支柱描述了当部署到达新环境时可用于验证部署的各种技术。

### 一般测试概念

测试是一个模糊的术语，通常有不明确的子类别。我们不会试图在这里提供测试类别的权威定义。我们的目标是提供通用测试实践的高度描述，并强调那些可以在部署过程中执行的测试实践。

#### 在部署过程中，我们不测试什么？

单元测试被认为是构建管道的一部分。这些测试与正在编译的代码紧密相关，它们必须成功，才能构建出最终的应用程序包。

集成测试也可以在构建过程中运行，以验证更高级别的组件是否如预期的那样交互。测试中的组件可以用一个双测试来代替[以提高可靠性，或者可以创建组件的真实实例作为测试的一部分。](https://martinfowler.com/bliki/TestDouble.html)

单元和集成测试由 CI 服务器运行，任何可用于部署的包都被认为已经通过了所有相关的单元和集成测试。

#### 部署期间我们可以测试什么？

需要可访问的活动应用程序或应用程序堆栈的测试是作为部署过程的一部分运行的理想候选。

冒烟测试是快速测试，旨在确保应用程序和服务已正确部署。冒烟测试实现了确保服务正确响应所需的最小交互。一些例子包括:

*   web 应用程序或服务检查成功响应的 HTTP 请求。
*   确保数据库可用的数据库登录。
*   检查目录是否已被文件填充。
*   查询基础结构层以确保创建了预期的资源。

集成测试可以作为部署的一部分来执行，也可以在构建期间执行。集成测试验证多个组件如您所期望的那样交互。部署中可能包含测试替身来代替被验证的组件，或者测试可能验证两个或更多的活动组件实例。例子包括:

*   登录 web 应用程序以验证它可以与身份验证提供程序交互。
*   向 API 查询来自数据库的结果，以确保可以通过服务访问数据库。

端到端测试提供了一种与系统交互的自动化方式，就像用户与系统交互一样。这些测试可能是长时间运行的测试，遵循应用程序中要求应用程序堆栈的大部分或所有组件正常工作的路径。例子包括:

*   自动化与在线商店的交互，以浏览目录、查看商品、将其添加到购物车、完成结账以及查看帐户订单历史。
*   完成对天气服务的一系列 API 调用，以查找城市的纬度和经度，获取返回位置的当前天气，并返回本周剩余时间的天气预报。

混乱测试包括故意移除或干扰组成应用程序的组件，以验证系统是否有足够的弹性来承受此类故障。混沌测试可以与其他测试相结合来验证退化系统的稳定性。

可用性和可接受性测试通常要求人们使用应用程序来验证它是否满足他们的需求。需求可以是主观的，例如，确定应用程序是否在视觉上有吸引力。或者测试人员可能不是技术人员，因此没有自动化测试的选项。这些测试的手动和主观性质使得它们很难(如果不是不可能的话)自动化，这意味着必须部署应用程序或应用程序堆栈的工作副本，并使其可供测试人员访问。

## 支柱 3。无缝部署

部署新版本的应用程序不可避免地涉及到使以前的版本脱机并将流量导向新版本。确保最终用户在此转换过程中不会受到负面影响需要一些规划。

合理的解决方案是在最终用户不太可能使用应用程序的时候执行面向公众的部署。如果您的客户位于相似的时区，那么在半夜部署新版本的应用程序可以有效地为最终用户带来无缝体验。

午夜部署可能并不吸引人，但是如果它们为你工作，这是一个非常有效的解决方案。

当必须将停机时间保持在最低限度，或者没有合适的停机时间时，可以使用一些常见的部署策略来无缝地部署新的应用程序版本。

### 无缝数据库部署

不首先解决数据库更新的问题，就不能开始讨论无缝部署。

大多数无缝部署策略的一个基本方面涉及到并行运行应用程序的两个版本，即使是在很短的时间内。如果应用程序的两个版本都访问共享数据库，那么对数据库模式和数据的任何更新都必须与两个应用程序版本兼容。这被称为向后和向前兼容性。

然而，实现向后和向前的兼容性并不容易。在演讲[中，Edison Yanaga 介绍了重命名数据库中单个列的过程。它涉及对数据库和应用程序代码的六次增量更新，并且所有六个版本都要按顺序部署。](https://www.youtube.com/watch?v=3mj6Ni7sRN4)

不用说，涉及数据库的无缝部署需要大量的规划、许多小步骤来展开更改，以及数据库和应用程序代码之间的紧密协调。

### 部署策略

有多种策略来管理现有部署和新部署之间的转换。

#### 再创造

重新创建策略不提供无缝部署，但此处包含了这一策略，因为它是大多数部署过程的默认选项。该策略包括删除现有部署并部署新版本，或者在现有部署的基础上部署新版本。

这两种选择都会导致现有版本停止或删除与新版本启动之间的停机时间。但是，因为现有版本和新版本不会并发运行，所以可以根据需要应用数据库升级，而没有向后和向前的兼容性要求。

#### 滚动更新

滚动更新策略包括用新部署增量更新当前部署的实例。这种策略确保在部署期间至少有一个当前或新部署的实例可用。这要求任何共享数据库都必须保持向后和向前的兼容性。

#### 金丝雀部署

canary 部署策略类似于滚动更新策略，因为两者都以增量方式向更多的最终用户公开新的部署。不同之处在于，在 canary 部署中进行部署的决策要么是由监控指标和日志的系统自动做出的，以确保新部署按预期执行，要么是由人工手动做出的。

Canary 部署还可以选择暂停部署，并在发现问题时恢复到现有部署。

#### 蓝色/绿色部署

蓝/绿策略包括将新版本(称为绿版本)与当前版本(称为蓝版本)一起部署，而不将绿版本暴露给任何流量。一旦绿色版本得到部署和验证，流量就会从蓝色版本切换到绿色版本。当蓝色版本不再使用时，可以将其移除。

绿色版本部署的任何数据库更改都必须保持向后和向前的兼容性，因为即使绿色版本不支持流量，蓝色版本也会受到数据库更改的影响。

#### 会话排出

当应用程序维护绑定到特定应用程序版本的状态时，使用会话清空策略。

这种策略与蓝/绿策略相似，都是将新版本与当前版本一起部署，并同时运行。与蓝/绿策略不同，蓝/绿策略会在一个步骤中将所有流量切换到新部署，而会话清空策略会将新会话定向到新部署，而现有部署会为现有会话提供流量。

旧会话过期后，可以删除现有部署。

因为当前部署和新部署是并行运行的，所以任何数据库更改都必须保持向后和向前的兼容性。

#### 功能标志

功能标记策略包括将功能构建到新的应用程序版本中，然后在部署过程之外为选定的最终用户公开或隐藏该功能。

在实践中，具有可标记特性的新应用程序版本的部署将使用上述策略之一来执行，因此特性标记策略是对其他策略的补充。

#### 特征分支

特性分支策略允许开发人员使用他们当前正在实现的更改来部署应用程序版本，通常是在非生产环境中，与主部署一起。

可能没有必要维护数据库与功能分支部署的向后和向前兼容性。因为特性分支是用于测试的，而且往往是短暂的，所以每个特性分支部署都可以访问它自己的测试数据库。

## 支柱 4。可恢复部署

实施可重复且可验证的部署(在发布到生产环境之前在非生产环境中进行测试)的目的是在错误影响最终用户之前识别它们。然而，一些错误不可避免地会出现在生产中。在这种情况下，将生产环境恢复到理想状态是非常重要的。

### 向后或向前滚动

从不良部署中恢复意味着回滚到以前的良好部署，或者前滚到使环境恢复到理想状态的新版本。

这两种解决方案都适合无状态应用程序，但是当涉及到数据库时，回滚必须小心处理。

这是来自飞行路线项目的[建议:](https://flywaydb.org/documentation/command/undo#important-notes)

> 虽然撤销迁移的想法很好，但不幸的是，在实践中有时会失败。一旦你有破坏性的改变(删除、删除、截断等等)，你就开始有麻烦了。即使您不这样做，您最终也会创建自制的备份恢复替代方案，这些方案也需要经过适当的测试。

[RedGate 为数据库回滚提供了以下建议](https://documentation.red-gate.com/sca/developing-databases/concepts/advanced-concepts/rollbacks/rollback-guidance):

> 与其在回滚计划上投入时间和精力，另一种选择是遵循一种让你不断前进的方法。

博客文章[SQL 回滚和自动化数据库部署的陷阱](https://octopus.com/blog/database-rollbacks-pitfalls)有这样的建议:

> 通常情况下，成功回滚部署的努力远远超过了将修复推向生产的努力。

当部署涉及数据库更改时，我建议您进行前滚，以便从不理想的部署中恢复。

### 正在回滚

对于可重复的部署，可以通过重新运行之前的部署来实现回滚。这是可能的，因为包版本、脚本和变量都是由可重复的部署定义的。

回滚也是几种无缝部署策略的一个显式特性:

*   Canary 部署通过将所有流量从新部署重定向到当前部署来实现回滚。
*   蓝/绿部署可以通过将流量切回蓝堆栈来回滚部署。
*   会话清空部署可以将新会话重定向到当前部署，并且可以选择终止新部署中的任何会话。

回滚有以下好处:

*   通过回滚到以前的部署，可以修复部署问题，而无需编写代码。
*   回滚使系统处于已知的已验证状态。
*   完成回滚所需的时间可以在非生产环境中测量。

回滚有以下缺点:

*   回滚是全有或全无的操作。您不能回滚单个功能，只能回滚整个部署。
*   回滚需要作为部署过程的一部分进行测试，以确保它们按预期工作，这增加了部署过程的复杂性和时间。
*   如果回滚失败，您可能需要通过前滚来解决问题。
*   数据库回滚需要特别考虑，以确保数据不会丢失。

### 向前滚动

前滚只是描述执行新部署的另一种方式。在这种情况下，新部署将只包含恢复环境所需的修复程序。

向前滚动有以下好处:

*   所有部署策略，不管有没有数据库，本质上都支持前滚。
*   团队在每次部署前滚的过程中获得经验。
*   向前滚动时，您可以选择更改或修复的范围。
*   在前滚时，可以连续进行多个部署，以解决不需要的部署。

向前滚动有以下缺点:

*   前滚通常需要开发人员实现一个修复，以包含在下一个部署中。
*   前滚可能涉及绕过通常用于验证部署的环境进展，以尽快获得部署的修复。
*   只要开发和部署下一个版本，生产环境就会一直处于不受欢迎的状态。

## 支柱 5。可见部署

部署是一个抽象的概念，通过检查系统的状态很难知道什么被部署在哪里。根据磁盘上的文件或数据库中的记录来确定应用程序的安装版本，就像计算出用于生产一罐颜料的颜色组合一样。

能够快速查看环境的当前状态对于理解以下内容至关重要:

*   向您的客户提供了哪些功能。
*   正在测试哪些功能。
*   修复了哪些问题。
*   任何变化的历史。

下面列出了全面了解部署状态所需的大量详细信息。

### 提交消息

提交消息捕获源代码编辑的意图，描述做了什么更改以及谁做了这些更改。当试图从较低的层次理解是什么改变了包的特定版本时，这些消息是非常宝贵的。

### 问题跟踪

通常，提交源代码是为了解决在专门的问题跟踪器中记录的问题。这些问题为描述、讨论和跟踪 bug 提供了空间。每个问题都有一个唯一的标识符。

捕获与包版本以及包含该包版本的任何部署中的更改相关的问题 id，可以提供对任何给定部署中已解决的问题的深入了解。

### CI 构建日志

典型的 CI/CD 管道有一个 CI 服务器，用于构建、测试和打包应用程序。这些构建的日志文件包含了大量的信息，比如哪些测试通过了，哪些测试被忽略了，包含了哪些依赖项，以及创建了哪些包。从部署到 CI 构建的链接允许快速查看这些日志文件。

### 库依赖性

几乎今天部署的每个应用程序都是自定义代码和共享库的组合，通常由第三方提供。这些库可能是错误或安全漏洞的来源，或者提供新的有用的功能。了解促成部署的库依赖关系对于审计和调试非常重要。

### 部署版本

一个发布版本在一个发布版本中捕获所有上述信息，以及包版本、变量值和脚本。然后将该版本部署到环境中。

显示哪个发布版本被部署到哪个环境提供了部署状态的高级视图。有了这些信息，任何人都可以看到在哪里部署了什么，并且通过深入了解发布的详细信息，可以看到提交消息、问题、依赖项以及到 CI 构建的链接。

## 支柱 6。可衡量的部署

将您的软件送到客户手中是任何部署过程的主要目标。要了解您的部署过程执行得有多好，您需要衡量定义成功的标准。

拥有可度量的部署意味着定义一组通用的、一致同意的度量标准，可靠地收集这些度量标准，并以易于理解的格式呈现它们。

适用于部署的一些常见指标有:

*   部署频率:部署到生产环境的频率如何？
*   变更的准备时间:将提交部署到生产中需要多长时间？
*   部署的前置时间:一个版本部署到生产需要多长时间？
*   恢复部署的时间:在部署失败后，成功部署一个版本需要多长时间？
*   部署失败率:失败的部署和成功的部署之间的比率是多少？
*   变更失败率:热修复部署和常规部署之间的比率是多少？
*   部署持续时间:每次部署需要多长时间？

## 支柱 7。可审计的部署

如果可见的部署支柱是关于呈现您的环境的当前状态以及作为发布的一部分所做的更改，那么审计是关于跟踪谁随着时间的推移参与了部署过程。

审核允许团队查看部署到环境的历史记录、部署过程的更改、环境的更改、谁批准了哪些部署，以及确定环境在过去某个时间点的状态。

为了使审计事件有用，它们必须能够被搜索、过滤、导出和报告。

## 支柱 8。标准化部署

正如可重复的部署会随着一个版本在不同环境中的推广而建立信心一样，跨不同项目的标准化部署过程允许团队信心十足地利用已经被证实的过程。

标准化部署有两个主要方面。

第一个方面是定义各种部署使用的初始部署过程。这个基本部署过程可以是一个共享模板，允许定制非常具体的设置。或者整个过程可以被复制和粘贴以引导新的项目，但是允许他们根据自己的需要定制过程。

第二个方面是定义谁有能力编辑部署过程。通过区分查看、运行、创建和编辑部署过程的能力，团队可以保证只有那些负责创建或编辑部署过程的个人才能这样做。

## 支柱 9。可维护的部署

将您的部署部署到生产环境只是一个开始。诊断问题、收集日志、执行备份、重新启动服务、轮换密钥和测试连接只是日常操作任务的一部分，可让您的应用程序保持运行并让您的客户满意。

虽然您可以通过 SSH 或 RDP 连接到服务器并开始探索，但是您所做的每个更改都会使您的生产环境偏离非生产环境，从而使实现可重复部署变得更加困难。也很难跟踪所做的更改，验证它们是否有效，以及审计谁更改了什么。

像部署一样，维护任务应该是可重复的、可验证的、可见的、可测量的、可审计的、标准化的和协调的。维护任务代表了保持部署运行所需的业务知识，应该与部署过程保持相同的标准。

## 支柱 10。协调部署

将包部署到环境中只是部署过程的一小部分。通常，部署需要与其他业务流程相协调，以确保:

*   合适的人已经同意了。
*   部署的成功或失败会通知相关方。
*   部署按正确的顺序进行。
*   部署只能在特定时间进行。
*   高优先级部署优先于低优先级部署。
*   部署被安排在预先确定的时间进行。
*   外部事件可以触发部署。
*   部署可以触发外部事件。

实际上，部署流程可能是业务流程管理工具的更大生态系统中的单个组件。从第三方平台协调部署并报告结果的能力允许团队将复杂的部署作为更广泛的业务流程的一部分来管理。

## 结论

这些是实用部署的十大支柱，它们塑造了 Octopus Deploy 的特性和理念。我确信它们会随着新的用户案例的出现而不断发展，但是我们相信它们为继续构建 Octopus Deploy 提供了一个坚实的基础。

愉快的部署！