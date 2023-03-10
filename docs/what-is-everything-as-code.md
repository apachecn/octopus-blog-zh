# 作为代码的一切是什么？-章鱼部署

> 原文：<https://octopus.com/blog/what-is-everything-as-code>

如果您在 DevOps 或 cloud 中工作，您可能使用过 GitHub Actions、Jenkins 或 Terraform 等工具来交付您的 DevOps 管道。您可能已经注意到，这些工具都将 DevOps 管道的一部分表示为代码，允许您在以后存储和重用管道的一部分。

DevOps 管道的代码表示是向一切即代码(EaC)转变的一部分。但是什么是 EaC，为什么它很重要？

“一切如代码”是一种软件开发方法，它使用代码来定义和管理 IT 资源。资源的代码表示使开发人员更容易:

*   审计变更
*   提高一致性
*   扩展资源
*   将设置从一个环境转移到另一个环境

从字面上看，EaC 是一种理想状态，软件生命周期的每一部分都是代码。

然而，如今 EaC 的实现远非理想，EaC 被用作一个总括术语，涵盖了“as code”框架的具体应用。基础设施即代码(IaC)和配置即代码(Config as Code)是广泛应用的 EaC 应用程序，其他应用程序涵盖了一系列 IT 领域。

在这篇文章中，我将讨论一切都是代码的一些应用、好处以及我对转向 EaC 的想法。

## 基础设施作为代码

基础设施即代码(IaC)是通过代码配置 IT 基础设施的过程。过去，开发人员手动调配 IT 基础架构，无法控制谁可以调配基础架构以及他们使用什么方法。

您可能遇到过这样的情况:一个人，即看门人，控制着您所有的 IT 基础架构。只有那个人知道东西在哪里，但即使是他们也不能确定所有的事情。

有了 IaC，组织中的每个人都可以看到所有基础结构的内容和位置。基础设施不再由少数人控制，而是在配置文件中受版本控制。好多了！

Terraform 是最流行的 IaC 框架之一。Terraform 提供了一种配置语言，开发人员使用这种语言来定义云和内部资源。许多云提供商使用 IaC，因此开发人员可以以可重复的方式按需供应基础架构。如果你能学会 Terraform 语言，你就可以使用它和任何云提供商或部署工具，比如 Octopus，来提供基础设施。

## 配置为代码

Config as Code 是在代码中捕获所有系统配置设置的过程。在 Octopus 中，配置设置指定了部署过程。我们在 2022 年 3 月发布了作为 Octopus 代码的 [Config。](https://octopus.com/blog/octopus-release-2022-q1)

我们将 Config 设计为代码，以提供 Git 的能力(分支、拉请求和完整的变更审计日志)和 Octopus UI 的可用性。你可以在我们的文章 [Shaping Config as Code](https://octopus.com/blog/shaping-config-as-code) 中读到一些我们的设计思想。

在 Octopus Deploy，Config as Code 中，用户可以选择使用 UI 或源代码控制的实现，而不会失去任何功能。您可以选择使用 UI 进行小的更改，或者使用源代码管理进行高级更改。

我们相信我们不折不扣的解决方案是最好的“代码”实现之一。

## 代码形式的其他例子

EaC 还有其他一些例子，有些比其他的更适合，例如:

*   **作为代码的环境:**提供计算环境的工具，如 Docker 和 vagger。许多云提供商都有自己的计算环境作为代码产品，比如 Google 的 Compute Engine 和 Amazon 的 EC2。

*   **数据分析如代码:**你可以将数据管道和机器学习过程表示为代码。数据管道作为代码允许数据科学家将数据分析组件从一个项目移植到另一个项目。

*   **DevOps 管道作为代码:**像 GitHub Actions 和 Jenkins 这样的工具将 DevOps 管道表示为代码。当开发人员推动代码变更时，会触发一个流程来构建存储库，并产生一个工件或部署一个版本。

*   **安全性即代码:**如果你正在管理一个云环境，你会关心安全性。代码形式的安全性允许您在配置文件中表示安全数据，如角色和权限。如果敏感数据需要“作为代码”，您应该考虑加密技术。

EaC 适用于开发人员可以对流程和资源进行编码的任何场景，因此它适用于 it 的更多部分。您能想到您的企业中有任何 EaC 应用程序吗？

## 利益

“一切都是代码”让您可以将 IT 资源表达为代码。主要好处是:

*   **一致性:**您可以在一个标准框架(如 Terraform)中捕获基础设施和配置设置。这个框架减少了人为错误，提高了可靠性，因为系统是受版本控制的。
*   **可伸缩性:**如果您想要伸缩，您只需对配置文件做一点小小的更改，就可以将任何问题回滚到以前的版本。
*   **可移植性:**您可以导出您的基础设施、配置或其他系统部件，并复制它们。
*   **可审计性:**您可以更容易地审计您的系统，因为版本控制使变更可见。

EaC 对您和您的软件栈有一些明显的好处，但是将系统转向 EaC 并不总是有意义的。

虽然 Octopus Deploy 中的代码部署有很大的好处，但是将其他平台部分转换成 EaC 的回报却在减少。与组织可以在同一时间段内开发的其他功能相比，开发一个额外的 EaC 功能总是有机会成本的。在实践中，组织应该在有意义并且对用户有最大好处的地方应用 EaC。

## 结论

一切如代码(EaC)是一种软件开发方法，使用代码来定义和管理 IT 资源。EaC 已经在基础设施代码、配置代码和其他 IT 领域找到了许多应用。如果您在 DevOps 和云计算领域工作，您可能已经亲眼看到了 EaC 的好处。

尽管一切以代码的形式出现对组织来说是一种有希望的最终状态，但是将平台的一部分转换成 EaC 是有机会成本的，这将告诉你在哪里投资你的资源。毫无疑问，您的平台的某些部分可以从 EaC 方法中受益，关键是识别那些区域。

愉快的部署！