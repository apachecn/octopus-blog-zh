# 八达通部署 2018 路线图-八达通部署

> 原文：<https://octopus.com/blog/roadmap-2018>

昨天我贴出了我们对 2017 年的反思。我写了一些我们完成的事情，一些我们没有完成的事情和原因，以及我们面临的一些挑战。今天，我想分享我们的 2018 年路线图。

[![Roadmap for 2018](img/147b2c98f6670c1319304c52ba277c6d.png)](#)

## 主题

我们将在 2018 年开展的工作有四个关键主题。

1.  将云托管版本的 Octopus 推向市场
2.  在微软生态系统之外扩展 Octopus
3.  将 Octopus 发展成一个完整的 DevOps 工具
4.  不断提高性能、可扩展性、稳定性和用户体验

## 云章鱼

当我们第一次构建 Octopus 时，世界上大部分地区仍在内部部署应用程序。Octopus 是您在企业中与数百个其他虚拟机一起运行的虚拟机。当我们与较大的客户交谈时，这实际上仍然是常态——不管云供应商希望您相信什么，仍然有大量的本地服务器。

也就是说，如果你部署的每一个应用都是 Lambda 函数、Azure 网站或 Docker 容器，那么仅仅为了 Octopus 而必须照看一个 VM 就是一种拖累。我们的朋友杰里米·凯德曾经说过:

> Octopus 是我用的最重要的东西，也是我仅剩的 VM。

一个云托管版本的 Octopus 需要做大量的工作，这是我们致力于很快交付的东西。与该路线图中的许多努力不同，这些努力往往是具有“完成”定义理念的项目，这个云托管版本的 Octopus 将是一项长期投资，也是我们组织上的一个重大变化。

我们已经为此工作了几个月，我们将在 2018 年 1 月推出一个封闭的 alpha 版本，然后在 3 月底之前推出一个开放的 beta 版本。最初，我们将尽可能保持它与 Octopus 的本地版本相似，随着我们从生产中运行它中学到更多，我们将针对云对它进行改进和优化。

## 扩展到微软生态系统之外

在过去的几年里，我们有微软商店和 Java 商店，大多数公司的立场非常明确。Octopus 最初只是一个. NET 部署工具，但是几年前我们就意识到这需要改变。人们和公司更少认同“一个”。NET 开发人员"和更多的作为"开发人员，谁碰巧做。NET 以及一些节点和一些其他平台。

我们已经支持通过 SSH(没有 Mono 依赖)部署到非 Windows 平台，运行 Bash 脚本，2017 年我们在部署 Java 应用程序方面取得了很大进展。迄今为止，我们的成功主要是与偶尔使用非微软技术的微软商店。

今年我们将继续这个主题:

*   我们将带来一流的 AWS 支持，达到 Azure 支持的水平
*   我们将把触手引入 Linux(以允许轮询连接)
*   我们将增加对运行 Python 和 Ruby 脚本的支持(除了 PowerShell、C#、Bash 等。今天)
*   我们将扩展现有的 Docker 支持来与 Kubernetes 合作

Octopus 部署应用程序，但它可以用来部署更多应用程序。当你想一想:

*   Octopus 对你的应用程序了如指掌——它们是用什么语言编写的，在什么网络服务器上运行
*   您的部署管道(开发、测试、生产等。)
*   在所有这些环境中，您的应用程序是如何配置的
*   一切赖以运行的基础设施

有了这一切，我们可以做的不仅仅是“仅仅”部署:

*   借助 AWS 云形成、Azure 资源组模板或 Terraform，我们可以为您创建的每个功能分支提供和部署到环境中
*   我们可以为测试人员提供测试环境，并在下午 5 点后取消提供
*   我们可以运行部署之外的流程。任何类型的“维护”或“操作”过程或操作手册都可以在 Octopus 中建模:
    *   灾难恢复故障转移流程
    *   运行状况检查和合规性检查流程
    *   将生产数据库备份恢复到您的测试环境
    *   针对最近部署的环境运行自动化 UI 测试
*   按计划运行这些程序，使用参数手动运行，或者在监控工具发出警报时作为挂钩运行

我们已经花了很多时间来设计这些想法和故事，我们相信我们可以在不损害章鱼“做一件事并把它做好”的哲学的情况下实现这一点。

## 可扩展性和持续改进

Octopus 正在成为越来越多公司的标准部署工具，我们希望确保这是一种无缝的体验。今年，我们将为那些经常使用八达通的人做出重大改进:

*   一个新的“空间”功能可以让你把你的 Octopus 服务器按团队或部门分开，给每个人独立的项目、环境、授权等等。
*   “工人”将允许今天标记为“在 Octopus 上运行”的脚本在其他地方运行(就像构建代理)。这将使你的八达通服务器更容易隔离副作用。
*   我们将对 Octopus 的可扩展性和性能进行重大改进。这对于拥有云托管 Octopus 的我们来说是必要的，但也将使 Octopus 客户受益匪浅。管理云托管的 Octopus 将让我们更深入地了解大规模运行 Octopus 是什么样子。
*   我们将继续寻求性能、质量和用户体验的改进。
*   我们将建立[远程发布促销](https://octopus.com/blog/remote-release-promotions-rfc)。
*   我们将继续处理用户意见建议，我们将确保对任何进入前 10 名的事情做些事情(或者，明确决定我们不做这些事情)。今天这些是:
    *   **[700 票](https://octopusdeploy.uservoice.com/forums/170787/suggestions/12948603)** 复合步骤模板
    *   **[570 票](https://octopusdeploy.uservoice.com/forums/170787/suggestions/6169634)**‘演习’部署
    *   **[447 票](https://octopusdeploy.uservoice.com/forums/170787/suggestions/6599104)** 周期性预定部署
    *   **[442 票](https://octopusdeploy.uservoice.com/forums/170787/suggestions/15698781)** 版本控制配置
    *   **[365 票](https://octopusdeploy.uservoice.com/forums/170787/suggestions/6986441)** 权限属性为变量集，库变量集，甚至变量
    *   **[317 票](https://octopusdeploy.uservoice.com/forums/170787/suggestions/9196032)** 输出变量为离线滴
    *   **[310 票](https://octopusdeploy.uservoice.com/forums/170787/suggestions/9811932)** 允许项目依赖——因此部署一个项目将自动部署所有依赖的项目
    *   **[288 票](https://octopusdeploy.uservoice.com/forums/170787/suggestions/5731235)** 环境组
    *   **[263 票](https://octopusdeploy.uservoice.com/forums/170787/suggestions/17930755)** 支持 Kubernetes
    *   **[257 票](https://octopusdeploy.uservoice.com/forums/170787/suggestions/6298548)** 允许预定部署的批准步骤发生在实际部署之前

## 包扎

> “没有一个作战计划能在与敌人的第一次接触中幸存”
> 
> 赫尔穆特·冯·毛奇

我们可能无法在 2018 年完成清单上的所有事情，随着时间的推移，我们可能会做一些清单上没有的事情。我相信每个阅读这篇文章的人都会理解这就是路线图的本质——这是我们试图勾勒出今年的计划和目标，而且很可能会改变。也就是说，我希望这个清单上有适合每个人的东西。愉快的部署！

## 了解更多信息