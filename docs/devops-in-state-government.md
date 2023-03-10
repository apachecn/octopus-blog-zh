# 在州政府实施 devo PS-Octopus 部署

> 原文：<https://octopus.com/blog/devops-in-state-government>

[![Illustration showing an infinite feedback loop surrounding a government building](img/a7fbfb771e74dc4b0984cfd1730a6622.png)](#)

政府通常是行动缓慢的官僚机构，但这并不意味着不可能在政府机构内部实施更好的流程。

2011 年，我被一家小型美国州政府机构聘为配置经理。我有一个挑战性的任务，花了好几年才完成:

*   自动化手动流程，提高软件部署的可靠性。
*   减少交付软件的时间长度。
*   消除周末部署的需要。

在这篇文章中，我将介绍我实现这一目标的方法，以及在类似环境中你可能会遇到的一些常见陷阱。

## 优先处理最大的问题

我加入的机构有很多问题，所以我做的第一件事就是学习一切是如何构建和运作的。然后，我优先考虑我要改善的第一步。该机构有几个内部应用程序，它们的部署过程使它们很容易出现问题。Web 应用构建是在开发人员的机器上完成的，被压缩并复制到一个文件共享中。数据库更改是通过用一个列出执行顺序的文档压缩一堆脚本来处理的，然后这个文档被复制到一个文件共享中，供 DBA 获取。不可避免地，部署会因为以下任何一个原因而失败:

*   一个开发者没有提到需要安装在 web 服务器上的第三方依赖。
*   数据库更改的脚本没有针对生产数据库的当前状态进行测试。
*   老式的人为失误。

事情必须改变。

## 早期胜利

我从构建过程的开始就开始了，这样我就可以消除那句格言:“在我的机器上工作，现在是操作问题。”该团队使用 Microsoft Team Foundation Server 进行源代码控制，这意味着构建控制器技术已经存在。我安装了控制器和几个代理，这样所有的软件都是在独立的机器上构建的。这突出了存在于开发人员机器上并且需要安装在服务器上的依赖性。

接下来，我编写了几个小的控制台应用程序来部署 web 代码和数据库。第一个使用 Microsoft Web Deploy 来自动化 Web 代码的一致部署。我们仍然手动更新连接字符串，这不是很好，但这是一个开始。第二个控制台应用程序在单个事务中运行一系列数据库脚本，并在出现故障时回滚数据库部署。这种方法减少了错误率和部署时间，因为数据库管理员不再需要打开脚本并手动执行它们。

有了这两项改进，怀疑和不愿改变的情绪开始消退。后来，我们将控制台应用程序合并到一个自动化部署解决方案中，进一步降低了部署失败率，加快了部署过程，几乎消除了周末工作的需要。这一进展让开发人员和操作人员更加高兴。

该团队使用我的内部部署解决方案，直到一个承包商演示了他用于自动化部署的工具 Octopus Deploy。我不愿意放弃我的创作，但我也承担了数据团队主管的责任，这意味着我有更少的时间来编码，并且我无法跟上功能需求。另一方面，Octopus Deploy 有一组开发人员专职从事这项工作。抛开我的骄傲，我只用了几个星期就用 Octopus 复制了我的解决方案的功能，几个月后 Octopus 就被完全采用了。

我很高兴用我开发的内部工具实现了一些早期改进，但从长远来看，转向现成的解决方案是正确的。

## 从对抗到合作

随着我们[自动化了更多的流程](https://octopus.com/devops/continuous-delivery/automate-everything/)，团队之间的紧张和持续的相互指责开始缓解。有了定义良好的自动化流程，团队开始[一起解决问题](https://octopus.com/devops/continuous-delivery/continuous-delivery-principles/#5-everyones-responsible)而不是寻找互相指责的方法。起初这并不容易，但我通过与团队单独交谈，让他们参与进来，然后一起就新流程达成一致，从而与团队建立了信任。

## 不断进步和后续步骤

此时，我们的大多数开发和部署流程都是自动化的，下一步是审查我们在开发和运营方面的优先事项。困扰我们的一个问题是不一致的环境。我了解了代码基础设施，并立即接受了这个概念。由于对任何现有技术(Chef、Puppet、Ansible、PowerShell DSC 等)都没有经验，我决定尝试 PowerShell DSC(理想状态配置)，并且我很快就明白了为什么所有 PowerShell 课程都像这样说...还有 PowerShell DSC，但这本身就是一门完整的课程。”

Octopus Deploy 给了我一次很棒的 PowerShell 体验，但 DSC 是一种不同的动物。经过一点学习，我可以在几分钟内演示如何将一个裸机服务器(说实话，是一个 VM)配置成一个正常工作的 IIS 服务器。不仅如此，我还可以将 Octopus Deploy 的部署能力与 PowerShell DSC 结合起来，将配置推送到服务器，就像应用程序部署一样！既然已经有了 web 管理员，我转向了数据库管理员。与 DBA 团队合作，我们创建了一个 DSC 脚本来安装、配置和维护 SQL 服务器，并将其与 Octopus 连接起来。DBA 团队现在可以随时监控他们的服务器，并根据需要进行更改。这减少了运营团队和 DBA 团队之间的摩擦。

DSC 还减少了操作和应用程序开发之间的摩擦，因为操作不再需要参与 IIS 或 SQL Server 的安装或配置；他们的工作只关注硬件和虚拟机(VM)的运行状况。DSC 是通过 Octopus Deploy 使用服务帐户执行的，这意味着非运营人员不再需要服务器的管理员权限，这让安全人员非常高兴。因为 DSC 是自文档化和版本控制的，如果操作人员需要了解某些东西是如何配置的，他们可以咨询版本控制。

## (最终)结果

这些都不是一夜之间发生的。在这一点上，我们处于 2019 年初，接近我在州政府的职业生涯的尾声，但我已经实现了持续集成(CI ),每次执行签入时都会自动运行构建。大多数项目已经实现了[连续交付](https://octopus.com/devops/continuous-delivery/) (CD)，因此在 CI 构建完成之后，它会自动部署到较低级别的环境中，供测试人员和业务分析师开始他们的批准过程。我自动化了以下内容:

1.  部署 ASP.NET 网站代码。
2.  Windows 服务。
3.  数据库部署。
4.  SQL Server Reporting Services(SSRS)报表。
5.  SQL Server Integration Services(SSIS)包。
6.  控制台应用程序。
7.  Java 应用到野花。

我记得一位开发人员的称赞，他说他喜欢这样的事实，他可以点击合并，去喝咖啡，当他回来时，他的应用程序已经部署好了。

## 结论

在政府组织中引入变革和 DevOps 概念可能会很慢，也很有挑战性，但这绝对是可能的。我的成功之处在于:优先处理最大的问题，购买工具进行简化和标准化，注重沟通和协作，让其他团队参与进来，我们不断取得进步，让我们一个接一个地克服障碍。

浏览 [DevOps 工程师手册](https://octopus.com/devops/)了解有关 DevOps 和 CI/CD 的更多信息。