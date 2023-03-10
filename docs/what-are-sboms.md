# 什么是 SBOMs？-章鱼部署

> 原文：<https://octopus.com/blog/what-are-sboms>

如果您是一名开发人员或管理一个企业软件应用程序，您可能会被问到应用程序中的组件。人们为什么想知道？客户希望信任您的应用程序，他们希望您的应用程序是安全的。企业供应商和政府机构想知道，因为他们关心使用您的软件的客户的安全问题。

软件应用程序由几个来源组成，开源的，内部的，或者两者的混合。随着依赖列表的增长，如果用于构建应用程序的单个组件都不知道，那么应用程序如何安全呢？

物料清单列出了可靠生产给定产量所需的库存。多年来，物料清单一直被用来提供制造过程中的透明度和可重复性。软件材料清单(SBOMs)应用了类似的概念。SBOMs 在一个列表中逐项列出了软件应用程序中的组件，开发人员可以跨团队共享这些组件。

## 关于改善国家网络安全的行政命令

2021 年 5 月 12 日，美国政府发布了一项关于改善国家网络安全的行政命令。

> 联邦政府必须动用其全部权力和资源来保护和保护其计算机系统，无论是基于云的、内部部署的还是混合的。保护和安全的范围必须包括处理数据的系统(信息技术(IT))和运行确保我们安全的重要机制的系统(操作技术(OT))。

该行政命令试图在人们获取软件时，将供应链中的网络安全风险降至最低。随着软件应用程序中未知组件数量的增加，风险也会增加。

该行政命令要求美国政府购买的所有软件生产 SBOM。这对美国内外的商业都有几个影响。出于安全目的，任何美国政府项目现在都必须生产 SBOMs。任何不能为其产品生产 SBOMs 的供应商都不会被批准参与政府项目。

该订单可能是世界各地需要 SBOMs 的许多类似订单的开始。随着对 sbom 认识的提高，企业可能会开始要求 sbom。如果你从事软件行业，未来很可能会有 SBOMs。

## SBOMs 包含哪些内容？

国家电信和信息管理局(NTIA)提供了关于构建 SBOM 的指南。NTIA 在医疗保健领域进行了 SBOM 概念验证，验证了 SBOM 所需的[基线要素。](https://ntia.gov/files/ntia/publications/howto_guide_for_sbom_generation_v1.pdf)

基线要素包括:

*   作者姓名-描述主要组件的 SBOM 文档的作者。作者可能与主要组件的供应商不同。
*   供应商名称-组件的供应商。
*   组件名称-组件的名称。
*   版本字符串-组件的版本。
*   组件散列-用于标识组件的二进制实例的加密散列。
*   唯一标识符-组件的唯一标识符。一个元素可能存在多个标识符，因为不同的系统可能使用另一个标识符。
*   关系-用于确定一个组件包含另一个组件。此外，Relationship 用于记录关于包含在另一个组件中的组件列表的完整性的知识。
*   组件关系
*   主要组件–由 SBOM 描述的组件。
*   包含的组件–包含在另一个组件中的组件。

## 为什么 SBOMs 很重要？

对 SBOMs 的要求会对软件产生重大影响。软件是协作构建的，通常包含几个使用其他第三方库的第三方库。如果没有生成 SBOMs 的能力，软件就不会符合行政命令。

根据行政命令行事的政府机构和组织需要选择能够按需生成 SBOM 并且能够证明每个组件都不是网络安全风险的软件。SBOMs 的广泛使用将增加供应商和政府机构之间的信任。

软件供应商需要一种可靠的方法来检测他们部署的应用程序中的任何已知漏洞。SBOMs 让您能够主动应对风险，降低您的工具被黑客攻击的可能性。漏洞扫描还可以帮助您避免与那些自己扫描您的应用程序并报告漏洞的客户进行尴尬的交谈。

对 SBOMs 的需求可以被看作是生成工件的构建过程中的一个额外步骤，但是它确实提出了一些问题，例如:

*   如何将一个 SBOM 与一个可部署的工件配对，以便一个可以与另一个匹配？
*   您如何知道应用程序的生产版本，以便在应用程序部署后的几周或几个月内扫描相关的 SBOM？
*   如何协调应用程序的部署并发布相关的 SBOM？
*   您如何安排 SBOM 扫描来主动检测新发现的漏洞？
*   如何扫描旧的 SBOM 版本来识别包含易受攻击组件的软件的早期版本？

在八达通，我们建立了一个免费的工具，为您回答所有这些问题，并满足您的 SBOM 要求。

## Octopus Workflow Builder 如何帮助满足 SBOM 要求

Octopus Workflow Builder 帮助您生成 SBOMs 并将其构建到您的部署流程中。该工具演示了如何在构建过程中构造 SBOM 文件，然后演示了 Octopus Deploy 如何在部署过程中扫描 SBOM。

使用该构建器，您将获得一个示例项目，该项目展示了可部署的构件及其相关的 SBOM 在一个版本中是如何紧密耦合的，允许您编排和发布具有任何应用程序部署的 SBOM，安排 SBOM 扫描，或者访问旧的 SBOM 版本。

## 结论

2021 年，美国政府发布了一项行政命令，以改善国家的网络安全。该命令要求政府知晓软件组件，以最大限度地降低安全风险。如果您从事软件工作，这需要您公开您的应用程序的组件，否则会有被排除在政府 IT 相关项目之外的风险。

您可以通过 SBOMs 让您的软件组件为人所知。SBOMs 是软件应用程序中可共享的组件列表，在每个应用程序版本中自动生成。

为了帮助您满足这一需求， [Octopus Workflow Builder](https://octopusworkflowbuilder.octopus.com) 在构建过程中生成 SBOMs，并在部署过程中扫描它们。

愉快的部署！