# 弃用身份验证扩展- Octopus 部署

> 原文：<https://octopus.com/blog/deprecating-authentication-extensions>

Octopus 服务器不支持 BYO 认证机制。

在这篇文章中，我将解释为什么会发生这种情况，如果您使用的是定制的身份验证提供者，当发生变化时，您需要做些什么。

## Octopus 服务器扩展性

在 Octopus 3.5 中，我们[引入了服务器可扩展性的概念](https://octopus.com/blog/octopus-deploy-3.5#octopus-deploy-server-extensibility)。这主要是由客户需求驱动的，允许我们的客户根据他们的独特要求调整八达通的认证机制。

当时，为定制开放认证管道是双赢的；我们的客户可以确保系统按照他们组织的要求精确地工作，并且我们可以减轻一些支持所有需要的不同机制的压力。

我们还认为这是一个机会，可以重新设计产品中包含的一些核心依赖项，以简化开发过程。通过分解 Octopus 服务器的职责，我们希望将系统的各个部分相互分离，从而加快开发周期。

从那以后，Octopus Server 有了显著的增长，随着我们的增长，我们以前的一些假设没有产生预期的影响，反而导致了不必要的副作用。在尝试将 Octopus 依赖项隔离到单独的库以进行开源认证时，如果不为我们的客户提供方便，跨依赖项的开发会变得更加困难。

## 问题

当一个扩展库的契约可供公众使用时，您就承诺维护和继续该契约的功能。这意味着早期做出的架构决策，在当时可能是正确的，变得难以适应，特别是当新功能需要对底层系统进行更改时。

由于库依赖层次结构的构造方式，对 Octopus 服务器使用的基本库的更改需要这些库的用户重新构建他们的定制以继续使用它们。这给本应简单的服务器升级过程增加了不必要的、常常令人沮丧的步骤。

让核心抽象跨库和存储库分布也会影响开发——当您想要进行横切变更时，当代码不在同一个存储库中时，它们更难推理和执行。

尽管让认证提供者*开放供审查*是一个合理的举措(并且我们继续通过我们的 GitHub 库为各种库这样做)，他们*开放供扩展*的方式不再符合我们希望 Octopus Server 进入的技术方向。因此，我们决定重新考虑我们当前的服务器可扩展性模型。

## 有什么变化

如果您还没有为 Octopus 服务器实例构建和安装定制的身份验证扩展，那么这些更改不会影响您。您现在可以停止阅读并继续升级，无需任何更改。

在短期内，自定义扩展将像过去一样工作。您需要重新编译您的库来考虑依赖关系的变化，但是将它们加载到 Octopus 服务器实例的机制将保持不变。然而，在 2023 年底 Octopus Server 的未来版本中，所有自定义认证机制都将停止工作。

这篇文章的早期版本指出，需要在 API 调用中设置环境变量和更改身份验证。该产品的最新更新意味着不再需要这些变通办法。

### 侧面影响-自定义路线处理程序

支持身份验证可扩展性的一个副作用是能够注入自定义 HTTP 处理程序来响应 API 请求。随着这些最初的改变，这种未记录的能力将不会受到影响，但是当扩展被完全废弃时，这种能力也将被移除。

## 接下来会发生什么

为了改善我们的交付流程并加强对身份验证提供商的安全检查，我们计划在 2023 年底之前全面取消 Octopus 部署中的 BYO 身份验证功能。

从现在到 2023 年底，我们将继续尽可能地支持扩展，并审查我们如何将客户创建的自定义更改整合到官方提供商本身。

根据目前的反馈，我们预计大多数定制只是对现有提供商的改进，但是我们将调查完全定制集成对用户的影响。如果你也包括在内，请在下面的评论中告诉我们。

### 如果变化对你有影响，请联系我们

你们中的一些人可能已经实现了从我们的某个库派生出来的身份验证机制，一些人可能已经开发了一个完全自定义的身份验证提供程序。如果您无法迁移到内置机制之一，我们希望更好地了解您的需求，以便我们能够解决缺失的功能。

通过 support@octopus.com[与我们联系，或者在下面评论，让我们知道这是否会影响你的八达通实例。](mailto:support@octopus.com)

## 结论

我们不会轻视章鱼的反对意见。在我们软件的悠久历史中，我们对强大的向后兼容性感到自豪。然而，有时我们需要重新考虑那些不再符合我们客户或 Octopus 服务器最佳利益的支持功能。

随着我们逐渐远离身份验证可扩展性模型，我们希望利用接下来的 12 个月来减少这可能对当前依赖它的任何人造成的影响。

愉快的部署！