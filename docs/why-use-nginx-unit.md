# 使用 NGINX 单元有什么好处？-章鱼部署

> 原文：<https://octopus.com/blog/why-use-nginx-unit>

[![NGINX Unit web server](img/304025ef70eb1f2fc28dbde2e08c51bf.png)](#)

NGINX 是当今最流行的 web 服务器和反向代理之一。它提供了高性能和几乎无限的可配置性，是 Kubernetes 等现代堆栈中常用的组件。现在，NGINX 团队有了一个名为 NGINX Unit 的新产品，旨在解决现代开发过程中的一些常见挑战。

如果你跳转到 [NGINX 单元文档](https://unit.nginx.org/#about)，你会发现一个精彩的来自 NGINX Conf 17 的[现场演示。在同一个会议上还有一个](https://youtu.be/I4IWEz2lBWU)[概述会议](https://www.youtube.com/watch?v=EU78CIR3CeU)。这些演示很好地回答了 NGINX 单元是做什么的，但仍然让我想知道为什么有人会选择使用 NGINX 单元。

在这篇博文中，我们将看看 NGINX 单元提供的一些优势。

## 标准化的 JSON 配置文件

传统的 NGINX 配置文件看起来像这样:

```
server {
  location / {
    fastcgi_pass localhost:9000;
    fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
    fastcgi_param QUERY_STRING  $query_string;
  }

  location ~ \.(gif|jpg|png)$ {
    root /data/images;
  }
} 
```

虽然花引号语言的开发人员应该对这种格式相当熟悉，但 NGINX 配置文件语法不符合任何通用标准。如果您想定期更新这个文件，您可以编写复杂的`sed`命令来编辑原始文本。

但是使用正则表达式修改配置文件并不是一种愉快的体验。不可避免地，你会发现你的正则表达式匹配了一些你没有预料到的东西，没有考虑到行尾或者没有完全捕捉到一个值的所有变化。

NGINX 单元通过利用 JSON 进行配置来解决这个问题。对于如何构造配置数据不再有任何模糊之处，而且以编程方式更新配置值要容易得多。依靠公共数据格式使得 NGINX 单元更容易管理。

## HTTP 配置 API

每一个现代计算平台都有一个由结构良好的 API 支持的丰富的 CLI 工具。很容易认为这个功能是理所当然的，直到您发现自己在对一个配置文件运行`sed`，然后重新启动一个服务。

虽然 NGINX 单元没有提供 CLI 工具(文档中的所有例子都使用了`curl`，但是它通过一个易于理解的 HTTP API 公开了所有的配置。这为您提供了很大的灵活性来选择如何公开 API(即，在本地主机上公开它或者[安全地代理它以使它公开可用](https://unit.nginx.org/howto/integration/#securely-proxying-unit-api))，这意味着您可以使用您选择的任何脚本工具来与之交互。

## 常见用例的声明性模型

鉴于 NGINX 无处不在，我发现自己不得不每年阅读几次随机的 NGINX 配置文件。每次我都必须查阅文档来回忆这些命令的作用，每次我都发现自己必须从命令序列中逆向工程配置的意图。

NGINX 单元摒弃了这种命令式配置模型，而是公开了一种声明式配置模型。诚然，NGINX 单元模型远不如传统的 NGINX 配置文件可配置，但它很好地展示了您将使用的常见路由和安全选项。

声明式配置模型使得 NGINX 单元对于那些有配置云服务和像 Kubernetes 这样的平台的经验的人来说更加容易接近。

## 一致的网络层

多语言开发越来越受欢迎。无论是因为您的团队找到了最适合他们需求的混合语言，还是因为您的基础架构包括用多种语言编写的第三方产品，拥有一个包含多种编程语言的应用程序堆栈并不罕见。

然而，网络层仍然需要以一种内聚的方式来处理，当每个应用程序都有一个以稍微不同的方式配置的网络组件时，这是一个挑战。

NGINX 单元是一种日益增长的趋势的一部分，它将网络问题从单个应用程序中分离出来，使之成为基础设施层的责任。NGINX 单元通过公开一个公共 API 和处理安全和路由等公共网络任务来整合网络问题。

## 无需重启的重新配置

NGINX 单元在它托管的应用程序进程和它置于顶层的网络层之间提供了一个清晰的分离。这使得无需重启托管应用程序就可以更改网络配置。这也意味着网络更改可以应用到正在运行的系统中，而无需停机。

将这种将更改应用到实时系统的能力与 NGINX 单元托管的所有应用程序都可以利用的一致网络配置相结合，您就拥有了一个可以随着部署数量和应用程序版本的增加而轻松扩展的基础架构。

## 采用门槛低

NGINX 单元只需几秒钟就能安装完毕，只需一个命令就能启动，并且只需对配置进行一次更新，就能托管您的第一个应用程序。它不需要代理、控制平面、数据库或专用集群来运行。

与普通云部署或第一次启动和运行 Kubernetes 集群的工作量相比，NGINX 是一股新鲜空气。

你所看到的就是你用 NGINX 单元得到的，使它易于部署、推理和诊断。

## 将应用程序作为服务进行管理

您目前如何管理 Node.js 应用程序的生产部署？Node.js 本身并没有真正提供一个解决方案，这已经产生了一系列的解决方案，如 [PM2](https://pm2.keymetrics.io/) 、[永远](https://www.npmjs.com/package/forever)、nohup 或通过 systemd 的本地服务管理。

NGINX Unit 是托管传统上依赖第三方解决方案来引导、运行和重启进程的应用程序的有效替代方案，其额外的好处是它可以托管用多种语言编写的应用程序。

## 使用单位的缺点

使用 NGINX 单元时，有一些缺点，或者至少是需要注意的问题:

*   没有 Windows 端口，这使得 NGINX 单元仅限于 Linux 和 MacOS 商店。
*   必须修改 Go 和 Node.js 代码库才能与 NGINX 单元一起工作。
*   NGINX 单元仅限于证书处理和基本路由。例如，您不会发现认证、复杂的重写规则、故障注入、重试、加权流量分割等。
*   NGINX 单元不为它管理的进程提供健康检查。

## 结论

总的来说，我得到的印象是 NGINX Unit 就是 NGINX 如果今天写出来的样子。它提供了一个基于 JSON 的配置模型，通过 HTTP API 公开该配置，并提供了一个用于配置最常见的网络用例的声明性模型，所有这些都是轻量级的，并且易于运行。

这种回归基础的方法确实意味着 NGINX 目前支持的一些用例在 NGINX 单元中是不可能的，但随着每隔几个月就会有一个[新版本，我预计 NGINX 单元的功能将会增长。](https://unit.nginx.org/CHANGES.txt)

如果 NGINX 单元提供的功能满足您今天的需求，那么它是部署传统 NGINX 服务的自然选择。