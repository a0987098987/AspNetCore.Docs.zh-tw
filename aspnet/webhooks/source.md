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
ms.openlocfilehash: 733a0839c77bcfc96214bdf235ce8fe22ee2d3cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2018
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="09ab0-103">ASP.NET Webhook 原始碼和 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="09ab0-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="09ab0-104">Microsoft ASP.NET Webhook 屬於模組的 Microsoft ASP.NET 系列和裝載為[GitHub 上的開放原始碼專案](https://github.com/aspnet/WebHooks)。</span><span class="sxs-lookup"><span data-stu-id="09ab0-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="09ab0-105">這表示我們接受參與，但請查看[比重指導方針](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md)之前提交提取要求。</span><span class="sxs-lookup"><span data-stu-id="09ab0-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="09ab0-106">您正在閱讀這個線上文件現在也裝載為[GitHub 上的開放原始碼](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide)和也可接受的貢獻。</span><span class="sxs-lookup"><span data-stu-id="09ab0-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="09ab0-107">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="09ab0-107">NuGet packages</span></span>

<span data-ttu-id="09ab0-108">[NuGet 封裝](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks)分為三個部分：</span><span class="sxs-lookup"><span data-stu-id="09ab0-108">The [NuGet packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are divided into three parts:</span></span>

* <span data-ttu-id="09ab0-109">[一般](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common)： 常見的封裝傳送者與接收者之間共用。</span><span class="sxs-lookup"><span data-stu-id="09ab0-109">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="09ab0-110">[寄件者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom)： 一組支援您自己的 Webhook 傳送給其他人的封裝。</span><span class="sxs-lookup"><span data-stu-id="09ab0-110">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="09ab0-111">傳送 Webhook 功能更詳細地說明[傳送 Webhook](sending/index.md)。</span><span class="sxs-lookup"><span data-stu-id="09ab0-111">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="09ab0-112">[接收者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers)： 一組支援接收 Webhook 與其他的封裝。</span><span class="sxs-lookup"><span data-stu-id="09ab0-112">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="09ab0-113">接收 Webhook 功能更詳細地說明[接收 Webhook](receiving/index.md)。</span><span class="sxs-lookup"><span data-stu-id="09ab0-113">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
