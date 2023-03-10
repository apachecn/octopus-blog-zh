# Azure 环境- Octopus 部署

> 原文：<https://octopus.com/blog/azure-envs>

在过去的几个月里，我们收到了越来越多的请求，要求支持 Azure 环境而不是 AzureCloud，例如 Azure 德国、Azure 中国、Azure 美国政府等。

最近，我们接受了一个针对 Calamari 的 Pull 请求，这样它就可以使用备用端点连接到其他 Azure 环境。这种配置是通过设置特殊变量来完成的，这些变量在部署执行时传递给 Calamari。举个例子，

![](img/06d2e40f697b1f7fcf0e7ec352ae878c.png)

这种方法有效，但是有两个限制。首先，变量的名称和值不容易被发现，其次，这些变化只允许 Calamari 处理不同的 Azure 环境。后一个限制的含义是，你不能在 Azure 步骤的 UI 中使用选择器，例如选择资源组或 Web 应用程序，因为它们不知道特殊变量或如何处理它们。解决方法是插入包含资源组名称等字符串文字的变量，也就是说，您只需知道要部署到的内容的字符串名称。例如，对于一个 Azure Web 应用程序，你必须有这样的东西

![](img/6484c9f897fb6fd398b5e58a1368cae6.png)

所以你可能已经从我所有的过去时态的谈话中猜到了，我们已经更新了 Octopus Deploy server 来更好地处理 Azure 环境。该更新(在 Octopus Deploy 3.9 中可用)建立在之前的工作基础上，但允许您将环境和端点覆盖指定为 Azure 帐户定义的一部分。这样，当连接到 Azure 时，服务器和 Calamari 都可以看到并使用这些值。这是一个新的 Azure 账户页面的例子

![](img/32510b45a48a86c3ce4065ee5e9e3302.png)

对于那些目前使用变量将值传递给 Calamari 的人来说，这种方法在升级后应该可以继续工作，只是现在不需要这样做了。服务器现在将根据帐户设置自动设置相同的变量，因此升级后您可以逐步使用帐户，然后您将拥有能够在 UI 中选择值的优势。

## 理论上，这一切都很好

你可能已经注意到了上一节中的几个“应该”。全面测试这种类型的功能是棘手的，我们没有订阅任何替代 Azure 环境，所以完全有可能会有一些小问题需要解决。这也是该功能的初始版本。我们最终希望能够让用户从下拉列表中选择环境，并在后台填充所有端点覆盖，但是我们还没有一个可靠的方法来以编程方式加载这些数据。目前，这应该可以让那些需要部署到这些 Azure 环境中的人畅通无阻，我们会随着时间的推移进一步改进 UI。

我们将密切关注反馈，并尽可能对这一领域的问题做出回应。

## 如果您需要部署到备用 Azure 环境

了解更多关于配置 Azure 环境的信息。