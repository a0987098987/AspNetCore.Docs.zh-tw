---
uid: webhooks/source
title: "ASP.NET Webhook 原始碼和 NuGet 套件 |Microsoft 文件"
author: rick-anderson
description: "ASP.NET Webhook 原始碼和 NuGet 套件的連結"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="49ca7-103">ASP.NET Webhook 原始碼和 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="49ca7-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="49ca7-104">Microsoft ASP.NET Webhook 屬於模組的 Microsoft ASP.NET 系列和裝載為[GitHub 上的開放原始碼專案](https://github.com/aspnet/WebHooks)。</span><span class="sxs-lookup"><span data-stu-id="49ca7-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="49ca7-105">這表示我們接受參與，但請查看[比重指導方針](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md)之前提交提取要求。</span><span class="sxs-lookup"><span data-stu-id="49ca7-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="49ca7-106">您正在閱讀這個線上文件現在也裝載為[GitHub 上的開放原始碼](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide)和也可接受的貢獻。</span><span class="sxs-lookup"><span data-stu-id="49ca7-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="49ca7-107">Nuget 封裝</span><span class="sxs-lookup"><span data-stu-id="49ca7-107">Nuget Packages</span></span>

<span data-ttu-id="49ca7-108">Microsoft ASP.NET Webhook 也是可以找到預覽 Nuget 封裝，這表示您必須選取 Visual Studio 中的預覽旗標，才能看到它們。</span><span class="sxs-lookup"><span data-stu-id="49ca7-108">Microsoft ASP.NET WebHooks is also available as preview Nuget packages which means that you have to select the Preview flag in Visual Studio in order to see them.</span></span>

<span data-ttu-id="49ca7-109">[Nuget 封裝](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks)devided 分成三個部分：</span><span class="sxs-lookup"><span data-stu-id="49ca7-109">The [Nuget packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are devided into three parts:</span></span>

* <span data-ttu-id="49ca7-110">[一般](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common)： 常見的封裝傳送者與接收者之間共用。</span><span class="sxs-lookup"><span data-stu-id="49ca7-110">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="49ca7-111">[寄件者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom)： 一組支援您自己的 Webhook 傳送給其他人的封裝。</span><span class="sxs-lookup"><span data-stu-id="49ca7-111">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="49ca7-112">傳送 Webhook 功能更詳細地說明[傳送 Webhook](sending/index.md)。</span><span class="sxs-lookup"><span data-stu-id="49ca7-112">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="49ca7-113">[接收者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)： 一組支援接收 Webhook 與其他的封裝。</span><span class="sxs-lookup"><span data-stu-id="49ca7-113">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="49ca7-114">接收 Webhook 功能更詳細地說明[接收 Webhook](receiving/index.md)。</span><span class="sxs-lookup"><span data-stu-id="49ca7-114">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
