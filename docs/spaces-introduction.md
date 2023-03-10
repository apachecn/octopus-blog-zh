# 空间-一种新的方式来组织你的八达通服务器-八达通部署

> 原文：<https://octopus.com/blog/spaces-introduction>

[![Teams using Octopus Spaces illustration](img/143c89fd788a44ed02a37a901d45da80.png)](#)

Octopus Deploy 帮助团队自动部署他们的应用程序和服务。随着时间的推移，随着他们不断添加项目、环境和机器，找到他们正在做的事情会变得很困难。如果这听起来很熟悉，那么您可能已经滚动了很长时间来查找您想要部署的项目，水平滚动来查找部署到环境中的最新版本，或者查看包含数百个项目的下拉列表。

这可能会令人沮丧，我们非常高兴地分享我们一直在致力于名为 Spaces 的解决方案。空间是一种组织你的八达通服务器的新方法。将您团队的项目、环境、机器、租户、图书馆资源等归入一个专为您和您的团队准备的“空间”将变得更加容易。

## 专为团队打造

共享空间是为团队打造的一项功能。

大多数开发人员在团队中构建和维护一个或多个项目以及相关的基础设施。这包括环境、机器、Azure 或 AWS 账户、step 模板、变量集等等。然而，公司是不同的。有些公司按部门组织团队。有些人按功能组织它们。空间使团队能够根据需要组织他们的项目和基础设施，并且他们可以是多个空间的成员。我们的 Spaces switcher 使空间之间的切换变得很容易，所以你不会看到数十或数百个不相关的项目和其他 Octopus 资源，你只会看到该空间的资源。

[![switcher user interface](img/cbd259316be1b6ad1ea836f4817e6f09.png)](#)

## 空间所有者

在与客户交谈后，很明显，Spaces 的关键驱动因素之一是能够委派一系列责任。通过这种方式，Octopus 管理员可以完全控制一个空间，如果他们愿意，可以完全放手。让管理员专注于系统问题，如添加新用户、管理任何系统团队和安装配置。对于希望直接参与团队空间的管理员，他们可以将自己添加为具有空间访问权限的团队成员。

[![spaces configuration user interface](img/64854a64811b4f823be23c35a4a239e7.png)](#)

## 坚硬的墙壁

> 空间 A 中的项目不应该能够从空间 B 部署到机器上

空间是关于隔离而不是分享。我们希望确保首先获得安全和隔离。很容易想象 Octopus 中的任何内容被共享的场景。当我们开始建造空间并进入实施细节时，我们清楚地意识到，如果没有真正的隔离，你的章鱼的状态会变得非常复杂。

我们在沙地上为空间划了一条线，将您的项目部署到环境和租户中的目标的所有基本要素都在一个空间内。

## 空间，当你需要的时候

共享空间是完全自愿加入的。我们已经做了大量的工作和测试，以确保该功能尽可能向后兼容。我们已经引入了“缺省空间”的概念，它允许我们像现在一样支持大多数 API。空间会在你需要的时候出现，转移到空间可以是一个渐进的过渡。

你今天使用八达通的方式在很大程度上不会改变，我们会有具体的细节发布。总的来说，Octopus 将更加一致地执行权限，并且围绕管理安装的一些 API 端点已经改变，例如[团队/权限](/blog/team-configuration-improvements)。

## 批准

[![Licensing](img/9247d320af638679ada51c8b4c9196ac.png)](#)

在与我们的现有客户交谈后，他们对业务单位和各种团队以及基础设施的分离的想法和愿望推动了空间的设计。

每个空间中存在的代表相同物理硬件的部署目标都将计入您的机器限制。

### 自托管

*   使用不受限制的社区(免费)、专业、团队或企业许可的客户只能使用一个空间。你可以升级到一个新的许可证，将你的八达通分成多个空间。
*   使用不受限制的高可用性许可证的客户可以获得无限的空间。
*   拥有标准许可证的客户将获得三个车位。您可以升级到数据中心以获得更多空间。
*   拥有标准无限或数据中心许可证的客户将获得无限空间。

### 章鱼云

*   新的和现有的标准版客户将获得无限的空间。
*   旧入门版上的现有客户将获得 1 个空间。

## 结论

我们一直在努力工作空间，我们很高兴它即将完成。我们计划在 2019 年 1 月发货。请继续关注即将发布的另一篇文章，了解更多细节。

与此同时，如果您还没有看过我们最近的[网络研讨会，我们在会上讨论了员工旁边的空间](https://hello.octopus.com/webinar-spaces-workers/on-demand)。